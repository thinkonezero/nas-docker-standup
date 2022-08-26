# NAS Docker Standup

---
This is a simple docker-compose configuration to standup a Synology NAS.

## Services

- [Deluge](https://deluge-torrent.org/) + [Private Internet Access](https://www.privateinternetaccess.com/pages/buy-vpn/toz) or [TorGuard](https://torguard.net/aff.php?aff=4350) - for downloading torrents... "safely"
- [Sonarr](https://sonarr.tv/) - for TV Series Management
- [Radarr](https://radarr.video/) - for Movie Management
- [Jackett](https://github.com/Jackett/Jackett) - for Torrent Tracker feeds
- [Tautulli](http://tautulli.com/) - for Plex library statistics and usage
- [Overseerr](https://overseerr.dev/) - for requesting additional library content
- [Portainer](https://portainer.io/) - for managing all of your Docker containers
- [Watchtower](https://containrrr.dev/watchtower) - for automatically updating running containers
- [Organizr](https://github.com/causefx/Organizr) - for web based portal to access services
- [InfluxDB](https://www.influxdata.com/) - for time series based database storage
- [Chronograf](https://www.influxdata.com/time-series-platform/chronograf/) - for making pretty dashboards out of the database data
- [SpeedTest](https://github.com/sivel/speedtest-cli/) - for performing a speedtest and posting data to the database
- [Traefik](hhttps://traefik.io/) - Reverse Proxy and SSL Support

This project was heavily inspired by the [MediaBox](https://github.com/tom472/mediabox) project... Many Thanks!

## Known Issues
- [ ] Sometimes the Deluge + VPN Container disconnects and can't re-establish a forwarded port connection. 


## Install Instructions

### Prerequisites
- Synology NAS
- VPN Account from [PIA](https://www.privateinternetaccess.com/pages/buy-vpn/toz) or [TorGuard](https://torguard.net/aff.php?aff=4350)
- [Git](https://git-scm.com/)
- [Docker](https://www.docker.com/)
- [Python 2.7](https://www.python.org/)
- [Docker-Compose](https://docs.docker.com/compose/)

### Server Configuration
1. Setup Synology NAS and [install DSM](https://www.synology.com/en-us/knowledgebase/DSM/tutorial/General_Setup/How_to_install_DSM)
1. Install [Docker](https://www.synology.com/en-us/dsm/packages/Docker) via Add-on Packages
1. Install Plex natively from [Plex.tv](https://www.plex.tv/media-server-downloads/)
1. Add the SynoCommunity package source via [easy install](https://synocommunity.com/#easy-install)
1. Install Git package from SynoCommunity
1. Enable SSH Access
1. ~~Upgrade Docker using [synology-docker](https://github.com/markdumay/synology-docker)~~ Known Issue: https://github.com/markdumay/synology-docker/issues/22
1. Create Docker Group and Add User

```
- create the group "docker" from the ui or cli (sudo synogroup --add docker)
- make it the group of the docker.sock: sudo chown root:docker /var/run/docker.sock
- assign the user to the docker group in the ui or cli (sudo synogroup --member docker {username})
- login into ssh as {username} and try (if you were already logged in before you created the group, logout and relogin)
```

1. Make sure DSM isn't using 80/443 with [guide](https://www.smarthomebeginner.com/synology-docker-media-server/#8_Ensure_Ports_80_and_443_are_Free)
1. Enable Tunnel stuff for VPN: https://forums.unraid.net/topic/44109-support-binhex-delugevpn/page/58/?tab=comments#comment-542434

```
sudo insmod /lib/modules/tun.ko
sudo insmod /lib/modules/iptable_mangle.ko
```
Make a scheduled task of those commands

1. Clone Repo `git clone https://gitlab.com/think-one-zero/nas-docker-standup.git`
1. Copy Sample Environement File `cp sample.env .env`
1. Edit `.env` to match your environment
1. Run Docker Environment `docker-compose up -d`
1. ???
1. Profit.

### Environment File
- `LOCALUSER=` - This is the local user of your linux account and account running docker
- `HOSTNAME=` - Hostame of the server, can be found by executing `hostname` from command line
- `IP_ADDRESS=` - Local IP Address of the server, should be static
- `PUID=` - UID of the local user, can be found by executing `id` from the command line
- `PGID=` - GID of the local user, can be found by executing `id` from the command line
- `VPNUNAME=` - Your VPN username from [PIA](https://www.privateinternetaccess.com/pages/buy-vpn/toz) or [TorGuard](https://torguard.net/aff.php?aff=4350)
- `VPNPASS=` - Your VPN password from [PIA](https://www.privateinternetaccess.com/pages/buy-vpn/toz) or [TorGuard](https://torguard.net/aff.php?aff=4350)
- `VPNPROVIDER=` - Your VPN provider, name must match a folder specified in `ovpn`. This defaults to `pia` if you copied `sample.env`.
- `VPN_REMOTE=` - The remote server you want to connect to (must support port forwarding)
- `CIDR_ADDRESS=` - IP/netmask entries which allow access to the server without requiring authorization. We recommend you set this only if you do not sign in your server. For example `192.168.1.0/24,172.16.0.0/16` will allow access to the entire `192.168.1.x` range and the `172.16.x.x`
- `TZ=` - Set the timezone inside the container. For example: `Europe/London`. The complete list can be found here: [https://en.wikipedia.org/wiki/List_of_tz_database_time_zones](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)
- `EMAIL=` - Email address to be used for Let's Encrypt SSL certificate validation - `someone@somewhere.com`
- `DOMAIN=` - Public domain to use for accessing services via a public domain - `server.domain.com`
- `WATCHTOWER_NOTIFICATIONS=` - Default to `email` for using email notifications
- `WATCHTOWER_NOTIFICATION_EMAIL_TO=` - Email address you'd like Watchtower to notify for any notifications - `someone@somewhere.com`
- `WATCHTOWER_NOTIFICATION_EMAIL_FROM=` - From address that your SMTP server uses to send email - `someone@somewhere.com`
- `WATCHTOWER_NOTIFICATION_EMAIL_SERVER=` - Servername of your SMTP server - `smtp.domain.com`
- `WATCHTOWER_NOTIFICATION_EMAIL_PORT=` - Port that your SMTP server uses to connect - `587`
- `WATCHTOWER_NOTIFICATION_EMAIL_USER=` - Username that your SMTP server uses to authenticate
- `WATCHTOWER_NOTIFICATION_EMAIL_PASSWORD=` - Password for your SMTP user to authenticate
- `SPEEDTEST_INTERVAL=` - Number of seconds between tests to the [Speedtest.net](http://www.speedtest.net/) services
- `TRAEFIK_AUTH=` - Basic Auth for the Traefik Admin [htpasswd Generator](http://www.htaccesstools.com/htpasswd-generator/)
- `STACK_NAME=` - This is used to specify the appropriate network Traefik should use. See #13 for details.
- `LOG_FILE_NUM=` - The number of log files to keep of the specified size before pruning them
- `LOG_FILE_SIZE=` - The size of the log files before truncating and rotating the log files
- `MEDIA_BASE_PATH=` - This is the base path of your media directory used to make similar paths easier - `/volume1/media`
- `SNYOLOGY_BASE_DOCKER_PATH=` - The base path to the docker directory where container data is stored - `/volume1/docker`
- `SNYOLOGY_PLEX_PATH=` - The path to the installation directory for Plex Media Server - `/volume1/PlexMediaServer`
- `TORRENTS_PATH=` - The base path for the torrents download/in-progress directories - `/volume1/torrents`

### Email/SMTP Service
- [Mailgun](https://documentation.mailgun.com/en/latest/quickstart.html) has an excellent QuickStart Guide
- Check out the sending via [SMTP](https://documentation.mailgun.com/en/latest/quickstart-sending.html#send-via-api)
- Make sure to also [verify your domain](https://documentation.mailgun.com/en/latest/quickstart-sending.html#verify-your-domain)

---

## Tips and Tricks

- [PIA List of Servers](https://helpdesk.privateinternetaccess.com/hc/en-us/articles/219460187-How-do-I-enable-port-forwarding-on-my-VPN-) that support port forwarding
- [Deluge + PIA FAQ](https://lime-technology.com/forums/topic/44108-support-binhex-general/?tab=comments#comment-433613)
- [Removing Old/Completed Torrents from Deluge](https://www.cuttingcords.com/home/2015/2/4/auto-deleting-finished-torrents-from-deluge)
- [Create Series Folder in Sonarr](https://forums.sonarr.tv/t/adding-new-series-path-issues/2751/2)
- [Proper Encoding of the Portainer Password](https://github.com/portainer/portainer/issues/1506)
- Open Shell in a Container - [Link 1](http://phase2.github.io/devtools/common-tasks/ssh-into-a-container/), [Link 2](https://stackoverflow.com/a/30173220)

[Potential Script to setup, renew and copy SSL for Plex](https://www.npcglib.org/~stathis/blog/2017/05/13/plex-media-server-over-https-with-letsencrypt-certificates/)

---

If this project has helped you in anyway, and you'd like to say thanks...

[![Donate](https://img.shields.io/badge/Donate-SquareCash-brightgreen.svg)](https://cash.me/$phikai)

You can also [gift a Plex Pass](https://www.plex.tv/plex-pass/gift/) subscription as a great way to show your appreciation.

_AFFILIATE DISCLOSURE: You can also support this project by purchasing a VPN Subscription via one of the links in this README._

---

# Disclaimer

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
