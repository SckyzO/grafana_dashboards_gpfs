# Grafana Dashboards for IBM Spectrum Scale (GPFS)

A set of production-ready Grafana dashboards for monitoring IBM Spectrum Scale (GPFS) clusters, built on top of the [ibm-spectrum-scale-bridge-for-grafana](https://github.com/IBM/ibm-spectrum-scale-bridge-for-grafana) exporter.

## Compatibility

| Component | Tested version |
|-----------|---------------|
| IBM Spectrum Scale (GPFS) | 5.2.3 |
| ibm-spectrum-scale-bridge-for-grafana | [v9.0.2](https://github.com/IBM/ibm-spectrum-scale-bridge-for-grafana/releases/tag/v9.0.2) (2026-03-05) |
| Grafana | ≥ 10.x |
| Prometheus | ≥ 2.x |

## Dashboards

| | | |
|:---:|:---:|:---:|
| **01 — GPFS Overview**<br>Cluster-wide health summary, throughput and capacity at a glance | **02 — Storage Capacity**<br>Per-pool usage (data, burst, system), free/used space trends | **03 — I/O Performance per Node**<br>Read/write throughput, IOPS and disk latency per node |
| [![01 — GPFS Overview](screenshots/01-GPFS-Overview.png)](screenshots/01-GPFS-Overview.png) | [![02 — Storage Capacity](screenshots/02-GPFS-Storage-Capacity.png)](screenshots/02-GPFS-Storage-Capacity.png) | [![03 — I/O Performance per Node](screenshots/03-GPFS-I-O-Performance-per-Node.png)](screenshots/03-GPFS-I-O-Performance-per-Node.png) |
| **04 — TSCOM Communication**<br>Connection status, RTT, retransmits and bandwidth between nodes | **05 — VFS Latency (VFSX)**<br>Per-node VFS call latency (read, write, open, lookup, readdir…) | **06 — Cluster Health by Node**<br>Per-component health status (GPFS, FILESYSTEM, NETWORK, NATIVE_RAID…) |
| [![04 — TSCOM Communication](screenshots/04-GPFS-TSCOM-Inter-node-Communication.png)](screenshots/04-GPFS-TSCOM-Inter-node-Communication.png) | [![05 — VFS Latency](screenshots/05-GPFS-VFS-Latency-VFSX.png)](screenshots/05-GPFS-VFS-Latency-VFSX.png) | [![06 — Cluster Health by Node](screenshots/06-GPFS-Cluster-Health-by-Node.png)](screenshots/06-GPFS-Cluster-Health-by-Node.png) |

All dashboards are linked together via a **"GPFS Dashboards"** navigation group.

## Quick start

1. In Grafana, go to **Dashboards → Import**
2. Upload the desired `dashboard_*.json` file from the `dashboards/` directory
3. Select your Prometheus datasource
4. Repeat for each dashboard

→ See [docs/getting-started.md](docs/getting-started.md) for full setup instructions including Prometheus scrape configuration.

## Template variables

Each dashboard auto-populates its variables from Prometheus label values — no manual configuration needed.

| Variable | Scope |
|----------|-------|
| `$cluster` | All dashboards |
| `$node` | Per-node dashboards (03, 04, 05, 06) |
| `$pool` | Capacity dashboard (02) |

## Metrics

Dashboards rely on the following Prometheus jobs exported by the bridge:

| Job | Coverage |
|-----|----------|
| `GPFSmmhealth` | Cluster health status per component |
| `GPFSPool` | Storage pool capacity |
| `GPFSFilesystem` | Filesystem-level I/O stats |
| `GPFSNode` | Per-node I/O stats |
| `GPFSTSCOM` | Inter-node communication stats |
| `GPFSVFSX` | VFS extended call stats and latency |

→ See [docs/metrics.md](docs/metrics.md) for the full metrics reference.

## Documentation

| Document | Description |
|----------|-------------|
| [Getting Started](docs/getting-started.md) | Prerequisites, exporter setup, Prometheus config, import steps |
| [Metrics Reference](docs/metrics.md) | All metrics by job, labels, units and PromQL examples |
| [Dashboard Reference](docs/dashboards.md) | Panel-by-panel description and interpretation tips |
