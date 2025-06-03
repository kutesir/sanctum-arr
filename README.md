# 📺 The Forge. A Self-Hosted Media Server Stack

A fully containerized media automation stack using Docker Compose, optimized for home servers or NAS environments.

This stack includes:

- **qBittorrent** – Torrent client
- **Sonarr** – TV shows manager
- **Radarr** – Movie manager
- **Lidarr** – Music organizer
- **Bazarr** – Subtitle fetcher
- **Prowlarr** – Indexer aggregator
- **Deunhealth** – Lightweight health checker

> ⚠️ This stack does not include a VPN or Gluetun container. It assumes responsible and legal usage. Torrenting is not illegal by itself, but distributing copyrighted material may be.

---

## 🧱 Folder Structure

Before running the stack, create a directory structure on your server that matches the volume paths used in `docker-compose.yml`.

**Example (customizable):**

![image](https://github.com/user-attachments/assets/943f510d-e652-4bff-923e-6793261836f1)

````

Replace `/path/to/your/media-server/` with a valid location on your system, such as:

- `/home/username/docker/mediaserver/`
- `/mnt/data/media-stack/`
- `/opt/media/`

---

## 🚀 Quick Start

1. Copy the `docker-compose.yml` to your server.
2. Optional: Create a `.env` file (see below).
3. Run:

```bash
docker compose up -d
````

4. Access services:

| Service     | URL                | Description       |
| ----------- | ------------------ | ----------------- |
| qBittorrent | `http://<IP>:8888` | Torrent client UI |
| Sonarr      | `http://<IP>:8989` | TV series manager |
| Radarr      | `http://<IP>:7878` | Movie manager     |
| Lidarr      | `http://<IP>:8686` | Music manager     |
| Bazarr      | `http://<IP>:6767` | Subtitles         |
| Prowlarr    | `http://<IP>:9696` | Indexer manager   |

---

## ⚙️ `.env` File (Optional)

To keep things clean and portable, create a `.env` file:

```dotenv
PUID=1000
PGID=1000
TZ=Africa/Kampala
WEBUI_PORT=8888
TORRENTING_PORT=6881
```

Reference these in your `docker-compose.yml`:

```yaml
environment:
  - PUID=${PUID}
  - PGID=${PGID}
  - TZ=${TZ}
```

---

## 📦 Volumes

This stack expects the following folders to exist on your host machine:

| Container     | Host Path (example)                                 | Purpose             |
| ------------- | --------------------------------------------------- | ------------------- |
| qBittorrent   | `/path/to/your/media-server/qbittorrent`            | Config & downloads  |
| Sonarr        | `/path/to/your/media-server/sonarr`                 | Config              |
| Radarr        | `/path/to/your/media-server/radarr`                 | Config              |
| Lidarr        | `/path/to/your/media-server/lidarr`                 | Config              |
| Bazarr        | `/path/to/your/media-server/bazarr`                 | Config              |
| Prowlarr      | `/path/to/your/media-server/prowlarr`               | Config              |
| Downloads     | `/path/to/your/media-server/downloads`              | Shared between apps |
| Media Library | `/path/to/your/media-server/media/tv shows`         | Final TV content    |
| Media Library | `/path/to/your/media-server/media/music` (optional) | Music for Lidarr    |

Update your `docker-compose.yml` accordingly to reflect your local paths.

---

## 🌐 Docker Network

All containers are isolated on a custom Docker network:

```yaml
networks:
  servarrnetwork:
    name: servarrnetwork
    ipam:
      config:
        - subnet: 172.69.0.0/24
```

---

## 🔁 Reverse Proxy (Optional)

If using NGINX Proxy Manager or similar:

1. Assign a subdomain (e.g. `radarr.yourdomain.com`) to your server.
2. Add a Proxy Host:

   * Forward hostname: `http://<LAN-IP>:7878`
   * Enable SSL (Let's Encrypt)
   * Add authentication or access control as needed.

Repeat for each app.

---

## 🧼 Maintenance & Backups

### Regular Backup

Backup the following folders:

* `/path/to/your/media-server/` – contains all container configs
* `/path/to/your/media-server/media/` – contains finalized media (optional)

Automate backups using rsync, BorgBackup, or cloud tools.

### Restore

To restore, simply copy folders back and run:

```bash
docker compose up -d
```

Everything should pick up where it left off.

---

## 📝 Notes & Best Practices

* `qbittorrent` uses the same folder for `completed`, `incomplete`, and `torrents`. You can split them for clarity.
* Avoid duplicate volume declarations (like Radarr had initially).
* Mount your music library to Lidarr and TV/movies to Sonarr/Radarr for proper scanning.
* Use `deunhealth` to restart unhealthy containers automatically.

---

## 📚 References

* [qBittorrent](https://github.com/linuxserver/docker-qbittorrent)
* [Sonarr](https://sonarr.tv/)
* [Radarr](https://radarr.video/)
* [Lidarr](https://lidarr.audio/)
* [Bazarr](https://www.bazarr.media/)
* [Prowlarr](https://wiki.servarr.com/prowlarr)
* [Deunhealth](https://github.com/qdm12/deunhealth)

---

## 🛡️ Disclaimer

This configuration is for personal use only. It assumes all content accessed is legally permissible. Use responsibly and follow local laws.

```

# sanctum-arr
Optimised and Decoupled
