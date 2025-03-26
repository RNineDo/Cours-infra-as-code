chrony.yml
```bash
---
- name: Playbook pour configurer et gérer Chrony NTP
  hosts: all
  become: yes
  tasks:

    - name: Installer le paquet chrony
      package:
        name: chrony
        state: present

    - name: Activer et démarrer le service chronyd
      service:
        name: chronyd
        state: started
        enabled: yes

    - name: Sauvegarder la configuration par défaut de chrony
      copy:
        src: /etc/chrony.conf
        dest: /etc/chrony.conf.bak
        remote_src: yes
        mode: '0644'

    - name: Installer la configuration personnalisée de chrony
      copy:
        content: |
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
        dest: /etc/chrony.conf
        mode: '0644'

    - name: Attendre quelques secondes pour que chrony se synchronise
      pause:
        seconds: 60

    - name: Redémarrer le service chronyd pour prendre en compte la nouvelle configuration
      service:
        name: chronyd
        state: restarted
```
