# CNPG Backups Monitoring – Zabbix Template (API-based)

Ce template Zabbix permet la supervision complète des **backups CloudNativePG (CNPG)** via l'**API Kubernetes**, sans aucun script spécifique sur les nœuds.

## 🏗 Contexte d’utilisation

- Cluster Kubernetes hébergeant **plusieurs bases CNPG** (CloudNativePG) réparties sur différents namespaces.
- **Zabbix agent déployé en DaemonSet** sur le cluster pour la supervision classique (CPU, mémoire, pods, etc.).
- Ce template vient **compléter** la supervision existante en ajoutant :
  - la **découverte automatique** des clusters CNPG,
  - la **surveillance détaillée des backups** (succès / échecs / ancienneté),
  - le tout **uniquement via l’API Kubernetes** (HTTP agent).

---

## ✨ Fonctionnalités

### 🔍 Découverte automatique (LLD)

- Découverte des backups CNPG via l’API Kubernetes :  
  `GET /apis/postgresql.cnpg.io/v1/backups`
- Extraction automatique :
  - `{#NS}` → namespace CNPG
  - `{#CL}` → nom du cluster CNPG
- Filtre optionnel sur les namespaces commençant par `cnpg-`.

### 📊 Items générés automatiquement

Pour chaque cluster CNPG découvert :

- `cnpg.backup.last_status[{#NS},{#CL}]`  
  → statut du dernier backup (`completed`, `failed`, `started`, `unknown`, …)
- `cnpg.backup.age[{#NS},{#CL}]`  
  → âge (en secondes) du **dernier backup réussi**, ou `0` si aucun backup réussi n’a jamais été exécuté.

### 🚨 Triggers inclus

Pour chaque cluster CNPG :

- ❌ **Backup FAILED**  
  → déclenché si le dernier backup n’est ni `completed` ni `started` (ex. `failed`, `unknown`, …).
- ⚠️ **Dernier backup trop ancien**  
  → déclenché si l’âge du dernier backup réussi dépasse `{$CNPG.BACKUP.MAXAGE}` secondes.
- ⚠️ **Aucun backup réussi**  
  → déclenché si aucun backup `completed` n’a jamais été trouvé pour le cluster.

---

## ✅ Testé avec

Ce template a été validé avec les versions suivantes :

- **Kubernetes** : `v1.34`
- **Zabbix Server / Frontend** : `7.0.16`
- **CloudNativePG** : image `ghcr.io/cloudnative-pg/cloudnative-pg:1.27.1`

---

## ⚙️ Permissions Kubernetes nécessaires (RBAC)

Le host Zabbix qui interroge l’API Kubernetes doit utiliser un ServiceAccount disposant au minimum des permissions suivantes :

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

Le token de ce ServiceAccount est ensuite utilisé dans Zabbix via la macro `{$KUBE.API.TOKEN}`.

---

## 📦 Installation

### 1. Importer le template

Dans Zabbix :

```text
Configuration → Templates → Import
```

Importer le fichier :

```text
template_cnpg_backups_api.json (ou .yaml selon votre export)
```

### 2. Associer le template au host API Kubernetes

Sur le host qui interrogera l’API Kubernetes (ex. `CNPG backups`, `kube-api-monitoring`, etc.) :

1. Aller dans :  
   `Configuration → Hosts → <votre host> → Templates`
2. Ajouter :  
   `Template CNPG Backups` (nom du template importé)
3. Vérifier que ce host dispose des macros suivantes :

| Macro | Description |
|-------|-------------|
| `{$KUBE.API.URL}`   | URL complète de l’API Kubernetes (ex : `https://192.168.0.100:6443`) |
| `{$KUBE.API.TOKEN}` | Token Bearer du ServiceAccount Kubernetes utilisé pour les requêtes HTTP |

---

## 🔧 Macro de configuration

| Macro | Description | Valeur par défaut |
|--------|-------------|-------------------|
| `{$CNPG.BACKUP.MAXAGE}` | Âge maximal acceptable du dernier backup **réussi** (en secondes). Au-delà, un trigger “backup trop ancien” est levé. | `604800` (7 jours) |

Vous pouvez surcharger cette macro :

- au niveau du template (valeur globale),
- ou au niveau du host (pour un seuil différent selon l’environnement).

---

## 📊 Résultat attendu

Après quelques minutes :

- Les backups CNPG apparaissent dans :  
  `Monitoring → Latest data`  
  avec un item “last_status” et un item “age” par cluster `{#NS}/{#CL}`.
- Les triggers associés apparaissent dans :  
  `Monitoring → Problems`  
  en cas de :
  - dernier backup en échec,
  - absence de backup réussi,
  - dernier backup réussi trop ancien.

---

## 🤝 Contribution

Les contributions, retours et idées d’amélioration sont les bienvenus via :

- Issues GitHub
- Pull Requests (nouveaux items, graphs, dashboards, etc.)

---

## 📜 Licence

Ce projet est distribué sous licence **MIT**.

Voir le fichier [`LICENSE`](LICENSE) pour plus de détails.
