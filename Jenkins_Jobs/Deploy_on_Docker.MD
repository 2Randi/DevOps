# Déployer sur un serveur hôte Docker avec Jenkins
# *Nom du job Jenkins :* `Deploy_on_Docker_Host`

### Pré-requis

1. Serveur Jenkins
1. Serveur hôte Docker

### Intégration entre l'hôte Docker et Jenkins

Installez "Publish Over SSH"
 - `Manage Jenkins` > `Manage Plugins` > `Available` > `Publish over SSH`

Activez la connexion entre l'hôte Docker et Jenkins

- `Manage Jenkins` > `Configure System` > `Publish Over SSH` > `SSH Servers`

	- SSH Servers :
                - Name : `docker-host`
		- Hostname : `<IP_du_serveur>`
		- username : `dockeradmin`
               
       -  `Advanced` > choisir `Use password authentication, or use a different key`
		 - password : `*******`
 
### Étapes pour créer le job Jenkins "Deploy_on_Docker_Host"
 #### Depuis la page d'accueil Jenkins, sélectionnez "New Item"
   - Entrez un nom d'item : `Deploy_on_Docker_Host`
     - Copy from : `Deploy_on_Tomcat_Server`
     
   - *Source Code Management :*
      - Repository : `https://github.com/2Randi/JEE_v2.git`
      - Branches to build : `*/master`  
   - *Poll SCM* :      - `* * * *`

   - *Build :*
     - Root POM : `pom.xml`
     - Goals and options : `clean install package`

 - *Post-build Actions*
   - Send build artifacts over SSH
     - *SSH Publishers*
      - SSH Server Name : `docker-host`
       - `Transfers` >  `Transfer set`
            - Source files : `webapp/target/*.war`
	       - Remove prefix : `webapp/target`
	       - Remote directory : `//home//ansadmin` ou `.`

Enregistrez et lancez le job maintenant.
