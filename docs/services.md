# Services

This document provides an overview of the services running in the homelab.

## Network Services

- **DHCP**
  - Provided by UDM-PRO
  - Scope: All VLANs
- **DNS**
  - Upstream: Quad9 (via UDM-PRO)
  - Internal: UDM-PRO (`krapulax.home`)
  - Guest/IoT: OpenDNS
- **VPN**
  - *To be configured / documented*

## Storage Services

- **NFS**
  - Provided by QNAP NAS
  - Targets:
    - Proxmox ISOs
    - Backups
    - Docker Data
    - K8s Data
- **Ceph**
  - Distributed storage provided by 3x Proxmox nodes

## Virtualization & Infrastructure

- **Proxmox VE**
  - 3-node cluster for compute
- **Kubernetes**
  - Cluster hosted on Proxmox nodes
- **Docker Host**
  - Dedicated VM for Media Server stack

## Applications

- **Media Server Stack**
  - Hosted on Docker
  - *Specific media apps (e.g., Plex, Jellyfin)*
  - *Download clients*
  - services
    - [ ] authentik
    - [ ] autobrr
    - [ ] bazarr
    - [ ] calibre
    - [ ] checkrr
    - [ ] deluge
    - [ ] emby
    - [ ] flaresolverr
    - [ ] heimdall
    - [X] homepage
    - [ ] huntarr
    - [X] jellyfin
    - [ ] jellyseerr
    - [ ] kavita
    - [ ] lidarr
    - [ ] maintainerr
    - [ ] netdata
    - [ ] notifiarr
    - [ ] nzbget
    - [x] overseerr
    - [ ] pasta
    - [ ] pinchflat
    - [X] plex
    - [ ] portainer
    - [ ] prowlarr
    - [ ] qbittorrent
    - [X] radarr
    - [ ] readarr
    - [ ] recyclarr
    - [ ] requestrr
    - [X] sabnzbd
    - [X] sonarr
    - [ ] speedtest-tracker
    - [ ] tautulli
    - [ ] tdarr
    - [ ] tinymediamanager
    - [X] traefik
    - [ ] transmission
    - [ ] tubearchivist
    - [ ] unpackerr
    - [ ] uptimekuma
    - [ ] wizarr

- **Home Automation**
  - *To be added*
  
- **Monitoring**
  - *To be added*
