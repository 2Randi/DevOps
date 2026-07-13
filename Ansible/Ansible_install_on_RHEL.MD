# Installation d'Ansible sur RHEL 9 / Rocky Linux 9

RHEL 8 étant en fin de vie standard, cette procédure cible **RHEL 9** ou **Rocky Linux 9** (équivalent gratuit et binaire-compatible, adapté pour un labo local).

### Prérequis

1. Un nœud de contrôle et un ou plusieurs hôtes gérés sous RHEL 9 / Rocky 9

### Étapes d'installation

1. Installez Python 3 (sur le nœud de contrôle et l'hôte géré)
   ```sh
   sudo dnf install -y python3 python3-pip
   python3 --version
   ```

2. Créez un utilisateur dédié `ansadmin` et accordez-lui l'accès sudo (sur le nœud de contrôle et l'hôte géré)
   ```sh
   sudo useradd -m -s /bin/bash ansadmin
   sudo passwd ansadmin
   echo "ansadmin ALL=(ALL) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/ansadmin
   sudo chmod 440 /etc/sudoers.d/ansadmin
   ```

3. Installez Ansible en tant qu'utilisateur `ansadmin` (sur le nœud de contrôle)
   ```sh
   sudo su - ansadmin
   sudo dnf install -y epel-release
   sudo dnf install -y ansible
   # Alternative si le paquet n'est pas disponible dans les dépôts activés :
   # pip3 install --user ansible
   ansible --version
   ```

4. Générez une paire de clés SSH pour `ansadmin` sur le nœud de contrôle et copiez-la vers chaque hôte géré (l'authentification par clé est la méthode recommandée ; ne réactivez `PasswordAuthentication` que ponctuellement, pour du dépannage, jamais en environnement de production)
   ```sh
   ssh-keygen -t ed25519
   ssh-copy-id ansadmin@<hote-cible>
   ```

5. Créez le répertoire de configuration et un inventaire (sur le nœud de contrôle)
   ```sh
   sudo mkdir -p /etc/ansible
   sudo tee /etc/ansible/hosts <<'EOF'
   [managed]
   <ip-hote-1>
   <ip-hote-2>
   EOF
   ```

### Test de validation

```sh
ansible all -m ping
```

Voir aussi : [Ansible_installation.MD](Ansible_installation.MD) pour Debian/Ubuntu, et le playbook d'exemple [playbook-install-docker.yml](playbook-install-docker.yml).
