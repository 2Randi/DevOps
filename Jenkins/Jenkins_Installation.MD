# Installer Jenkins sur AWS EC2
Jenkins est un programme Java autonome, prêt à l'emploi, avec des paquets pour Windows, Mac OS X et les autres systèmes Unix-like. En tant que serveur d'automatisation extensible, Jenkins peut être utilisé comme simple serveur CI ou transformé en hub de livraison continue pour n'importe quel projet.

### Pré-requis
1. Instance EC2
   - Avec accès Internet
   - Security Group avec le port `8080` ouvert vers Internet
1. Java 11 doit être installé


## Installer Jenkins
Vous pouvez installer Jenkins via le rpm ou en configurant le dépôt. On configure le dépôt afin de pouvoir le mettre à jour facilement par la suite.
1. Récupérez la dernière version de Jenkins depuis https://pkg.jenkins.io/redhat-stable/ et installez-la
   ```sh
   sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
   sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
   amazon-linux-extras install epel 
   amazon-linux-extras install java-openjdk11  
   
   #sur RedHat/CentOS 
   #yum install epel-release # dépôt fournissant 'daemonize'
   #yum install java-11-openjdk-devel
   #yum install jenkins
   ```

   ### Démarrer Jenkins
   ```sh
   # Démarrer le service jenkins
   service jenkins start

   # Configurer Jenkins pour qu'il démarre au boot
   chkconfig jenkins on
   ```

   ### Accéder à Jenkins
   Par défaut, Jenkins tourne sur le port `8080`. Vous pouvez y accéder via :
   ```sh
   http://IP-PUBLIQUE-DE-VOTRE-SERVEUR:8080
   ```
  #### Configurer Jenkins
- Le nom d'utilisateur par défaut est `admin`
- Récupérez le mot de passe par défaut
- Emplacement du mot de passe : `/var/lib/jenkins/secrets/initialAdminPassword`
- `Passez` l'installation des plugins ; _on peut le faire plus tard_
- Changez le mot de passe admin
   - `Admin` > `Configure` > `Password`
- Configurez le chemin `java`
  - `Manage Jenkins` > `Global Tool Configuration` > `JDK`  
- Créez un autre identifiant administrateur

### Tester des jobs Jenkins
1. Créez un « new item »
1. Entrez un nom d'item – `My-First-Project`
   - Choisissez un projet `Freestyle`
1. Dans la section Build
	Execute shell : echo "Welcome to Jenkins Demo"
1. Enregistrez votre job
1. Lancez le job (Build)
1. Consultez le "console output"
