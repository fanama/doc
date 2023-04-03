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

or

```sh
$ sudo apt install docker docker-compose docker-doc docker-registry docker.io
```

## ajouter l'utilisateur

```sh
$ sudo usermod -aG docker $USER
$ newgrp docker
```
