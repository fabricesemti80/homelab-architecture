# Architecture Decisions & Roadmap

This document tracks key architectural decisions, their context, and the future roadmap for the `krapulax` homelab.

## Decisions

### 1. DNS Filtering with AdGuard Home

* **Decision**: Implement a self-hosted AdGuard Home instance for network-wide DNS filtering, while retaining the UniFi router for internal DNS resolution.
* **Context**: This decision was made to enhance network security and block advertisements without sacrificing the convenience of local DNS management provided by the UniFi router.
* **Details**: For a comprehensive overview of this decision, see [ADR-0001: Implement AdGuard for DNS Filtering](./architecture-decisions/0001-implement-adguard-for-dns-filtering.md).

### 2. Secret Management

* **Decision**: Transition to **1Password** for secret storage, injecting secrets at runtime.
* **Context**: Currently, secrets often live in local `.env` files. While these are backed up via iCloud Drive/Time Machine (mitigating data loss), they pose a security risk (plain text on disk).
* **Goal**: Move to a "zero-trust" model where secrets are pulled from 1Password via CLI or Operator at deployment time, avoiding persistent files on disk.

### 3. Storage & Single Points of Failure (SPOF)

* **Decision**: Use **QNAP NAS** for bulk storage and Backups, but aim to migrate "Hot" data to **Ceph**.
* **Context**: The QNAP NAS is currently a SPOF for Docker/K8s volumes. If it fails, services stop.
* **Strategy**:
    1. Leverage the 3-node Ceph cluster for "live" application data (high availability).
    2. Retain QNAP for massive static media (Plex libraries) and backup targets (Proxmox Backup Server).
    3. **Gap**: The NAS itself needs a backup strategy (e.g., external USB HDD or secondary NAS).

## Roadmap / Todo List

### Short Term (Immediate Improvements)

- [ ] **Secret Migration**: Systematically replace local `.env` file references with `1password` CLI injection in scripts/Terraform.
* [ ] **NAS Backup**: Implement a "Cold Backup" for the QNAP NAS (e.g., large external USB drive) to mitigate total data loss risk.

### Medium Term (Architecture Evolution)

- [ ] **Data Migration**: Move critical "live" Docker/Kubernetes volumes from NFS (QNAP) to Ceph (Proxmox Cluster) to remove the storage SPOF.
* [ ] **Ceph Networking (Dual-Homed)**: Confirm the Proxmox/Ceph nodes maintain their dual-homed configuration during the migration to VLAN 50:
    1. **Management Interface** (VLAN 40): Has Default Gateway (`10.0.40.1`). Used for Proxmox clustering, SSH, and **OS Updates** (apt/internet access).
    2. **Storage Interface** (Moving to VLAN 50): High bandwidth, **NO Default Gateway**. Used *only* for Ceph replication/traffic.
  * *Status*: Current setup allows updates successfully via VLAN 40. This pattern must be preserved.

### Long Term (Scaling)

- [x] **Advanced DNS**: Deployed AdGuard Home for network-wide filtering, as documented in [ADR-0001](./architecture-decisions/0001-implement-adguard-for-dns-filtering.md).
