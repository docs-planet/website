---
title: "Git (Parte 2)"
menuTitle: "Git (Parte 2)"
description: "Tutorial de GIT"
weight: 3
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

```html {linenos=false,hl_lines=["6"],linenostart=1}
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

#### Edite o arquivo e adicione as mudanças ao stage

Faça mudanças ao arquivo `hello.html` na forma de um comentário indesejado.

```html {linenos=false,hl_lines=["3"],linenostart=1}
<html>
  <head>
    <!-- This is an unwanted but staged comment -->
  </head>
  <body>
    <h1>Hello, World!</h1>
  </body>
</html>
```
E logo adicione o arquivo ao stage.

```shell
git add hello.html
git status
```

{{< asciinema key="git-basico20" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

#### Revertendo a zona de buffer

Felizmente, o status informado nos mostra exatamente o que devemos fazer para cancelar mudanças no stage.

```
git reset HEAD hello.html
```

{{< asciinema key="git-basico21" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

O comando `reset` retorna a zona do buffer para HEAD. Isso limpa a zona do buffer das mudanças que nós acabamos de adicionar ao stage.

O comando `reset` (padrão) não altera o diretório de trabalho. Logo, o diretório de trabalho ainda tem os comentários indesejados. Nós podemos usar o comando checkout do tutorial anterior para remover as mudanças do repositório de trabalho.

#### Mudando para a versão do commit

```
git checkout hello.html
git status
```
{{< asciinema key="git-basico22" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}} 

Nosso diretório de trabalho está limpo novamente.

## 16. Desfazendo commits


Algumas vezes você percebe que os novos commits estão errados e você quer desfazê-los. Existem várias maneiras de resolver esse problema, mas nós usamos a mais segura aqui.

Para desfazer o commit, vamos criar um novo commit desfazendo as modificações não desejadas.

#### Edite o arquivo e faça um commit

Substitua o arquivo `hello.html` com o seguinte.

```html {linenos=false,hl_lines=["6"],linenostart=1}
<html>
  <head>
  </head>
  <body>
    <h1>Hello, World!</h1>
    <!-- This is an unwanted but committed change -->
  </body>
</html>
```

```
git add hello.html
git commit -m "Oops, we didn't want this commit"
```

#### Faça um commit com as novas modificações que desfazem as modificações anteriores

Para desfazer o commit, precisamos criar um commit que deleta as modificações feitas pelo commit indesejado.

```shell
git revert HEAD
```

{{< asciinema key="git-basico23" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}} 

Voce tb poderia haver usado o comando `git revert HEAD --no-edit`. O comando `--no-edit` pode ser ignorado. Ele era desnecessário para gerar as informações de saída sem abrir o editor.

#### Confira o log

```
git hist
```

{{< asciinema key="git-basico24" rows="28" cols="110" preload="1" poster="npt:0:06" speed="1.5" >}}

A seguir vamos olhar a técnica que pode ser usada para remover o último commit do histórico do repositório.

## 17. Removendo um commit de um branch

`Revert` é um comando poderoso da seção anterior que te permite cancelar quaisquer commits para um repositório. Apesar disso, tanto os commits originais quanto os cancelados permanecem visíveis no histórico do branch (quando usamos o comando `git log`).

Frequentemente depois que um commit é feito percebemos que ele era um erro. Seria legal ter um comando de desfazer que permitisse deletar o commit incorreto imediatamente. Esse comando preveniria a aparição de um commit indesejado no histórico do `git log`.

#### O comando reset

Nós já usamos o comando reset para equiparar o buffer zone e o commit selecionado (commit HEAD foi usado na lição anterior).

Quando uma referência a um commit é dada (Exemplo: um branch, hash, ou tag name), o comando reset vai...

1. Sobrescrever o branch atual para que ele aponte para o commit correto
2. Opcionalmente resetar o buffer zone para que ele satisfazer o commit especificado
3. Opcionalmente resetar o dirétorio de trabalho para que ele equipare-se ao commit especificado

#### Cheque nosso histórico

```
git hist
```

{{< asciinema key="git-basico25" rows="28" cols="110" preload="1" poster="npt:0:06" speed="1.5" >}}

Nós vemos que os dois últimos commits desse branch são "Oops" and "Revert Oops". Vamos removê-los com o comando reset.

#### Marque esse branch primeiro

Vamos marcar nosso último commit com tag, para que possamos achá-lo após remover commits.

```
git tag oops
```

#### Resete o commit para o Oops anterior

No log de histórico (veja acima), o commit com tag «v1» está fazendo commit sobre um commit anterior incorreto. Vamos resetar o branch para aquele ponto. Como o branch tem uma tag, podemos usar o nome da tag no comando reset (se não possuir uma tag, podemos usar o valor hash).

```
git reset --hard v1
git hist
```

{{< asciinema key="git-basico26" rows="28" cols="110" preload="1" poster="npt:0:06" speed="1.5" >}}

#### Nada é perdido para sempre

O que acontece com os commits errados? Eles ainda estão no repositório. Na verdade, ainda podemos nos referir a eles. No início da lição, criamos a tag «oops» para o commit cancelado. Vamos dar uma olhada em all (todos) commits.

```
git hist --all
```

{{< asciinema key="git-basico27" rows="28" cols="110" preload="1" poster="npt:0:06" speed="1.5" >}}

Podemos ver que os commits errados não foram embora. Eles não estão listados mais no branch master mas ainda permanecem no repositório. Eles ainda estariam no repositório caso não tivéssemos colocado uma tag neles, mas só poderíamos referenciá-los por seus nomes hash. Commits não referenciados continuam no repositório até que um software garbage collection é acionado pelo sistema.

#### Perigos de resetar

Resets em branches locais geralmente são inofensivos. As consequências de quaisquer "acidentes" podem ser revertidos usando um commit apropriado.

Apesar disso, outros usuários que compartilham o branch podem ficar confusos se o branch compartilhado fica armazenado em repositórios remotos.

## 18. Removendo a tag oops

A tag oops já fez o seu trabalho. Vamos removê-la e permitir que o garbage collector delete o commit referenciado.

```
git tag -d oops
git hist --all
```

{{< asciinema key="git-basico28" rows="28" cols="110" preload="1" poster="npt:0:13" speed="1.5" >}}

## 19. Mudando commits 

#### Mude a página e faça um commit 

Coloque um comentário de autor na página.

```html {linenos=false,hl_lines=["1"],linenostart=1}
<!-- Author: Juan Enciso -->
<html>
  <head>
  </head>
  <body>
    <h1>Hello, World!</h1>
  </body>
</html>
```

```
git add hello.html
git commit -m "Add an author comment"
```

#### Oops... precisa do e-mail

Depois de fazer o commit, você percebe que todo bom comentário deveria incluir o e-mail do autor. Edite a página hello para fornecer um e-mail.

```html {linenos=false,hl_lines=["1"],linenostart=1}
<!-- Author: Juan Enciso (juan.enciso@gmail.com) -->
<html>
  <head>
  </head>
  <body>
    <h1>Hello, World!</h1>
  </body>
</html>
```

#### Mude o commit anterior

Nós não queremos criar outro commit apenas para o e-mail. Vamos mudar o commit anterior e adicionar o endereço de e-mail.

```
git add hello.html
git commit --amend -m "Add an author/email comment"
```

{{< asciinema key="git-basico29" rows="28" cols="110" preload="1" poster="npt:1:14" speed="1.5" >}}

#### Olhar o histórico

```
git hist
```

{{< asciinema key="git-basico30" rows="28" cols="110" preload="1" poster="npt:0:4" speed="1.5" >}}

O novo commit "author/email" substitui o commit original "author". O mesmo pode ser obtido usando o comando `reset` no branch e fazendo novamente o commit com as mudanças.


## 20. Movendo arquivos

#### Mova o arquivo hello.html para a pasta lib.

Agora criaremos a estrutura do nosso repositório. Vamos mover a página no diretório lib

```
mkdir lib
git mv hello.html lib
git status
```

Movendo arquivos com git, nós notificamos o git sobre duas coisas

1. O arquivo hello.html foi deletado.
2. O arquivo lib/hello.html foi criado.

Ambos os fatos vão para stage imediatamente e ficam prontos para o commit. O comando git status reporta que o arquivo foi movido.

#### Mais um jeito de mover arquivos

Um fato positivo sobre o git é que você não precisa se lembrar de controle de versão no momento em que você faz o commit do código. O que poderia acontecer se nós estivéssemos usando a linha de comando do sistema operacional ao invés do comando git para mover arquivos?

O próximo set de comandos é idêntico às nossas últimas ações. É necessário mais trabalho para o mesmo resultado.

Nós podemos fazer:

```
mkdir lib
mv hello.html lib
git add lib/hello.html
git rm hello.html
```

#### Faça commit do novo diretório

Vamos fazer commit dessa mudança.

```
git commit -m "Moved hello.html to lib"
```

{{< asciinema key="git-basico31" rows="28" cols="110" preload="1" poster="npt:0:17" speed="1.5" >}}

## 21. Mais informação sobre a estrutura

#### Adicionando index.html

Vamos adicionar um arquivo `index.html` ao nosso repositório. O arquivo a seguir é perfeito para esse propósito.

```html
<html>
  <body>
    <iframe src="lib/hello.html" width="200" height="200" />
  </body>
</html>
```

Adicione o arquivo e faça um commit.

```
git add index.html
git commit -m "Added index.html."
```

Agora quando você abrir `index.html`, você deverá ver uma parte da página hello em uma pequena janela.

{{< asciinema key="git-basico32" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

```
firefox index.html
```

![](https://i.imgur.com/FHxL1jk.png)

## 22. Dentro do Git: diretório .git

#### O diretório .git

Essa é uma pasta especial onde todas as coisas do git estão. Vamos explorar o diretório.

{{< asciinema key="git-basico33" rows="28" cols="110" preload="1" poster="npt:0:17" speed="1.5" >}}

#### Banco de Dados de Objetos

```shell
ls -C .git/objects/<dir>
```

Você deve ver várias pastas nomeadas com dois caracteres. As duas primeiras letras do hash sha1 do objeto armazenado no git são o nome dos seus diretórios.

Vamos dar uma olhada em uma das pastas nomeadas com dois caracteres. Devem ter arquivos com nomes de 38 caracteres. Esses arquivos contém os objetos armazenados no git. Eles são comprimidos e encriptados, então é impossível ver seu conteúdo diretamente. Vamos dar uma olhada melhor no diretório Git.

{{< asciinema key="git-basico34" rows="28" cols="110" preload="1" poster="npt:0:17" speed="1.5" >}}

#### Arquivo Config

```
cat .git/config
```

{{< asciinema key="git-basico35" rows="28" cols="110" preload="1" poster="npt:0:17" speed="1.5" >}}

#### Branches e tags

```
ls .git/refs
ls .git/refs/heads
ls .git/refs/tags
cat .git/refs/tags/v1
```

{{< asciinema key="git-basico36" rows="28" cols="110" preload="1" poster="npt:0:23" speed="1.5" >}}

Arquivos no subdiretório de tags devem ser familiares pra você. Cada arquivo corresponde a tag anteriormente criada usando o comando git tag. Seu conteúdo não é nada mais que um hash de um commit associado à tag.

A pasta heads é quase idêntica e é usada não para tags, mas para branches. No momento, nós só temos um branch e tudo que você vê nessa pasta é um branch master.

#### Arquivo HEAD

```
cat .git/HEAD
```

{{< asciinema key="git-basico37" rows="28" cols="110" preload="1" poster="npt:0:23" speed="1.5" >}}

Existe uma referência para o branch atual no arquivo HEAD. Nesse momento, ela tem que ser para o branch master.

## 23. Dentro do Git: Trabalhando diretamente com objetos do git

#### Procurando pelo último commit

```
git hist --max-count=1
```

{{< asciinema key="git-basico38" rows="28" cols="110" preload="1" poster="npt:0:23" speed="1.5" >}}

#### Exibição do último commit

Com a hash SHA1, tal como acima...

```
git cat-file -t <hash>
git cat-file -p <hash>
```

{{< asciinema key="git-basico39" rows="28" cols="110" preload="1" poster="npt:0:26" speed="1.5" >}}

#### Busca em árvore

Nós podemos exibir a árvore referenciada no commit. Isso deveria ser uma descrição do arquivo no nosso projeto (para um commit específico). Use a hash SHA1 da string da árvore listada acima.

```
git cat-file -p <treehash>
```

{{< asciinema key="git-basico40" rows="28" cols="110" preload="1" poster="npt:0:23" speed="1.5" >}}

#### Exibir diretório da lib e Exibir o arquivo hello.html

```
git cat-file -p <libhash>
git cat-file -p <hellohash>
```

{{< asciinema key="git-basico41" rows="28" cols="110" preload="1" poster="npt:0:29" speed="1.5" >}}

E aí está. Objetos árvores, objetos de commits e objetos blob são exibidos diretamente do repositório do git. E isso é tudo que tem - árvores, blobs e commits.


#### Explore você mesmo

O repositório git pode ser explorado manualmente. Tente achar manualmente o arquivo hello.html original do primeiro commit com ajuda da hash `SHA1` referenciada no último commit.

