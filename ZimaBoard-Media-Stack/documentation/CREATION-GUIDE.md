# Creation Guide — ZimaBoard Media Stack

This guide covers installing and wiring the **movies, TV, and books applications** on guests that already exist in Proxmox.

Create the platform first:

**[ZimaBoard-2-Homelab CREATION-GUIDE](https://github.com/Archin3t/ZimaBoard-2-Homelab/blob/main/documentation/CREATION-GUIDE.md)**

---

# 0. Before You Start

## Prerequisites

- [ ] Proxmox host online
- [ ] Media player guest created (Jellyfin)
- [ ] Media automation guest created (*arr + qBit + Seerr)
- [ ] Books guest created (Kavita + librarr + optional CWA)
- [ ] Media volume mounted into **player + automation** at the **same path**
- [ ] Books library disk available on the books guest
- [ ] Private notes file for IPs, passwords, API keys

Do **not** store secrets in Git.

---

# 1. Media Volume Path Check

Confirm both guests see the same path.

Example:

```text
/mnt/media
```

Suggested layout inside the media volume:

```text
/mnt/media/
  movies/
  tv/
  downloads/
    incomplete/
    complete/
```

Adjust names to your preference, but keep them consistent across apps.

---

# 2. Install Media Automation Stack

On the **media automation guest**, install:

| App | Role | Typical port |
|-----|------|--------------|
| Prowlarr | Indexers | `:9696` |
| Sonarr | TV | `:8989` |
| Radarr | Movies | `:7878` |
| qBittorrent | Downloads | `:8080` |
| Jellyseerr | Requests | `:5055` |

Deployment style can be:

- Native packages / services
- Docker Compose
- LXC nesting + containers

Choose one style and stay consistent.

---

## Wiring Order (Automation)

1. Bring up **qBittorrent**
2. Bring up **Prowlarr** and add indexers
3. Add **Sonarr** and **Radarr**
4. In Prowlarr → Apps, sync indexers to Sonarr/Radarr
5. Point Sonarr/Radarr download client to qBittorrent
6. Set root folders to your media paths
7. Bring up **Jellyseerr** and connect it to Sonarr/Radarr + Jellyfin

---

## Recommended qBittorrent Habits

Document privately, but aim for:

- No artificial download cap (unless your ISP requires one)
- Minimal or no seeding after completion
- Remove torrent from client after *arr import (keep files)
- Avoid dozens of dead torrents starving good ones

---

# 3. Install Jellyfin (Player Guest)

On the **media player guest**:

1. Install Jellyfin
2. Add Movie and TV libraries pointing at the shared media paths
3. Optional: enable Intel VAAPI / Quick Sync via `/dev/dri`
4. Create users
5. Confirm Direct Play / transcode behavior on a test title

Connect Jellyfin notifications from Sonarr/Radarr so libraries refresh after imports.

---

# 4. Install Books Stack

On the **books guest**:

| App | Role | Typical port |
|-----|------|--------------|
| Kavita | Reader | `:5000` |
| librarr | Search / download | `:5050` |
| Calibre-Web Automated | Convert / ingest | `:8083` |

## Suggested books folders

```text
/books/
  Computing/
  Cybersecurity/
  Textbooks/
  Learning/
  Fiction/
  ingest/
```

Rules:

- One subdirectory per book title
- Prefer EPUB/PDF for Kavita
- Keep books off the movies/TV media disk

## Wiring Order (Books)

1. Start **Kavita** and create libraries for each category folder
2. Start **librarr** and point library/import paths at `/books` (or your path)
3. Connect librarr → Kavita (URL + account) so scans/import work
4. Optionally start **Calibre-Web Automated** for conversions into `ingest/`
5. Test: search a title in librarr → download → confirm it appears in Kavita

---

# 5. End-to-End Validation

## Movies / TV

- [ ] Request a test title in Jellyseerr
- [ ] Sonarr/Radarr shows activity
- [ ] qBittorrent downloads
- [ ] Import succeeds
- [ ] Jellyfin shows the title after refresh

## Books

- [ ] Search works in librarr
- [ ] File lands under the correct category folder
- [ ] Kavita scan finds the book
- [ ] Reading progress saves for your user

---

# 6. Backup Notes (App Layer)

| Data | Idea |
|------|------|
| Sonarr/Radarr/Prowlarr configs | Guest backup / config volume backup |
| Jellyfin config + metadata | Guest backup |
| qBittorrent config | Guest backup (not the media files) |
| Kavita config/DB | Books guest config volume |
| Media files | Separate large-media backup strategy |
| Book files | Backup books disk / sync library folder |

Secrets stay in a password manager — never Git.

---

# Credits & Attribution

Created and maintained by **Archin3t**

If this guide is referenced in videos, articles, courses, or other repos, please provide visible credit and link back to this repository.

---

© 2026 Archin3t. All Rights Reserved.
