# DNS Strategy

This document outlines the DNS strategy for the homelab network.

## Upstream DNS

The main router, a Unifi Dream Machine, is configured to use NextDNS as its upstream DNS provider. The specific servers are:

- `45.90.28.45`
- `45.90.30.45`

**Note:** For NextDNS to work correctly with a dynamic IP address, the public IP of the network must be linked. This is typically achieved by setting up DDNS (Dynamic DNS) on the Unifi Dream Machine.

## Network DNS Resolution

By default, all networks and VLANs are configured to use the Unifi Dream Machine as their DNS resolver. This ensures that most DNS traffic is filtered through NextDNS.

### Exceptions

The following networks are exceptions to the default rule and are configured to use OpenDNS servers directly:

- **GUEST Network**: Uses OpenDNS for content filtering and security.
- **IOT Network**: Uses OpenDNS to isolate IoT device traffic and prevent them from reaching the internal resolver.

The OpenDNS servers used are:

- `208.67.222.222`
- `208.67.220.220`

## Internal DNS

For internal network resolution, the domain `krapulax.home` is used. DNS records for this domain are managed directly on the Unifi Dream Machine. This allows for simple name resolution for internal services without exposing them to the public internet.

## Public DNS

For services that are accessible from the internet, the domain `krapulax.dev` is used. The DNS records for this domain are managed through Cloudflare, which also provides performance and security benefits.