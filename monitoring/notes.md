# Monitoring Notes

- Describe Prometheus targets, exporters, and Grafana dashboards here.
## Ced's NOC Panel Documentation

| Panel Name          | Data Source  | Description                                 |
|---------------------|---------------|---------------------------------------------|
| Proxmox CPU Usage   | Prometheus    | Tracks CPU load across all Proxmox nodes    |
| Proxmox Memory      | Prometheus    | Memory utilization per VM                   |
| ZFS Pool Health     | TrueNAS exporter | Reports pool state & errors             |
| ZFS Free Space      | TrueNAS exporter | Dataset and free capacity                |
| K3s Node Status     | kube-state-metrics | Node Ready, etc                        |
| Pod Restart Count   | kube-state-metrics | Detects unhealthy workloads             |
| Network Throughput  | Node exporter | RX/TX bandwidth per node                    |
| Uptime Checks       | Uptime Kuma (Prometheus) | External service checks       |
| Cloudflare Tunnel   | custom exporter | Tunnel availability / heartbeat            |
