# Installation d'Ansible (Debian/Ubuntu)

Ansible est une plateforme d'automatisation open source (sans agent, basée sur SSH). Elle permet la gestion de configuration, le déploiement d'applications et l'automatisation de tâches.

### Prérequis

1. Un nœud de contrôle (VM locale, ex. Ubuntu 24.04 / Debian 12)
2. Un ou plusieurs hôtes gérés joignables en SSH depuis le nœud de contrôle

### Étapes d'installation

#### Sur le nœud de contrôle (Ubuntu/Debian)

1. Installez Python et pip (déjà présents par défaut sur la plupart des images récentes, à vérifier)
   ```sh
   sudo apt update
   sudo apt install -y python3 python3-pip
   ```

2. Installez Ansible via le dépôt officiel (recommandé, versions à jour et signées)
   ```sh
   sudo apt install -y software-properties-common
   sudo add-apt-repository --yes --update ppa:ansible/ansible
   sudo apt install -y ansible
   ansible --version
   ```

3. Créez un utilisateur dédié `ansadmin` (sur le nœud de contrôle et sur chaque hôte géré)
   ```sh
   sudo useradd -m -s /bin/bash ansadmin
   sudo passwd ansadmin
   ```

4. Accordez l'accès sudo à `ansadmin` via un fichier dédié dans `/etc/sudoers.d/` (à préférer à une modification directe de `/etc/sudoers`)
   ```sh
   echo "ansadmin ALL=(ALL) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/ansadmin
   sudo chmod 440 /etc/sudoers.d/ansadmin
   ```

5. Connectez-vous en tant qu'`ansadmin` sur le nœud de contrôle et générez une paire de clés SSH
   ```sh
   sudo su - ansadmin
   ssh-keygen -t ed25519
   ```

6. Copiez la clé publique sur chaque hôte géré (l'authentification par clé doit être la méthode par défaut ; évitez l'authentification par mot de passe en production)
   ```sh
   ssh-copy-id ansadmin@<hote-cible>
   ```

7. Créez le répertoire de configuration et un inventaire
   ```sh
   sudo mkdir -p /etc/ansible
   sudo tee /etc/ansible/hosts <<'EOF'
   [managed]
   <ip-hote-1>
   <ip-hote-2>
   EOF
   ```

### Test de validation

Exécutez la commande suivante en tant qu'utilisateur `ansadmin` ; elle doit réussir (`pong` pour chaque hôte) :
```sh
ansible all -m ping
```

Voir aussi : [Ansible_install_on_RHEL.MD](Ansible_install_on_RHEL.MD) pour une installation sur RHEL/Rocky Linux, et le playbook d'exemple [playbook-install-docker.yml](playbook-install-docker.yml).
