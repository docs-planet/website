---
title: "Docker - Instalacão"
description: "Instalando Docker"
asciinema: true
weight: 1
comments: true
tags:
  - kvm
---

![](https://w7.pngwing.com/pngs/475/563/png-transparent-docker-software-deployment-industrial-design-docker-logo-microsoft-azure-industrial-design.png)

## Requisitos 

- Ubuntu Linux 18.04 ou superior

## Procedimento

Apagando versões anteriores

```shell
sudo apt-get remove docker docker-engine docker.io containerd runc
```

Instalando os prerequisitos

```shell
sudo apt-get update

sudo apt-get install \
   apt-transport-https \
   ca-certificates \
   curl \
   gnupg-agent \
   software-properties-common
```

Adicionando o repositório
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

```shell
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

Instalando docker:

```shell
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

Testando:

```shell
sudo docker run hello-world
```

---- 

{{< asciinema key="docker01" rows="28" cols="110" preload="1" poster="npt:2:16" speed="2" >}}

## Referências

- https://docs.docker.com/engine/install/ubuntu/
