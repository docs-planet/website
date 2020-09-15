---
title: "KVM - Provision"
description: "Provisionando maquinas virtuais de maneira simples"
asciinema: true
weight: 3
comments: true
tags:
  - kvm
---

## Requisitos

- KVM instalado
- Ubuntu 18.04 ou superior
- Usar uma conta com privilegios de root (sudo)

## Instalação

```shell
cd ~
git clone https://github.com/jenciso/kvm-provision
```

## Preparação

Precisamos baixar uma imagem base para poder usar na criacação de VM's. Usaremos a image Centos 7 ja preperada para trabalhar com cloud-init. Colocaremos esta imagem na pasta boot do libvirt. Execute o seguinte comando:

```shell
sudo curl -fSL -C - http://cloud.centos.org/centos/7/images/CentOS-7-x86_64-GenericCloud.qcow2 \
  -o /var/lib/libvirt/boot/CentOS-7-x86_64-GenericCloud.qcow2
```

Para poder executar alguns comandos do kvm, precisamos adicionar nosso usuario ao grupo `libvirt`, e para que os cambios sejam efetivados temos q sair da sessao e volver ingressar

```shell
sudo usermod -a -G libvirt `whoami`
logout
```

## Começando

Criando uma VM (Virtual Machine)

```shell
./new-vm.sh centos7
```

- Esta VM irá ter a IP: 192.168.122.10. 
- O user é `centos` e o password será `SuperSecret2012`. 

Entrando na VM via console:

```shell
sudo virsh console centos7 
```
> Para sair use:  Ctrl + ]

Entrando via SSH

```shell
ssh centos@192.168.122.10
```

Apagando a VM

```shell
./del-vm.sh centos7
```
> Apaga a VM e todo seu conteúdo, inclusive os discos duros

## Mais comandos

### Copiando suas Chaves SSH

Nossa chave SSH publica pode ser colocada no parametro `SSH_KEY_0`, de tal maneira que consigamos acessar ao servidor virtual sem precisar user/password.

Se ainda não criou um par de chaves ssh para seu usuario, siga este [procedimento](/artigos/init-config/#criando-par-de-chaves-ssh). Logo disso, voce ira ter o arquivo `~/.ssh/id_rsa.pub`. Copie e cole o conteúdo desse arquivo dentro do arquivo `config.conf` exatamente como o valor da variavel `SSH_KEY_0`.

```sh
SSH_KEY_0=ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDYN0xPPa9KV0pZ5vNyet5e5fvWHNCgOTJ5ON9SHef3d
```

### Criando uma VM com uma configuração específica

Crie a VM `demo01` com 1GB de RAM, 2 VCPUs e com a IP 192.168.122.12

```console
./new-vm.sh -n demo01 -m 1024 -c 2 -i 192.168.122.12
```

### Usando um novo password para suas VM's 

Cambiando o password default para este valor: `MySuperPassword`, precisamos executar o comando:

```shell
openssl passwd -1 -salt SaltSalt MySuperPassword
```

Com a saida desse comando, defina a variavel PASSWORD:

`PASSWORD='$1$SaltSalt$jjGsH9DHuY/4Vda.6JZVH1'`

> Dessa forma, a proxima VM ira ter o password _MySuperPassword_ para o user centos

### Aumentando o tamanho do disco duro

É só editar a variavel:

`DISK_SIZE=40G`

> Aquilo ira criar uma VM com 40GB de espaco de disco

### Adicionando um novo disco

Caso a VM tenha como nome `centos7` e queramos ter um novo disco adicional de 60GB.

```shell
./add-disk.sh -n centos7 -d vdb -s 60G
```

### Listar todas as VM's criadas

```shell
virsh list --all
```

### Gestione as VM's em modo grafico

```shell
virt-manager
```

Ira carregar o seguinte gestor de VMs

![](https://i.imgur.com/gUHgiht.png)

-------

## Demo

{{< asciinema key="tmpdazdnlg5-ascii" rows="28" cols="110" preload="1" poster="npt:0:01" speed="2" >}}

## Referencias

- http://blog.programster.org/kvm-creating-thinly-provisioned-guests
