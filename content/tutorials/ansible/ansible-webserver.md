---
title: Ansible - Apache Web Server Provision
description: Provisionamente de servidor web apache via ansible
tags:
  - ansible
---

## Intro 

Este tutorial tem o seguinte desafio

- Criar 2 VMs e fazer o deploy do apache web server via um playbook ansible.
- Colocar um arquivo index.html no diretorio /var/www/html em cada servidor. Este arquivo ira conter o necessario para mostrar a mensagem: "Olá Mundo".
- Obtenha a IP address de cada servidor e o nome da interface de rede associada.
- Criar uma VM adicional e coloque ela num grupo diferente chamado "hlg", as anteriores VMs irao estar no grupo prd.
- Mude o playbook para agora mostrar a mensagem: "Olá Mundo - $ambiente". Aonde $ambiente ira variar de acordo ao grupo de servidor.   

### Provision

Criando VMs para o ambiente de PRD 

```shell
./new-vm.sh -n web-prd01 -m 1024 -c 2 -i 192.168.122.21
./new-vm.sh -n web-prd02 -m 1024 -c 2 -i 192.168.122.22
```

### Playbook

Criando o inventory

```ini
[webservers]
192.168.122.21
192.168.122.22
```

Criando a primeira versao do playbook

```yaml
---
- name: install and start apache
  hosts: web
  become: yes

  tasks:
    - name: httpd package is present
      yum:
        name: httpd
        state: present
```

Rodando o playbook

```shell
ansible-playbook  site.yml -i inventory
```

Criando a segunda versao:

```yaml
---
- name: install and start apache
  hosts: web
  become: yes

  tasks:
    - name: httpd package is present
      yum:
        name: httpd
        state: present

    - name: latest index.html file is present
      template:
        src: files/index.html
        dest: /var/www/html/
```

Rodando o playbook

```shell
ansible-playbook  site.yml -i inventory
```

Criando a terceira versao:

```yaml
---
- name: install and start apache
  hosts: web
  become: yes

  tasks:
    - name: httpd package is present
      yum:
        name: httpd
        state: present

    - name: latest index.html file is present
      template:
        src: files/index.html
        dest: /var/www/html/

    - name: httpd | started
      service:
        name: httpd
        state: started
```

Rodando o playbook

```shell
ansible-playbook  site.yml -i inventory
```

Criando a VM para o ambiente de HLG

```shell
./new-vm.sh -n web-hlg01 -m 1024 -c 2 -i 192.168.122.23
```

Modificando o esquema

```sh
$ tree
.
├── group_vars
│   ├── all
│   │   └── main.yml
│   ├── hlg
│   │   └── main.yml
│   └── prd
│       └── main.yml
├── inventory
├── README.md
├── roles
│   └── apache
│       ├── tasks
│       │   └── main.yml
│       └── templates
│           └── index.html
└── site.yml

8 directories, 8 files
```

Arquivo index.html

```html
<html>
<body></body>
<p>
  Ola Mundo - {{ ambiente }}
</p>
</html>
```

Projeto final: https://github.com/jenciso/ansible-apache.git
