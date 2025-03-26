templates/chrony.conf.j2
```bash
{# Template pour chrony.conf #}

server 0.fr.pool.ntp.org iburst
server 1.fr.pool.ntp.org iburst
server 2.fr.pool.ntp.org iburst
server 3.fr.pool.ntp.org iburst
driftfile /var/lib/chrony/drift
makestep 1.0 3
rtcsync
logdir /var/log/chrony

```
chrony.yml
```bash
---
- name: Installer et configurer Chrony sur les hôtes cibles
  hosts: all
  become: true
  vars:
    # Définir le chemin du fichier de configuration en fonction de la distribution
    chrony_conf_path: "{{ '/etc/chrony/chrony.conf' if ansible_facts['distribution'] in ['Debian', 'Ubuntu'] else '/etc/chrony.conf' }}"

  tasks:
    # Installer chrony si ce n'est pas déjà fait
    - name: Installer chrony
      package:
        name: chrony
        state: present

    # Copier la configuration personnalisée de chrony.conf sur les cibles
    - name: Copier le fichier de configuration chrony.conf
      template:
        src: chrony.conf.j2
        dest: "{{ chrony_conf_path }}"
        mode: 0644
      notify: Redémarrer chrony

  handlers:
    # Redémarrer chrony si la configuration a changé
    - name: Redémarrer chrony
      systemd:
        name: chronyd
        state: restarted
        enabled: yes

```
