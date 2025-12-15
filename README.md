# Homelab Architecture Documentation

Welcome to the documentation for my homelab setup. This repository contains detailed documentation of the physical and virtual infrastructure, network configurations, and services running in the homelab environment.

## My aim and key principles are

1. `Automation` and `Infrastructure-as-Code` first
2. While I think complexity is unavoidable, I try to use `K.I.S.S` principles
3. The deployments heavily utilize tools such as `Ansible`, `Docker`, `Terraform`
4. For management of secrets currently there is two main options:
   1. I often relly on `env` files, which are generally excluded from version control and live only on the local system
   2. I recently started transitioning to use `1password` (primarily reading data from it with Terraform) via `op cli` to outsource secret storage
5. I favour to use open source tools and free tools, but if needed, I am happy to pay for software or services with a fairly limited budget (I currently have subscription for `1password` and VPN services)

## Contents

- [Network](docs/network.md) - Detailed network design, including VLANs, subnets, and firewall rules.
- [Hardware](docs/hardware.md) - An overview of the physical servers and network gear.
- [Services](docs/services.md) - A list of services running in the homelab.
- [DNS Strategy](docs/dns.md) - The DNS resolution strategy for the network.
- [Architecture Decisions](docs/architecture-decisions.md) - Record of architectural decisions and future roadmap.
- [Tools](docs/tools.md) - Some tools of importance / complexity that validates mentioning

This documentation is actively maintained and will be updated as the homelab evolves.
