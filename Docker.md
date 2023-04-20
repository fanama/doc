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
## Docker compose V2

```bash
$ mkdir -p ~/.docker/cli-plugins/
$ curl -SL https://github.com/docker/compose-cli/releases/download/v2.0.0-rc.1/docker-compose-linux-amd64 -o ~/.docker/cli-plugins/docker-compose
$ chmod +x ~/.docker/cli-plugins/docker-compose
```

## ajouter l'utilisateur

```sh
$ sudo usermod -aG docker $USER
$ newgrp docker
```

## doc

https://www.digitalocean.com/community/tutorials/how-to-remove-docker-images-containers-and-volumes
