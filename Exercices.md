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
vagrant ssh rocky
sudo dnf update
sudo dnf install -y python3-pip
python3 -m venv ~/.venv/ansible
source ~/.venv/ansible/bin/activate
pip install --upgrade pip
pip install ansible
ansible --version
2.15.13
deactivate
exit
vagrant destroy -f rocky
```
# Authentification
## Exercice

```bash
cd ~/formation-ansible/atelier-03
vagrant up
vagrant ssh control
```
Modifier /etc/hosts
```bash
sudo nano /etc/hosts
192.168.56.10  control
192.168.56.20  atelier01
192.168.56.30  atelier02
192.168.56.40  atelier03
```
```bash
ssh-keyscan -t rsa atelier01 atelier02 atelier03 >> .ssh/known_hosts
ssh-keygen
ssh-copy-id vagrant@atelier01
ssh-copy-id vagrant@atelier02
ssh-copy-id vagrant@atelier03
```
```bash
ansible all -i target01,target02,target03 -m ping
target01 | SUCCESS
target02 | SUCCESS
target03 | SUCCESS
```
