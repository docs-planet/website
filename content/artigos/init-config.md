---
title: Configurações iniciais
description: Primeiros passos para configurar sua estacao de trabalho
asciinema: true
tags:
  - init
---

### Configuração de Sudoers

Use `sudo` sem precisar digitar a senha

```
echo "`whoami`  ALL=(ALL) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/admins
```

### Criando par de chaves ssh

```shell
ssh-keygen
```
{{< asciinema key="tmp5r_pmybi-ascii" rows="28" cols="110" preload="1" poster="npt:0:10" speed="1.5" >}}


