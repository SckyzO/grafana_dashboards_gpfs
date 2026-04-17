# Getting Started

## Prerequisites

| Component | Version |
|-----------|---------|
| Grafana | ≥ 10.x |
| Prometheus | ≥ 2.x |
| IBM Spectrum Scale (GPFS) | any recent version |
| [ibm-spectrum-scale-bridge-for-grafana](https://github.com/IBM/ibm-spectrum-scale-bridge-for-grafana) | latest |

---

## 1. Deploy the IBM Spectrum Scale exporter

The exporter must run on the EMS (Execution Management Server) node of your GPFS cluster. It exposes metrics on port `9250` by default.

Follow the official setup guide: https://github.com/IBM/ibm-spectrum-scale-bridge-for-grafana

---

## 2. Configure Prometheus

Add the following scrape job to your `prometheus.yml`. The exporter exposes all metric groups through a single endpoint:

```yaml
scrape_configs:
  - job_name: GPFSmmhealth
    static_configs:
      - targets: ['<ems-node>:9250']
    params:
      query: [health]

  - job_name: GPFSPool
    static_configs:
      - targets: ['<ems-node>:9250']
    params:
      query: [pool]

  - job_name: GPFSFilesystem
    static_configs:
      - targets: ['<ems-node>:9250']
    params:
      query: [filesystem]

  - job_name: GPFSNode
    static_configs:
      - targets: ['<ems-node>:9250']
    params:
      query: [node]

  - job_name: GPFSTSCOM
    static_configs:
      - targets: ['<ems-node>:9250']
    params:
      query: [tscom]

  - job_name: GPFSVFSX
    static_configs:
      - targets: ['<ems-node>:9250']
    params:
      query: [vfsx]
```

Replace `<ems-node>` with the hostname or IP of your EMS node.

Verify that metrics are flowing before importing the dashboards:

```bash
curl -s 'http://<prometheus-host>:9090/api/v1/query?query=gpfs_health_status' | python3 -m json.tool
```

---

## 3. Import dashboards into Grafana

1. In Grafana, go to **Dashboards → Import**
2. Click **Upload dashboard JSON file**
3. Select one of the `dashboard_*.json` files from the `dashboards/` directory
4. Select your Prometheus datasource when prompted
5. Click **Import**
6. Repeat for each dashboard

Import order does not matter. All dashboards are independent and linked together through a shared **"GPFS Dashboards"** dropdown.

---

## 4. Template variables

Each dashboard auto-populates its variables from Prometheus label values — no manual configuration needed.

| Variable | Description | Dashboards |
|----------|-------------|------------|
| `$cluster` | GPFS cluster name (from `cluster` label) | All |
| `$node` | One or more nodes (multi-select supported) | 03, 04, 05, 06 |
| `$pool` | Disk pool name (burst, data, system) | 02 |

---

## Troubleshooting

**No data in dashboards**
- Check that all 6 Prometheus jobs are scraping successfully (`/targets` in Prometheus UI)
- Verify the `cluster` label value matches what the exporter emits — the `$cluster` variable is populated from it

**Health status panels show no color**
- `gpfs_health_status` uses numeric values: `10` = OK, `20` = warning, `30` = error
- Value mappings are defined in the panel — check they are not overridden by your Grafana theme
