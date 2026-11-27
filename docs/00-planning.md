# Phase 0 – Planning

## Goal

Decide what runs where before touching anything, so I don't have to redo the network later.

## What I did

- Mapped my network with `ipconfig`: subnet `192.168.0.0/24`.
- Picked one static IP per machine (see the table in the main README).
- Checked the hardware: the Acer has a single 512 GB NVMe, so the VMs and the media share it and backups go to an external SSD.
- Went through every planned service and asked "would I actually use this every week?" The list went from 23 services to 14.

## Decisions

| Decision | Why |
|---|---|
| Proxmox instead of installing Docker straight on the Acer | Snapshots, backups, and I can run LXCs and VMs side by side |
| DNS and VPN in LXCs, apps in Docker VMs | Breaking Docker shouldn't kill the internet for the whole house |
| No port forwarding, Tailscale only | Smaller attack surface, and it works through my double NAT |
| Dropped Portainer, Beszel, Gitea, Guacamole, NetBox, Semaphore and draw.io | Each one was either covered by something else (the Docker CLI, Proxmox's graphs, GitHub) or something I wouldn't use day to day. Fewer services = fewer things to update and secure |
| Backups to an external SSD instead of a laptop running Proxmox Backup Server | Simpler, and it frees the laptop. Trade-off: the backups are in the same machine, so I add a monthly off-site copy |
| Media disk on the NVMe, left out of the backups | Only one internal disk. Media can be copied again; VMs can't |

## What broke

_Nothing yet._

## What I'd do differently

_TBD_
