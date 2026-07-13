# Déployer sur un conteneur Docker en utilisant Ansible
# *Nom du job Jenkins :* `Deploy_on_Container_using_ansible`

### Pré-requis

1. Serveur Jenkins
1. Serveur hôte Docker
1. Serveur Ansible
1. `Dockerfile` dans *`/opt/docker`* sur le serveur Ansible (voir [Ansible_installation.MD](../Ansible/Ansible_installation.MD))
   ```sh 
   # Récupérer la dernière image tomcat depuis dockerhub 
   From tomcat
   # Maintainer
   MAINTAINER "RANDRIAMISAINA Tsiory" 

   # copier le fichier war dans le conteneur 
   COPY ./webapp.war /usr/local/tomcat/webapps
1. Créez `create-docker-image.yml` sous *`/opt/docker`* sur le serveur Ansible
   ```sh
   ---
   - hosts: all
     #ansadmin n'a pas besoin des droits root pour créer une image
     become: true 

     tasks:
     - name: building docker image
       command: "docker build -t simple-devops-image ." 
       args:
         chdir: /opt/docker
   ```
1. Créez `create-docker-container.yml` sous *`/opt/docker`* sur le serveur Ansible
   ```ssh
   ---
   - hosts: all
     become: true

     tasks:
     - name: creating docker container using docker command
       command: docker run -d --name simple-devops-container -p 8080:8080 simple-devops-image
   ```

### Intégration entre le nœud de contrôle Ansible et Jenkins

Installez "Publish Over SSH"
 - `Manage Jenkins` > `Manage Plugins` > `Available` > `Publish over SSH`

Activez la connexion entre le nœud de contrôle Ansible et Jenkins

- `Manage Jenkins` > `Configure System` > `Publish Over SSH` > `SSH Servers`

	- SSH Servers :
                - Name : `ansible-server`
		- Hostname : `<IP_du_serveur>`
		- username : `ansadmin`
               
       -  `Advanced` > choisir `Use password authentication, or use a different key`
		 - password : `*******`
 
### Étapes pour créer le job Jenkins "Deploy_on_Container_using_ansible"
#### Depuis la page d'accueil Jenkins, sélectionnez "New Item"
   - Entrez un nom d'item : `Deploy_on_Container_using_ansible`
     - Copy from : `Deploy_on_Container`
     
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
      - SSH Server Name : `ansible-server`
       - `Transfers` >  `Transfer set`
            - Source files : `webapp/target/*.war`
	       - Remove prefix : `webapp/target`
	       - Remote directory : `//opt//docker`
	       - Exec command : 
                ```sh 
                ansible-playbook -i /opt/docker/hosts /opt/docker/create-docker-image.yml;
                ```

Enregistrez et lancez le job maintenant.
