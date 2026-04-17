# Getting Started

## Prerequisites

| Component | Tested version |
|-----------|---------------|
| IBM Spectrum Scale (GPFS) | 5.2.3 |
| ibm-spectrum-scale-bridge-for-grafana | v9.0.2 |
| Grafana | ≥ 10.x |
| Prometheus | 3.x |

---

## 1. Deploy the IBM Spectrum Scale exporter

The exporter must run on the EMS (Execution Management Server) node of your GPFS cluster.

Follow the official setup guide: **https://github.com/IBM/ibm-spectrum-scale-bridge-for-grafana/wiki**

---

## 2. Configure Prometheus

Follow the official Prometheus configuration guide for the bridge:
**https://github.com/IBM/ibm-spectrum-scale-bridge-for-grafana/wiki/Setup-Prometheus**

The dashboards expect the following scrape job names to be present in Prometheus:
`GPFSmmhealth`, `GPFSPool`, `GPFSFilesystem`, `GPFSNode`, `GPFSTSCOM`, `GPFSVFSX`

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
