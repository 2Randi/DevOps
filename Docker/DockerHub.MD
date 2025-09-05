## Qu'est-ce que le registre Docker Hub ?

Docker Hub est un service de registre basé sur le cloud qui vous permet de vous connecter à des référentiels de code, de créer vos images et de les tester, de stocker des images poussées manuellement et de vous connecter à Docker Cloud afin de pouvoir déployer des images sur vos hôtes. Il fournit une ressource centralisée pour la découverte d'images de conteneurs, la distribution et la gestion des changements, la collaboration entre utilisateurs et équipes, et l'automatisation des flux de travail tout au long du pipeline de développement.

##### URL de référence : https://docs.docker.com/docker-hub/

1. Créez un compte Docker Hub sur https://hub.docker.com/

1. Extrayez une image Docker

```sh 
   docker pull ubuntu
   ```

1. Extrayez une image Docker avec l'ancienne version

```sh
   docker pull ubuntu:16.04
   ```

1. Créez une balise personnalisée pour l'image Docker
```sh
   docker tag ubuntu:latest valaxy/ubuntu:demo
   ```

1. Connectez-vous à votre registre Docker Hub 
```sh
   docker login
   docker push valaxy/ubuntu:demo
   ```

### Test 

1. Supprimez toutes les images du serveur Docker 
   ```sh 
   docker image rm -f <Image_id>
   ```

1. Récupérez votre image personnalisée depuis votre compte Docker
   ```sh
   docker pull valaxy/ubuntu:demo
   ```

