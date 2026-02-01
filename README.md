# NAS Media Docker Standup

A comprehensive docker-compose configuration to stand up a full-featured media management and download stack on a Synology NAS.

## 🚀 Services

| Service | Category | Description |
| :--- | :--- | :--- |
| **DelugeVPN** | Download | Torrent client routed through a secure VPN tunnel. |
| **Sonarr/Radarr** | Management | Automated TV and Movie collection management. |
| **Calibre/Web** | Reading | E-book library management and web reader. |
| **Plex Companion** | Monitoring | **Tautulli** for stats and **Overseerr** for requests. |
| **Maintenance** | Optimization | **Profilarr**, **Huntarr**, and **Jackett**. |
| **Organizr** | Portal | A unified dashboard to access all services. |

## 📋 Prerequisites

- Synology NAS (DSM 7.x recommended)
- VPN Account ([PIA](https://www.privateinternetaccess.com/pages/buy-vpn/toz) or [TorGuard](https://torguard.net/aff.php?aff=4350) supported)
- [Git](https://git-scm.com/) installed via SynoCommunity
- [Docker / Container Manager](https://www.docker.com/)

## 🛠️ Setup

### 1. Synology System Preparation
Before deploying, your NAS needs specific kernel modules for the VPN tunnel:
```bash
sudo insmod /lib/modules/tun.ko
sudo insmod /lib/modules/iptable_mangle.ko
```
*Tip: Add these to a "Triggered Task" in Synology Task Scheduler to run on boot.*

### 2. Docker Permissions
Ensure your user has access to the Docker socket:
```bash
sudo synogroup --add docker
sudo chown root:docker /var/run/docker.sock
sudo synogroup --member docker {your_username}
```

### 3. Installation
1. **Clone**: `git clone https://gitlab.com/think-one-zero/nas-docker-standup.git`
2. **Environment**: `cp sample.env .env` and update with your VPN credentials and paths.
3. **Deploy**: `docker compose up -d`

## 🌐 Accessing Apps

| App | Default URL |
| :--- | :--- |
| **Organizr** | `http://<NAS_IP>:${ORGANIZR_PORT}` |
| **Deluge Web** | `http://<NAS_IP>:${DELUGE_PORT}` |
| **Sonarr** | `http://<NAS_IP>:${SONARR_PORT}` |
| **Radarr** | `http://<NAS_IP>:${RADARR_PORT}` |
| **Overseerr** | `http://<NAS_IP>:${OVERSEERR_PORT}` |

## ⚙️ Environment Variables

- `VPNPROVIDER`: Set to `pia` or `torguard`.
- `VPN_REMOTE`: The VPN server address.
- `MEDIA_BASE_PATH`: Where your media is stored (e.g., `/volume1/media`).
- `TORRENTS_PATH`: Where downloads are stored.

---

## 🤝 Support & Appreciation

If this project has helped you, please consider saying thanks!

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/G2G71SUNID)
[Gift a Plex Pass](https://www.plex.tv/plex-pass/gift/)

_AFFILIATE DISCLOSURE: Supporting this project via VPN affiliate links helps maintain these configurations._

---

# Disclaimer

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED. IN NO EVENT SHALL THE AUTHORS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY.