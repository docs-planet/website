---
title: Gitlab Install
description: Formas de instalar Gitlab CE
tags:
  - gitlab
---

## Intro

Como Instalar Gitlab

### Instalacão

Provisionar um servidor com 4GB RAM

```shell
cd kvm-provision
./new-vm.sh -n gitlab -m 4096 -c 2 -i 192.168.122.41
```

Acessar ao servidor

```shell
ssh centos@192.168.122.41
```

Instalar os pacotes necessarios

```shell
sudo yum install -y curl policycoreutils-python openssh-server openssh-clients firewalld
sudo systemctl enable sshd
sudo systemctl start sshd
```

Habilitar o firewalld

```shell
sudo systemctl start firewalld
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-service=https
sudo systemctl reload firewalld
```

Adicionar o repo

```shell
curl -sS https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.rpm.sh | sudo bash
```

### Configurar sem https

```shell
sudo EXTERNAL_URL="http://gitlab.docs-planet.site" yum install -y gitlab-ce
```

Verificar os logs:

```shell
tail -f /var/log/gitlab/gitlab-rails/production.log
```

Acessar no site: [http://gitlab.docs-planet.site](http://gitlab.docs-planet.site).
 - Configurar o seguinte password: `SuperSecret2012` para o usuario root.

### Configurando com https e integracao Active Directory

Com os seguintes certificados gerados via [Let's encrypt](/artigos/wildcard-certificate-letsencrypt-cloudflare/)

- certificate-bundle.crt 
- private.key

Movimente eles dentro da pasta: `/etc/gitlab/ssl`

```shell
mv certificate-bundle.crt /etc/gitlab/ssl/gitlab.docs-planet.site.crt
mv private.key /etc/gitlab/ssl/gitlab.docs-planet.site.key
```

Use este modelo de arquivo `gitlab.rb` para configurar o Gitlab com integracao AD e https. Este arquivo deve ficar no diretorio: `/etc/gitlab/`

```ini
external_url "https://gitlab.docs-planet.site/"

gitlab_rails['time_zone'] = 'America/Sao_Paulo'

nginx['client_max_body_size'] = '250m'
nginx['redirect_http_to_https'] = 'True'
nginx['ssl_certificate'] = "/etc/gitlab/ssl/gitlab.docs-planet.site.crt"
nginx['ssl_certificate_key'] = "/etc/gitlab/ssl/gitlab.docs-planet.site.key"

gitlab_rails['ldap_enabled'] = true

gitlab_rails['ldap_servers'] = {
'main' => {
  'label' => 'Gitlab AD',
  'host' =>  'ad.docs-planet.site',
  'port' => 389,
  'uid' => 'sAMAccountName',
  'encryption' => 'plain',
  'verify_certificates' => false,
  'bind_dn' => 'Administrator@internal.docs-planet.site',
  'password' => 'SuperSecret2012',
  'active_directory' => true,
  'base' => 'OU=USERS,OU=CORE,DC=internal,DC=docs-planet,DC=site',
  'group_base' => 'OU=GROUPS,OU=CORE,DC=internal,DC=docs-planet,DC=site',
  'admin_group' => 'Global Admins'
  }
}

gitlab_rails['gitlab_signup_enabled'] = true
gitlab_rails['smtp_enable'] = true
gitlab_rails['smtp_address'] = "relay.docs-planet.site"
gitlab_rails['smtp_port'] = 25
```

Reconfigure o Gitlab

```shell
gitlab-ctl reconfigure
```

Acesse agora na URL [https://gitlab.docs-planet.site](https://gitlab.docs-planet.site)

### Referencias

- https://about.gitlab.com/install/#centos-7?version=ce
- https://docs.gitlab.com/omnibus/settings/nginx.html#manually-configuring-https
- https://www.howtoforge.com/tutorial/how-to-install-gitlab-server-with-docker-on-ubuntu-1804/
