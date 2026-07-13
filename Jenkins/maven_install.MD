#  Installer et configurer l'outil de build Maven sur Jenkins
Maven est un outil de build qui transforme votre code en artefact. C'est un plugin largement utilisé pour le build en intégration continue.

#### Pré-requis
1. Serveur Jenkins

#### Installer Maven sur Jenkins
1. Téléchargez les paquets Maven depuis https://maven.apache.org/download.cgi sur le serveur Jenkins. Ici, on utilise /opt/maven comme répertoire d'installation.
 - Lien : https://maven.apache.org/download.cgi
    ```sh
     # Créer le répertoire maven sous /opt
     mkdir /opt/maven
     cd /opt/maven
     # télécharger maven version 3.6.1
     wget http://mirrors.estointernet.in/apache/maven/maven-3/3.6.1/binaries/apache-maven-3.6.1-bin.tar.gz
     tar -xvzf apache-maven-3.6.1-bin.tar.gz
     ```
	
1. Configurez les chemins M2_HOME et M2 dans le .bash_profile de l'utilisateur et ajoutez-les à la variable PATH
   ```sh
   vi ~/.bash_profile
   M2_HOME=/opt/maven/apache-maven-3.6.1
   M2=$M2_HOME/bin
   PATH=<PATH_EXISTANT>:$M2_HOME:$M2
   ```
#### Point de contrôle
1. Déconnectez-vous puis reconnectez-vous pour vérifier la version de maven

    ```sh
    mvn --version
    ```
L'installation du logiciel Maven pour supporter le plugin maven sur la console Jenkins est terminée. Passons à Jenkins pour finaliser les étapes restantes.

### Configurer Maven dans la console Jenkins
1. Installez le plugin maven sans redémarrage
  - `Manage Jenkins` > `Jenkins Plugins` > `available` > `Maven Invoker`
  - `Manage Jenkins` > `Jenkins Plugins` > `available` > `Maven Integration`

2. Configurez le chemin maven
  - `Manage Jenkins` > `Global Tool Configuration` > `Maven`
