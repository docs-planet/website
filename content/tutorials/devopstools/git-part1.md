---
title: "Git Howto (Parte 1)"
menuTitle: "Git (Parte 1)"
description: "Tutorial de GIT"
weight: 1
date: "2020-08-09"
LastModifierDisplayName: Juan Enciso
LastModifierEmail: juan.enciso@gmail.com
asciinema: true
tags:
  - git
---

{{% notice note %}}
Este tutorial esta baseado do seguinte documento: https://githowto.com
{{% /notice %}}

## 1. Prerequisitos

* Instalar git-bash-prompt seguindo este [tutorial](/tutorials/devopstools/git-bash-prompt/)
* Ter uma estacao de trabalho Linux

## 2. Preparação

Configurando nome e endereço de e-mail

```shell
git config --global user.name "Seu Nome Completo"
git config --global user.email "seu_email@sua_empresa.com"
```

Opções de Instalação: términos de linhas (Linux):

```shell
git config --global core.autocrlf input
git config --global core.safecrlf warn
```

{{< asciinema key="352499" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

## 3. Criando o projeto git

Vamos a criar um repositório git "hello" e uma página hello.html com o seguinte conteúdo: "Hello, World"

Crie uma página de "Hello, World" e inicialize o repositório

```sh
mkdir hello && cd hello
echo "Hello, World" > hello.html
```

```sh
git init
```

Adicione a página ao repositório:

```sh
git add hello.html
git commit -m "First Commit"
```

{{< asciinema key="git-basico01" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

## 4. Conferindo o status do repositório

Use o comando git status para checar o estado atual do repositório.

```sh
git status
```

{{< asciinema key="git-basico02" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

## 5. Fazendo modificações

Modificar a página `hello.html` com o seguinte conteúdo:

```html
<h1>Hello, World!</h1>
```

Conferindo o status:

```shell
git status
```

{{< asciinema key="git-basico03" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

Notar que:

- O git sabe que o arquivo `hello.html` foi modificado, mas essas modificações ainda não sofreram commit para o repositório.
- A mensagem de status oferece dicas sobre o que fazer em seguida. Se você quiser adicionar essas modificações para o repositório, use `git add`. Para desfazer as modificações use `git checkout`.

## 6. Adicionando modificações ao stage

Mande o git adicionar as modificações ao stage. Confira o status

```shell
git add hello.html
git status
```

{{< asciinema key="git-basico04" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

Observar:
- Modificações no `hello.html` foram adicionadas ao stage. Isso quer dizer que o git sabe da modificação, mas não é permanente no repositório. O próximo commit incluirá as modificações que estão no stage.
- Se você decidir não fazer commit da modificação, o comando status vai te lembrar que você pode usar o comando `git reset` para remover essas mudanças do stage.

## 7. Colocando em stage e fazendo commits

Adicionar algo ao stage no git permite que você continue fazendo modificações no seu diretório de trabalho e, quando decidir que quer interagir com o controle de versão, permite que guarde as mudanças em pequenos commits.

Pense que você editou três arquivos (a. html, b. html e c. html). Depois disso você tem que fazer commit de todas as modificações para que as mudanças em `a.html` e `b.html` sejam um único commit, enquanto as mudanças em `c.html`, que não são lógicamente relacionadas com as duas outras mudanças nos outros dois arquivos, sejam enviadas em um commit diferente.

Em teoria, você pode fazer o seguinte:

```sh
git add a.html
git add b.html
git commit -m "Changes for a and b"
```

```sh
git add c.html
git commit -m "Unrelated change to c"
```

Separando a adição ao stage e o commit, você pode customizar o que vai em cada commit.

## 8. Fazendo commit das modificações

Na hora de executar o comando `git commit` vamos omitor o uso do flag `-m`, desta forma entramos na edição interativa de comentários para o commit. O git ira abrir o editor configurado a partir desta lista (em ordem de prioridade)

* Variável de ambiente GIT_EDITOR
* Definição de configuração core.editor
* Variável de ambiente VISUAL
* Variável de ambiente EDITOR

No me caso meu editor é 'vi'. Caso deseje modificar seu editor de forma permanente, edite o arquivo `~/.gitconfig`

Logo de executar o commit, iremos colocar como comentario "Added h1 tag"

{{< asciinema key="git-basico05" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

## 9. Modificações, não arquivos

O git trabalha com as modificações, não com os arquivos.

* Primeira mudança: Adicionando tags padrão de páginas e executando `git add hello.html`. O novo conteúdo para o arquivo: `hello.html` será:

```html {linenos=false,hl_lines=["1-2","4-5"],linenostart=1}
<html>
  <body>
    <h1>Hello, World!</h1>
  </body>
</html>
```

```shell
git add hello.html
```

* Segunda mudança: Adicione os headers do HTML. Agora adicione os headers (<head> section) na página

```html {linenos=false,hl_lines=["2-3"],linenostart=1}
<html>
  <head>
  </head>
  <body>
    <h1>Hello, World!</h1>
  </body>
</html>
```

Agora excute:
```shell
git status
```

{{< asciinema key="git-basico06" rows="28" cols="110" preload="1" poster="npt:1:12" speed="1.5" >}}

Note que o arquivo `hello.html` está listado duas vezes no status. 

- A primeira mudança (a adição das tags padrão) está no stage e pronta para um commit. 
- A segunda mudança (adição dos headers) não está no stage.

Se você fizesse um commit agora, os headers não teriam sido salvos no repositório.

* Commit: Faça um commit das mudanças que estão no stage (as tags padrão) e então confira novamente o status.

```shell
git commit -m "Added standard HTML page tags"
git status
```

{{< asciinema key="git-basico07" rows="28" cols="110" preload="1" poster="npt:0:11" speed="1.5" >}}

O comando status diz que o arquivo hello.html tem modificações não gravadas, mas não está mais em buffer.

* Adicionando a segunda modificação: Adicione a segunda modificação ao stage, depois execute o comando `git status`

```shell
git add .
git status
git commit -m "Added HTML header"
```

{{< asciinema key="git-basico08" rows="28" cols="110" preload="1" poster="npt:0:14" speed="1.5" >}}

## 10. Histórico

Usaremos o comando `git log`

```shell
git log
```

{{< asciinema key="git-basico09" rows="28" cols="110" preload="1" poster="npt:0:04" speed="1.5" >}}

Histórico em uma linha:

```
git log --pretty=oneline
```

Controlando a exibição de entradas

```
git log --pretty=oneline --max-count=2
git log --pretty=oneline --since='5 minutes ago'
git log --pretty=oneline --until='5 minutes ago'
git log --pretty=oneline --author=<your name>
git log --pretty=oneline --all
```


Para rever as modificações feitas na última semana

```
git log --all --pretty=format:"%h %cd %s (%an)" --since='7 days ago'
```

Um formato bacana

```
git log --pretty=format:"%h %ad | %s%d [%an]" --graph --date=short
```

{{< asciinema key="git-basico10" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

Vamos olhar os detalhes:

* --pretty="..." define o formato da saída
* %h é o hash abreviado do commit
* %d mostra decorações do commit (ex.: head de branches ou tags)
* %ad é a data do commit
* %s é o comentário
* %an é o nome do autor =
* --graph fala para o git mostrar a árvore de commits no formato de um gráfico de ASCII
* --date=short mantém o formato de data pequeno e simples

Então, toda vez que você quiser ver um log, você terá que digitar muito. Felizmente, nós aprenderemos sobre aliases na próxima lição.

## 11. Aliases

Adicione as seguintes linhas no arquivo `~/.gitconfig`

```properties
[alias]
        co = checkout
        ci = commit
        st = status
        br = branch
        hist = log --pretty=format:\"%h %ad | %s%d [%an]\" --graph --date=short
        type = cat-file -t
        dump = cat-file -p
```

Agora execute:

```shell
git st
git hist
```

{{< asciinema key="git-basico11" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

## 12. Usando versões anteriores

#### Conseguindo os hashes das versões anteriores

```shell
git hist
```

O resultado será:

{{< asciinema key="git-basico12" rows="28" cols="110" preload="1" poster="npt:0:05" speed="1.5" >}}

Confira a data do log e encontre o hash do primeiro commit. Você vai achar ele na última linha do git hist Use o código (os seus 7 primeiros caracteres são suficientes) no comando abaixo. Depois disso, cheque o conteúdo do arquivo hello.html.

```shell
git checkout <hash>
cat hello.html
```

{{< asciinema key="git-basico13" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

#### Voltando para a versão mais atual no branch master

```shell
git checkout master
cat hello.html
```

{{< asciinema key="git-basico14" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

‘master’ é o nome do branch padrão. Ao entrar em um branch pelo seu nome, você vai para a sua versão mais atual.


## 13. Adicionando tags a versões

Vamos chamar a versão atual do nosso programa "Hello" de versão 1 (v1).

#### Criando a tag do primeiro

```shell
git tag v1
```

#### Tags em versões antigas

Vamos adicionar uma tag à versão anterior da nossa atual versão com o nome `v1-beta`. Nós vamos usar a notação ^ indicando “o pai de v1”.
Se a notação `v1^` gera problemas, tente usar `v1~1` para referenciar a mesma versão. Essa notação significa “a primeira versão antes de v1”.

```shell
git checkout v1^
cat hello.html
```

{{< asciinema key="git-basico15" rows="28" cols="110" preload="1" poster="npt:0:20" speed="1.5" >}}

Essa é a versão com as tags `<html>` e `<body>`, mas sem `<head>`. Vamos fazer dessa a versão `v1-beta`.

```shell
git tag v1-beta
```

#### Acessando através do nome da tag

```shell
git checkout v1
git checkout v1-beta
```

{{< asciinema key="git-basico16" rows="28" cols="110" preload="1" poster="npt:0:20" speed="1.5" >}}

#### Vendo tags com o comando tag

```shell
git tag
```
#### Vendo tags nos logs

```shell
git hist master --all
```
{{< asciinema key="git-basico17" rows="28" cols="110" preload="1" poster="npt:0:20" speed="1.5" >}}

Você pode ver as tags (v1 e v1-beta) listadas no log juntamente com o nome do branch (master). O HEAD mostra o commit em que você está atualmente (v1-beta).

