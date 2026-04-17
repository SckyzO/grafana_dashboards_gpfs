# Metrics Reference

All metrics are collected by the [ibm-spectrum-scale-bridge-for-grafana](https://github.com/IBM/ibm-spectrum-scale-bridge-for-grafana) exporter and exposed to Prometheus.

---

## Common labels

| Label | Description |
|-------|-------------|
| `cluster` | GPFS cluster name |
| `env` | Environment (e.g. `prod`) |
| `instance` | Exporter instance (`<ems-node>:9250`) |
| `job` | Prometheus scrape job name |
| `node` | GPFS node FQDN |

---

## GPFSmmhealth — Cluster Health

Additional labels: `gpfs_health_component`, `gpfs_health_entity`

| Metric | Type | Description |
|--------|------|-------------|
| `gpfs_health_status` | gauge | Component health: `10`=OK, `20`=warning, `30`=error |
| `gpfs_health_detail_status` | gauge | Detailed health status (same scale) |
| `gpfs_health_error_events` | gauge | Number of active error events |
| `gpfs_health_warning_events` | gauge | Number of active warning events |

Health components include: `CANISTER/SERVER`, `FILESYSTEM`, `GPFS`, `GUI`, `NATIVE_RAID`, `NETWORK`, and others.

---

## GPFSPool — Storage Capacity

Additional labels: `gpfs_cluster_name`, `gpfs_diskpool_name`, `gpfs_fs_name`

| Metric | Unit | Description |
|--------|------|-------------|
| `gpfs_pool_free_dataKB` | KB | Free data space in pool |
| `gpfs_pool_total_dataKB` | KB | Total data space in pool |
| `gpfs_pool_free_metaKB` | KB | Free metadata space in pool |
| `gpfs_pool_total_metaKB` | KB | Total metadata space in pool |

> All values are in **kilobytes**. Multiply by `1024` to get bytes, or use Grafana unit `kbytes`.

---

## GPFSFilesystem — Filesystem I/O

Additional labels: `gpfs_cluster_name`, `gpfs_fs_name`

| Metric | Type | Description |
|--------|------|-------------|
| `gpfs_fs_bytes_read` | counter | Cumulative bytes read |
| `gpfs_fs_bytes_written` | counter | Cumulative bytes written |
| `gpfs_fs_read_ops` | counter | Cumulative read operations |
| `gpfs_fs_write_ops` | counter | Cumulative write operations |
| `gpfs_fs_disks` | gauge | Number of disks in the filesystem |
| `gpfs_fs_tot_disk_wait_rd` | counter | Cumulative disk read wait time (µs) |
| `gpfs_fs_tot_disk_wait_wr` | counter | Cumulative disk write wait time (µs) |
| `gpfs_fs_max_disk_wait_rd` | gauge | Max disk read wait time observed (µs) |
| `gpfs_fs_max_disk_wait_wr` | gauge | Max disk write wait time observed (µs) |
| `gpfs_fs_min_disk_wait_rd` | gauge | Min disk read wait time observed (µs) |
| `gpfs_fs_min_disk_wait_wr` | gauge | Min disk write wait time observed (µs) |
| `gpfs_fs_max_queue_wait_rd` | gauge | Max queue read wait time (µs) |
| `gpfs_fs_max_queue_wait_wr` | gauge | Max queue write wait time (µs) |

---

## GPFSNode — Per-node I/O Stats

| Metric | Type | Description |
|--------|------|-------------|
| `gpfs_ns_bytes_read` | counter | Cumulative bytes read by node |
| `gpfs_ns_bytes_written` | counter | Cumulative bytes written by node |
| `gpfs_ns_read_ops` | counter | Cumulative read ops by node |
| `gpfs_ns_write_ops` | counter | Cumulative write ops by node |
| `gpfs_ns_disks` | gauge | Disks attached to the node |
| `gpfs_ns_clusters` | gauge | Clusters visible from node |
| `gpfs_ns_filesys` | gauge | Filesystems mounted on node |
| `gpfs_ns_tot_disk_wait_rd` | counter | Cumulative disk read wait (µs) |
| `gpfs_ns_tot_disk_wait_wr` | counter | Cumulative disk write wait (µs) |

---

## GPFSTSCOM — Inter-node Communication

Additional labels: `gpfs_tscom_connection`, `gpfs_tscom_destination`

| Metric | Type | Description |
|--------|------|-------------|
| `gpfs_tscom_stat` | gauge | Link status: `1`=connected, `0`=disconnected |
| `gpfs_tscom_ca_stat` | gauge | CA link status |
| `gpfs_tscom_lost` | counter | Lost packets |
| `gpfs_tscom_reconnect` | counter | Reconnection events |
| `gpfs_tscom_retrans` | counter | Retransmitted packets |
| `gpfs_tscom_retransmits` | counter | Retransmission count |
| `gpfs_tscom_sent_bytes` | counter | Cumulative sent bytes |
| `gpfs_tscom_recvd_bytes` | counter | Cumulative received bytes |
| `gpfs_tscom_sent_msgs` | counter | Cumulative sent messages |
| `gpfs_tscom_recvd_msgs` | counter | Cumulative received messages |
| `gpfs_tscom_rtt` | gauge | Round-trip time (µs) |
| `gpfs_tscom_rttvar` | gauge | RTT variance (µs) |
| `gpfs_tscom_rto` | gauge | Retransmission timeout (µs) |
| `gpfs_tscom_pmtu` | gauge | Path MTU (bytes) |

---

## GPFSVFSX — VFS Extended Stats

| Metric | Type | Description |
|--------|------|-------------|
| `gpfs_vfsx_read` | counter | VFS read call count |
| `gpfs_vfsx_write` | counter | VFS write call count |
| `gpfs_vfsx_open` | counter | VFS open call count |
| `gpfs_vfsx_close` | counter | VFS close call count |
| `gpfs_vfsx_create` | counter | VFS create call count |
| `gpfs_vfsx_lookup` | counter | VFS lookup call count |
| `gpfs_vfsx_readdir` | counter | VFS readdir call count |
| `gpfs_vfsx_fsync` | counter | VFS fsync call count |
| `gpfs_vfsx_read_t` | counter | Cumulative read latency (µs) |
| `gpfs_vfsx_write_t` | counter | Cumulative write latency (µs) |
| `gpfs_vfsx_open_t` | counter | Cumulative open latency (µs) |
| `gpfs_vfsx_close_t` | counter | Cumulative close latency (µs) |
| `gpfs_vfsx_create_t` | counter | Cumulative create latency (µs) |
| `gpfs_vfsx_lookup_t` | counter | Cumulative lookup latency (µs) |
| `gpfs_vfsx_readdir_t` | counter | Cumulative readdir latency (µs) |
| `gpfs_vfsx_fsync_t` | counter | Cumulative fsync latency (µs) |
| `gpfs_vfsx_*_tmax` | gauge | Max observed latency per call type (µs) |
| `gpfs_vfsx_*_tmin` | gauge | Min observed latency per call type (µs) |

---

## Computing average latency

Latency metrics (`tot_disk_wait`, `vfsx_*_t`) are **cumulative counters**, not instant values. To get average latency per operation use:

```promql
# Average read disk latency per node (ms)
rate(gpfs_ns_tot_disk_wait_rd{cluster="$cluster"}[5m])
  / rate(gpfs_ns_read_ops{cluster="$cluster"}[5m]) / 1000

# Average VFS read latency per node (µs)
rate(gpfs_vfsx_read_t{cluster="$cluster", node=~"$node"}[5m])
  / rate(gpfs_vfsx_read{cluster="$cluster", node=~"$node"}[5m])
```
