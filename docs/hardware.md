# Hardware

This document outlines the hardware components of the homelab.

## Physical Infrastructure

The list includes only devices set up with fixed IPs or holding critical roles. Consumer devices (phones, laptops, Alexa, etc.) are excluded.

### Servers

- **QNAP NAS**
    - **Role**: NFS Storage
    - **OS**: QTS
    - **IP**: `10.0.40.2`
    - **Notes**: Provides NFS for Proxmox (ISO, Backup), Docker data, Media files, K8s data. Ports: 80, 443, 22.

- **Proxmox Nodes (x3)**
    - **Role**: Hypervisor
    - **Hardware**: Minisforum Mini PCs
    - **OS**: Proxmox VE
    - **Notes**: Hosts VMs and Containers (including Docker media server).

- **Ceph Nodes (x3)**
    - **Role**: Distributed Storage
    - **Hardware**: Minisforum Mini PCs
    - **OS**: Proxmox VE
    - **Notes**: Dedicated storage cluster nodes.

- **Docker Media Server**
    - **Role**: Application Server
    - **OS**: Linux
    - **Notes**: Hosted on Proxmox.

- **Pi Zeros (x2)**
    - **Role**: Utility / DNS
    - **Hardware**: Raspberry Pi Zero
    - **OS**: Linux
    - **Notes**: Connected to UDM-PRO. Potential future home for DHCP.

### Network Gear

- **UDM-PRO**
    - **Model**: Unifi Dream Machine Pro
    - **Role**: Router / Firewall / Gateway
    - **Connections**:
        - **Gateway**: `.1` on ALL ranges.
        - **Uplink**: ISP (YourFibre).
        - **Downlinks**: Unifi 24P PoE, QNAP NAS, 2x Pi Zeros.
    - **Services**: DHCP, Firewall, Routing.
    - **Ports**: 80, 443, 22 open.

- **Main Switch**
    - **Model**: Unifi 24P PoE
    - **Role**: Core Switch
    - **Connections**:
        - **Uplink**: UDM-PRO.
        - **Downlink**: Unifi 8P PoE.
        - **PoE Devices**: 3x Cameras, 1x Doorbell, 1x Chime, 2x WAPs.

- **Cluster Switch**
    - **Model**: Unifi 8P PoE
    - **Role**: Distribution Switch
    - **Connections**:
        - **Uplink**: Unifi 24P PoE.
        - **Downlinks**: 3x Proxmox Nodes, 3x Ceph Nodes.
