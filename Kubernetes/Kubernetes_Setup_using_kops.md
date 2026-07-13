> **Référence cloud / optionnel, outil largement remplacé** : `kops` est aujourd'hui moins utilisé qu'`eksctl` ou un cluster EKS managé pour AWS. Ce guide nécessite un compte AWS et n'est pas exécutable dans un labo local. Pour un labo local, utilisez plutôt [Kubernetes_Setup_using_kubeadm.md](Kubernetes_Setup_using_kubeadm.md). Conservé ici à titre de référence historique.

# Installer un cluster Kubernetes (K8s) sur AWS

1. Créez une instance EC2 Ubuntu
1. Installez AWSCLI
   ```sh
    curl https://s3.amazonaws.com/aws-cli/awscli-bundle.zip -o awscli-bundle.zip
    sudo apt update
    sudo apt install unzip python
    unzip awscli-bundle.zip
    #sudo apt-get install unzip - si unzip n'est pas présent sur votre système
    ./awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws
    ```

1. Installez kubectl sur l'instance Ubuntu
   ```sh
   curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
    chmod +x ./kubectl
    sudo mv ./kubectl /usr/local/bin/kubectl
   ```

1. Installez kops sur l'instance Ubuntu
   ```sh
    curl -LO https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64
    chmod +x kops-linux-amd64
    sudo mv kops-linux-amd64 /usr/local/bin/kops
    ```
1. Créez un utilisateur/rôle IAM avec accès complet à Route53, EC2, IAM et S3

1. Attachez le rôle IAM à l'instance Ubuntu
   ```sh
   # Remarque : si vous créez un utilisateur IAM avec accès programmatique, fournissez les clés d'accès. Sinon, l'information de région suffit.
   aws configure
    ```

1. Créez une zone hébergée privée Route53 (vous pouvez créer une zone publique si vous avez un domaine)
   ```sh
   Route53 --> hosted zones --> created hosted zone  
   Domain Name: exemple.net
   Type: Private hosted zone for Amazon VPC
   ```

1. Créez un bucket S3
   ```sh
    aws s3 mb s3://demo.k8s.exemple.net
   ```
1. Exposez la variable d'environnement :
   ```sh
    export KOPS_STATE_STORE=s3://demo.k8s.exemple.net
   ```

1. Créez des clés SSH avant de créer le cluster
   ```sh
    ssh-keygen
   ```

1. Créez les définitions du cluster Kubernetes sur le bucket S3
   ```sh
   kops create cluster --cloud=aws --zones=ap-south-1b --name=demo.k8s.exemple.net --dns-zone=exemple.net --dns private 
    ```

1. Si vous souhaitez modifier la taille des workers du cluster, utilisez la commande ci-dessous
   ```sh 
   kops edit ig --name=NOM_DU_CLUSTER nodes
   ```

1. Créez le cluster kubernetes
    ```sh
    kops update cluster demo.k8s.exemple.net --yes
    ```

1. Validez votre cluster
     ```sh
      kops validate cluster
    ```

1. Pour lister les nœuds
   ```sh
   kubectl get nodes
   ```

1. Pour supprimer le cluster
    ```sh
     kops delete cluster demo.k8s.exemple.net --yes
    ```
   
#### Déployer des pods Nginx sur Kubernetes
1. Déployer un conteneur Nginx
    ```sh
    kubectl create deploy sample-nginx --image=nginx --replicas=2 --port=80
    # kubectl deploy simple-devops-project --image=2randi/simple-devops-image --replicas=2 --port=8080
    kubectl get all
    kubectl get pod
   ```

1. Exposez le déploiement en tant que service. Cela créera un ELB devant ces 2 conteneurs et permettra d'y accéder publiquement.
   ```sh
   kubectl expose deployment sample-nginx --port=80 --type=LoadBalancer
   # kubectl expose deployment simple-devops-project --port=8080 --type=LoadBalancer
   kubectl get services -o wide
   ```
