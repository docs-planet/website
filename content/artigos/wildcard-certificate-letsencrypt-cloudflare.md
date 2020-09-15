---
title: Criando certificados wildcard Let's Encrypt
menuTitle: Wildcard Let's Encrypt
description: Como criar certificados wildcard Let's Encrypt usando o servico DNS da Cloudflare
tags:
  - cloudflare
  - dns
  - ssl
---

## Steps

### 1. Obter as credenciais do Cloudflare API

Pegar o Global API Key:

![](https://i.imgur.com/JKE1mJa.png)

### 2. Salvando as credenciais 

```shell
sudo mkdir -p /root/.secrets
```

```sh
cat << 'EOF' | sudo tee /root/.secrets/cloudflare.ini
dns_cloudflare_email = "youremail@example.com"
dns_cloudflare_api_key = "4003c330b45f4fbcab420eaf66b49c5cbcab4"
EOF
```

### 3. Instalando certbot e cloudflare DNS authenticator plugin

```shell
sudo apt-get update
sudo apt-get install software-properties-common
sudo add-apt-repository universe
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get install certbot python-certbot-nginx python3-certbot-dns-cloudflare
```

### 4. Rodar o Certbot com o cloudflare authenticator

```shell
sudo certbot certonly \
  --dns-cloudflare \
  --dns-cloudflare-credentials /root/.secrets/cloudflare.ini \
  -d docs-planet.site,*.docs-planet.site \
  --preferred-challenges dns-01
```

> Aquilo criará a pasta `/etc/letsencrypt/live/docs-planet.site/` contendo todos certificados.


### 5. Verifique seu certificado

Copie o conteúdo do arquivo `/etc/letsencrypt/live/docs-planet.site/fullchain.pem` e visitar o site https://www.sslshopper.com/certificate-decoder.html para verificar q o certificado esteja OK

Ou executar este comando:

```shell
sudo openssl x509 -in /etc/letsencrypt/live/docs-planet.site/fullchain.pem -text -noout
```

### 6. Renovacão automática

É só criar um linha no crontab

```yaml
0 10 * * * sudo certbot renew --noninteractive
```

## Referencias

- https://certbot.eff.org/lets-encrypt/ubuntubionic-nginx
- https://www.bjornjohansen.com/wildcard-certificate-letsencrypt-cloudflare
- https://www.digitalocean.com/community/tutorials/how-to-retrieve-let-s-encrypt-ssl-wildcard-certificates-using-cloudflare-validation-on-centos-7
- https://www.cyberciti.biz/faq/issue-lets-encrypt-wildcard-certificate-with-acme-sh-and-cloudflare-dns/
