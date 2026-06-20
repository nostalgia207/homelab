# Docker Startup & Tailscale Access

## Bringing the stack up

```bash
cd /srv/docker/<service>
docker compose up -d
docker compose ps   # confirms containers are "Up" / healthy
```

## Startup persistence

`restart: unless-stopped` in the compose file covers per-container persistence. Separately, confirm the Docker daemon itself starts on boot:

```bash
sudo systemctl enable docker
sudo systemctl is-enabled docker
```

Sanity-check without rebooting the whole VM:

```bash
sudo systemctl restart docker
docker compose ps   # containers should come back up automatically
```

## Tailscale

```bash
sudo tailscale status
```

If not installed:

```bash
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up   # provides a login URL to authenticate the device to the tailnet
```

Tailscale runs as a service alongside the OS, so it comes up automatically on boot once installed and authenticated.

## Getting relevant addresses

```bash
hostname -I        # LAN address
tailscale ip -4     # Tailscale address
```

## Accessing services on both LAN and Tailscale

- **Jellyfin** listens on all interfaces by default, so it's reachable via either address with no config change. For apps (e.g. mobile) to correctly switch between local and remote, set the "Published server URI" under Dashboard → Networking in Jellyfin's admin settings.
- **Bookstack** uses an `APP_URL` setting (`.env` or compose environment variables) to generate links throughout the app. Setting `APP_URL` to only the LAN address can break links when accessed via Tailscale, and vice versa — making both work seamlessly needs further configuration (a proxy or dynamic URL handling) rather than a single static `APP_URL`.

## Testing from outside the LAN

From a device on the tailnet but off the LAN (e.g. phone on mobile data), browse to the Tailscale address on the relevant service port.

If a service works on LAN but not via Tailscale, check:
- the container's `ports:` mapping binds to `0.0.0.0`, not `127.0.0.1`
- no firewall (`ufw`) on the VM is blocking the port — Tailscale traffic still passes through the host's firewall rules
