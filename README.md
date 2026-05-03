# 🧠 Ced’s HomeLab (Enterprise Infrastructure & Monitoring Lab)

> A production-style infrastructure lab showcasing real-world systems engineering, monitoring, and platform operations.

This environment functions as a **personal datacenter**, combining virtualization, Kubernetes orchestration, observability, and secure external access.

---

## 🚀 What This Lab Demonstrates

- Infrastructure design (Proxmox + virtualized services)
- Kubernetes orchestration (12-node K3s cluster)
- Network segmentation (VLAN architecture)
- Monitoring & observability (Grafana, Prometheus, Uptime Kuma)
- Secure service exposure (Cloudflare Tunnels + reverse proxy)
- Real-world system integration (data, services, automation)

## 🔥 Featured Project: SOC Lab

A focused project within this homelab that demonstrates monitoring, logging, and security concepts.

👉 [View SOC Lab Project](soc-lab/README.md)
---

## 🏗️ Architecture Overview

This lab is built around three core layers:

### 🖥️ Infrastructure Layer
- Proxmox VE hypervisor
- Virtual Machines + LXC containers
- TrueNAS storage backend (ZFS, NFS, SMB)

### ☸️ Orchestration Layer
- 12-node K3s Kubernetes cluster
- Workload segmentation (ingress, data, monitoring)
- MetalLB + NGINX ingress

### 🌐 Access & Networking Layer
- VLAN segmented network (UDR)
- Nginx Proxy Manager
- Cloudflare Tunnel (zero port-forwarding)

---

## 🎯 Purpose

This lab is designed to:

- Simulate production-style environments
- Build hands-on infrastructure experience
- Develop monitoring and system visibility skills
- Serve as a real-world engineering portfolio project

---

## 📸 Key System Views

### 🖥️ Proxmox Infrastructure
![Proxmox](screenshots/proxmox-overview.png)

### 🖥️ Proxmox Workloads
![Proxmox](screenshots/proxmox-overview2.png)

### ☸️ K3s Cluster Nodes & Pods
![K3s Nodes](screenshots/K3s-nodes.png)

### 🌐 Reverse Proxy (Nginx Proxy Manager)
![Nginx](screenshots/NGN.png)

### 📊 Service Monitoring (Uptime Kuma)
![Uptime Kuma](screenshots/uptime-kuma.png)

### 📊 Grafana Dashboard
![Grafana](screenshots/grafana.png)

---

## 🌐 Network & VLANs

The lab runs behind a UniFi Dream Router (UDR) with VLAN segmentation:

| Network | Subnet | Purpose |
|--------|--------|--------|
| Main | 10.10.10.0/24 | Daily-use devices |
| MyHomeIOT | 10.10.20.0/24 | IoT devices, TVs, consoles |
| HomeLab | 10.10.30.0/24 | Servers, services, K3s, storage |
| Guest | 10.10.99.0/24 | Guest Wi-Fi |

The HomeLab VLAN (10.10.30.0/24) hosts all core infrastructure.

---

## 🧱 Core Components

### 🖥️ Proxmox VE
- Main hypervisor for VMs and LXCs
- Future expansion to a Proxmox cluster (+5 nodes)
- Uses TrueNAS for shared storage (NFS / iSCSI)  
📁 See: `proxmox/`

---

### 💾 TrueNAS (ZFS Storage)
- Manages ZFS pools and datasets
- NFS exports for Proxmox VM storage
- SMB / media dataset for Jellyfin & Arr stack  
📁 See: `truenas/`

---

### ☸️ K3s Raspberry Pi Cluster
A 12-node K3s cluster built on Raspberry Pi hardware for orchestrating containerized workloads across the lab.

Current and planned uses include:

- Containerized applications
- Ingress-based routing
- Monitoring workloads
- Future GitOps and Helm-based deployments

📌 Full cluster repo:  
https://github.com/ced4568/ced-k3s-homelab

---

### 🌐 Reverse Proxy & Cloudflare Tunnel
- Nginx Proxy Manager (NPM)
- Cloudflare Tunnel (no port forwarding)
- Wildcard DNS: `*.cedshomelab.com`

**Traffic Flow:**
Internet → Cloudflare Edge → Tunnel → NPM → Internal Services

📁 Docs: `docs/Add_New_Service_Guide.md`

---

### 🏠 Home Automation
- Home Assistant (Proxmox VM)
- IoT isolated on MyHomeIOT VLAN
- Secure access via Cloudflare + NPM
- `trusted_proxies` configured

📁 Config: `home-assistant/`

---

### 📊 Observability (Ced’s NOC)

The lab includes an observability stack built around:

- Prometheus for metrics collection
- Grafana for dashboards and visualization
- Uptime Kuma for service-level monitoring

Current and planned monitoring coverage includes:
- Proxmox performance
- K3s cluster health
- Service uptime
- Infrastructure visibility improvements over time

📁 See: `monitoring/`

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

- Add new service: `docs/Add_New_Service_Guide.md`
- Architecture diagrams: `docs/Ced_Homelab_Diagrams.md`
- Roadmap: `docs/roadmap.md`

---

## 🚀 Future Plans

- Full Proxmox cluster
- Cloudflare Zero Trust integration
- GitOps for K3s deployments
- Internal container registry
- Advanced monitoring + alerting
- Portfolio site:
  - chasedumphord.com  
  - cedshomelab.com  

---

## ⚠️ Security Practices

This repo never stores:

- API tokens  
- Private keys  
- Passwords  
- Sensitive configs  

All secrets are handled locally or via `.example` files.

---

## 👤 Author

**Chase Dumphord**  
Digital Systems Engineer | Infrastructure | Data Systems | Automation  

LinkedIn: https://www.linkedin.com/in/toochase-dumphord/  
GitHub: https://github.com/ced4568
