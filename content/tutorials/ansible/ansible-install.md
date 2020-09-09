---
title: Ansible - Instalacão
description: Instalando Ansible
asciinema: true
weight: 1
comments: true
tags:
  - kvm
---

![](https://www.ansible.com/hubfs/blog/Untitled_design.jpg)

## Requisitos 

- Ubuntu Linux 18.04 ou superior

## Procedimento

### Instalacão

```shell
sudo apt update
sudo apt install software-properties-common
sudo apt-add-repository --yes --update ppa:ansible/ansible
sudo apt install ansible
```

### Configuração

Crie uma pasta para a configuração do Ansible e para colocar seus playbooks

```shell
mkdir ~/ansible
```

Crie um arquivo `~/.ansible.cfg`

```shell
cat << 'EOF' > ~/.ansible.cfg
[defaults]
inventory         = ~/ansible/hosts
roles_path        = ~/ansible/roles
host_key_checking = False

[privilege_escalation]
become=True
become_method=sudo
become_user=root
EOF
```

### Testando

```
ansible --version
```

-----

{{< asciinema key="tmp38k3hpwz-ascii" rows="28" cols="110" preload="1" poster="npt:0:01" speed="2" >}}

-----

## Referências

- https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-ansible-on-ubuntu
