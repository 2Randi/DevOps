# Intégrer un cluster Kubernetes avec Ansible

1. Connectez-vous au serveur Ansible et copiez la clé publique sur le compte du nœud master du cluster Kubernetes

1. Mettez à jour le fichier hosts avec un nouveau groupe appelé kubernetes et ajoutez-y le master Kubernetes.

1. Créez des playbooks Ansible pour créer le **[deployment](kubernetes-webapp-deployment.yml)** et le **[service](kubernetes-webapp-service.yml)** (voir aussi les manifests [webapp-deploy.yml](webapp-deploy.yml) et [webapp-service.yml](webapp-service.yml))
		
1.  Vérifiez les pods, deployments et services sur le master Kubernetes
    ```sh 
    kubectl get pods -o wide 
    kubectl get deploy -o wide
    kubectl get service -o wide
    ```
	
1. Accédez à l'application via l'IP du service
   ```sh
   wget <IP-du-Master-Kubernetes>:31200
   ```
