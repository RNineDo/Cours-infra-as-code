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
2.10.8
```
Notez la version d’Ansible.
```bash
ansible --version
2.10.8
```
Déconnectez-vous et supprimez la VM.
```bash
exit
vagrant destroy -f ubuntu
```
### Exercice 2

Répétez l’exercice précédent en configurant un dépôt PPA (Personal Package Archive) pour Ansible :
```bash
vagrant up ubuntu
vagrant ssh ubuntu
sudo apt-add-repository ppa:ansible/ansible
[ENTER]
sudo apt update
apt-cache search --names-only ansible
sudo apt install -y ansible
ansible --version
2.17.9
exit
vagrant destroy -f ubuntu
```
La version est différente.
Notez la version fournie par ce dépôt tiers et comparez avec la version officielle de l’exercice précédent.
### Exercice 3

Lancez une VM Rocky Linux et installez Ansible en utilisant PIP et Virtualenv.
```bash

vagrant up rocky
vagrant ssh debian
sudo apt update
sudo apt install -y python3-pip python3-venv
python3 -m venv ~/.venv/ansible
source ~/.venv/ansible/bin/activate
pip install --upgrade pip
pip install ansible
ansible --version
deactivate
exit
vagrant destroy -f rocky
```

