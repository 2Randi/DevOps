
Dockerfile

1. Un Dockerfile est un document texte contenant toutes les commandes qu'un utilisateur pourrait exécuter en ligne de commande pour assembler une image.
2. La commande `docker build` construit une image à partir d'un Dockerfile et d'un contexte.
 ```sh 
  docker build .
  docker build -f /path/to/a/Dockerfile .
  docker build -t monutilisateur/monapp .
 ```

3. Le démon Docker exécute les instructions du Dockerfile une par une, en validant le résultat de chaque instruction dans une nouvelle image si nécessaire, avant de finalement produire l'ID de votre nouvelle image.
4. Autant que possible, Docker utilise un cache de build pour accélérer significativement le processus.
5. Une fois votre build terminé, vous pouvez analyser votre image avec `docker scan`
 ```sh
 docker scan hello-world
 
 ```

## Création d'un Dockerfile

 Format d'un Dockerfile
  ```sh
   # Commentaire
   INSTRUCTION arguments
  ```

1. Un Dockerfile doit commencer par une instruction FROM.

2. Docker distribue des versions officielles d'images utilisables pour construire des Dockerfiles sous le dépôt docker/dockerfile sur Docker Hub.

- `FROM` - Un Dockerfile valide doit commencer par une instruction FROM.

- `RUN` - L'instruction RUN exécute une commande dans une nouvelle couche au-dessus de l'image actuelle et valide le résultat. L'image validée résultante sera utilisée pour l'étape suivante du Dockerfile.

- `CMD` - Le but principal de CMD est de fournir des valeurs par défaut pour l'exécution d'un conteneur.

   `Remarque :` il ne peut y avoir qu'une seule instruction CMD dans un Dockerfile. Si vous en listez plusieurs, seule la dernière sera prise en compte.

   `Remarque` - Ne confondez pas RUN et CMD. RUN exécute réellement une commande et valide le résultat ; CMD n'exécute rien au moment du build, mais précise la commande prévue pour l'image.

- `LABEL` - L'instruction LABEL ajoute des métadonnées à une image, sous forme de paire clé-valeur.

- `EXPOSE`
  1. L'instruction EXPOSE indique à Docker que le conteneur écoute sur les ports réseau spécifiés à l'exécution.
  2. Pour configurer la redirection de port sur le système hôte, voir l'option `-P`.
  3. La commande `docker network` permet de créer des réseaux pour la communication entre conteneurs sans avoir besoin d'exposer ou de publier des ports spécifiques, car les conteneurs connectés au réseau peuvent communiquer entre eux sur n'importe quel port.

- `ENV` - L'instruction ENV définit la variable d'environnement `<clé>` à la valeur `<valeur>`.

- `ADD` - L'instruction ADD copie de nouveaux fichiers, répertoires ou URLs de fichiers distants depuis `<src>` et les ajoute au système de fichiers de l'image au chemin `<dest>`.

    Si `<src>` est une archive tar locale dans un format de compression reconnu (identity, gzip, bzip2 ou xz), elle est décompressée en tant que répertoire.

- `COPY` - L'instruction COPY copie de nouveaux fichiers ou répertoires depuis `<src>` et les ajoute au système de fichiers du conteneur au chemin `<dest>`.

- `ENTRYPOINT` - ENTRYPOINT permet de configurer un conteneur qui s'exécutera comme un exécutable.

  1. ENTRYPOINT écrase tous les éléments spécifiés via CMD.
  2. CMD et ENTRYPOINT définissent tous les deux la commande exécutée au lancement d'un conteneur. Quelques règles décrivent leur coopération.

  3. Un Dockerfile doit spécifier au moins une commande CMD ou ENTRYPOINT.

  4. ENTRYPOINT doit être défini lorsqu'on utilise le conteneur comme un exécutable.

  5. CMD doit être utilisé pour définir des arguments par défaut pour une commande ENTRYPOINT, ou pour exécuter une commande ponctuelle dans un conteneur.

  6. CMD est écrasé lorsqu'on lance le conteneur avec des arguments alternatifs.

- `VOLUME` - L'instruction VOLUME crée un point de montage portant le nom spécifié et le marque comme contenant des volumes montés depuis l'hôte natif ou d'autres conteneurs.

- `USER` - L'instruction USER définit le nom d'utilisateur à utiliser lors de l'exécution de l'image
- `WORKDIR` - L'instruction WORKDIR définit le répertoire de travail pour toutes les instructions RUN, CMD, ENTRYPOINT, COPY et ADD

- `ARG` - L'instruction ARG définit une variable que les utilisateurs peuvent passer au moment du build via la commande `docker build` avec l'option `--build-arg <nom>=<valeur>`.

- `SHELL` - L'instruction SHELL permet de remplacer le shell par défaut utilisé pour la forme shell des commandes.
