# Durcissement sécurité (SecOps) — `playbook-hardening.yml`

Playbook Ansible appliquant un socle de durcissement inspiré des **CIS Benchmarks** (Linux) sur des hôtes Debian/Ubuntu et RHEL/Rocky. Répond directement au volet "sensibilité aux problématiques de sécurité opérationnelle" et "mettre en œuvre des préconisations définies par le RSSI" de la fiche de poste.

## Prérequis

```sh
ansible-galaxy collection install community.general ansible.posix
```

**Avant d'exécuter ce playbook sur des hôtes réels : vérifiez que l'accès SSH par clé fonctionne** (`ansible all -m ping`). Certaines tâches (pare-feu, SSH) peuvent couper l'accès si elles sont mal paramétrées — voir la section Précautions.

## Ce que fait le playbook

| Domaine       | Contrôle appliqué                                                        | Référence CIS (indicative) |
|---------------|---------------------------------------------------------------------------|------------------------------|
| SSH           | `PermitRootLogin no`, `PermitEmptyPasswords no`, `MaxAuthTries 3`         | 5.2.x |
| SSH           | `PasswordAuthentication no` (**opt-in**, voir `disable_ssh_password_auth`) | 5.2.10 |
| Pare-feu      | ufw (Debian) / firewalld (RHEL) : deny par défaut, ports whitelistés      | 3.5.x |
| Anti-bruteforce | fail2ban : jail sshd avec ban progressif                               | — |
| Mises à jour  | `unattended-upgrades` (Debian) / `dnf-automatic` (RHEL) : correctifs de sécurité automatiques | 1.9 |
| Audit         | `auditd` : surveillance de `/etc/passwd`, `/etc/shadow`, `sshd_config`, `sudoers` | 4.1.x |
| Temps         | chrony actif (corrélation fiable des logs entre hôtes)                   | 2.1.1 |

## Usage

```sh
# Exécution complète
ansible-playbook -i /etc/ansible/hosts playbook-hardening.yml

# Un domaine seulement (ex. juste fail2ban + audit)
ansible-playbook -i /etc/ansible/hosts playbook-hardening.yml --tags fail2ban,audit

# Simulation sans appliquer (dry-run)
ansible-playbook -i /etc/ansible/hosts playbook-hardening.yml --check --diff
```

## Limite connue en environnement WSL2

Le module `[audit] Démarrer et activer auditd` échoue sous WSL2 (`Error setting audit daemon pid (Operation not permitted)`) : le noyau WSL2 n'expose pas complètement le sous-système d'audit du noyau Linux. Ce n'est pas un bug du playbook — sur un vrai serveur, une VM classique (VirtualBox/VMware/Proxmox) ou une instance cloud, cette tâche fonctionne normalement.

## Précautions

- **`disable_ssh_password_auth` est à `false` par défaut.** Ne le passez à `true` (`-e disable_ssh_password_auth=true`) qu'après avoir confirmé que l'authentification par clé fonctionne sur 100% des hôtes ciblés — sinon vous vous coupez l'accès.
- Le pare-feu passe en politique **deny par défaut sur les flux entrants** : `allowed_tcp_ports` (par défaut `[22]`) doit lister tous les ports dont vous avez besoin (ex. `8080` pour Jenkins, `6443` pour l'API Kubernetes) avant exécution sur un hôte qui héberge ces services.
- Testez toujours d'abord sur une VM de labo jetable, jamais directement sur un hôte de production.

## Pour aller plus loin (hors périmètre de ce playbook)

- Audit complet et scoring CIS : [OpenSCAP](https://www.open-scap.org/) / `oscap xccdf eval`
- Gestion centralisée des secrets : Ansible Vault (`ansible-vault encrypt`) pour ne jamais committer de mots de passe en clair
- Corrélation avec la stack de supervision : envoyer les logs `auditd`/`fail2ban` vers un ELK/EFK centralisé (prochaine étape de la roadmap du projet)
