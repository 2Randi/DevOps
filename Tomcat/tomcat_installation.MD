# Installation de Tomcat sur une instance EC2
### Pré-requis
1. Instance EC2 avec Java 11
### Installer Apache Tomcat
1. Téléchargez les paquets Tomcat depuis https://tomcat.apache.org/download-80.cgi dans /opt sur l'instance EC2
   > Remarque : pensez à remplacer `<version>` par la version de Tomcat que vous téléchargez.
   ```sh 
   # Créer le répertoire tomcat
   cd /opt
   wget http://mirrors.fibergrid.in/apache/tomcat/tomcat-8/v8.5.35/bin/apache-tomcat-8.5.35.tar.gz
   tar -xvzf /opt/apache-tomcat-<version>.tar.gz
   ```
1. Donnez les droits d'exécution à startup.sh et shutdown.sh, situés dans bin.
   ```sh
   chmod +x /opt/apache-tomcat-<version>/bin/startup.sh 
   chmod +x /opt/apache-tomcat-<version>/bin/shutdown.sh
   ```
   > Remarque : vous pouvez rencontrer l'erreur suivante au démarrage de Tomcat si Java n'est pas installé :
   `Neither the JAVA_HOME nor the JRE_HOME environment variable is defined At least one of these environment variable is needed to run this program`
1. Créez des liens symboliques pour startup.sh et shutdown.sh
   ```sh
   ln -s /opt/apache-tomcat-<version>/bin/startup.sh /usr/local/bin/tomcatup
   ln -s /opt/apache-tomcat-<version>/bin/shutdown.sh /usr/local/bin/tomcatdown
   tomcatup
   ```
  #### Point de contrôle :
Accédez à l'application Tomcat depuis un navigateur sur le port 8080
 - http://<IP_Publique>:8080

  Utiliser un port unique par application est une bonne pratique. Or Tomcat et Jenkins tournent tous les deux par défaut sur le port 8080. Changeons donc le port de Tomcat pour 8090. Modifiez le numéro de port dans le fichier conf/server.xml, sous le répertoire d'installation de Tomcat.
   ```sh
 cd /opt/apache-tomcat-<version>/conf
# mettre à jour le numéro de port dans le champ "connector port" de server.xml
# redémarrer tomcat après la mise à jour de la configuration
tomcatdown
tomcatup
```
#### Point de contrôle :
Accédez à l'application Tomcat depuis un navigateur sur le port 8090
 - http://<IP_Publique>:8090

1. L'application est maintenant accessible sur le port 8090, mais Tomcat n'autorise pas la connexion depuis le navigateur. Modifier un paramètre par défaut dans context.xml corrige ce problème.
   ```sh
   #rechercher les fichiers context.xml
   find / -name context.xml
   ```
1. La commande ci-dessus retourne 3 fichiers context.xml. Commentez (avec `<!--` et `-->`) le champ `Value ClassName` dans les fichiers situés sous le répertoire webapp.
Redémarrez ensuite les services Tomcat pour appliquer ces changements.
Au moment de la rédaction de ce guide, les 2 fichiers suivants sont concernés :
   ```sh 
   /opt/tomcat/webapps/host-manager/META-INF/context.xml
   /opt/tomcat/webapps/manager/META-INF/context.xml
   
   # Redémarrer les services Tomcat
   tomcatdown  
   tomcatup
   ```
1. Mettez à jour les informations utilisateur dans le fichier tomcat-users.xml
Allez dans le répertoire d'installation de Tomcat et ajoutez les utilisateurs suivants dans conf/tomcat-users.xml
   ```sh
	<role rolename="manager-gui"/>
	<role rolename="manager-script"/>
	<role rolename="manager-jmx"/>
	<role rolename="manager-status"/>
	<user username="admin" password="admin" roles="manager-gui, manager-script, manager-jmx, manager-status"/>
	<user username="deployer" password="deployer" roles="manager-script"/>
	<user username="tomcat" password="s3cret" roles="manager-gui"/>
   ```
1. Redémarrez le service et essayez de vous connecter à l'application Tomcat depuis le navigateur. Cette fois, la connexion devrait réussir.
