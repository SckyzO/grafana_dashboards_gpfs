# Contributing

Thank you for your interest in contributing to this project.

## Reporting issues

Use [GitHub Issues](https://github.com/SckyzO/grafana_dashboards_gpfs/issues) to report bugs or request new features. Please include:

- Your IBM Spectrum Scale (GPFS) version
- Your ibm-spectrum-scale-bridge-for-grafana version
- Your Grafana and Prometheus versions
- A clear description of the problem or request
- Screenshots if relevant

## Contributing dashboards or documentation

### Workflow

1. Fork the repository
2. Create a branch: `feature/<name>` or `fix/<name>`
3. Make your changes
4. Open a Pull Request against `main`

### Rules for dashboard changes

- **Test on a real cluster** — specify the GPFS, bridge, Grafana and Prometheus versions used
- **No hardcoded values** — cluster names, node names, hostnames and credentials must never appear in JSON files; use template variables (`$cluster`, `$node`, `$pool`) or generic placeholders
- **Keep UIDs stable** — UIDs are used for cross-dashboard links; changing them breaks navigation
- **Include a screenshot** — attach a screenshot of any modified or new panel in the Pull Request
- **Update the documentation** — if you add or modify a panel, update `docs/dashboards.md` accordingly; if you add a metric, update `docs/metrics.md`; this rule is mandatory and PRs without doc updates will not be merged

### Rules for documentation changes

- Write in English
- Do not duplicate content already present in the README
- Keep `docs/metrics.md` in sync with `docs/dashboards.md` — if a metric appears in a panel, it must be documented

### Commit message format

Use [Conventional Commits](https://www.conventionalcommits.org/):

```
feat: add dashboard 07 — fileset quotas
fix: correct unit on TSCOM retransmission rate panel
docs: add PromQL example for VFS average latency
refactor: move dashboards to dashboards/ directory
```

### CHANGELOG

Update `CHANGELOG.md` for every change under `[Unreleased]`. Use the following categories:

- `Added` — new dashboards, panels or documentation
- `Changed` — modifications to existing dashboards or docs
- `Fixed` — bug fixes
- `Removed` — removed panels or files

## License

By contributing, you agree that your contributions will be licensed under the [GNU Affero General Public License v3.0](LICENSE).
