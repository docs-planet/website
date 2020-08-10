---
title: "Git Howto (Parte 2)"
menuTitle: "Git (Parte 2)"
description: "Tutorial de GIT"
weight: 2
date: "2020-08-10"
LastModifierDisplayName: Juan Enciso
LastModifierEmail: juan.enciso@gmail.com
asciinema: true
tags:
  - git
---

{{% notice note %}}
Este tutorial é a continuação do tutorial [Git (Part 1)](../git-part1)
{{% /notice %}}


## 14. Descartando mudanças locais (antes do stage)

#### Acessando o branch Master

Verifique que você esta no último commit do branch master antes de continuar.

```shell
git checkout master
```

#### Mude o hello.html

Acontece de você modificar o arquivo no seu diretório de trabalho local e às vezes querer descartar as mudanças que você fez commit. É aqui que o comando checkout vai te ajudar.

Faça mudanças ao arquivo hello.html na forma de um comentário indesejado.

```html {linenos=inline,hl_lines=["6"],linenostart=1}
<html>
  <head>
  </head>
  <body>
    <h1>Hello, World!</h1>
    <!-- This is a bad comment.  We want to revert it. -->
  </body>
</html>
```

Confira o status

```shell
git status
```

{{< asciinema key="git-basico18" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

#### Desfazendo as mudanças no diretório de trabalho

```shell
cat hello.html
git checkout hello.html
git status
cat hello.html
```

{{< asciinema key="git-basico19" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

O comando status mostra que não existem mudanças que não estão no stage no repositório de trabalho. E o “comentário ruim” não está mais no arquivo.

## 15. Descartando mudanças no stage (antes do commit)
