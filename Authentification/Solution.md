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
