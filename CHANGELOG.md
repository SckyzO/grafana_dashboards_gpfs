# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [1.0.0] - 2026-04-17

### Added
- `01 — GPFS Overview`: cluster-wide health summary, throughput and capacity
- `02 — GPFS Storage Capacity`: per-pool usage with trends over time
- `03 — GPFS I/O Performance per Node`: throughput, IOPS and disk latency per node
- `04 — GPFS TSCOM Inter-node Communication`: RTT, retransmits, bandwidth between nodes
- `05 — GPFS VFS Latency (VFSX)`: per-node VFS call latency (read, write, open, fsync…)
- `06 — GPFS Cluster Health by Node`: per-component health status and event timeline
- Cross-dashboard navigation via shared "GPFS Dashboards" dropdown
- `author` and `source` metadata fields embedded in all dashboard JSON files
- Documentation: `docs/getting-started.md`, `docs/metrics.md`, `docs/dashboards.md`

### Tested with
- IBM Spectrum Scale (GPFS) 5.2.3
- ibm-spectrum-scale-bridge-for-grafana v9.0.2
- Grafana 12.x
- Prometheus 3.x
