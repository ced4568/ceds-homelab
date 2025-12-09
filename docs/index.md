# Ced's HomeLab (Architecture Portfolio)

> A cloud-connected, multi-node homelab replicating modern datacenter design patterns for learning, experimentation, and portfolio work.

---
## 🔎 Summary

Ced’s HomeLab is a **fully segmented, container-first infrastructure** designed to simulate the architecture, security, and operational practices found in modern production environments.  
It brings together virtualization, distributed storage, Kubernetes, infrastructure networking, reverse proxy routing, cloud edge security, observability, and home automation — all interconnected through a clean DNS and identity entrypoint.

This environment powers:
- **A Proxmox virtualization platform**
- **A 12-node Raspberry Pi K3s cluster** for Kubernetes
- **TrueNAS ZFS storage** with NFS media datasets
- **Cloudflare Tunnel for remote access** (no open ports)
- **NGINX Proxy Manager** for internal routing
- **Grafana + Prometheus** observability
- **Home Assistant automation**
- **Media stack (Arr suite + Jellyfin)**

The result is a compact, self-hosted environment that mirrors the systems used in enterprise IT and cloud-native deployments.

---

## 🏗️ Architecture Overview

The lab is built around **three pillars**:

### 1) Virtualization Layer
**Proxmox VE** hosts all core infrastructure VMs/LXCs:
- Nginx Proxy Manager
- Home Assistant
- Grafana / Prometheus / Uptime Kuma
- Arr suite + Jellyfin
- etc.

It is the compute layer for workloads that benefit from full system isolation.

---

### 2) Kubernetes Layer
A **12-node Raspberry Pi K3s cluster** provides a distributed environment for:
- container deployments
- ingress-based routing
- GitOps automation (future)
- service-oriented workloads

This is where “cloud-native” architecture lives.

K3s is documented here:
👉 https://github.com/ced4568/ced-k3s-homelab

---

### 3) Storage Layer
**TrueNAS SCALE** provides:
- ZFS pools for reliability
- NFS shares for Proxmox VM storage
- Snapshots for versioning
- SMB for media datasets

Storage is intentionally kept external from compute to mirror modern datacenter patterns.

---

## 🔐 Zero-Trust Access

All external access uses **Cloudflare Tunnel**, not port forwarding.  

### Public entrypoint
