---
title: "Docker - Instalacão"
description: "Instalando Docker"
asciinema: true
weight: 1
comments: true
tags:
  - kvm
---

![](https://deploybot.com/assets/blog/Using-Docker-Containersposting.png?width=300px)

## Requisitos 

- Ubuntu Linux 18.04 ou superior

## Procedimento

### Instalando docker

```shell
sudo apt-get remove docker docker-engine docker.io containerd runc
```

```shell
sudo apt-get update

sudo apt-get install \
   apt-transport-https \
   ca-certificates \
   curl \
   gnupg-agent \
   software-properties-common
```

```shell
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

```shell
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

```shell
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

```shell
sudo docker run hello-world
```

-----

{{< asciinema key="docker01" rows="28" cols="110" preload="1" poster="npt:2:16" speed="2" >}}

-----

### Instalando docker-compose

```shell
sudo curl -L "https://github.com/docker/compose/releases/download/1.27.0/docker-compose-$(uname -s)-$(uname -m)" \
  -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose

sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose

docker-compose --version
```

Dando permissões para seu usuario rodar o comando docker

```shell
sudo usermod -a -G docker $(id -nu)
reboot
```

## Referências

- https://docs.docker.com/engine/install/ubuntu/
