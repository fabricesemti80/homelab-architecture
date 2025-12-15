# Network Documentation

This document describes the network setup in the homelab. The network architecture aims to use K.I.S.S. principles. The central point of the system is the Unifi-based firewall and switch.

## Networks

The following networks are present:

| VLAN id | VLAN name | Subnet CIDR | DFGW | DHCP allowed | DHCP Scope | DNS servers | FW rules |
|---|---|---|---|---|---|---|---|
| Default | 192.168.1.0/24 | 192.168.1.1 | True | 192.168.1.1 - 192.168.1.254 | 192.168.1.1 | ALLOW ALL |
10 | USERS | 10.0.10.0/24 | 10.0.10.1 | True | 10.0.10.10 - 10.0.10.250 | 10.0.40.3, 10.0.40.4 | ALLOW ALL |
20 | IOT | 10.0.20.0/24 | 10.0.20.1 | True | 10.0.20.10 - 10.0.20.250 | 10.0.40.3, 10.0.40.4 | ISOLATED |
30 | GUEST | 10.0.30.0/24 | 10.0.30.1 | True | 10.0.30.10 - 10.0.30.250 | 1.1.1.1, 8.8.8.8 | ISOLATED |
40 | SERVERS | 10.0.40.0/24 | 10.0.40.1 | False | | 10.0.40.3, 10.0.40.4 | ALLOWED from USERS, SERVERS, MGMT |
50 | MGMT | 10.0.50.0/24 | 10.0.50.1 | False | | 10.0.40.3, 10.0.40.4 | ALLOWED from USERS |
60 | K8S-NODES | 10.0.60.0/24 | 10.0.60.1 | False | | 10.0.40.3, 10.0.40.4 | ALLOWED from USERS, SERVERS, MGMT |
70 | K8S-SERVICES | 10.0.70.0/22 | | | | | |
80 | K8S-PODS | 10.0.80.0/22 | | | | | |

## Network Diagram

```mermaid
graph TD
    subgraph "Unifi Network"
        A[UDM-PRO<br/>Firewall/Router]
    end

    subgraph "VLANs"
        B[Default<br/>192.168.1.0/24]
        C[USERS<br/>10.0.10.0/24]
        D[IOT<br/>10.0.20.0/24]
        E[GUEST<br/>10.0.30.0/24]
        F[SERVERS<br/>10.0.40.0/24]
        G[MGMT<br/>10.0.50.0/24]
        H[K8S-NODES<br/>10.0.60.0/24]
        I[K8S-SERVICES<br/>10.0.70.0/22]
        J[K8S-PODS<br/>10.0.80.0/22]
    end

    A --> B
    A --> C
    A --> D
    A --> E
    A --> F
    A --> G
    A --> H
    A --> I
    A --> J