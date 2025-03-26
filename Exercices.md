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
# Configuration de base
## Exercice

```bash
cd ~/formation-ansible/atelier-03
vagrant up
vagrant ssh control
```

Éditez /etc/hosts de manière à ce que les Target Hosts soient joignables par leur nom d’hôte simple.
Modifier /etc/hosts
```bash
sudo nano /etc/hosts
192.168.56.10  control
192.168.56.20  atelier01
192.168.56.30  atelier02
192.168.56.40  atelier03
```
Configurez l’authentification par clé SSH avec les trois Target Hosts.
```bash
ssh-keyscan -t rsa atelier01 atelier02 atelier03 >> .ssh/known_hosts
ssh-keygen
ssh-copy-id vagrant@atelier01
ssh-copy-id vagrant@atelier02
ssh-copy-id vagrant@atelier03
```
Installez Ansible.
On vérifie quelle type de machine on a :
```bash
cat /etc/os-release
```
Nous sommes sur une machine Ubuntu.
```bash
sudo apt install -y ansible
```
Envoyez un premier ping Ansible sans configuration.
```bash
ansible all -i target01,target02,target03 -m ping
```
Créez un répertoire de projet ~/monprojet.
Créez un fichier vide ansible.cfg dans ce répertoire.
```bash
mkdir monprojet
cd monprojet
touch ansible.cfg
```
Vérifiez si ce fichier est bien pris en compte par Ansible.
```bash
ansible --version | head -n 2
ansible
  config file = /home/vagrant/monprojet/ansible.cfg
```
Spécifiez un inventaire nommé hosts.
bash```
nano ansible.cfg
```
bash```
[defaults]
inventory = ./hosts
```
Activez la journalisation dans ~/journal/ansible.log.
bash```
nano ansible.cfg
```
bash```
[defaults]
inventory = ./hosts
log_path = ~/journal/ansible.log
```
Testez la journalisation.
bash```
ansible all -i rocky,debian,suse -m ping
```
bash```
cat ~/logs/ansible.log
```
Créez un groupe [testlab] avec vos trois Target Hosts.
nano hosts
bash```
[testlab]
target01
target02
target03

[testlab:vars]
ansible_user=vagrant
```
Définissez explicitement l’utilisateur vagrant pour la connexion à vos cibles.
[testlab:vars]
ansible_user=vagrant
```
Envoyez un ping Ansible vers le groupe de machines [all].
bash```
ansible all -m ping
```
Définissez l’élévation des droits pour l’utilisateur vagrant sur les Target Hosts.
bash```
[testing:vars]
ansible_user=vagrant
ansible_become=yes
```
Affichez la première ligne du fichier /etc/shadow sur tous les Target Hosts.
bash```
ansible all -a "head -n 1 /etc/shadow"
```
Quittez le Control Host et supprimez toutes les VM de l’atelier.
bash```
exit
vagrant destroy -f
```
# Idempotence
## Exercice
Installez successivement les paquets tree, git et nmap sur toutes les cibles.
```bash
ansible all -m package -a "name=nmap state=present"
ansible all -m package -a "name=tree state=present"
ansible all -m package -a "name=git state=present"
```
Désinstallez successivement ces trois paquets en utilisant le paramètre supplémentaire state=absent.
```bash
ansible all -m package -a "name=git state=absent"
ansible all -m package -a "name=tree state=absent"
ansible all -m package -a "name=nmap state=absent"
```
Copier le fichier /etc/fstab du Control Host vers tous les Target Hosts sous forme d’un fichier /tmp/test3.txt.
```bash
ansible all -m copy -a "src=/etc/fstab dest=/tmp/test3.txt"
```
Supprimez le fichier /tmp/test3.txt sur les Target Hosts en utilisant le module file avec le paramètre state=absent.
```bash
ansible all -m file -a "path=/tmp/test3.txt state=absent"
suse | CHANGED | rc=0 >>
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda3       124G  2.7G  118G   3% /
debian | CHANGED | rc=0 >>
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda3       124G  2.3G  115G   2% /
rocky | CHANGED | rc=0 >>
Filesystem                  Size  Used Avail Use% Mounted on
/dev/mapper/rl_rocky9-root   70G  2.3G   68G   4% /

```
Enfin, affichez l’espace utilisé par la partition principale sur tous les Target Hosts. Que remarquez-vous ?
```bash
ansible all -a "df -h /"
```
Cela envoie un "Stat = CHANGED"
# Un serveur web simple
## Exercice
```bash
cd /ansible/projets/ema/playbooks
touch apache-debian.yml
touch apache-rocky.yml
touch apache-suse.yml
```
Un premier playbook apache-debian.yml qui installe Apache sur l’hôte debian avec une page personnalisée Apache web server running on Debian Linux.
### apache-debian.yml
```bash
---  # apache-debian.yml

- hosts: debian

  tasks:

    - name: Update package information
      apt:
        update_cache: true
        cache_valid_time: 3600

    - name: Install Apache
      apt:
        name: apache2

    - name: Start & enable Apache
      service:
        name: apache2
        state: started
        enabled: true

    - name: Install custom web page
      copy:
        dest: /var/www/html/index.html
        mode: 0644
        content: |
          <!doctype html>
          <html>
            <head>
              <meta charset="utf-8">
              <title>Test</title>
            </head>
            <body>
              <h1>Apache web server running on Debian Linux</h1>
            </body>
          </html>
...
```
Un deuxième playbook apache-rocky.yml qui installe Apache sur l’hôte rocky avec une page personnalisée Apache web server running on Rocky Linux.
### apache-rocky.yml
```bash
---
- name: Installer Apache sur Rocky Linux et supprimer FirewallD
  hosts: rocky
  become: yes
  tasks:
    - name: Désinstaller FirewallD
      dnf:
        name: firewalld
        state: absent

    - name: Désactiver et arrêter le service firewalld
      systemd:
        name: firewalld
        state: stopped
        enabled: no

    - name: Installer Apache
      dnf:
        name: httpd
        state: present

    - name: Copier la page personnalisée
      copy:
        dest: /var/www/html/index.html
        content: |
          <html>
            <head><title>Apache Web Server Running on Rocky Linux</title></head>
            <body>
              <h1>Apache Web Server Running on Rocky Linux</h1>
            </body>
          </html>
        owner: apache
        group: apache
        mode: '0644'

    - name: Démarrer et activer Apache
      systemd:
        name: httpd
        state: started
        enabled: yes
...
```
Un troisième playbook apache-suse.yml qui installe Apache sur l’hôte suse avec une page personnalisée Apache web server running on SUSE Linux.
### apache-suse.yml
```bash
---
- name: Installer Apache sur SUSE
  hosts: suse
  become: yes
  tasks:
    - name: Installer Apache
      zypper:
        name: apache2
        state: present

    - name: Copier la page personnalisée
      copy:
        dest: /srv/www/htdocs/index.html
        content: |
          <html>
            <head><title>Apache Web Server Running on SUSE Linux</title></head>
            <body>
              <h1>Apache Web Server Running on SUSE Linux</h1>
            </body>
          </html>
        owner: wwwrun
        group: www
        mode: '0644'

    - name: Démarrer et activer Apache
      systemd:
        name: apache2
        state: started
        enabled: yes
...
```
