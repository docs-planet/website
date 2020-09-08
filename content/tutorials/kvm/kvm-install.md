---
title: "KVM - Instalacão"
description: "Instalando KVM"
asciinema: true
weight: 1
comments: true
tags:
  - kvm
---

## Requisitos 

- Ubuntu Linux 18.04 ou superior

## Procedimento

### Passo 1: Verificar

Verificar se nosso sistema suporta hardware virtualization

```
egrep -c '(vmx|svm)' /proc/cpuinfo
```
> Se a saida é maior que zero, nosso sistema suporta virtualização. Esta tudo ok.

Agora instalar `kvm-ok` para saber se o servidor pode rodar hardware accelerated KVM

```
sudo apt install cpu-checker
sudo kvm-ok
```

{{< asciinema key="kvm01" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

### Passo 2: Instalar KVM


```shell
sudo apt update
sudo apt install qemu qemu-kvm libvirt-bin  bridge-utils  virt-manager
```

{{< asciinema key="kvm02" rows="28" cols="110" preload="1" poster="npt:0:01" speed="2" >}}


### Passo 3: Start & enable libvirtd service

```shell
sudo service libvirtd start
sudo update-rc.d libvirtd enable
service libvirtd status
```

{{< asciinema key="kvm03" rows="28" cols="110" preload="1" poster="npt:0:01" speed="2" >}}

## Referências

- https://www.linuxtechi.com/install-configure-kvm-ubuntu-18-04-server/
