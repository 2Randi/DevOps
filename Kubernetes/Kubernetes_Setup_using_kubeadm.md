# Installation d'un cluster Kubernetes avec kubeadm (labo local)

Ce guide met en place un cluster avec un nœud master et deux workers sur des VM **Ubuntu 22.04/24.04 LTS**, avec **containerd** comme runtime de conteneurs (Docker Engine n'est plus utilisable comme CRI depuis Kubernetes 1.24 — le composant `dockershim` a été retiré du kubelet).

> Vérifiez la version mineure de Kubernetes actuellement supportée (politique de support : les 3 dernières versions mineures) sur https://kubernetes.io/releases/ et adaptez `K8S_VERSION` ci-dessous en conséquence. Exemple utilisé ici : `1.31`.

## Prérequis

1. Dimensionnement (labo local en VM)
   > Master : 2 vCPU / 2 Go RAM minimum
   > Workers : 2 vCPU / 2 Go RAM minimum

2. Ports à ouvrir entre les VM (pare-feu/groupe de sécurité)
   #### Nœud master :
   `6443` (API server), `2379-2380` (etcd), `10250` (kubelet), `10259` (kube-scheduler), `10257` (kube-controller-manager)
   #### Nœuds worker :
   `10250` (kubelet), `30000-32767` (NodePort services)
   #### Calico (CNI) :
   `179` (BGP) sur master et workers

3. Effectuez toutes les commandes en `root` (via `sudo`) sauf mention contraire.

## `Sur le master ET les workers :`

1. Désactivez le swap (obligatoire pour kubelet)
   ```sh
   sudo swapoff -a
   sudo sed -i '/ swap /s/^/#/' /etc/fstab
   ```

2. Chargez les modules kernel requis et persistez-les
   ```sh
   cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
   overlay
   br_netfilter
   EOF
   sudo modprobe overlay
   sudo modprobe br_netfilter
   ```

3. Configurez les paramètres réseau sysctl requis
   ```sh
   cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
   net.bridge.bridge-nf-call-iptables  = 1
   net.bridge.bridge-nf-call-ip6tables = 1
   net.ipv4.ip_forward                 = 1
   EOF
   sudo sysctl --system
   ```

4. Installez et configurez containerd comme runtime
   ```sh
   sudo apt update
   sudo apt install -y containerd
   sudo mkdir -p /etc/containerd
   containerd config default | sudo tee /etc/containerd/config.toml
   # Active le driver cgroup systemd (requis pour s'aligner avec kubelet)
   sudo sed -i 's/SystemdCgroup = false/SystemdCgroup = true/' /etc/containerd/config.toml
   sudo systemctl restart containerd
   sudo systemctl enable containerd
   ```

5. Installez kubeadm, kubelet et kubectl depuis le dépôt officiel `pkgs.k8s.io`
   ```sh
   K8S_VERSION=1.31

   sudo apt install -y apt-transport-https ca-certificates curl gpg
   curl -fsSL https://pkgs.k8s.io/core:/stable:/v${K8S_VERSION}/deb/Release.key | \
     sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

   echo "deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v${K8S_VERSION}/deb/ /" | \
     sudo tee /etc/apt/sources.list.d/kubernetes.list

   sudo apt update
   sudo apt install -y kubelet kubeadm kubectl
   sudo apt-mark hold kubelet kubeadm kubectl
   ```

6. Désactivez le pare-feu local pour le labo (en production, ouvrez uniquement les ports listés ci-dessus au lieu de désactiver le pare-feu)
   ```sh
   sudo ufw disable
   ```

## `Sur le master :`

1. Initialisez le cluster
   ```sh
   sudo kubeadm init \
     --apiserver-advertise-address=<IP_MASTER> \
     --pod-network-cidr=192.168.0.0/16
   ```

2. Créez un utilisateur dédié à l'administration Kubernetes et copiez le kubeconfig
   ```sh
   sudo useradd -m kubeadmin
   sudo mkdir -p /home/kubeadmin/.kube
   sudo cp /etc/kubernetes/admin.conf /home/kubeadmin/.kube/config
   sudo chown -R kubeadmin:kubeadmin /home/kubeadmin/.kube
   ```

3. Déployez le CNI Calico (en tant qu'utilisateur `kubeadmin`)
   ```sh
   sudo su - kubeadmin
   kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.28.0/manifests/calico.yaml
   ```
   > Vérifiez la dernière version stable de Calico sur https://docs.tigera.io/calico/latest/getting-started/kubernetes/self-managed-onprem/onpremises

4. Générez la commande de jointure pour les workers
   ```sh
   kubeadm token create --print-join-command
   ```

## `Sur chaque worker :`

Exécutez la commande obtenue à l'étape précédente (`kubeadm join ...`) avec `sudo`.

## Validation du cluster

```sh
kubectl get nodes -o wide
kubectl get pods -A
kubectl get componentstatuses
```

## Alternative légère pour un labo mono-machine

Si vous testez sur une seule VM/poste sans monter 3 machines, `kind` (Kubernetes in Docker) ou `minikube` permettent de créer un cluster multi-nœuds en quelques minutes, sans passer par kubeadm :
```sh
# kind (nécessite Docker)
curl -Lo ./kind https://kind.sigs.k8s.io/dl/latest/kind-linux-amd64
chmod +x ./kind && sudo mv ./kind /usr/local/bin/
kind create cluster --name labo-devops
```
C'est utile pour itérer vite sur les manifests, mais **kubeadm sur VM reste l'exercice à privilégier** pour un poste d'exploitation : il vous fait manipuler les mêmes étapes qu'en environnement réel (réseau, certificats, jointure de nœuds).
