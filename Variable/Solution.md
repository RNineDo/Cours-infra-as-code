myvars1.yml
```bash
---
- name: Afficher et modifier ma voiture et ma moto préférée
  hosts: all
  vars:
    mycar: "Porsche"
    mybike: "Yamaha"
  tasks:
    - name: Afficher la voiture préférée avant modification
      debug:
        msg: "Avant modification, ma voiture préférée est {{ mycar }}"
      
    - name: Afficher la moto préférée avant modification
      debug:
        msg: "Avant modification, ma moto préférée est {{ mybike }}"

    - name: Modifier la voiture préférée
      set_fact:
        mycar: "BMW"
    
    - name: Modifier la moto préférée
      set_fact:
        mybike: "Kawasaki"

    - name: Afficher la voiture préférée après modification
      debug:
        msg: "Après modification, ma voiture préférée est {{ mycar }}"
      
    - name: Afficher la moto préférée après modification
      debug:
        msg: "Après modification, ma moto préférée est {{ mybike }}"
```
myvars2.yml
```bash
---
- name: Afficher et modifier ma voiture et ma moto préférée avec set_fact
  hosts: all
  tasks:
    - name: Définir la voiture préférée
      set_fact:
        mycar: "Tesla"

    - name: Définir la moto préférée
      set_fact:
        mybike: "Harley-Davidson"

    - name: Afficher la voiture préférée avant modification
      debug:
        msg: "Avant modification, ma voiture préférée est {{ mycar }}"
      
    - name: Afficher la moto préférée avant modification
      debug:
        msg: "Avant modification, ma moto préférée est {{ mybike }}"

    - name: Modifier la voiture préférée
      set_fact:
        mycar: "BMW"
    
    - name: Modifier la moto préférée
      set_fact:
        mybike: "Kawasaki"

    - name: Afficher la voiture préférée après modification
      debug:
        msg: "Après modification, ma voiture préférée est {{ mycar }}"
      
    - name: Afficher la moto préférée après modification
      debug:
        msg: "Après modification, ma moto préférée est {{ mybike }}"
```
group_vars/all.yml
```bash
---  # group_vars/all.yml

mycar: VW
mybike: BMW

...
```
myvars3.yml
```bash
- name: Personnaliser les variables pour target02
  hosts: target02
  vars:
    mycar: "Mercedes"
    mybike: "Honda"
  tasks:
    - name: Afficher la voiture préférée sur target02
      debug:
        msg: "Sur target02, ma voiture préférée est {{ mycar }}"

    - name: Afficher la moto préférée sur target02
      debug:
        msg: "Sur target02, ma moto préférée est {{ mybike }}"
```
display_user.yml
```bash
---
- name: Afficher l'utilisateur et le mot de passe
  hosts: localhost
  vars_prompt:
    - name: user
      prompt: "Veuillez entrer le nom d'utilisateur"
      default: "microlinux"
      private: false
    - name: password
      prompt: "Veuillez entrer le mot de passe"
      default: "yatahongaga"
      private: true
  tasks:
    - name: Afficher le nom d'utilisateur
      debug:
        msg: "Nom d'utilisateur : {{ user }}"
    - name: Afficher le mot de passe
      debug:
        msg: "Mot de passe : {{ password }}"

```
