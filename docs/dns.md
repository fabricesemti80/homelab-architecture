# DNS Strategy

This document outlines the DNS strategy for the homelab network. The goal is to provide ad-blocking via a local AdGuard Home instance while retaining the UniFi router for internal DNS resolution and ensuring redundancy.

## DNS Resolution Flow

The primary DNS resolver for trusted networks is an AdGuard Home instance located at `10.0.40.53`.

The resolution path is as follows:
1.  **Client Device** sends a DNS query to AdGuard Home (`10.0.40.53`).
2.  **AdGuard Home** filters the request against its blocklists.
3.  If the query is for the internal `krapulax.home` domain, AdGuard forwards it to the UniFi router (`10.0.40.1`) for local resolution.
4.  For all other queries, AdGuard forwards them to its configured upstream DNS providers (e.g., Quad9).

## Upstream DNS (AdGuard Configuration)

The AdGuard Home instance is configured in **Settings -> DNS settings -> Upstream DNS servers**. The following combined configuration provides external resolution via Quad9 (using DNS-over-HTTPS) and internal resolution via the UniFi router:

```
https://dns10.quad9.net/dns-query
[/krapulax.home/]10.0.40.1
```

## Network DNS Resolution

### Trusted Networks
The following networks are configured via DHCP to use the AdGuard Home instance as their primary DNS server, with the UniFi router as a secondary for fallback:

- **USERS (`10.0.10.0/24`)**: `10.0.40.53`, `10.0.10.1`
- **DEV-INFRA (`10.0.30.0/24`)**: `10.0.40.53`, `10.0.30.1`
- **PROD-INFRA (`10.0.40.0/24`)**: `10.0.40.53`, `10.0.40.1`

### Isolated Networks (Exceptions)
The following networks remain isolated and use public DNS servers directly to prevent access to the internal network:

- **GUEST Network**: Uses OpenDNS for content filtering and security.
- **IOT Network**: Uses OpenDNS to isolate IoT device traffic.

The OpenDNS servers used are:
- `208.67.222.222`
- `208.67.220.220`

## Internal DNS

Internal network resolution for the `krapulax.home` domain is managed by the UniFi router. As specified in the upstream DNS configuration, AdGuard uses a conditional forwarding rule (`[/krapulax.home/]10.0.40.1`) to direct any queries for this domain to the router (`10.0.40.1`). This keeps local DNS management centralized on the UniFi platform.

## Public DNS

For services that are accessible from the internet, the domain `krapulax.dev` is used. The DNS records for this domain are managed through Cloudflare.

## Verification

To verify that the AdGuard instance is correctly forwarding local domain queries to the UniFi router, use the `dig` command from a client machine on the network. Point the query directly at the AdGuard server IP (`10.0.40.53`).

A successful query for an internal hostname (e.g., `dokploy.krapulax.home`) should return the correct internal IP address from the AdGuard server, indicating that the conditional forwarding is working.

### Example Test

```sh
dig @10.0.40.53 dokploy.krapulax.home
```

A successful response will look similar to this, showing the internal IP in the `ANSWER SECTION`:

```
;; ANSWER SECTION:
dokploy.krapulax.home.	0	IN	A	10.0.30.20
```

## Tailscale DNS Configuration

Tailscale is configured to use Quad9 as its DNS resolver.

Additionally, Tailscale is set up to use the UniFi router (10.0.40.1) for name resolution within the `krapulax.home` domain. This ensures that internal services are resolvable via Tailscale while maintaining the DNS records management through the UniFi router.
