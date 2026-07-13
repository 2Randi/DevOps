# Intégration Ansible avec Jenkins

### Pré-requis :
1. Serveur Ansible (voir [Ansible_installation.MD](../Ansible/Ansible_installation.MD))
2. Serveur Jenkins (voir [Jenkins_Installation.MD](Jenkins_Installation.MD))

### Partie 1 : étapes d'intégration

Installez "Publish Over SSH"
 - `Manage Jenkins` > `Manage Plugins` > `Available` > `Publish over SSH`

Activez la connexion entre Ansible et Jenkins
- `Manage Jenkins` > `Configure System` > `Publish Over SSH` > `SSH Servers`

	- SSH Servers :
		- Hostname : `<IP_du_serveur>`
		- username : `ansadm`
		- password : `*******`

Testez la connexion avec "Test Connection"
