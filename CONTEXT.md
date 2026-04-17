# GPFS Dashboards - Contexte de production

## Infrastructure
- **Cluster** : mycluster / env: prod
- **FQDN cluster** : mycluster.example.com
- **Instance Prometheus** : node-ems:9250
- **Nœuds** : node-ems (EMS), node-io0, node-io1
- **Filesystem GPFS** : gpfs
- **Pools disques** : burst, data, system
- **Disques par nœud** : 10

## Source données
- Prometheus : http://<prometheus-host>:9090/prometheus
- Auth : <user> / <password>

## Métriques disponibles par groupe

### Health (job: GPFSmmhealth) - 708 series
Labels: cluster, env, gpfs_health_component, gpfs_health_entity, instance, job, node
- gpfs_health_status        (valeurs: 10=ok, 20=warning, 30=error)
- gpfs_health_detail_status
- gpfs_health_error_events
- gpfs_health_warning_events
Composants: CANISTER/SERVER, FILESYSTEM, GPFS, GUI, NATIVE_RAID, NETWORK, ...

### Pool Capacité (job: GPFSPool) - 3 series par metric
Labels: cluster, env, gpfs_cluster_name, gpfs_diskpool_name, gpfs_fs_name, instance, job
- gpfs_pool_free_dataKB     (burst: 298TB free / 414TB total, data: 2157TB/2190TB)
- gpfs_pool_total_dataKB
- gpfs_pool_free_metaKB     (system: 20TB free meta)
- gpfs_pool_total_metaKB

### Filesystem I/O (job: GPFSFilesystem) - 3 series par metric
Labels: cluster, env, gpfs_cluster_name, gpfs_fs_name, instance, job, node
- gpfs_fs_bytes_read        (io0: 1.5TB, ems: 74GB, io1: 10.9TB)
- gpfs_fs_bytes_written     (io0: 1.4TB, ems: 2.4GB, io1: 11TB)
- gpfs_fs_read_ops
- gpfs_fs_write_ops
- gpfs_fs_disks             (= 10 per node)
- gpfs_fs_tot_disk_wait_rd  (latence cumulée disk read)
- gpfs_fs_tot_disk_wait_wr  (latence cumulée disk write)
- gpfs_fs_max_disk_wait_rd/wr
- gpfs_fs_min_disk_wait_rd/wr
- gpfs_fs_max_queue_wait_rd/wr

### Node Stats (job: GPFSNode) - 3 series par metric
Labels: cluster, env, instance, job, node
- gpfs_ns_bytes_read/written (idem gpfs_fs mais par noeud)
- gpfs_ns_read_ops/write_ops
- gpfs_ns_disks, gpfs_ns_clusters, gpfs_ns_filesys
- gpfs_ns_tot_disk_wait_rd/wr

### TSCOM - Communication inter-nœuds (job: GPFSTSCOM) - 557 series
Labels: cluster, env, gpfs_tscom_connection, gpfs_tscom_destination (IP), instance, job, node
- gpfs_tscom_stat           (1=connecté, 0=déconnecté)
- gpfs_tscom_ca_stat
- gpfs_tscom_lost           (paquets perdus)
- gpfs_tscom_reconnect
- gpfs_tscom_retrans/retransmits
- gpfs_tscom_sent_bytes/recvd_bytes
- gpfs_tscom_sent_msgs/recvd_msgs
- gpfs_tscom_rtt            (µs, ex: 173-332)
- gpfs_tscom_rttvar         (variance RTT)
- gpfs_tscom_rto            (ex: 201000 µs)
- gpfs_tscom_pmtu           (ex: 2044 bytes)

### VFS Extended (job: GPFSVFSX) - 3 series par metric
Labels: cluster, env, instance, job, node
- gpfs_vfsx_read/write/open/close/create/lookup/readdir/fsync
- gpfs_vfsx_*_t  (latence cumulée)
- gpfs_vfsx_*_tmax / *_tmin

### FIS - Filesystem Internal Stats (job: GPFSFilesystem?)
- gpfs_fis_bytes_read/written
- gpfs_fis_disks
- gpfs_fis_read_calls/write_calls/open_calls/close_calls

### IS - Internal Stats (job: GPFSNode?)
- gpfs_is_bytes_read/written
- gpfs_is_read_calls/write_calls

### Quotas (no data actuellement)
- gpfs_fset_allocInodes / freeInodes / maxInodes
- gpfs_rq_blk_current/hard_limit/soft_limit/in_doubt
- gpfs_rq_file_current/hard_limit/soft_limit/in_doubt

## Dashboards IBM officiels (exemples, qualité médiocre)
- gpfs_cluster_health_overview/Prometheus as Datasource/
- Filesystem/Monitoring Filesystems-1692341412466.json
- gpfs_fileset_quotas/ (plusieurs fichiers)
- GPFS_cluster_communication_statistics/
- GPFS Clusters overview-1747665435687.json
