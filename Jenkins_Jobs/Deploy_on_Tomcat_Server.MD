# Déployer sur un serveur Tomcat
# *Nom du job Jenkins :* `Deploy_on_Tomcat_Server`

### Pré-requis

1. Serveur Jenkins
2. Serveur Tomcat

### Ajout des étapes de déploiement

1. Installez le plugin 'deploy to container'. Ce plugin est nécessaire pour déployer sur un serveur Tomcat.

  - Installez le plugin 'deploy to container' sans redémarrage
    - `Manage Jenkins` > `Jenkins Plugins` > `available` > `deploy to container`

2. Jenkins a besoin d'accéder au serveur Tomcat pour déployer les artefacts buildés. Configurez des identifiants pour permettre ça, via l'option credentials sur la page d'accueil Jenkins.

- Configurer les identifiants
  - `credentials` > `jenkins` > `Global credentials` > `add credentials`
    - Username	: `deployer`
    - Password : `deployer`
    - id      :  `deployer`
    - Description : `utilisateur pour déployer sur la VM tomcat`

### Étapes pour créer le job Jenkins "Deploy_on_Tomcat_Server"
 #### Depuis la page d'accueil Jenkins, sélectionnez "New Item"
   - Entrez un nom d'item : `Deploy_on_Tomcat_Server`
     - Copy from : `My_First_Maven_Build`
     
   - *Source Code Management :*
      - Repository : `https://github.com/2Randi/JEE_v2.git`
      - Branches to build : `*/master`  
   - *Poll SCM* :      - `* * * *`

   - *Build :*
     - Root POM : `pom.xml`
     - Goals and options : `clean install package`

 - *Post-build Actions*
   - Deploy war/ear to container
      - WAR/EAR files : `**/*.war`
      - Containers : `Tomcat 8.x`
         - Credentials : `deployer` (utilisateur créé ci-dessus)
         - Tomcat URL : `http://<IP_PUBLIQUE>:8080`

Enregistrez et lancez le job maintenant.
