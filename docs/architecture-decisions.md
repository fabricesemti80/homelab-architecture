# Architecture Decisions & Roadmap

This document tracks key architectural decisions, their context, and the future roadmap for the `krapulax` homelab.

## Decisions

### 1. DNS Strategy (Keep it Simple)
*   **Decision**: Continue using the **UDM-PRO** as the *sole* DNS resolver provided via DHCP for internal networks.
*   **Context**:
    *   **Single Source of Truth**: We must NOT add NextDNS IPs (e.g., `45.xx`) as secondary DNS servers in DHCP. Clients might query secondary servers randomly, bypassing the UDM. Since NextDNS does not know about `krapulax.home`, this would cause intermittent resolution failures for internal services.
    *   **Automation**: While dedicated solutions like Technitium or AdGuard Home offer better APIs, the UDM handles the current load adequately.
*   **Future Trigger**: As the migration to Kubernetes accelerates, the need for automated DNS record management (ExternalDNS) will grow. At that point, a dedicated DNS solution (running on a VM/Container, not Pi Zeros due to performance) will be re-evaluated.

### 2. Secret Management
*   **Decision**: Transition to **1Password** for secret storage, injecting secrets at runtime.
*   **Context**: Currently, secrets often live in local `.env` files. While these are backed up via iCloud Drive/Time Machine (mitigating data loss), they pose a security risk (plain text on disk).
*   **Goal**: Move to a "zero-trust" model where secrets are pulled from 1Password via CLI or Operator at deployment time, avoiding persistent files on disk.

### 3. Storage & Single Points of Failure (SPOF)
*   **Decision**: Use **QNAP NAS** for bulk storage and Backups, but aim to migrate "Hot" data to **Ceph**.
*   **Context**: The QNAP NAS is currently a SPOF for Docker/K8s volumes. If it fails, services stop.
*   **Strategy**:
    1.  Leverage the 3-node Ceph cluster for "live" application data (high availability).
    2.  Retain QNAP for massive static media (Plex libraries) and backup targets (Proxmox Backup Server).
    3.  **Gap**: The NAS itself needs a backup strategy (e.g., external USB HDD or secondary NAS).

## Roadmap / Todo List

### Short Term (Immediate Improvements)
- [ ] **Secret Migration**: Systematically replace local `.env` file references with `1password` CLI injection in scripts/Terraform.
- [ ] **NAS Backup**: Implement a "Cold Backup" for the QNAP NAS (e.g., large external USB drive) to mitigate total data loss risk.

### Medium Term (Architecture Evolution)
- [ ] **Data Migration**: Move critical "live" Docker/Kubernetes volumes from NFS (QNAP) to Ceph (Proxmox Cluster) to remove the storage SPOF.
- [ ] **Ceph Networking (Dual-Homed)**: Confirm the Proxmox/Ceph nodes maintain their dual-homed configuration during the migration to VLAN 50:
    1.  **Management Interface** (VLAN 40): Has Default Gateway (`10.0.40.1`). Used for Proxmox clustering, SSH, and **OS Updates** (apt/internet access).
    2.  **Storage Interface** (Moving to VLAN 50): High bandwidth, **NO Default Gateway**. Used *only* for Ceph replication/traffic.
    *   *Status*: Current setup allows updates successfully via VLAN 40. This pattern must be preserved.

### Long Term (Scaling)
- [ ] **Advanced DNS**: Deploy Technitium or AdGuard Home (virtualized) to replace UDM DNS when Kubernetes Ingress requirements become complex.