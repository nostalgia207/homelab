# Services

## Media stack (VM 101)

| Service | Purpose |
|---|---|
| Jellyfin | Media server — streams the library to clients |
| Jellyseerr | Front-end for users to request movies/shows |
| Radarr | Movie acquisition and library management |
| Sonarr | TV show acquisition and library management |
| Bazarr | Subtitle download and management |
| Prowlarr | Indexer management, feeds Radarr/Sonarr |
| qBittorrent | Download client |
| Bookstack | Internal documentation wiki |

## Monitoring stack (VM 105) — planned

| Service | Purpose |
|---|---|
| Prometheus | Metrics collection |
| Grafana | Dashboards/visualization |
| Pi-hole | Network-wide DNS ad-blocking |

## Request flow

When a user requests a movie or show in Jellyseerr:

1. **Jellyseerr** sends the request to Radarr (movies) or Sonarr (TV)
2. **Radarr/Sonarr** search configured indexers (via Prowlarr) and pick the best release based on quality profile rules
3. The release is sent to **qBittorrent**
4. **qBittorrent** downloads the file
5. **Radarr/Sonarr** move and rename the file into the media library
6. **Jellyfin** scans the library and the new content becomes available to stream

```
Jellyseerr → Radarr/Sonarr → (Prowlarr indexers) → qBittorrent → Radarr/Sonarr (rename/move) → Jellyfin
```

## Notes

- All services run as Docker containers via Compose, with config/data under bind-mounted host directories (see [`docs/docker/compose-basics.md`](./docker/compose-basics.md))
- Media files are stored on a separately mounted disk, decoupled from container config backups
- `mkvtoolnix` is planned for converting mp4/mov + srt files into MKV format
