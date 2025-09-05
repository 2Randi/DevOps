
1. Comment rechercher une image Docker sur hub.docker.com
```sh
docker search httpd
```
2. Télécharger une image Docker depuis hub.docker.com
```sh
docker image pull <nom_image>:<version_image/balise>
```

3. Lister les images Docker depuis votre système local
```sh
docker image ls
```

4. Créer/exécuter/démarrer un conteneur Docker à partir d'une image
```sh
docker run -d --name <nom_conteneur> <nom_image>:<version/balise_image>

d - exécute votre conteneur en arrière-plan (déconnecté)
```

5. Exposer votre application au serveur hôte
```sh
docker run -d  -p <port_hôte>:<port_conteneur> --name <nom_conteneur> <nom_image>:<version_image/balise>

docker run -d --name httpd_server -p 8080:80 httpd:2.2
```

6. Lister les conteneurs en cours d'exécution
```sh
docker ps
```

7. Lister tous les conteneurs Docker (en cours d'exécution, arrêtés, terminés, etc.)
```sh
docker ps -a
```

8. Exécuter un conteneur basé sur un système d'exploitation en mode interactif (rien d'autre que la connexion au conteneur une fois qu'il est opérationnel)

```sh
docker run -i -t --name centos_server centos:latest
i - interactif
t - Terminal
```

9. Arrêter un conteneur 
```sh
docker stop <container_id>
```

10. Démarrer un conteneur qui est à l'état arrêté ou terminé

```sh
docker start <container_id>
```
11. Supprimer un conteneur

```sh
docker rm <container_id>
```

12. Se connecter à un conteneur Docker
```sh
docker exec -it <container_Name> /bin/bash
```
