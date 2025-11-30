# Changelog

## v1.0.0 — 2025-11-30
### Initial Release

- Initial public release of the Zabbix template for CloudNativePG (CNPG) backups monitoring
- Kubernetes API–based discovery of CNPG backup resources
- Added item prototypes:
  - `cnpg.backup.last_status[{#NS},{#CL}]`
  - `cnpg.backup.age[{#NS},{#CL}]`
- Added trigger prototypes:
  - Backup FAILED detection
  - Backup older than `{$CNPG.BACKUP.MAXAGE}`
  - Missing or never-successful backup detection
- Added macro: `{$CNPG.BACKUP.MAXAGE}` (max backup age)
- Full documentation (README) with RBAC, installation steps, architecture and usage
- MIT License included
