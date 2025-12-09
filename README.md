# Ced's HomeLab

Ced's HomeLab is my personal mini–datacenter: a segmented, cloud-connected environment built for learning, experimentation, and portfolio work. It includes:

- Proxmox virtualization
- TrueNAS ZFS storage
- A 12-node Raspberry Pi K3s cluster
- Nginx Proxy Manager + Cloudflare Tunnel
- Home Assistant + IoT integration
- Media stack (Arr suite + Jellyfin)
- Observability (Grafana, Prometheus, Uptime Kuma)

This repo is the **documentation and configuration hub** for the entire environment.

---

## 🌐 Network & VLANs

The lab runs behind a UniFi Dream Router (UDR) with VLAN segmentation:

| Network      | Subnet         | Purpose                         |
|--------------|----------------|---------------------------------|
| Main         | 10.10.10.0/24  | Daily-use devices               |
| MyHomeIOT    | 10.10.20.0/24  | IoT devices, TVs, consoles      |
| HomeLab      | 10.10.30.0/24  | Servers, services, K3s, storage |
| Guest        | 10.10.99.0/24  | Guest Wi-Fi                     |

The HomeLab VLAN (10.10.30.0/24) hosts all core infrastructure.

---

## 🧱 Core Components

### Proxmox VE

- Main hypervisor for VMs and LXCs
- Future expansion to a Proxmox cluster (+5 nodes)
- Uses TrueNAS for shared storage (NFS / iSCSI)

See: [`proxmox/`](proxmox/)

---

### TrueNAS (ZFS Storage)

- Manages ZFS pools and datasets
- NFS exports for Proxmox VM storage
- SMB / media dataset for Jellyfin & Arr stack

See: [`truenas/`](truenas/)

---

### K3s Raspberry Pi Cluster

A 12-node K3s cluster (Raspberry Pis) for running:

- Containerized apps
- Ingress-based routing
- GitOps and helm-based workloads (future)

K3s is part of the HomeLab but documented in detail in its own repo:
- https://github.com/ced4568/ced-k3s-homelab

This repo may reference that cluster and contain high-level config patterns.

---

### Reverse Proxy & Cloudflare Tunnel

- Nginx Proxy Manager (NPM) on the HomeLab VLAN
- Cloudflare Tunnel (no port forwarding)
- Wildcard DNS: `*.cedshomelab.com`

External access flow:

```text
Internet → Cloudflare Edge → Tunnel → NPM → Internal Services
```

Docs: [`docs/Add_New_Service_Guide.md`](docs/Add_New_Service_Guide.md)

---

### Home Automation

- Home Assistant (HA) in Proxmox
- IoT devices isolated on MyHomeIOT VLAN
- HA exposed via Cloudflare + NPM
- `trusted_proxies` configured for NPM

Config snippets: [`home-assistant/configuration-snippets/`](home-assistant/configuration-snippets/)

---

### Observability

- Prometheus scrapes metrics from:
  - Proxmox
  - K3s
  - TrueNAS (future)
  - Home Assistant / MQTT (future)
- Grafana dashboards, including a **Ced's NOC** view
- Uptime Kuma for service-level monitoring

Notes and dashboards: [`monitoring/`](monitoring/)

---

## 🧭 Documentation

- **How to add a new service via NPM + Cloudflare**  
  [`docs/Add_New_Service_Guide.md`](docs/Add_New_Service_Guide.md)

- **Architecture diagrams (Mermaid)**  
  [`docs/Ced_Homelab_Diagrams.md`](docs/Ced_Homelab_Diagrams.md)

- **Roadmap / future ideas**  
  [`docs/roadmap.md`](docs/roadmap.md)

---

## 🚀 Future Plans

- Full Proxmox cluster
- Cloudflare Zero Trust on critical services
- GitOps for K3s deployments (in ced-k3s-homelab)
- Internal container registry
- K3s-specific wildcard routing (`*.apps.cedshomelab.com`)
- More detailed monitoring + alerting
- Visual portfolio site on:
  - `chasedumphord.com` or
  - `cedshome.com`

---

## ⚠️ Secrets & Security

This repo **never** stores:

- API tokens
- Private keys
- Passwords
- Secret YAML files

Any real config with secrets should be kept locally only, or represented here as `*.example` files.

---

## ✨ Author

**Ced (Chase Dumphord)**  
Cybersecurity / GRC / SOC • Full-Stack Dev • Homelab builder
