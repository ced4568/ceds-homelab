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

## 📊 Live NOC & Automation Pipeline

The homelab powers a public-facing live NOC dashboard at **noc.chasedumphord.com**.

An automated Python script runs on Biggie (Proxmox node 10.10.30.192) every 5 minutes via cron. It pings all 28 systems, records latency and online/offline status, writes a structured JSON file, and pushes it to GitHub. The dashboard updates automatically.

```text
Biggie (10.10.30.192) — cron: */5 * * * *
  → noc_update.py
    → ICMP ping 28 systems
    → record latency + status
    → write data/noc-status.json
    → git commit + push to ced-portfolio
      → GitHub Pages deploys (~30s)
        → noc.chasedumphord.com auto-refreshes
```

- Script: `~/scripts/noc_update.py` on Biggie
- Log: `/var/log/noc_update.log`
- NOC source: https://github.com/ced4568/ceds-noc
- Live dashboard: https://noc.chasedumphord.com

---

## 🖥️ Full Node Inventory (28 Systems)

### Proxmox Cluster — 6 Nodes

| Node | IP | Role |
|------|----|------|
| BigWorld | 10.10.30.250 | Proxmox VE Hypervisor |
| Biggie | 10.10.30.192 | Proxmox VE Hypervisor — NOC script host |
| Snoop | 10.10.30.153 | Proxmox VE Hypervisor |
| TooShort | 10.10.30.120 | Proxmox VE Hypervisor |
| Tupac | 10.10.30.74 | Proxmox VE Hypervisor |
| DrDre | 10.10.30.227 | Proxmox VE Hypervisor |

### K3s Cluster — 12 Nodes

| Role | Node | IP |
|------|------|----|
| Control Plane | django-1 | 10.10.30.72 |
| Control Plane | django-2 | 10.10.30.245 |
| Control Plane | django-3 | 10.10.30.128 |
| Ingress Worker | node-1 | 10.10.30.219 |
| Ingress Worker | node-2 | 10.10.30.134 |
| Ingress Worker | node-3 | 10.10.30.222 |
| Data Worker | node-4 | 10.10.30.126 |
| Data Worker | node-5 | 10.10.30.239 |
| Data Worker | node-6 | 10.10.30.208 |
| Monitoring Worker | node-7 | 10.10.30.198 |
| Monitoring Worker | node-8 | 10.10.30.216 |
| Monitoring Worker | node-9 | 10.10.30.29 |

### Services, Monitoring & Edge — 10 Systems

| System | IP | Layer | Role |
|--------|----|-------|------|
| TrueNAS | 10.10.30.143 | Infrastructure | Network Attached Storage |
| Nginx Proxy Manager | 10.10.30.210 | Networking | Reverse Proxy |
| Prometheus | 10.10.30.140 | Monitoring | Metrics Scraping (Active) |
| Grafana | 10.10.30.68 | Monitoring | Metrics Dashboard |
| Uptime Kuma | 10.10.30.14 | Monitoring | Service Monitor |
| Dashy | 10.10.30.61 | Services | HomeLab Dashboard |
| Home Assistant | 10.10.30.104 | Services | Home Automation |
| PrimeStation | 10.10.30.233 | Services | Media Server |
| APRS iGate Home | 10.10.30.129 | Edge | RF to APRS-IS Gateway |
| APRS iGate Mobile | 10.10.30.35 | Edge | RF Mobile Node |

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
