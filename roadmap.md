# DevOps Roadmap

The plan: build a small full-stack app, then practice progressively real ways of deploying it. Manual Docker, then IaC, then Kubernetes, then CI/CD while using this homelab as the target environment.

## Stage 1 — Linux Fundamentals (reinforce)

- [x] Re-do basic Ubuntu setup from scratch as practice: users/groups, SSH key auth, ufw firewall basics
- [ ] systemd: write a custom unit file for a script/service
- [x] Shell scripting: write a small backup script for Docker volumes to external disk

## Stage 2 — Git / Version Control

- [ ] Install Gitea (compose, like other services) or use GitHub
- [ ] Move existing compose files / scripts into a git repo
- [ ] Practice branches, commits, `.gitignore`, basic PR workflow

## Stage 3 — Docker

- [ ] Write a custom Dockerfile (not just pulling images) for a simple app
- [ ] Multi-stage builds
- [ ] Custom Docker networks, created manually with `docker network create`

## Stage 4 — Node.js + React (the app)

- [ ] Build a small full-stack app (homelab dashboard showing service status / links)
- [ ] Backend: Node.js + Express, simple API
- [ ] Frontend: React (Vite), calls the API
- [ ] Containerize both

## Stage 5 — Infrastructure as Code: Terraform

- [ ] Install Terraform on a management VM or desktop
- [ ] Use a Proxmox provider to define VMs as code instead of the GUI
- [ ] Use cloud-init templates so new VMs auto-configure on first boot
- [ ] Goal: destroy and recreate a test VM purely from `.tf` files

## Stage 6 — Configuration Management

- [ ] Ansible: write a playbook that installs Docker + compose stacks on a fresh VM
- [ ] Combine with Terraform. Terraform creates the VM, Ansible configures it

## Stage 7 — Kubernetes (start small: k3s)

- [ ] Install k3s on a single node to start
- [ ] Learn core objects: Pod, Deployment, Service, Ingress, ConfigMap, PVC
- [ ] Deploy the Node/React app to k3s instead of plain Docker
- [ ] Once comfortable, try a 2–3 node k3s cluster

## Stage 8 — CI/CD

- [ ] GitHub Actions: on push, build a Docker image, push to a registry
- [ ] Pipeline auto-deploys the updated image to k3s (via `kubectl`)

## Stage 9 — Monitoring & Observability

- [ ] Prometheus + Grafana, targeting the k3s cluster as well as the monitoring VM
- [ ] Dashboards for the media stack containers and the new app

## Stage 10 — Tie It All Together

- [ ] Full loop: push code → CI builds image → Terraform-provisioned infra → k3s deploys → Grafana shows it's running
- [ ] At this point: Linux, Git, Docker, Node/React, Terraform, Kubernetes, CI/CD, and monitoring — the full "DevOps" set

## Notes

- No need to follow these strictly in order — Stages 1–4 are good foundational practice, but Terraform (5) and k3s (7) can be explored in parallel once comfortable with Docker
- The stable media stack VM stays untouched; all experimentation happens on a separate, disposable VM
