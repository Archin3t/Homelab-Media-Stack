# ZimaBoard Media Stack

> **Movies, TV, and books automation for a Proxmox-based ZimaBoard 2 (1664) Homelab.**  
> Request → find → download → organize → stream or read — documented as a reusable stack.

This repository documents the **media and books application layer** of my personal Homelab. The reference platform is the **IceWhale ZimaBoard 2 (1664)** running Proxmox VE, but the same stack can be deployed on Intel N-series mini PCs, NUCs, SFF desktops, or larger Proxmox hosts with only minor path and resource changes.

Unlike a full platform blueprint, this repository focuses on **playback, acquisition, and reading** — how the services connect, what each one is for, and how to recreate the pipeline.

Platform architecture (hypervisor, guests, storage roles) lives in the companion Homelab repo.

---

# Companion Repositories

| Repository | Covers |
|------------|--------|
| **[ZimaBoard-2-Homelab](https://github.com/Archin3t/ZimaBoard-2-Homelab)** | Platform blueprint — Proxmox layout, guests, storage philosophy, networking |
| **[ZimaBoard-NAS](https://github.com/Archin3t/ZimaBoard-NAS)** | NAS, shares, disk passthrough, file drop services, backups, and extras |

---

# HTML Documentation

📖 Browse the documentation as a website (enable GitHub Pages on the `docs/` folder):

**https://archin3t.github.io/Homelab-Media-Stack/**

---

# What This Stack Covers

| Area | Applications |
|------|----------------|
| **Watch** | Jellyfin |
| **Request** | Jellyseerr |
| **TV / Movies automation** | Sonarr · Radarr · Prowlarr |
| **Downloads** | qBittorrent |
| **Read** | Kavita |
| **Book search / acquire** | librarr (Anna’s Archive · LibGen-class mirrors · Z-Library when enabled) |
| **Book convert / ingest** | Calibre-Web Automated |

Exact ports and hostnames are placeholders in public docs — keep your real map private.

---

# Reference Hardware Context

This stack was built and validated on:

| Component | Specification |
|-----------|---------------|
| **Model** | IceWhale ZimaBoard 2 (1664) |
| **CPU** | Intel N150 (Quick Sync / VAAPI for Jellyfin) |
| **Memory** | 16 GB LPDDR5 |
| **Hypervisor** | Proxmox VE |
| **Media volume** | Dedicated large disk/USB for movies, TV, and torrent completes |
| **Books volume** | Separate guest disk for ebook libraries (not on the media volume) |

The same concepts apply on other low-power x86 systems — scale RAM and disk; keep **media storage** and **books storage** separated.

---

# What It Does

```text
                 You
                  │
                  ▼
            Jellyseerr
         (request a title)
                  │
                  ▼
        Sonarr / Radarr
         (+ Prowlarr)
                  │
                  ▼
             qBittorrent
                  │
                  ▼
           Media Storage
         (movies / TV disk)
                  │
                  ▼
              Jellyfin
                  │
                  ▼
           Stream / Watch

──────────────────────────────────────

                 You
                  │
                  ▼
              librarr
     (search Anna’s / LibGen / Zlib)
                  │
                  ▼
          Book Library Disk
      (per-category folders)
                  │
                  ▼
               Kavita
                  │
                  ▼
            Read / Progress

   Optional: Calibre-Web Automated
            (convert / ingest)
```

---

# Repository Documentation

| File | Purpose |
|------|---------|
| **[DOCUMENTATION.md](documentation/DOCUMENTATION.md)** | Architecture of the media + books pipeline |
| **[CREATION-GUIDE.md](documentation/CREATION-GUIDE.md)** | Install and wire services on the guests |
| **[USER-GUIDE.md](documentation/USER-GUIDE.md)** | Day-to-day: request, download, read, troubleshoot |
| **[SECURITY.md](documentation/SECURITY.md)** | Credential redaction and safe publishing rules |
| **[index.html](docs/index.html)** | Single-page HTML documentation for GitHub Pages |

---

# Design Philosophy

This media stack follows a few simple principles:

- Player ≠ automation
- Share one media mount path for hardlinks
- Keep books on a separate disk
- Prefer well-seeded releases over noisy concurrent junk
- Finish downloads, import, remove from the client list — keep files
- Document everything with placeholders only
- Prefer stability over complexity

---

# Security

This repository intentionally excludes sensitive information.

Examples include:

- Passwords
- API Keys
- Authentication Tokens
- VPN Credentials
- SSH Keys
- Personal IP Addresses
- Private Inventory
- Usernames

Documentation uses placeholders instead.

```text
<JELLYFIN_HOST>
<MEDIA_STACK_HOST>
<BOOKS_HOST>
<MEDIA_STORAGE>
<USERNAME>
<PASSWORD>
```

---

# Partnership Disclosure

I am an official **IceWhale (Zima) Partner and Affiliate**.

IceWhale provides hardware for the purpose of producing educational content, tutorials, documentation, and product reviews. I may also earn a commission through qualifying purchases made using my affiliate links.

All configurations, recommendations, and opinions shared in this repository are based on my own real-world experience using the hardware in my personal Homelab.

---

# About

This repository documents the **media and books stack** of my Proxmox Homelab — how movies, TV, and ebooks are requested, acquired, organized, and consumed.

While the featured hardware is the **IceWhale ZimaBoard 2**, the pipeline is designed to remain hardware-agnostic wherever possible.

---

## Credits

Created and maintained by **Archin3t**

- 🌐 GitHub: https://github.com/Archin3t
- ▶️ YouTube: https://www.youtube.com/@Archinet-Labs
- 📸 Instagram: https://www.instagram.com/Archin3t

---

© 2026 Archin3t. All Rights Reserved.

If this repository helped you build your own Homelab, create documentation, make a video, or publish a guide, **please provide visible credit and link back to this repository** (and to [ZimaBoard-2-Homelab](https://github.com/Archin3t/ZimaBoard-2-Homelab) for the platform blueprint).
