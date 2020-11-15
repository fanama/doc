# docker

## installer les dépendances

```sh
$ sudo apt update
$ sudo apt -y install apt-transport-https ca-certificates curl gnupg2 software-properties-common
```

## Installation des paquets Docker

```sh
$ sudo apt update
$ sudo apt install docker.io
```

## ajouter l'utilisateur

```sh
$ sudo usermod -aG docker $USER
$ newgrp docker
```