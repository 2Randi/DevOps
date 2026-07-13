## Intégration Kubernetes avec Jenkins

# *Job Jenkins CI :* `Deploy_on_Kubernetes-CI`

### Pré-requis

1. Serveur Jenkins
1. Serveur Ansible
1. Cluster Kubernetes

### Étapes pour créer le job Jenkins "Deploy_on_Kubernetes_CI"
#### Depuis la page d'accueil Jenkins, sélectionnez "New Item"
   - Entrez un nom d'item : `Deploy_on_Kubernetes_CI`
     - Copy from : `Deploy_on_Docker_Container_using_Ansible_playbooks`
     
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
                ansible-playbook -i /opt/docker/hosts /opt/docker/create-simple-devops-image.yml --limit localhost;
                ```

Enregistrez et lancez le job.

# *Job Jenkins CD :* `Deploy_on_Kubernetes-CD`

### Étapes pour créer le job Jenkins "Deploy_on_Kubernetes_CD"
#### Depuis la page d'accueil Jenkins, sélectionnez "New Item"
   - Entrez un nom d'item : `Deploy_on_Kubernetes_CD`
     - Freestyle Project
	 
  - *Post-build Actions*  
    - Send build artifacts over SSH  
      - *SSH Publishers*  
	       - Exec command : 
                ```sh 
                ansible-playbook -i /opt/docker/hosts /opt/docker/kubernetes-webapp-deployment.yml;
                ansible-playbook -i /opt/docker/hosts /opt/docker/kubernetes-webapp-service.yml;
                ```
Enregistrez et lancez le job.
