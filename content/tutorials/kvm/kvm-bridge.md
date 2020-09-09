---
title: "KVM - Bridge Setup"
description: "Configurando KVM em modo bridge"
asciinema: true
comments: true
tags:
  - kvm
---

## Requisitos 

- KVM instalado
- Ter uma interfaz de rede ethernet (não wireless)
- Ubuntu 18.04 ou superior

## Procedimento

Verificando qual é o nome da minha interfaz de rede

```
nmcli device status
```
> Observe o resultado da columna DEVICE

Com esse dado configuramos via netplan a configuração bridge adicionando um arquivo yaml.

```shell
sudo vi /etc/netplan/50-cloud-init.yaml
```

Copiar um conteúdo similar. No me caso o DEVICE da minha NIC é `enp2s0`:

```yaml {linenos=false,hl_lines=["4","9"],linenostart=1}
network:
  version: 2
  ethernets:
    enp2s0:
      dhcp4: yes
      dhcp6: no
  bridges:
    br0:
      interfaces: [enp2s0]
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

## Referências

- https://netplan.io/examples/
- https://webby.land/2018/04/27/bridging-under-ubuntu-18-04/
- https://github.com/jenciso/notes/blob/master/KVM.md
- https://askubuntu.com/questions/1106766/netplan-wireless-bridge-ubuntu-18
- https://fabianlee.org/2019/04/01/kvm-creating-a-bridged-network-with-netplan-on-ubuntu-bionic/
