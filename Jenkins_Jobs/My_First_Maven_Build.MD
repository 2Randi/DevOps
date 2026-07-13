# Créer un premier job Jenkins Maven pour builder le projet hello-world
# *Nom du job Jenkins :* `My_First_Maven_Build`

On sait utiliser Git et Jenkins indépendamment. Et si on faisait collaborer les deux ? C'est là que ce projet DevOps simple vous aide. Suivez les étapes ci-dessous si vous débutez en DevOps. Vous allez adorer.


#### Pré-requis

1. Serveur Jenkins


### Étapes pour créer le job Jenkins "My_First_Maven_Build"
1. Connectez-vous à la console Jenkins
1. Créez un *Jenkins job*, remplissez les détails suivants,
   - *Source Code Management :*
      - Repository : `https://github.com/2Randi/JEE_v2.git`
      - Branches to build : `*/master`  
   - *Build :*
     - Root POM : `pom.xml`
     - Goals and options : `clean install package`
