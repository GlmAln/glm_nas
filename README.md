# 🏠 NAS Configuration

> Self-hosted media server stack running on a laptop NAS with CasaOS, Docker Compose, and a 4TB IronWolf HDD.

## 🖥️ Hardware

| Component | Specs |
|-----------|-------|
| **Host** | Laptop (gaming) |
| **OS** | Ubuntu 24 + CasaOS |
| **SSD** | 476 GB NVMe (OS + configs) |
| **HDD** | Seagate IronWolf 4TB (media) |
| **Network** | Tailscale VPN |

## 📦 Stack

| Service | Purpose | Port |
|---------|---------|------|
| [Jellyfin](https://jellyfin.org) | Media server | 8097 |
| [Sonarr](https://sonarr.tv) | TV & Anime management | 8989 |
| [Radarr](https://radarr.video) | Movie management | 7878 |
| [Prowlarr](https://prowlarr.com) | Indexer manager | 9696 |
| [qBittorrent](https://qbittorrent.org) | Download client | 8080 |
| [Gluetun](https://github.com/qdm12/gluetun) | VPN container (ProtonVPN) | - |
| [FlareSolverr](https://github.com/FlareSolverr/FlareSolverr) | Cloudflare bypass | 8191 |
| [Bazarr](https://www.bazarr.media) | Subtitle management | 6767 |
| [Jellyseerr](https://github.com/Fallenbagel/jellyseerr) | Media requests | 5055 |
| [AdGuard Home](https://adguard.com) | DNS ad blocker | 3001 |
| [Homarr](https://homarr.dev) | Dashboard | 7575 |
| [ntfy](https://ntfy.sh) | Push notifications | 7200 |

## 📁 Directory Structure

```
/
├── /DATA/AppData/          # Docker app configs (SSD)
└── /mnt/ironwolf/data/     # Media storage (IronWolf 4TB)
    ├── media/
    │   ├── anime/
    │   ├── movies/
    │   └── tv/
    └── torrents/
        ├── anime/
        ├── movies/
        ├── tv/
        └── incomplete/
```

## 🔧 Setup

### Prerequisites

- Ubuntu 24.04
- Docker + Docker Compose
- CasaOS
- Tailscale

### Installation

```bash
# Clone the repo
git clone https://github.com/GlmAln/glm_nas.git
cd glm_nas

# Copy and fill environment variables
cp .env.example .env
nano .env

# Deploy the stack
cd docker-compose/modest_martin
docker compose up -d
```

### Environment Variables

Copy `.env.example` to `.env` and fill in your values:

```bash
cp .env.example .env
```

See [.env.example](.env.example) for all required variables.

## 🌐 Remote Access

All services are accessible remotely via **Tailscale**:
- Jellyfin: `http://100.x.x.x:8097`
- Jellyseerr: `http://100.x.x.x:5055`
- Homarr: `http://100.x.x.x:7575`

## 📱 Mobile Apps

| App | Platform | Purpose |
|-----|----------|---------|
| [Infuse](https://firecore.com/infuse) | iOS | Jellyfin client |
| [Tailscale](https://tailscale.com) | iOS/Android | Remote access |
| [ntfy](https://ntfy.sh) | iOS/Android | Push notifications |
| [Jellyseerr](https://github.com/Fallenbagel/jellyseerr) | Web | Media requests |

## 🔒 Security

- All secrets stored in `.env` (not committed)
- qBittorrent traffic routed through **ProtonVPN** via Gluetun
- Remote access via **Tailscale** (no open ports)
- DNS filtering via **AdGuard Home**

## 📝 Notes

- Hardlinks enabled between torrents and media (TRaSH Guides structure)
- All arr apps use the same `/data` mount for proper hardlink support
- IronWolf 4TB connected via USB 3.0 dock
