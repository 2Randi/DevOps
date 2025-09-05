# Installation d'Ansible

Ansible est une plateforme d'automatisation open source. Elle est très simple à configurer, mais néanmoins très puissante. Ansible peut vous aider dans la gestion de la configuration, le déploiement d'applications et l'automatisation des tâches.

### Prérequis

1. Une instance AWS EC2 (sur le nœud de contrôle)

### Étapes d'installation :
#### sur une instance Amazon EC2

1. Installez python et python-pip
```sh
   yum install python
   yum install python-pip
   ```
1. Installez ansible à l'aide de pip et vérifiez la version
```sh
    pip install ansible
   ansible --version
   ```

1. Créez un utilisateur appelé ansadmin (sur le nœud de contrôle et l'hôte géré)  
   ```sh
   useradd ansadmin
   passwd ansadmin
   ```
1. La commande ci-dessous accorde l'accès sudo à l'utilisateur ansadmin. Cependant, nous vous recommandons vivement d'utiliser la commande « visudo » si vous connaissez l'éditeur vi ou nano.  (sur le nœud de contrôle et l'hôte géré)
```sh
   echo « ansadmin ALL=(ALL) NOPASSWD: ALL » >> /etc/sudoers
   ```
   
1. Connectez-vous en tant qu'utilisateur ansadmin sur le maître et générez une clé ssh (sur le nœud de contrôle)
```sh 
   sudo su - ansadmin
   ssh-keygen
   ```
1. Copiez les clés sur tous les hôtes gérés par Ansible (sur le nœud de contrôle)
```sh 
   ssh-copy-id ansadmin@<serveur-cible>
   ```

1. Le serveur Ansible est utilisé pour créer des images et les stocker dans le registre Docker. Installez donc Docker, démarrez les services Docker et ajoutez ansadmin au groupe Docker. 
```sh
   yum install docker
   
   # démarrer les services Docker 
   service docker start
   service docker start 
   
   # Ajouter un utilisateur au groupe docker 
   usermod -aG docker ansadmin

   ```
1. Créez un répertoire /etc/ansible et créez un fichier d'inventaire appelé « hosts » ; ajoutez-y les adresses IP du nœud de contrôle et des hôtes gérés. 
 
### Test de validation

   
1. Exécutez la commande ansible en tant qu'utilisateur ansadmin ; elle devrait aboutir (maître)
   ```sh 
   ansible all -m ping
   ```
