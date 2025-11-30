# CNPG Backups Monitoring – Zabbix Template (API-based)

Ce template Zabbix permet la supervision complète des **backups CloudNativePG (CNPG)** via l'**API Kubernetes**, sans aucun script ni agent spécifique sur les nœuds.

---

## ✨ Fonctionnalités

### 🔍 Découverte automatique (LLD)
- Découverte via l’API :  
  `GET /apis/postgresql.cnpg.io/v1/backups`
- Extraction automatique :
  - `{#NS}` → Namespace  
  - `{#CL}` → Nom du cluster

### 📊 Items générés automatiquement
- `cnpg.backup.last_status[{#NS},{#CL}]` → *completed / failed / started / inconnu*
- `cnpg.backup.age[{#NS},{#CL}]` → âge (sec) du dernier backup **réussi**, sinon `0`

### 🚨 Triggers automatiques inclus
- ❌ **Backup FAILED**  
- ⚠️ **Dernier backup trop ancien** (> `{$CNPG.BACKUP.MAXAGE}`)
- ⚠️ **Aucun backup réussi depuis toujours**

---

## ⚙️ Permissions Kubernetes nécessaires (RBAC)

Le host Zabbix doit utiliser un ServiceAccount ayant ce rôle :

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

---

## 📦 Installation

### 1. Importer le template  
Zabbix → *Configuration* → *Templates* → **Import**

### 2. Associer le template au host API Kubernetes  
Le host doit disposer des macros suivantes :

| Macro | Description |
|-------|-------------|
| `{$KUBE.API.URL}` | URL complète de l’API (ex : `https://192.168.0.101:443`) |
| `{$KUBE.API.TOKEN}` | Token du ServiceAccount utilisé |

---

## 🔧 Macro de configuration

| Macro | Description | Valeur par défaut |
|--------|-------------|-------------------|
| `{$CNPG.BACKUP.MAXAGE}` | Âge max acceptable d’un backup (sec) | `604800` (7 jours) |

---

## 📊 Visualisation  
Après 1 à 2 minutes :
- Les backups CNPG apparaissent dans *Latest Data*
- Les triggers se mettent à jour automatiquement

---

## 🤝 Contribution
Les PR sont les bienvenues !

---

## 📜 Licence  
**MIT**

---

## 👤 Auteur  
Olivier — Kubernetes & Infra Engineering  
