# Installation de Docker (labo local)

## Installation de Docker Engine sur Ubuntu / Debian
##### Référence : https://docs.docker.com/engine/install/ubuntu/

### Pré-requis
1. VM locale Ubuntu 24.04 LTS (ou Debian 12)

### Étapes d'installation

1. Désinstallez les anciens paquets qui pourraient entrer en conflit
   ```sh
   sudo apt remove -y docker docker-engine docker.io containerd runc
   ```

2. Installez les prérequis et ajoutez le dépôt officiel Docker
   ```sh
   sudo apt update
   sudo apt install -y ca-certificates curl gnupg
   sudo install -m 0755 -d /etc/apt/keyrings
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
   sudo chmod a+r /etc/apt/keyrings/docker.gpg

   echo \
     "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
     $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
     sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   ```

3. Installez Docker Engine, le CLI, containerd et le plugin Compose
   ```sh
   sudo apt update
   sudo apt install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin
   docker --version
   docker compose version
   ```

4. Démarrez et activez le service
   ```sh
   sudo systemctl enable --now docker
   sudo systemctl status docker
   ```

5. Créez un utilisateur applicatif dédié et ajoutez-le au groupe `docker` (évite d'utiliser `sudo` pour chaque commande, mais notez que ce groupe équivaut à un accès root sur l'hôte : à réserver aux comptes de confiance)
   ```sh
   sudo useradd -m dockeradmin
   sudo passwd dockeradmin
   sudo usermod -aG docker dockeradmin
   ```

### Test de validation
```sh
docker run -d --name test-tomcat-server -p 8090:8080 tomcat:latest
docker ps
```

## Installation de Docker Engine sur RHEL 9 / Rocky Linux 9
##### Référence : https://docs.docker.com/engine/install/rhel/ et https://docs.docker.com/engine/install/centos/

CentOS Linux (7/8) étant en fin de vie, on utilise ici **Rocky Linux 9** (ou RHEL 9), binaire-compatible et activement maintenu.

1. Installez les paquets requis
   ```sh
   sudo dnf install -y dnf-plugins-core
   ```

2. Ajoutez le dépôt Docker CE
   ```sh
   sudo dnf config-manager --add-repo https://download.docker.com/linux/rhel/docker-ce.repo
   ```

3. Installez Docker CE
   ```sh
   sudo dnf install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin
   ```
   > Note : à l'invite d'acceptation de la clé GPG, vérifiez que l'empreinte correspond à celle publiée sur https://download.docker.com/linux/rhel/gpg avant d'accepter.

4. Démarrez Docker
   ```sh
   sudo systemctl enable --now docker
   ```

5. Vérifiez l'installation
   ```sh
   sudo docker run hello-world
   ```
