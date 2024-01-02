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

## Linux Mibt

```sh
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$UBUNTU_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

```

## ajouter l'utilisateur

```sh
$ sudo usermod -aG docker $USER
$ newgrp docker
```

## doc

https://www.digitalocean.com/community/tutorials/how-to-remove-docker-images-containers-and-volumes
