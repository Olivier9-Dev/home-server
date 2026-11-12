# Home Server

This is the home server I run at my place. It's not a fake company lab, it's stuff my brother and I actually use every day: DNS with ad blocking, a VPN to get home from school, a media server, my own Git server and a bunch of monitoring so I know when something breaks.

I built it to learn Docker properly and to practice the things I'm studying (networking, Linux, automation, backups). Everything here is documented so someone else could rebuild it from scratch, including the parts where I messed up.

> **Status:** in progress. I'm building it phase by phase, see the [roadmap](#roadmap).

_Architecture diagram coming soon._

## Hardware

| Machine | Specs | Role |
|---|---|---|
| Acer Aspire C27 (all-in-one) | i5-1035G1, 24 GB RAM, 400 GB SSD + 500 GB HDD | Proxmox VE host, runs everything |
| Laptop | 16 GB RAM | Proxmox Backup Server, bare metal |
| Router | | DHCP, NAT, behind the ISP modem (double NAT) |

## How it's split up

| Name | Type | IP | RAM | What runs on it |
|---|---|---|---|---|
| `pve` | Proxmox VE host | 192.168.1.10 | ~2 GB | Hypervisor |
| `dns` | LXC | 192.168.1.11 | 512 MB | AdGuard Home |
| `vpn` | LXC | 192.168.1.12 | 256 MB | Tailscale subnet router |
| `pbs` | Bare metal (laptop) | 192.168.1.13 | 16 GB | Proxmox Backup Server |
| `docker-core` | VM (Debian 13) | 192.168.1.20 | 9 GB | 16 always-on services |
| `docker-apps` | VM (Debian 13) | 192.168.1.21 | 4 GB | Jellyfin + monitoring agents |

Why this layout:

- **DNS and VPN are in their own LXCs**, not in Docker. If I break the Docker VM, the whole house still has internet and I can still reach the server remotely.
- **Two Docker VMs** so a heavy app (Jellyfin transcoding) can't take down the reverse proxy or the monitoring.
- **Backups go to a different machine.** If the Acer dies or gets stolen, the backups are still on the laptop.

## Services

| Service | What I use it for |
|---|---|
| AdGuard Home | DNS + ad blocking for every device on the router |
| Tailscale | Reach the LAN from anywhere, no port forwarding |
| Nginx Proxy Manager | One HTTPS entry point, `*.home.example.com` with a wildcard Let's Encrypt cert (DNS-01 through Cloudflare) |
| Portainer | Look at containers, volumes, networks |
| Dozzle | Live logs from both Docker VMs |
| Uptime Kuma | Is each service up? |
| Beszel | CPU / RAM / disk of every host and container |
| ntfy | Push notifications to my phone (everything sends alerts here) |
| Diun | Tells me when an image has an update. It doesn't auto-update on purpose |
| NetAlertX | Scans the LAN and alerts me when a new device shows up |
| Homepage | Dashboard with every service |
| SearXNG | Private search engine |
| IT-Tools | Subnet calculator, hashes, base64, etc. |
| draw.io | Network diagrams for school and this repo |
| Gitea + PostgreSQL | My own Git server + container registry |
| Apache Guacamole | SSH / RDP from a browser, TOTP required |
| NetBox | Documents every IP, VM and later my Cisco switch |
| Ansible Semaphore | Runs my Ansible playbooks on a schedule |
| Jellyfin | Movies and shows, Intel Quick Sync transcoding |
| Proxmox Backup Server | Nightly encrypted, deduplicated backups |

## Security choices

- **Nothing is port-forwarded.** Remote access only goes through Tailscale.
- Admin tools (Portainer, Guacamole, Proxmox) are only reachable on the LAN or over the VPN.
- Each app with a database gets its **own** PostgreSQL container on a private Docker network. The database isn't on the proxy network and has no published port.
- Secrets live in `.env` files that are git-ignored. This repo only has `.env.example` files.
- Backups are encrypted on the client side. The key is stored outside the server.

## Backups (3-2-1-ish)

| Copy | Where | How often |
|---|---|---|
| 1 | Proxmox Backup Server on the laptop | Every night, 02:00 |
| 2 | 500 GB disk in the Acer | Weekly |
| 3 | USB drive kept outside the house | Monthly (planned) |

I test a restore every month. A backup I never restored is just a guess.

## Repo layout

```
.
├── ansible/          playbooks run by Semaphore
├── diagrams/         architecture + network diagrams (draw.io + PNG)
├── docs/             one write-up per phase: what I did, what broke, how I fixed it
├── screenshots/      proof it actually runs
└── stacks/
    ├── docker-core/  one folder per service, each with a compose.yaml
    └── docker-apps/
```

## Roadmap

- [ ] Phase 0 – Planning (IP plan, hardware, domain)
- [ ] Phase 1 – Proxmox install + base config
- [ ] Phase 2 – AdGuard Home
- [ ] Phase 3 – Tailscale subnet router
- [ ] Phase 4 – Docker VMs
- [ ] Phase 4B – Proxmox Backup Server on the laptop
- [ ] Phase 5 – First containers (Uptime Kuma, Portainer, Dozzle)
- [ ] Phase 6 – Monitoring and alerts
- [ ] Phase 7 – Domain, HTTPS, reverse proxy
- [ ] Phase 8 – Daily tools + Gitea
- [ ] Phase 9 – Guacamole, NetBox, Semaphore
- [ ] Phase 10 – Jellyfin + iGPU passthrough
- [ ] Phase 11 – Backups, updates, maintenance

## What I learned

I'll fill this in as I go. The detailed notes are in [`docs/`](docs/).

## About me

I'm Olivier, a student in the DEP Soutien informatique at CFP des Riverains (Quebec). I'm into networking, cybersecurity and cloud, and I'm working toward the CCNA.

- GitHub: [@Olivier9-Dev](https://github.com/Olivier9-Dev)
- LinkedIn: _add link_
