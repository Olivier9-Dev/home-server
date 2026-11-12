# Phase 0 – Planning

## Goal

Decide what runs where before touching anything, so I don't have to redo the network later.

## What I did

- Mapped my network with `ipconfig`: subnet `192.168.1.0/24`, gateway `192.168.1.1`.
- Checked the router's DHCP pool and kept my static IPs outside of it.
- Picked one IP per machine (see the table in the main README).
- Turned on VT-x (and VT-d) in the Acer's BIOS.
- Bought a domain on Cloudflare for the HTTPS certificates.

## Decisions

| Decision | Why |
|---|---|
| Proxmox instead of installing Docker straight on the Acer | Snapshots, backups, and I can run LXCs and VMs side by side |
| DNS and VPN in LXCs, apps in Docker VMs | Breaking Docker shouldn't kill the internet for the whole house |
| No port forwarding, Tailscale only | Smaller attack surface, and it works through my double NAT |
| Laptop as a dedicated backup server | Backups on the same machine as the data don't protect against much |

## What broke

_Nothing yet._

## What I'd do differently

_TBD_
