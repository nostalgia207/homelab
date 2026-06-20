# Homelab Infrastructure

> Self-hosted media internally, documentation, and monitoring stack running on Proxmox. I am slowly building this homelab as a way for me to develop the necessary skills to transition from an IT Support role into DevOps/SRE.

![Proxmox](https://img.shields.io/badge/Proxmox-E57000?style=flat&logo=proxmox&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white)
![Ubuntu](https://img.shields.io/badge/Ubuntu-E95420?style=flat&logo=ubuntu&logoColor=white)
![Tailscale](https://img.shields.io/badge/Tailscale-1A1A1A?style=flat&logo=tailscale&logoColor=white)
![Status](https://img.shields.io/badge/status-active-success)

## About

This repo documents a homelab I run and actively maintain — virtualized on Proxmox, with a Docker-based media stack, Active Directory lab, and a monitoring node in progress. I work as an IT Support Technician (Intune, Entra ID, M365, Veeam, Hyper-V day to day) and use this lab to build hands-on infrastructure and DevOps skills outside of work: Linux administration, containerization, networking, and — per the [roadmap](./roadmap.md) — Terraform, Ansible, Kubernetes (k3s), and CI/CD.

Notes here were originally written for myself in Obsidian; this repo is the cleaned-up, sanitized version for anyone curious how it's built.

## Architecture

![Homelab architecture diagram](./diagrams/architecture.png)

**Hypervisor:** Proxmox VE, single node currently, second node planned
**Remote access:** Tailscale (mesh VPN, no exposed ports to WAN)
**Network:** dual-bridge design — `vmbr0` for management/DC traffic, `vmbr1` for DHCP-served client VMs

| VM  | OS                  | Role                                                             |
| --- | ------------------- | ---------------------------------------------------------------- |
| 100 | Windows Server 2025 | AD DS + DNS + DHCP                                               |
| 101 | Ubuntu Server       | Docker host — media stack + Bookstack                            |
| 105 | Debian              | Docker host — monitoring (Prometheus/Grafana, Pi-hole) *planned* |

Full diagram notes: [`diagrams/architecture.md`](./diagrams/architecture.md)

## Services

| Service     | Host  | Purpose                               |
| ----------- | ----- | ------------------------------------- |
| Jellyfin    | VM101 | Media server                          |
| Jellyseerr  | VM101 | Media request UI                      |
| Radarr      | VM101 | Movie management                      |
| Sonarr      | VM101 | Shows management                      |
| Bazarr      | VM101 | Subtitle management                   |
| Bookstack   | VM101 | Internal documentation wiki - For fun |
| qBittorrent | VM101 | Download client                       |

Request flow (Jellyseerr → Radarr/Sonarr → qBittorrent → Jellyfin): see [`docs/services.md`](./docs/services.md)

## Skills demonstrated

- **Virtualization**: VM provisioning, disk resizing/expansion (LVM), physical disk passthrough on Proxmox
- **Containerization**: Docker Compose, bind mounts, backup/restore methodology, container networking
- **Networking**: dual-bridge segmentation, DHCP/DNS via AD, Tailscale mesh VPN, troubleshooting LAN-vs-VPN service binding issues
- **Identity**: Active Directory Domain Services + DNS/DHCP lab environment
- **Systems migration**: full stack migration between hosts using a backup/restore strategy instead of full VM export, with documented reasoning
- **Documentation**: structured, versioned infra docs (this repo)

## Roadmap

Currently working through a structured plan to extend this lab into a full DevOps toolchain — Terraform (Proxmox provider), Ansible, k3s, GitOps, and Prometheus/Grafana monitoring. Full breakdown: [`roadmap.md`](./roadmap.md)

## Documentation index

- [`docs/proxmox/`](./docs/proxmox) — VM creation, disk management, node updates
- [`docs/docker/`](./docs/docker) — Compose syntax, backup/restore, Tailscale access
- [`docs/networking/`](./docs/networking) — AD DS/DNS/DHCP setup
- [`docs/services.md`](Services.md) — service catalog + request flow diagram

## A note on the data in this repo

All IPs, hostnames, and credentials shown here are placeholders or internal-only, sanitized for public sharing. No real infrastructure details are exposed.
