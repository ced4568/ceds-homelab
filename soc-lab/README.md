# SOC Lab Monitoring, Logging & Security Simulation

## Overview

This SOC Lab is a focused project within my homelab environment designed to simulate real-world monitoring, logging, and basic security detection workflows.

The goal is to replicate how modern infrastructure teams observe system behavior, detect anomalies, and maintain service reliability.

---

## Infrastructure Backbone

* 6-node Proxmox VE cluster (virtualization platform)
* Virtual Machines and LXC containers
* Segmented network environment (VLANs)

![Proxmox](../screenshots/proxmox-overview.png)

---

## Kubernetes Environment

* 12-node K3s cluster (Raspberry Pi)
* Control plane + worker node architecture
* Workload segmentation:

  * ingress
  * data
  * monitoring

```bash
kubectl get nodes -o wide
kubectl get pods -A
```

![K3s](../screenshots/K3s-nodes.png)

---

## Traffic & Service Routing

* Nginx Proxy Manager (reverse proxy)
* Cloudflare Tunnel (secure external access)
* Subdomain-based service exposure

![Nginx](../screenshots/NGN.png)

---

## Monitoring & Observability

* Prometheus (metrics collection)
* Grafana (dashboard visualization)
* Uptime Kuma (service monitoring)

### Key Capabilities:

* System performance tracking
* Service uptime monitoring
* Infrastructure visibility

![Uptime Kuma](../screenshots/uptime-kuma.png)

---

## Security Layer (In Progress)

* CrowdSec (intrusion detection & prevention)
* Basic firewall and access control concepts
* Monitoring suspicious traffic patterns

---

## Logging Pipeline (Planned)

* Grafana Loki (log aggregation)
* Centralized log visibility
* Correlation between logs and system activity

---

## Attack Simulation (Planned)

To validate monitoring and logging systems, the following simulations are planned:

* Network scanning (nmap)
* Failed authentication attempts
* Traffic pattern analysis

```bash
nmap -A <target-ip>
```

---

## Skills Demonstrated

* Infrastructure design and deployment
* Kubernetes cluster management
* Monitoring and observability implementation
* Reverse proxy and traffic routing
* Production systems thinking and operational discipline

---

## Future Enhancements

* Full logging pipeline (Loki integration)
* Alerting (Grafana alerts)
* Security event tracking
* Automated deployments (CI/CD)

---

## Key Takeaway

This lab demonstrates real infrastructure ownership designing, building, and operating a distributed system with full observability stack, security detection, and cloud-native tooling reflecting production-grade platform engineering practices.