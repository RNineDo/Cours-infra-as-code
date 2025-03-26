kernel.yml
```bash
---
- name: Afficher les informations du noyau sur tous les hôtes cibles
  hosts: all
  tasks:
    - name: Exécuter la commande uname -a
      command: uname -a
      register: kernel_info

    - name: Afficher les informations du noyau avec msg
      debug:
        msg: "Informations du noyau : {{ kernel_info.stdout }}"

    - name: Afficher les informations du noyau avec var
      debug:
        var: kernel_info.stdout
```
packages.yml
```bash
---
- name: Afficher le nombre total de paquets RPM installés
  hosts: rocky, suse
  tasks:
    - name: Compter le nombre de paquets RPM installés
      shell: rpm -qa | wc -l
      register: rpm_count
      changed_when: false

    - name: Afficher le nombre de paquets RPM installés
      debug:
        msg: "Nombre total de paquets RPM installés : {{ rpm_count.stdout }}"
```
