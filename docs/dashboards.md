# Dashboard Reference

## 01 — GPFS Overview

**UID:** `gpfs-overview-prod` | **Refresh:** 30s | **Default range:** last 1h

Entry point dashboard providing a cluster-wide snapshot. Designed to be the first screen checked during an incident.

| Panel | Type | What it shows |
|-------|------|---------------|
| Overall Health | Stat | Worst health status across all components (10/20/30) |
| Error Events | Stat | Total active error events across all components |
| Warning Events | Stat | Total active warning events |
| Active Nodes | Stat | Number of nodes with GPFS running |
| Component Health Status | Table | Health status per component and entity |
| Burst / Data / System Pool Usage | Gauge | % used per disk pool |
| Global I/O Throughput | Time series | Aggregate read + write throughput (bytes/s) |
| Read / Write Throughput | Stat | Current read and write throughput |
| Read / Write IOPS | Stat | Current read and write IOPS |

---

## 02 — GPFS Storage Capacity

**UID:** `gpfs-capacity-prod` | **Refresh:** 5m | **Default range:** last 24h

Capacity-focused dashboard. Refresh is intentionally slower (5 min) as pool sizes do not change rapidly.

| Panel | Type | What it shows |
|-------|------|---------------|
| Free / Total Data Space | Stat | Aggregated free and total data across pools |
| Free / Total Metadata Space | Stat | Aggregated metadata space |
| Burst / Data / System Pool % | Gauge | Usage percentage per pool |
| Data / Free Space per Pool | Bar gauge | Comparison across pools |
| Free Data / Metadata Over Time | Time series | Space trends over the selected time range |

> Pool metrics are in **kilobytes** — the `kbytes` Grafana unit is used throughout this dashboard.

---

## 03 — GPFS I/O Performance per Node

**UID:** `gpfs-io-perf-prod` | **Refresh:** 30s | **Default range:** last 1h

Per-node I/O breakdown. The `$node` variable supports multi-select for side-by-side comparison.

| Panel | Type | What it shows |
|-------|------|---------------|
| Read / Write Throughput per Node | Time series | Bytes/s per node |
| Read / Write IOPS per Node | Time series | Operations/s per node |
| Average Read / Write Disk Latency | Time series | `rate(tot_disk_wait) / rate(ops)` in ms |
| Read / Write Throughput — Node Comparison | Bar gauge | Snapshot comparison across nodes |

> Disk latency is derived from cumulative counters — the panel uses `irate()` divided by the ops rate. A spike in latency with stable IOPS indicates disk saturation.

---

## 04 — GPFS TSCOM Inter-node Communication

**UID:** `gpfs-tscom-prod` | **Refresh:** 30s | **Default range:** last 1h

Monitors the GPFS daemon-level TCP communication between nodes (TSCOM layer). Useful for diagnosing network-related performance issues.

| Panel | Type | What it shows |
|-------|------|---------------|
| Active Links | Stat | Number of connected TSCOM links |
| Lost Links | Stat | Links currently down |
| Avg RTT / Avg RTO | Stat | Mean round-trip time and retransmission timeout |
| Avg RTT per Source Node | Time series | RTT trend per node |
| RTT Variance per Node | Time series | RTT instability indicator |
| Top 15 Connections by RTT | Table | Slowest connections at current time |
| Retransmission Rate per Node | Time series | Packet retransmits/s — key indicator of network issues |
| Sent / Received Bytes | Time series | Bandwidth per node |
| Messages per Second | Time series | Message rate per node |

> High RTTVAR (RTT variance) with stable RTT often indicates intermittent congestion rather than sustained latency.

---

## 05 — GPFS VFS Latency (VFSX)

**UID:** `gpfs-vfsx-prod` | **Refresh:** 30s | **Default range:** last 1h

Client-side view of GPFS performance: measures the time applications spend inside VFS calls. Complements dashboard 03 which is disk-side.

| Panel | Type | What it shows |
|-------|------|---------------|
| Read / Write ops/s per Node | Time series | VFS read and write call rate |
| Open / Close / Create / Lookup / Readdir ops/s | Time series | Metadata operation rates |
| Read / Write / Open / Close / Fsync Latency | Time series | Average latency per call type (µs) |
| Max Read / Write Latency | Time series | Peak latency — highlights outlier events |

> VFS latency includes network + disk + GPFS overhead. If VFS latency is high but disk latency (dashboard 03) is normal, the bottleneck is likely in the GPFS layer or network.

---

## 06 — GPFS Cluster Health by Node

**UID:** `gpfs-health-detail-prod` | **Refresh:** 30s | **Default range:** last 1h

Detailed health view, scoped to a specific node via `$node`. Useful for post-incident review.

| Panel | Type | What it shows |
|-------|------|---------------|
| Components in Error / Degraded / Healthy | Stat | Count summary for the selected node |
| Total Warning Events | Stat | Warning event count |
| Health Status per Component | Table | Current status for each component + entity |
| Error & Warning Events per Component | Table | Event counts per component |
| Health Status Timeline | State timeline | Historical health state transitions per component |
| Error & Warning Events Over Time | Time series | Event counts trend |

> The state timeline panel is particularly useful for correlating health degradations with I/O or latency anomalies visible in other dashboards. Use the **"GPFS Dashboards"** dropdown to switch quickly.
