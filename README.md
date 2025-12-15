# Homelab Architecture Documentation

Welcome to the documentation for my homelab setup. This repository contains detailed documentation of the physical and virtual infrastructure, network configurations, and services running in the homelab environment.

## My aim and key principle is:

1. `Automation` and `Infrastructure-as-Code` first
2. The deployments heavily utilize tools such as `Ansible`, `Docker`, `Terraform`
3. For management of secrets currently there is two main options:
   1. I often relly on `env` files, which are generally excluded from version control and live only on the local system
   2. I recently started transitioning to use `1password` (primarily reading data from it with Terraform) via `op cli` to outsource secret storage

## Contents

- [Network](docs/network.md) - Detailed network design, including VLANs, subnets, and firewall rules.
- [Hardware](docs/hardware.md) - An overview of the physical servers and network gear.
- [Services](docs/services.md) - A list of services running in the homelab.
- [DNS Strategy](docs/dns.md) - The DNS resolution strategy for the network.
- [Tools](docs/tools.md) - Some tools of importance / complexity that validates mentioning

This documentation is actively maintained and will be updated as the homelab evolves.
