# Installation de Docker sur un serveur Amazon Linux

### Pré-requis
1. Instance Amazon Linux EC2

## Étapes d'installation

1. Installez Docker et démarrez les services Docker
```sh 
   yum install docker -y
   docker --version 
   
   # démarrez les services Docker
   service docker start
   service docker status
   ```
1. Créez un utilisateur appelé dockeradmin
```sh
   useradd dockeradmin
   passwd dockeradmin
   ```
1. Ajoutez un utilisateur au groupe Docker pour gérer Docker 
```
   usermod -aG docker dockeradmin
   ```
### Test de validation
1. Créez un conteneur Docker Tomcat en récupérant une image Docker à partir du registre Docker public
   ```sh
   docker run -d --name test-tomcat-server -p 8090:8080 tomcat:latest
   ```

## Installation de Docker sur un serveur CentOS
##### URL de référence : https://docs.docker.com/install/linux/docker-ce/centos/#install-using-the-repository
### Pré-requis

Veuillez suivre les étapes ci-dessous pour installer Docker CE sur une instance de serveur CentOS. Pour RedHat, seul Docker EE est disponible.

1. Installez les paquets requis.

```sh 
   sudo yum install -y yum-utils \
   device-mapper-persistent-data \
   lvm2
   ```
  
1. Utilisez la commande suivante pour configurer le référentiel stable.
 
```sh 
   sudo yum-config-manager \
   --add-repo \
   https://download.docker.com/linux/centos/docker-ce.repo
   ```

### INSTALLATION DE DOCKER CE

1. Installez la dernière version de Docker CE.
```sh 
   sudo yum install docker-ce
   ``` 

   Remarque : si vous êtes invité à accepter la clé GPG, vérifiez que l'empreinte digitale correspond à 
060A 61C5 1B55 8A7F 742B 77AA C52F EB6B 621E 9F35, et si c'est le cas, acceptez-la.

1. Démarrez Docker.
```sh 
   sudo systemctl start docker
   ```

1. Vérifiez que Docker est correctement installé en exécutant l'image hello-world.
```sh
   sudo docker run hello-world
   ```

