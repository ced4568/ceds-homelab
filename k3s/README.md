# K3s Cluster (Raspberry Pi)

This folder documents how the Raspberry Pi K3s cluster fits into Ced's HomeLab.

The detailed manifests and cluster-specific configuration live in a separate repo:

- https://github.com/ced4568/ced-k3s-homelab

## Goals

- Learn Kubernetes on real hardware
- Run homelab apps on K3s instead of directly on VMs
- Use Ingress + wildcard DNS for clean routing
- Eventually adopt GitOps workflows

## Structure (in this repo)

- `ingresses/` – example host-based ingress rules
- `deployments/` – high-level deployment patterns or sample manifests

Example target pattern:

- `myapp.apps.cedshomelab.com` → Traefik Ingress → K3s Service
