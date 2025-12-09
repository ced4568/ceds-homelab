# Ced's HomeLab Roadmap

## Short Term

- Document current services and IPs
- Export Grafana "Ced's NOC" dashboard JSON
- Add Prometheus scrape configs for:
  - Proxmox
  - K3s
  - TrueNAS
- Set up standardized NPM host naming

## Medium Term

- Build `*.apps.cedshomelab.com` routing for K3s
- Add Cloudflare Zero Trust in front of:
  - Proxmox
  - TrueNAS
  - Grafana
  - Home Assistant
- Set up backup strategies:
  - Proxmox → TrueNAS
  - TrueNAS snapshots

## Long Term

- Full Proxmox cluster with HA
- GitOps flows for K3s (ArgoCD or Flux)
- Internal Docker/OCI registry
- Portfolio site hosted on:
  - `chasedumphord.com` or
  - `cedshome.com`
- More detailed, automated documentation

## Monitoring To-Do

- [ ] Add Prometheus exporters:
  - [ ] Proxmox exporter
  - [ ] TrueNAS / ZFS exporter
  - [ ] SMART / disk health exporter
  - [ ] Uptime Kuma → Prometheus
  - [ ] Cloudflare / tunnel metrics (if possible)
