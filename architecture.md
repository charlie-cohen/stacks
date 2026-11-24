# Architecture

I track the system at four levels. This page covers the first two.

## Level 1: The big picture

Two machines sit behind one router. The Jetson is the always-on front door. The Proxmox server runs inference and every service.

Remote access goes through a Cloudflare tunnel with Cloudflare Access in front of it. No inbound ports are open.

```mermaid
flowchart LR
  devices["My devices<br/>Mac, iPhone, laptop"] -->|remote access| cf["Cloudflare<br/>Tunnel + Access"]
  cf --> jetson
  devices -->|at home| router
  subgraph home["Home network"]
    router["ISP router"]
    jetson["Jetson Orin Nano<br/>DNS, proxy, tunnel"]
    pve["Proxmox server<br/>Inference + services"]
    router --> jetson
    router --> pve
  end
```

## Level 2: The services

```mermaid
flowchart TB
  subgraph jetson["Jetson Orin Nano"]
    cfd["cloudflared<br/>tunnel into the network"] --> npm["Nginx Proxy Manager<br/>HTTPS + TLS certificates"]
    npm --> adg["AdGuard Home<br/>DNS filtering, DoH, DoT"]
  end
  subgraph pve["Proxmox server"]
    ai["AI container (CT 102)<br/>models + chat interface"] --> db[("Database (CT 101)<br/>PostgreSQL + pgvector")]
    office["Office (CT 104)<br/>Nextcloud"] --> db
    lab["Isolated lab network<br/>for testing containers"]
  end
```

### Two separate paths

- **Web services:** tunnel, then Nginx Proxy Manager, then the service.
- **DNS:** devices at home use AdGuard Home through the router. Remote devices use encrypted DNS over HTTPS through the tunnel.

### Service map

| Service | Runs on | Purpose |
| --- | --- | --- |
| cloudflared | Jetson | Carries the tunnel into the network |
| Nginx Proxy Manager | Jetson | HTTPS and TLS certificates |
| AdGuard Home | Jetson | DNS filtering, DoH, DoT |
| AI container (CT 102) | Proxmox | Models and chat interface |
| Database (CT 101) | Proxmox | PostgreSQL with pgvector, shared by services |
| Office (CT 104) | Proxmox | Nextcloud; ONLYOFFICE planned |
| Lab network | Proxmox | Isolated network for testing new containers |

### Design choices

- **One container per service.** Services stay isolated and easy to rebuild.
- **One shared database container.** Services use it instead of bundling their own.
- **Small always-on edge device.** The Jetson handles access and DNS, so the big server can be rebooted freely.
