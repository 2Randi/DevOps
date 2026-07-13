> **Référence cloud / optionnel** : ce guide nécessite un compte AWS et n'est pas exécutable dans un labo local sans accès cloud. Pour un labo local, utilisez plutôt [Kubernetes_Setup_using_kubeadm.md](Kubernetes_Setup_using_kubeadm.md). Les numéros de version ci-dessous (kubectl 1.21, etc.) sont datés : vérifiez les dernières versions sur la documentation AWS EKS avant utilisation.

# Installer Kubernetes sur Amazon EKS

Vous pouvez suivre la même procédure dans le document officiel AWS [Getting started with Amazon EKS – eksctl](https://docs.aws.amazon.com/eks/latest/userguide/getting-started-eksctl.html)

#### Pré-requis :
  - Une instance EC2
  - Installer la dernière version d'AWSCLI

1. Installer kubectl
   a. Télécharger kubectl version 1.21
   b. Accorder les droits d'exécution à l'exécutable kubectl
   c. Déplacer kubectl dans /usr/local/bin
   d. Vérifier que l'installation de kubectl a réussi

   ```sh 
   curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.21.2/2021-07-05/bin/linux/amd64/kubectl
   chmod +x ./kubectl
   mv ./kubectl /usr/local/bin 
   kubectl version --short --client
   ```
2. Installer eksctl
   a. Télécharger et extraire la dernière version
   b. Déplacer le binaire extrait dans /usr/local/bin
   c. Vérifier que l'installation d'eksctl a réussi

   ```sh
   curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
   sudo mv /tmp/eksctl /usr/local/bin
   eksctl version
   ```
  
3. Créer un rôle IAM et l'attacher à l'instance EC2
   `Remarque : créez un utilisateur IAM avec accès programmatique si votre système de bootstrap est en dehors d'AWS`
   L'utilisateur IAM doit avoir accès à
   IAM
   EC2
   CloudFormation
   Remarque : consultez la documentation eksctl pour les [politiques IAM minimales](https://eksctl.io/usage/minimum-iam-policies/)
   
4. Créer votre cluster et vos nœuds
   ```sh
   eksctl create cluster --name nom-du-cluster  \
   --region nom-de-la-region \
   --node-type type-instance \
   --nodes-min 2 \
   --nodes-max 2 \ 
   --zones <AZ-1>,<AZ-2>
   
   exemple :
   eksctl create cluster --name mon-cluster \
      --region ap-south-1 \
   --node-type t2.small \
    ```

5. Pour supprimer le cluster EKS
   ```sh 
   eksctl delete cluster mon-cluster --region ap-south-1
   ```
   
6. Validez votre cluster en vérifiant les nœuds et en créant un pod
   ```sh 
   kubectl get nodes
   kubectl run tomcat --image=tomcat 
   ```
   
   #### Déployer des pods Nginx sur Kubernetes
1. Déployer un conteneur Nginx
    ```sh
    kubectl create deployment  demo-nginx --image=nginx --replicas=2 --port=80
    # kubectl deployment regapp --image=2randi/regapp --replicas=2 --port=8080
    kubectl get all
    kubectl get pod
   ```

1. Exposez le déploiement en tant que service. Cela créera un ELB devant ces 2 conteneurs et permettra d'y accéder publiquement.
   ```sh
   kubectl expose deployment demo-nginx --port=80 --type=LoadBalancer
   # kubectl expose deployment regapp --port=8080 --type=LoadBalancer
   kubectl get services -o wide
   ```
