### Projet DevOps - Déploiement et Automatisation

#### Contenu du projet

Ce projet inclut :
- Déploiement d'une application Java avec Tomcat (manuel et via Docker)
- Conteneurisation avec Docker (installation, commandes, Dockerfile)
- Gestion de configuration avec Ansible (installation, playbook d'installation Docker, playbook de durcissement sécurité)
- Intégration continue avec Jenkins (installation, jobs, intégration Ansible/Docker)
- Orchestration avec Kubernetes (cluster via kubeadm en labo local ; références cloud eksctl/kops)
- Supervision avec Prometheus, Alertmanager et Grafana (métriques hôtes + conteneurs, alerting)
- Durcissement sécurité opérationnelle (SecOps) via Ansible : SSH, pare-feu, fail2ban, audit, mises à jour automatiques

#### Objectif

Créer un environnement reproductible permettant :
- Le déploiement automatisé d'une application web
- La gestion de la configuration via des scripts ou Ansible
- L'intégration et le déploiement continus (CI/CD)
- La préparation à un environnement cloud ou conteneurisé

#### Prochaines étapes (roadmap)

- [x] Supervision : stack Prometheus + Grafana déployée (métriques hôtes et conteneurs)
- [x] Durcissement sécurité des serveurs via Ansible (SSH, fail2ban, mises à jour automatiques)
- [ ] Centralisation des logs (ELK/EFK)
- [ ] Pipeline GitLab CI en complément de Jenkins

#### Arborescence
```
DevOps/
├── Tomcat/
│   └── tomcat_installation.MD
├── Jenkins/
│   ├── Jenkins_Installation.MD
│   ├── Git_plugin_install.MD
│   ├── maven_install.MD
│   └── Ansible_integration.MD
├── Jenkins_Jobs/
│   ├── My_First_Maven_Build.MD
│   ├── Deploy_on_Tomcat_Server.MD
│   ├── Deploy_on_Docker.MD
│   ├── Deploy_on_Container.MD
│   ├── Deploy_on_Container_using_Ansible.MD
│   └── *.yml (définitions de jobs)
├── Docker/
│   ├── Docker_Installation_Steps.MD
│   ├── Docker_Commands.MD
│   ├── Dockerfile_Instructions.md
│   ├── DockerHub.MD
│   └── tomcat_dockerfile
├── Ansible/
│   ├── Ansible_installation.MD
│   ├── Ansible_install_on_RHEL.MD
│   ├── playbook-install-docker.yml
│   ├── playbook-hardening.yml           (durcissement sécurité SecOps)
│   └── Hardening_Playbook.MD
├── Kubernetes/
│   ├── Kubernetes_Setup_using_kubeadm.md   (labo local, à privilégier)
│   ├── kubernetes_setup_using_eksctl.md    (référence cloud AWS)
│   ├── Kubernetes_Setup_using_kops.md      (référence cloud AWS, historique)
│   ├── Integrating_Kubernetes_with_Jenkins.MD
│   ├── Integrating_Kubernetes_with_Ansible.MD
│   └── *.yml (manifests : pods, déploiements, services)
└── Monitoring/
    ├── docker-compose.yml                 (Prometheus, Alertmanager, node-exporter, cAdvisor, Grafana)
    ├── prometheus/ (prometheus.yml, alert.rules.yml)
    ├── alertmanager/alertmanager.yml
    ├── grafana/provisioning/ (datasource + dashboards)
    └── README.md
```
