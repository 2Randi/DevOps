# Supervision : Prometheus + Grafana + Alertmanager

Stack de supervision locale, containerisée, pour superviser l'hôte Docker et les VM du labo (Kubernetes, Ansible, Jenkins). Répond au point "outils de supervision et de centralisation de logs (ELK, Grafana, Prometheus)" de la fiche de poste.

## Composants

| Service       | Rôle                                             | Port  |
|---------------|---------------------------------------------------|-------|
| Prometheus    | Collecte et stockage des métriques (time series)  | 9090  |
| Alertmanager  | Routage/déduplication des alertes                 | 9093  |
| node-exporter | Métriques système (CPU, RAM, disque, réseau)      | 9100  |
| cAdvisor      | Métriques des conteneurs Docker                   | 8081  |
| Grafana       | Dashboards et visualisation                        | 3000  |

## Démarrage

```sh
cd Monitoring
docker compose up -d
docker compose ps
```

## Accès

- Prometheus : http://localhost:9090 (onglet **Alerts** pour voir l'état des règles)
- Alertmanager : http://localhost:9093
- Grafana : http://localhost:3000 (identifiants par défaut `admin` / `changeme` — **à changer immédiatement**, la variable est dans `docker-compose.yml`)

La source de données Prometheus est provisionnée automatiquement dans Grafana (pas de configuration manuelle nécessaire).

## Importer des dashboards prêts à l'emploi

Dans Grafana : `Dashboards` → `New` → `Import`, puis entrez l'ID :
- **1860** — Node Exporter Full (métriques hôte détaillées)
- **14282** — cAdvisor / Docker containers

## Superviser des hôtes distants (VM du cluster kubeadm, etc.)

1. Installez `node_exporter` sur chaque VM à superviser (binaire officiel ou paquet Debian/RHEL).
2. Ajoutez son adresse IP dans `prometheus/prometheus.yml` (job `remote-hosts`, déjà présent en commentaire).
3. Rechargez Prometheus :
   ```sh
   docker compose kill -s SIGHUP prometheus
   ```

## Alerting

Les règles de base sont dans `prometheus/alert.rules.yml` : instance injoignable, CPU/RAM élevés, espace disque faible. `alertmanager/alertmanager.yml` ne définit aucun canal de notification par défaut (labo) — décommentez et complétez la section `slack_configs` ou `email_configs` pour un usage réel.

## Arrêt

```sh
docker compose down          # conserve les volumes (historique des métriques, dashboards)
docker compose down -v       # supprime aussi les volumes
```
