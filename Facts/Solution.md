pkg-info.yml
```bash
---
- name: Afficher le gestionnaire de paquets utilisé
  hosts: all
  gather_facts: yes
  tasks:
    - name: Afficher le gestionnaire de paquets utilisé
      debug:
        msg: "Le gestionnaire de paquets utilisé est : {{ ansible_facts.pkg_mgr }}"
      when: ansible_facts.pkg_mgr is defined
```
python-info.yml
```bash
---
- name: Afficher la version de Python installée
  hosts: all
  tasks:
    - name: Obtenir la version de Python
      ansible.builtin.command:
        cmd: python3 --version
      register: python_version
      changed_when: false
      ignore_errors: true

    - name: Afficher la version de Python
      debug:
        msg: "La version de Python installée est : {{ python_version.stdout }}"
      when: python_version is defined

```
dns-info.yml
```bash
---
- name: Afficher le(s) serveur(s) DNS utilisé(s)
  hosts: all
  gather_facts: yes

  tasks:
    - name: Lire le fichier resolv.conf
      ansible.builtin.slurp:
        src: /etc/resolv.conf
      register: resolv_conf_content

    - name: Extraire les serveurs DNS
      set_fact:
        dns_servers: "{{ resolv_conf_content.content | b64decode | regex_findall('nameserver\\s+([\\d.]+)') }}"

    - name: Afficher les serveurs DNS utilisés
      debug:
        msg: "Les serveurs DNS utilisés sont : {{ dns_servers }}"

```
