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

### Passo 4: Configurar network bridge

Verificando qual é o nome da minha interfaz de rede

```
nmcli device status
```
> Observer o resultado da columna DEVICE

Com esse dado configuramos via netplan a configuração bridge adicionando um arquivo yaml.

```shell
sudo vi /etc/netplan/50-cloud-init.yaml
```

Copiar um conteúdo similar. No me caso o DEVICE da minha NIC é `wlp1s0`:

```yaml {linenos=inline,hl_lines=["4","9"],linenostart=1}
network:
  version: 2
  ethernets:
    wlp1s0:
      dhcp4: yes
      dhcp6: no
  bridges:
    br0:
      interfaces: [wlp1s0]
      dhcp4: yes
      dhcp6: no
```

Logo aplicamos os cambios:

```
sudo netplan apply
sudo networkctl status -a
```

Aqui podemos reiniciar nosso computador para poder verificar que as configurações persistem.

```
sudo reboot
```

{{< asciinema key="kvm04" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

## Referências

- https://www.linuxtechi.com/install-configure-kvm-ubuntu-18-04-server/
