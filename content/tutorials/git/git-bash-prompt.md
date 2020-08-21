---
title: "Git Bash Prompt"
description: "Instalando um git-bash-prompt para melhorar nossa visibilidade no git"
asciinema: true
weight: 1
comments: true
tags:
  - git
  - bash
---

## Intro

Para ter uma melhor visibilidade do que passa em nossos projetos que estão versionados quando estamos trabalhando de forma local, podemos usar ferramentas como é o caso de [git-bash-prompt](https://github.com/magicmonty/bash-git-prompt)

## Instalação

Baixar o repositorio contendo o software

```console
cd ~
git clone https://github.com/jenciso/bash-git-prompt.git .bash-git-prompt --depth=1
```

Adicionar algumas linhas dentro do arquivo `.bashrc`

```sh
GIT_PROMPT_ONLY_IN_REPO=0
GIT_PROMPT_THEME=Single_line_Minimalist
source ~/.bash-git-prompt/gitprompt.sh
```

-----

{{< asciinema key="351993" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}


## Testando

Baixar qualquer repositorio git. Ex: https://github.com/jenciso/node-express-azure e fazemos uma simples modificação

```shell
git clone https://github.com/jenciso/node-express-azure
cd node-express-azure
rm README.md
```
Agora podemos ver que estamos na branch `master` e o shell nos indica que temos um cambio em nosso repositorio.

-----

{{< asciinema key="351996" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

