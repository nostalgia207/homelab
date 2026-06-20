# Docker Stack Backup & Restore

## What gets backed up

Everything for each service lives under `/srv/docker/<service>/` — the compose file plus its bind-mounted config/data directories.

> Container images don't need backing up since Docker re-pulls those. Media files live on a separately mounted disk and aren't part of this backup.

## Creating the backup archive

```bash
sudo tar -czvf docker-stack-backup.tar.gz /srv/docker
```

This produces a single archive containing every compose file and every app's config/database files.

## Transferring the archive

Use SSH/WinSCP to pull the archive off the source VM, then push it to the destination VM's home directory.

## Installing Docker on the destination VM

```bash
# Remove any old/conflicting packages
sudo apt remove docker docker-engine docker.io containerd runc

# Install prerequisites
sudo apt update
sudo apt install -y ca-certificates curl

# Add Docker's GPG key
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the Docker repository
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker and the Compose plugin
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Allow the current user to run Docker without sudo
sudo usermod -aG docker $USER   # log out/in or `newgrp docker` to apply

# Verify
docker --version
docker compose version
```

## Restoring the stack

```bash
sudo mkdir -p /srv/docker
sudo tar -xzvf docker-stack-backup.tar.gz -C /
sudo chown -R 1000:1000 /srv/docker   # important even if UID matches, especially on a new user with a different UID

cd /srv/docker/<service>
docker compose up -d
```

## Things to double-check after restoring

- Re-attach/mount the media disk at the same path as before — otherwise libraries will appear empty
- Check for port conflicts if other services are already running on the new VM
- Apps that store API keys/URLs pointing at specific hostnames or IPs (e.g. Jellyseerr → Radarr/Sonarr, Prowlarr indexers) may need updating if the new VM's address differs from the old one
