# Services

## Media stack (VM 101)

| Service    | Purpose                                       |
| ---------- | --------------------------------------------- |
| Jellyfin   | Media server — streams the library to clients |
| Jellyseerr | Front-end for users to request movies/shows   |
| Radarr     | Movies library management                     |
| Sonarr     | Shows library management                      |
| Bazarr     | Subtitle download and management              |
| Prowlarr   | Indexer management, feeds Radarr/Sonarr       |
| Bookstack  | Internal documentation wiki                   |

## Monitoring stack (VM 105) — planned

| Service | Purpose |
|---|---|
| Prometheus | Metrics collection |
| Grafana | Dashboards/visualization |
| Pi-hole | Network-wide DNS ad-blocking |

## Notes

- All services run as Docker containers via Compose, with config/data under bind-mounted host directories (see [`docs/docker/compose-basics.md`](Docker/compose-basics.md))
- Media files are stored on a separately mounted disk, decoupled from container config backups
- `mkvtoolnix` is planned for converting mp4/mov + srt files into MKV format
