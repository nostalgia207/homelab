# Docker Compose Basics

## Setting up a service directory

```bash
sudo mkdir -p /srv/docker/<service>/{config,data}
sudo chown -R 1000:1000 /srv/docker/<service>
```

Then create a `compose.yml` file inside the service directory.

## Compose file structure

- **services**: top-level block describing each container to run
- **image**: pulled as `repository/image:tag` — `latest` always pulls the newest version; pin a specific version (e.g. `jellyfin/jellyfin:10.8.13`) for stability
- **container_name**: used to reference the container in Docker commands (e.g. `docker restart <name>`)
- **user**: `"UID:GID"` — should match whichever user owns the bind-mounted folders
- **ports**: `"host_port:container_port"`
- **volumes**: `host_path:container_path` — so writes inside the container land on the host filesystem
- **restart**: `unless-stopped` — restarts the container on crash or VM reboot, unless it was manually stopped

## Starting the stack

```bash
cd /srv/docker/<service>
docker compose up -d
```
