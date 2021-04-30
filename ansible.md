# Ansible

## Installation

pour installer ansible faire :
- > apt install ansible

## Creation de playbook

il est ensuite possible de créer des playbook afin d'installer des outils

### exemple de playbook

- install_node.yaml

```yaml
---
- hosts: localhost
  become: yes
  gather_facts: no
  tasks:
  - name: "Install NPM repo"
    shell: curl -sL https://rpm.nodesource.com/setup_10.x | bash -
  - name: "Install NPM depenencies"
    yum:
      name:
        - nodejs
        - gcc-c++
        - make
      state: latest
```

ensuite executer le playbook

- > sudo ansible-playbook <file.yaml>
