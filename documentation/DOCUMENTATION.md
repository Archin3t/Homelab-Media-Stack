# System Documentation — ZimaBoard Media Stack

## 1. Purpose

This document describes the **movies, TV, and books application architecture** used with a Proxmox Homelab built around the IceWhale ZimaBoard 2 (1664).

The goal is a clear pipeline:

- Request content
- Discover sources
- Download
- Organize
- Stream or read

| Role | Responsibility |
|------|----------------|
| Media player | Jellyfin streaming |
| Media requests | Jellyseerr |
| TV / movie libraries | Sonarr · Radarr |
| Indexers | Prowlarr |
| Download client | qBittorrent |
| Book reader | Kavita |
| Book search / acquire | librarr |
| Book convert / ingest | Calibre-Web Automated |

Platform (Proxmox guests, disk philosophy) is documented in:

**[ZimaBoard-2-Homelab](https://github.com/Archin3t/ZimaBoard-2-Homelab)**

NAS / shares / file drops are documented in:

**[ZimaBoard-NAS](https://github.com/Archin3t/ZimaBoard-NAS)**

---

# 2. Reference Hardware Context

| Resource | Configuration |
|----------|--------------|
| Platform | IceWhale ZimaBoard 2 1664 (x86) |
| CPU | Intel N150 |
| RAM | 16 GB LPDDR5 |
| Hypervisor | Proxmox VE |
| Media Storage | Dedicated disk for movies, TV, downloads |
| Books Storage | Dedicated books guest disk |
| GPU | Intel iGPU for Jellyfin VAAPI / Quick Sync |

## Hardware Flexibility

The same design can be adapted to:

- Intel N100/N150 mini PCs
- Intel NUC systems
- Small form factor desktops
- Custom Proxmox servers

Keep:

- Media separate from books
- Player separate from automation
- Storage organized by purpose

---

# 3. Guest Roles (Application Layer)

Guest IDs, names, and IPs stay private.

| Guest Role | Typical apps |
|------------|--------------|
| Media player | Jellyfin |
| Media automation | Jellyseerr · Sonarr · Radarr · Prowlarr · qBittorrent |
| Books platform | Kavita · librarr · Calibre-Web Automated |

## Example Resource Starting Points

| Guest | vCPU | RAM | Notes |
|-------|------|-----|-------|
| Jellyfin | 2 | 2–4 GB | Media bind mount + optional `/dev/dri` |
| Automation | 2 | 2–4 GB | **Same** media mount path as Jellyfin |
| Books | 2 | 2–4 GB | Own library disk (64–256 GB class) |

---

# 4. Service Address Template

| Service | Example |
|---------|---------|
| Jellyfin | `http://<JELLYFIN_HOST>:8096` |
| Jellyseerr | `http://<MEDIA_STACK_HOST>:5055` |
| qBittorrent | `http://<MEDIA_STACK_HOST>:8080` |
| Sonarr | `http://<MEDIA_STACK_HOST>:8989` |
| Radarr | `http://<MEDIA_STACK_HOST>:7878` |
| Prowlarr | `http://<MEDIA_STACK_HOST>:9696` |
| Kavita | `http://<BOOKS_HOST>:5000` |
| librarr | `http://<BOOKS_HOST>:5050` |
| Calibre-Web Automated | `http://<BOOKS_HOST>:8083` |

---

# 5. Storage Rules for This Stack

| Data | Location | Reason |
|------|----------|--------|
| Movies / TV / Completes | Dedicated media volume | Shared by Jellyfin + *arr |
| Books / Textbooks | Books guest disk | Isolated from media churn |
| App configs | Guest config volumes | Rebuildable |

## Critical Media Mount Rule

Jellyfin and the automation guest must use the **same absolute path** to the media volume.

Example:

```text
/mnt/media
```

This enables hardlinks and efficient imports.

Prefer:

```text
LABEL=<MEDIA_LABEL>
/dev/disk/by-id/
```

Avoid:

```text
/dev/sdX
```

## Books Layout Rule

Prefer one folder per title under category libraries:

```text
/books/
  Computing/<Title>/file.epub
  Cybersecurity/<Title>/file.pdf
  Textbooks/
  Learning/
  Fiction/
```

Kavita libraries map cleanly to those category roots.

---

# 6. Pipelines

## Media Pipeline

```text
Jellyseerr
      |
      v
Sonarr / Radarr
      |
      v
Prowlarr (indexers)
      |
      v
qBittorrent
      |
      v
Import + Organize
      |
      v
Jellyfin Library Update
      |
      v
Streaming
```

## Books Pipeline

```text
librarr search
 (Anna’s / LibGen / Zlib)
      |
      v
Download to library disk
      |
      v
Optional CWA convert
      |
      v
Kavita scan
      |
      v
Read
```

---

# 7. Download Client Policy (Recommended)

For a bandwidth-conscious Homelab:

- Prefer unlimited **download** rate
- Keep **upload / seeding** capped or disabled after completion
- Remove finished torrents from the client **after import** while keeping files on disk
- Prefer releases with healthier seeder counts

Document your exact client settings privately.

---

# 8. Architecture Diagram

```text
                 +----------------------+
                 |   Proxmox Host       |
                 |   (platform repo)    |
                 +----------+-----------+
                            |
           +----------------+----------------+
           |                |                |
           v                v                v

      Jellyfin         Media Stack        Books Guest
      (player)         Automation         Platform

                       Jellyseerr         Kavita
                       Sonarr             librarr
                       Radarr             CWA
                       Prowlarr
                       qBittorrent

           |                |                |
           +--------+-------+                |
                    |                        |
                    v                        v

             Media Storage              Books Storage
             Movies / TV                Ebook libraries
```

---

# 9. Out of Scope

This repository does not cover:

- Proxmox install from bare metal (see Homelab blueprint)
- Router vendor click-paths
- Real credentials or LAN inventory
- Public exposure / tunnels
- NAS share design (see NAS repo)

---

# 10. Documentation Safety

Never publish:

- Passwords
- API tokens
- SSH keys
- Private IP addresses
- Private domains
- Personal usernames

Use placeholders:

```text
<JELLYFIN_HOST>
<MEDIA_STACK_HOST>
<BOOKS_HOST>
<MEDIA_STORAGE>
<USERNAME>
<PASSWORD>
```

---

# Credits & Attribution

Created and maintained by **Archin3t**

If this documentation, architecture, diagrams, structure, or concepts are used as a reference, please provide visible credit and link back to this repository.

---

© 2026 Archin3t. All Rights Reserved.
