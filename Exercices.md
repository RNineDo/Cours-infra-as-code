# Cours-infra-as-code
## Installation
### Exercice 1

Démarrez la VM ubuntu depuis le répertoire atelier-01.
```bash
vagrant up ubuntu
```
    Connectez-vous à cette VM.
```bash
vagrant ssh ubuntu
```
    Rafraîchissez les informations sur les paquets.
```bash
sudo apt update
```
    Recherchez le paquet ansible avec les options qui vont bien.
```bash
apt-cache search --names-only ansible
```
    Installez le paquet officiel fourni par la distribution.
```bash
sudo apt install -y ansible
```
    Vérifiez si l’installation s’est bien déroulée.
```bash
ansible --version
```
    Notez la version d’Ansible.
```bash
ansible --version
```
    Déconnectez-vous et supprimez la VM.
```bash
exit
vagrant destroy -f ubuntu
```
### Exercice 2

Répétez l’exercice précédent en configurant un dépôt PPA (Personal Package Archive) pour Ansible :
```bash
```

$ sudo apt-add-repository ppa:ansible/ansible

Notez la version fournie par ce dépôt tiers et comparez avec la version officielle de l’exercice précédent.
### Exercice 3

```bash
```
Lancez une VM Rocky Linux et installez Ansible en utilisant PIP et Virtualenv.
