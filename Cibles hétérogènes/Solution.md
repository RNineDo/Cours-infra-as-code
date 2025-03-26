chrony-01.yml
```bash
---
- name: Installer et configurer Chrony sur les hôtes cibles
  hosts: all
  become: true
  tasks:
    
    # Installer chrony en fonction de la distribution
    - name: Installer chrony sur Debian/Ubuntu
      apt:
        name: chrony
        state: present
      when: ansible_facts['distribution'] in ['Debian', 'Ubuntu']
      notify: Redémarrer chrony

    - name: Installer chrony sur Rocky Linux
      dnf:
        name: chrony
        state: present
      when: ansible_facts['distribution'] == 'Rocky'
      notify: Redémarrer chrony

    - name: Installer chrony sur SUSE
      zypper:
        name: chrony
        state: present
      when: ansible_facts['distribution'] == 'SUSE'
      notify: Redémarrer chrony

    # Copier la configuration chrony.conf
    - name: Définir la configuration de chrony.conf pour Debian/Ubuntu
      copy:
        dest: /etc/chrony/chrony.conf
        content: |
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
      when: ansible_facts['distribution'] in ['Debian', 'Ubuntu']
      notify: Redémarrer chrony

    - name: Définir la configuration de chrony.conf pour Rocky Linux
      copy:
        dest: /etc/chrony.conf
        content: |
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
      when: ansible_facts['distribution'] == 'Rocky'
      notify: Redémarrer chrony

    - name: Définir la configuration de chrony.conf pour SUSE
      copy:
        dest: /etc/chrony.conf
        content: |
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
      when: ansible_facts['distribution'] == 'SUSE'
      notify: Redémarrer chrony

  handlers:
    # Redémarrer chrony si la configuration a changé
    - name: Redémarrer chrony
      systemd:
        name: chronyd
        state: restarted
        enabled: yes

```
chrony-02.yml
```bash
---
- name: Installer et configurer Chrony sur les hôtes cibles
  hosts: all
  become: true
  vars:
    chrony_package: 
      debian: "chrony"
      ubuntu: "chrony"
      rocky: "chrony"
      suse: "chrony"
    
    chrony_service: "chronyd"
    
    chrony_confdir: "/etc/chrony.conf"

  tasks:
    # Installer chrony sur toutes les distributions
    - name: Installer chrony
      package:
        name: "{{ chrony_package[ansible_facts['distribution'].lower()] }}"
        state: present
      when: ansible_facts['distribution'] != "openSUSE Leap"  # Condition pour openSUSE Leap
      notify: Redémarrer chrony

    - name: Installer chrony sur openSUSE Leap
      package:
        name: "chrony"
        state: present
      when: ansible_facts['distribution'] == "openSUSE Leap"  # Cas spécifique pour openSUSE Leap
      notify: Redémarrer chrony

    # Copier la configuration de chrony.conf
    - name: Définir la configuration de chrony.conf
      copy:
        dest: "{{ chrony_confdir }}"
        content: |
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
      notify: Redémarrer chrony

  handlers:
    # Redémarrer chrony si la configuration a changé
    - name: Redémarrer chrony
      systemd:
        name: "{{ chrony_service }}"
        state: restarted
        enabled: yes

```
