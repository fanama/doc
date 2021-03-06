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


```yaml
---
- hosts: my_app
  sudo: yes

  vars:
    - homeDir: /home/ubuntu
    - appDir : app
    - repo: my_app
    - account: mbejda
    - privateKey: /Users/milos/.ssh/id_rsa

  tasks:
  - name: Install Packages
    apt: name={{ item }} update_cache=yes state=latest
    with_items:
      - build-essential
      - npm
      - nodejs-legacy
      - git
      - mcrypt
      - nginx
      - curl
  - name: Install pm2
    npm: name=pm2 global=yes production=yes
  - name: Create APP Directory
    file: path={{homeDir}}/{{appDir}} state=directory
  - name: Copy Private Key
    copy: src={{privateKey}} dest={{homeDir}} mode=0600
  - name: Git Clone Repo
    git: repo=git@github.com:{{account}}/{{repo}}.git dest={{homeDir}}/{{appDir}} update=yes force=yes accept_hostkey=yes key_file={{homeDir}}/id_rsa
    register: git_finished
  - name: Running NPM install
    npm: path={{homeDir}}/{{appDir}}/backened
    register: npm_finished
    when: git_finished.changed
  - name: Stop APP
    sudo_user: ubuntu
    command: pm2 stop app chdir={{homeDir}}/{{appDir}}/backened
    ignore_errors: yes
  - name: Start APP
    sudo_user: ubuntu
    command: pm2 start index.js --name app chdir={{homeDir}}/{{appDir}}/backened
    ignore_errors: yes
    when: npm_finished.changed

```
