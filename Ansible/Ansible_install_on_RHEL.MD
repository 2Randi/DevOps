#### Sur un serveur RHEL 8.x

1. Installez la dernière version de Python  (sur le nœud de contrôle et l'hôte géré)
```sh 
   yum install python3 -y
   ```

1. Par défaut, python3 est la commande permettant d'exécuter les commandes Python. Pour utiliser uniquement python, utilisez la commande « alternatives ».  (sur le nœud de contrôle et l'hôte géré)
```sh 
   alternatives --set python /usr/bin/python3
   ```

1. Vérifiez la version de Python
```sh 
   python --version
   ```
1. Installez le gestionnaire de paquets python-pip  (sur le nœud de contrôle)
```sh 
   yum -y install python3-pip
   ```

1. Créez un nouvel utilisateur pour l'administration ansible et accordez-lui un accès administrateur  (sur le nœud de contrôle et l'hôte géré)
```sh 
   useradd ansadmin
   passwd ansadmin
   ```
1. La commande ci-dessous ajoute ansadmin au fichier sudoers. Cependant, nous vous recommandons vivement d'utiliser la commande « visudo » si vous connaissez l'éditeur vi ou nano.  (sur le nœud de contrôle et l'hôte géré)
```sh
   echo « ansadmin ALL=(ALL) NOPASSWD: ALL » >> /etc/sudoers
   ```
1. Il est conseillé d'utiliser l'authentification par clé. Si vous êtes encore en phase d'apprentissage, utilisez l'authentification par mot de passe (sur le nœud de contrôle et l'hôte géré)
```sh 
   La commande sed remplace « PasswordAuthentication no » par « yes » sans modifier le fichier 
    sed -ie “s/PasswordAuthentication no/PasswordAuthentication yes/” /etc/ssh/sshd_config
    sudo service sshd reload
   ``` 

    #### Installez Ansible en tant qu'utilisateur ansadmin (sur le nœud de contrôle)
   ```sh 
   su - ansadmin
   pip3 install ansible --user
   ``` 
   
1. Il est conseillé d'utiliser l'authentification par clé. Si vous êtes encore en phase d'apprentissage, utilisez l'authentification par mot de passe (sur le nœud de contrôle et l'hôte géré)
```sh 
   La commande sed remplace « PasswordAuthentication no » par « yes » sans modifier le fichier 
    sed -ie “s/PasswordAuthentication no/PasswordAuthentication yes/” /etc/ssh/sshd_config
    sudo service sshd reload
   ``` 

    #### Installez Ansible en tant qu'utilisateur ansadmin (sur le nœud de contrôle)
   ```sh 
   su - ansadmin
   pip3 install ansible --user
   ``` 
      Remarque : Ansible doit être installé en tant qu'utilisateur 



1. Il est recommandé d'utiliser l'authentification par clé. Si vous êtes encore en phase d'apprentissage, utilisez l'authentification par mot de passe (sur le nœud de contrôle et l'hôte géré).
```sh 
   # La commande sed remplace « PasswordAuthentication no » par « yes » sans modifier le fichier. 
    sed -ie “s/PasswordAuthentication no/PasswordAuthentication yes/” /etc/ssh/sshd_config
    sudo service sshd reload
   ``` 

    #### Installez Ansible en tant qu'utilisateur ansadmin (sur le nœud de contrôle)
   ```sh 
   su - ansadmin
   pip3 install ansible --user
   ``` 
      Remarque : Ansible doit être installé en tant qu'utilisateur (ici ansadmin)
1. Vérifiez la version d'Ansible 
   ```sh
   ansible --version
   ```

1. Connectez-vous en tant qu'utilisateur ansadmin sur le maître et générez une clé ssh (sur le nœud de contrôle)
   ```sh 
   ssh-keygen
   ```
1. Copiez les clés sur tous les hôtes gérés par ansible (sur le nœud de contrôle)
   ```sh 
   ssh-copy-id ansadmin@<serveur-cible>
   ```
### Test de validation

1. Créez un répertoire /etc/ansible et créez un fichier d'inventaire appelé « hosts » dans lequel vous ajouterez l'adresse IP du nœud de contrôle. 
  
1. Exécutez la commande ansible en tant qu'utilisateur ansadmin. Elle devrait fonctionner (maître)
```sh 
   ansible all -m ping
   ```

