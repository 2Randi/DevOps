# Configurer le plugin Git sur Jenkins
Git est l'un des outils de gestion de versions les plus utilisés. Vous pouvez récupérer du code depuis des dépôts Git via Jenkins en utilisant le plugin GitHub.

#### Pré-requis
1. Serveur Jenkins

#### Installer Git sur le serveur Jenkins
1. Installez les paquets git sur le serveur Jenkins
   ```sh
   yum install git -y
   ```

#### Configurer Git dans la console Jenkins
- Installez le plugin git sans redémarrage
  - `Manage Jenkins` > `Jenkins Plugins` > `available` > `github`

- Configurez le chemin git
  - `Manage Jenkins` > `Global Tool Configuration` > `git`
