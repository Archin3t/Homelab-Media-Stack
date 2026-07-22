# User Guide — ZimaBoard Media Stack

This guide covers everyday use of the **movies, TV, and books** stack.

Keep your real:

- IP addresses
- Hostnames
- Credentials
- API keys

in private notes or a password manager.

---

# Quick Access Doors

| I want to... | Open |
|--------------|------|
| Watch movies and TV | `http://<JELLYFIN_HOST>:8096` |
| Request a movie or show | `http://<MEDIA_STACK_HOST>:5055` |
| Check downloads | `http://<MEDIA_STACK_HOST>:8080` |
| Manage TV automation | `http://<MEDIA_STACK_HOST>:8989` |
| Manage movie automation | `http://<MEDIA_STACK_HOST>:7878` |
| Manage indexers | `http://<MEDIA_STACK_HOST>:9696` |
| Read books | `http://<BOOKS_HOST>:5000` |
| Search / download books | `http://<BOOKS_HOST>:5050` |
| Convert / ingest books | `http://<BOOKS_HOST>:8083` |

---

# Mental Model

| Component | Purpose |
|-----------|---------|
| Jellyfin | Watch |
| Jellyseerr | Ask for new movies/shows |
| Sonarr / Radarr | Manage TV/movie libraries |
| Prowlarr | Manage indexers |
| qBittorrent | Download client |
| Kavita | Read books |
| librarr | Search and acquire books |
| Calibre-Web Automated | Convert awkward formats / ingest |

---

# Common Workflows

## Watching

```text
Jellyfin → Library → Playback
```

If missing:

1. Confirm the file exists on media storage
2. Confirm Jellyfin scanned the library
3. Check Sonarr/Radarr import history

---

## Requesting Movies / TV

```text
Jellyseerr
   → Sonarr / Radarr
   → qBittorrent
   → Import
   → Jellyfin update
```

Normally you only touch **Jellyseerr** and **Jellyfin**.

---

## Reading / Getting Books

```text
librarr search
   → download
   → library folder
   → Kavita scan
   → read
```

Use Calibre-Web Automated when you need format conversion (example: MOBI/LIT → EPUB).

---

# Troubleshooting

## Request stuck

Check in order:

1. Jellyseerr status
2. Sonarr/Radarr Activity / History
3. qBittorrent state (stalled vs downloading)
4. Import errors
5. Jellyfin library refresh / notifier

---

## Slow torrents

Common causes:

- Weak public indexers / dead swarms
- Too many concurrent dead downloads
- Release with almost no seeders

Actions:

- Prefer higher-seeder releases
- Remove hopeless stalled torrents (keep files if already imported)
- Improve indexer set in Prowlarr

---

## Books missing in Kavita

Check:

1. File exists under the category folder
2. Folder-per-book layout
3. Library root in Kavita matches the folder
4. Manual library scan
5. Unsupported/corrupt format (convert via CWA)

---

## Guest offline

Open Proxmox:

```text
https://<PROXMOX_HOST>:8006
```

Restart the affected guest and confirm mounts.

---

# Maintenance Habits

## Weekly

- Review failed downloads/imports
- Check free space on media + books disks
- Clear dead torrents from the client

## Monthly

- Update containers/services
- Test that Jellyfin notifications still fire
- Confirm librarr → Kavita import still works

---

# Safety Habits

Always:

- Keep credentials private
- Keep real IPs private
- Use stable disk identifiers
- Separate books from movies/TV storage

Never publish:

- Passwords
- API keys
- Tokens
- SSH keys
- Private network details

---

# Related Documentation

- Platform: **[ZimaBoard-2-Homelab](https://github.com/Archin3t/ZimaBoard-2-Homelab)**
- NAS / shares: **[ZimaBoard-NAS](https://github.com/Archin3t/ZimaBoard-NAS)**

---

# Credits & Attribution

Created and maintained by **Archin3t**

If this documentation is referenced elsewhere, please provide visible credit and link back to this repository.

---

© 2026 Archin3t. All Rights Reserved.
