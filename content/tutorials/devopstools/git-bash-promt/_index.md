---
title: "Git bash prompt"
description: "Instalando o git bash prompt e melhorando nossa visibilidade"
tags:
  - git
---

## Intro

Para ter uma melhor visibilidade do que passa em nossos projetos que estão versionados quando estamos trabalhando de forma local, podemos usar ferramentas como é o caso de [git-bash-prompt](https://github.com/magicmonty/bash-git-prompt)

## Instalação

1. Baixar o repositorio contendo o software

```shell
cd ~
git clone https://github.com/jenciso/bash-git-prompt.git .bash-git-prompt --depth=1
```

2. Adicionar algumas linhas dentro do arquivo `.bashrc`

```bash
GIT_PROMPT_ONLY_IN_REPO=0
GIT_PROMPT_THEME=Single_line_Minimalist
source ~/.bash-git-prompt/gitprompt.sh
```

## Testando

