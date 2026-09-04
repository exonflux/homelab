# homelab

Personal homelab for self-hosted media, ebooks, and offline tools. Runs on a small Proxmox cluster, mostly Docker containers inside LXC.

## Architecture

```
Proxmox Node A (low-power)
  └─ misc lightweight services

Proxmox Node B (GPU-equipped)
  └─ LXC container
       └─ Docker
            ├─ Jellyfin (hardware-accelerated transcoding)
            ├─ Kavita
            ├─ Portainer
            ├─ Kiwix
            ├─ LibreTranslate
            └─ arr-stack (see below)

External USB drive — bulk storage, mounted into the LXC
```

## Hardware

- Node A: low-power mini PC, used for lightweight always-on services
- Node B: repurposed desktop with a consumer NVIDIA GPU, passed through for hardware transcoding (NVENC/NVDEC)
- External USB HDD for bulk media/ebook storage

## Stack

| Service | Purpose | Notes |
|---|---|---|
| Jellyfin | Media streaming | GPU-accelerated transcoding |
| Sonarr / Radarr | TV/movie library automation | |
| Prowlarr | Indexer management | Synced to Sonarr/Radarr/LazyLibrarian |
| qBittorrent | Download client | Routed through a VPN container (Gluetun), no port forwarding, no seeding after download |
| LazyLibrarian | Book library automation | |
| Kavita | Ebook/manga server | Hosts a ~65k-book public-domain (Project Gutenberg) library |
| Kiwix | Offline Wikipedia mirror | |
| LibreTranslate | Local translation, no cloud API | |
| Portainer | Container management UI | |

## Notes

- All torrent traffic goes through a WireGuard VPN container; explicitly no port forwarding.
- The arr-stack is only used for public-domain/Creative-Commons content.
- Quality profiles are capped to avoid pulling oversized remux releases.

## Planned

- [ ] Additional Proxmox node
- [ ] Tailscale for remote access
- [ ] Dedicated NAS build
