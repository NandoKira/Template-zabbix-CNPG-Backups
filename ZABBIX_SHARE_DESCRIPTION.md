# CloudNativePG (CNPG) Backups Monitoring – Template for Zabbix 7 (Kubernetes API)

## Summary
A Zabbix 7 template for monitoring CloudNativePG (CNPG) PostgreSQL backups on Kubernetes. Automatically discovers CNPG backup resources, checks backup status and age, and triggers alerts for failed or outdated backups. Fully API-based.

## Description
This template adds production-grade monitoring for CloudNativePG (CNPG) backups directly from the Kubernetes API.  
It automatically discovers CNPG clusters, monitors last backup states, backup age, and never-successful backup conditions — all without running scripts or kubectl inside the cluster.

### Features
- Kubernetes API–based monitoring  
- Auto discovery (LLD) of CNPG clusters and backups  
- Backup status: completed / failed / started / unknown  
- Backup age monitoring in seconds  
- Alerting on:
  - Failed backup  
  - Backup older than configurable limit  
  - No successful backup since cluster creation  
- Requires minimal RBAC  
- Works even without Zabbix agents on nodes  

### Requirements
- Kubernetes ≥ 1.24  
- CloudNativePG ≥ 1.27  
- Zabbix 7.x (tested on 7.0.16)  

### RBAC Example
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: zabbix-cnpg-backup-reader
rules:
  - apiGroups: ["postgresql.cnpg.io"]
    resources: ["backups", "scheduledbackups"]
    verbs: ["get", "list", "watch"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: zabbix-cnpg-backup-reader
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: zabbix-cnpg-backup-reader
subjects:
  - kind: ServiceAccount
    name: zabbix-service-account
    namespace: monitoring
```

Retrieve the Kubernetes API token:
```bash
kubectl -n monitoring create token zabbix-service-account
```

## Installation
1. Import template in *Configuration → Templates → Import*  
2. Attach to your Kubernetes API host  
3. Set macros:
   - `{$KUBE.API.URL}`
   - `{$KUBE.API.TOKEN}`
   - `{$CNPG.BACKUP.MAXAGE}` (default 604800 sec = 7 days)

## Files Included
- Template JSON  
- README  
- RBAC manifest  
- Changelog  
- MIT License  

## License
MIT
