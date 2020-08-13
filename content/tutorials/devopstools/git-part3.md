---
title: "Git (Parte 3)"
menuTitle: "Git (Parte 3)"
description: "Tutorial de GIT"
weight: 3
date: "2020-08-11"
LastModifierDisplayName: Juan Enciso
LastModifierEmail: juan.enciso@gmail.com
asciinema: true
tags:
  - git
---

{{% notice note %}}
Este tutorial é a continuação do tutorial [Git (Part 2)](../git-part2)
{{% /notice %}}


## 24. Criando um Branch

Vamos nomear o nosso novo branch como «style».

```shell
git checkout -b style
git status
```
Nota:

* `git checkout -b <branch name>` é o atalho de `git branch <branch name>` seguido por `git checkout <branch name>`
* Note que o comando `git status` avisa que você está no branch style.

#### Adicione o arquivo style.css

```
touch lib/style.css
```

Arquivo `lib/style.css`

```css
h1 {
  color: red;
}
```

Execute:

```shell
git add lib/style.css
git commit -m "Added css stylesheet"
```

{{< asciinema key="git-basico42" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

#### Mude a página principal

Atualize o arquivo `lib/hello.html` para usar o `style.css`.

```html {linenos=false,hl_lines=["4"],linenostart=1}
<!-- Author: Juan Enciso (juan.enciso@gmail.com) -->
<html>
  <head>
    <link type="text/css" rel="stylesheet" media="all" href="style.css" />
  </head>
  <body>
    <h1>Hello, World!</h1>
  </body>
</html>
```

```
git add lib/hello.html
git commit -m "Hello uses style.css"
```

{{< asciinema key="git-basico43" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

#### Mude o index.html

Atualice o arquivo `index.html` para que ele use `style.css`

```html {linenos=false,hl_lines=["2-4"],linenostart=1}
<html>
  <head>
    <link type="text/css" rel="stylesheet" media="all" href="lib/style.css" />
  </head>
  <body>
    <iframe src="lib/hello.html" width="200" height="200" />
  </body>
</html>
```

```
git add index.html
git commit -m "Updated index.html"
```

{{< asciinema key="git-basico44" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}


## 25. Navegando em Branches

Agora o seu projeto possui dois branches:

```
git hist --all
```

{{< asciinema key="git-basico45" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

#### Trocando para o branch master

```
git checkout master
cat lib/hello.html
```

{{< asciinema key="git-basico46" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

#### Vamos retornar para o branch do style.

```
git checkout style
cat lib/hello.html
```

{{< asciinema key="git-basico47" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

## 26. Mudanças no branch master

Enquanto você está mudando o branch style, alguém decide mexer na branch master. Ele adicionou um arquivo `README.md`.

Crie um arquivo `README.md`

```md
## Tutorial

This is the Hello World example from the git tutorial.
```

Faça um commit das mudanças do arquivo README no branch master.

```
git checkout master
git add README.md
git commit -m "Added README"
```

{{< asciinema key="git-basico48" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}


## 27. Visualizando os diferentes branches

Agora nós temos um repositório com dois branches diferentes. Para ver branches e suas diferenças, use o comando log como segue.

```
git hist --all
```

{{< asciinema key="git-basico49" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}


Nós temos a oportunidade de ver o `--graph` do git hist em ação. Adicionando a opção `--graph` ao git log faz com que ele crie uma árvore de commits com a ajuda de caracteres ASCII simples. Nós vemos ambos os branches (style e master) e que o branch atual é o master HEAD. O arquivo `index.html` adicionado vai antes de ambos branches.

A flag --all garante que nós vejamos todos os branches. Por padrão, apenas o branch atual é mostrado.


## 28. Merging

#### Merging em um único branch

Merge junta as modificações de dois branches em um. Vamos voltar para o branch style e fazer um merge dele com o master.

```
git checkout style
git merge master
git hist --all
```

{{< asciinema key="git-basico50" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

Pelo merge periodico entre os branches master e style, você pode acompanhar quaisquer mudanças ou modificações ocorridas para manter a compatibilidade das mudanças de estilo na linha principal.

Porém, isso faz com que os gráficos de commits fiquem feios. Mais tarde vamos considerar a relocação como uma alternativa à fusão.

## 29. Criando um conflito

#### Voltar para o master e criar o conflito

Volte para o branch master e faça as seguintes alterações:

```
git checkout master
```

Arquivo `lib/hello.html`

```html {linenos=false,hl_lines=["4","7"],linenostart=1}
<!-- Author: Juan Enciso (juan.enciso@gmail.com) -->
<html>
  <head>
    <!-- no style -->
  </head>
  <body>
    <h1>Hello, World! Life is great!</h1>
  </body>
</html>
```

```
git add lib/hello.html
git commit -m 'Life is great!'
```

Visualize os branches

```
git hist --all
```

{{< asciinema key="git-basico51" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

Depois de um commit "Added README" o branch master foi feito um merge com o branch style, mas existe um commit aditional do master, que não foi feito um merge com o branch style.

A última modificação feita no master entra em conflito com algumas mudanças do style. No próximo passo nós vamos resolver esse conflito.

## 30. Resolvendo conflitos

#### Fazer merge do branch master com o style

```
git checkout style
git merge master
```

{{< asciinema key="git-basico52" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

A primeira seção e a versão do branch atual (style) head. A segunda seção é a versão do branch master.

#### Resolução do conflito

Você precisa resolver o conflito manualmente. Faça mudanças no `lib/hello.html` para alcançar o seguinte resultado.

```html
<!-- Author: Juan Enciso (juan.enciso@gmail.com) -->
<html>
  <head>
    <link type="text/css" rel="stylesheet" media="all" href="style.css" />
  </head>
  <body>
    <h1>Hello, World! Life is great!</h1>
  </body>
</html>
```

{{< asciinema key="git-basico53" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

#### Merging avançado

Git não tem ferramentas gráficas de merging, mas aceita qualquer ferramenta de merge produzida por terceiros. (leia mais sobre essas [ferramentas no StackOverflow](https://stackoverflow.com/questions/137102/whats-the-best-visual-merge-tool-for-git).)

## 31. Realocating como uma alternativa à merge

Vamos olhar para as diferenças entre realocating e merge. Para fazer isso, precisamos voltar no repositório no momento antes do primeiro merge e então repetir os mesmos passos realocando ao invés de fazer merge.

Nós usaremos o comando reset para voltar o branch para um estado anterior.


## 32. Resetando o branch style

Vamos para o branch style no ponto antes de darmos merge com o branch master. Podemos resetar o branch para qualquer commit. Na verdade, isso faz com que o ponteiro do branch aponte para qualquer commit contido na árvore.

Aqui, queremos voltar no style branch para um ponto anterior ao merge com o master. Temos que encontrar o último commit antes do merge


```
git checkout style
git hist
```

{{< asciinema key="git-basico54" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

É um pouco difícil de ler, mas podemos perceber pelos dados que o commit Updated index.html foi o último no branch style anterior ao merging. Vamos resetar o style branch para esse commit.

```
git reset --hard <hash>
git hist --all
```

{{< asciinema key="git-basico55" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

Agora no branch style não existem commits de merge no nosso histórico.

## 33. Reset do branch master

#### Resetando branch master

O modo interativo que adicionamos ao branch master se tornou uma mudança que conflita com as mudanças do branch style. Vamos reverter as mudanças do branch master para o ponto anterior à mudança conflitante. Isso nos permite demonstrar o comando rebase sem nos preocupar com conflitos.

```
git checkout master
git hist
```

{{< asciinema key="git-basico56" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

O commit "Added README" está imediatamente antes do modo interativo conflitante ser adicionado. Agora precisamos resetar o branch master para o commit "Added README" .

```
git reset --hard <hash>
git hist --all
```

Examine o log. Ele deve parecer como se tivéssemos retrocedido o repositório para um ponto no tempo anterior a qualquer merge.

{{< asciinema key="git-basico57" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

## 34. Rebase

Então nós voltamos no histórico até antes do primeiro merge e queremos realocar as mudanças do master para o nosso branch style.

Dessa vez nós vamos usar o comando rebase ao invés do merge.

```
git checkout style
git rebase master
git hist
```
{{< asciinema key="git-basico58" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

#### Merge VS Rebase

O resultado do comando rebase parece muito com o do merge. O branch style atualmente contém todas as suas mudanças, além das mudanças do branch master. A árvore de commits, porém, está um pouco diferente. A árvore de commit do branch style foi reescrita para fazer o branch master parte do histórico de commits. Isso faz com que a cadeia de commits seja mais linear e legível.

#### Quando usar rebase, quando usar merge?

Não use o comando rebase...

1. Se o branch é público e compartilhado. Reescrever tais branches vai atrapalhar o trabalho de outros colegas.
2. Quando o histórico exato de commits do branch é importante (porque o comando rebase reescreve o histórico de commits).

Dadas as recomendações acima, eu prefiro usar rebase para branches locais e de curto prazo e merge para branches em repositórios públicos.

## 35. Merging com o branch master

Nós mantivemos nosso branch style atualizado em relação ao branch master (usando rebase), mas agora vamos fazer merge das modificações de volta no master.

```
git checkout master
git merge style
```

Já que o último commit do master é anterior ao o último commit do branch style, o git consegue fundir em modo de avanço rápido - simplesmente movendo o ponteiro do branch para frente, apontando para o mesmo commit que o branch style.

Conflitos não surgem no fast-forward merge.

{{< asciinema key="git-basico59" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

#### Confira os logs

```
git hist
```

{{< asciinema key="git-basico60" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

Agora o style e o master são idênticos. Verifique:

```
git hist --all
```

## 36. Repositórios múltiplos

Até agora só trabalhamos com um repositório git. Apesar disso, git é ótimo para trabalhar com vários repositórios. Os repositórios adicionais podem ser armazenados localmente ou acessados por conexão de rede.

Na próxima seção iremos criar um novo repositório chamado "cloned_hello". Nós iremos discutir como mover mudanças de um repositório para o outro, lidando com conflitos que possam surgir.

![](https://i.imgur.com/zuM9b2c.png)

NOTA: Nós faremos mudanças em ambas as cópias do nosso repositório. Preste atenção no repositório em que você está em cada estágio das próximas lições.

## 37. Clonando repositórios

Se você está trabalhando em grupo, é importante que você entenda os próximos 12 capítulos, porque você geralmente terá que trabalhar com repositórios clonados.

```
cd ..
pwd
git clone hello cloned_hello
ls -ld hello cloned_hello
```

{{< asciinema key="git-basico61" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}


## 38. Examine o repositório clonado

Visualizando o histórico do repositório

```
cd cloned_hello
ls
git hist --all
```

{{< asciinema key="git-basico62" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

Você verá uma lista de todos os commits no novo repositório, que deveriam ser iguais aos do repositório original. A única diferença deveria ser o nome dos branches.


#### Branches remotos

Você verá um branch master (HEAD) no histórico. Você também verá branches com nomes estranhos (origin/master, origin/style e origin/HEAD). Nós falaremos deles depois.

## 39. O que é origin?

```
git remote
git remote show origin
```

{{< asciinema key="git-basico63" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

Nós podemos ver que o “origin” do repositório remoto é o repositório **hello** original. Repositórios remotos são tipicamente guardados em uma máquina separada ou em um servidor centralizado. Porém, como podemos ver, eles também podem apontar para um repositório na mesma máquina. Não tem nada especial sobre o nome “origin”, mas existe uma convenção de usá-lo para o repositório primário central (se houver algum).

## 40. Branches remotos

Vamos dar uma olhada nos branches do nosso repositório clonado.

```
git branch
```

{{< asciinema key="git-basico64" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

Como podemos ver, apenas o branch master está listado. Onde está o branch style? git branch lista apenas os branches locais, por padrão.

#### Lista dos branches remotos

Para ver todos os branches, use o seguinte comando:

```
git branch -a
```

{{< asciinema key="git-basico65" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

O Git lista todos os commits do repositório original, mas os branches do repositório remoto não são tratados como os locais. Se nós precisamos do nosso próprio branch style, teremos que criá-lo. Em um minuto você verá como isso é feito.

## 41. Mudando o repositório original

#### Faça uma mudança no repositório original hello

```
cd ../hello
```

Faça as seguintes mudanças no arquivo README:

```md
## Tutorial

This is the Hello World example from the git tutorial.
(changed in original)
```

Agora adicione e faça commit dessas mudanças

```
git add README
git commit -m "Changed README in original repo"
```

{{< asciinema key="git-basico66" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

Agora o repositório original tem mudanças mais recentes que não estão incluídas na versão clonada. Em seguida, vamos receber essas mudanças no repositório clonado.

## 42. Trazendo modificações

```
cd ../cloned_hello
git fetch
git hist --all
```

{{< asciinema key="git-basico67" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

Neste momento, o repositório contém todos os commits do repositório original. Porém, eles não estão integrados com os branchs locais do repositório clonado.

Você vai ver o commit de nome “Changed README in original repo” no histórico. Perceba que o commit inclui “origin/master” e “origin/HEAD”.

Agora vamos dar uma olhada no commit “Updated index.html”. Você vai ver que o branch master local aponta para esse commit, não para o commit que acabamos de trazer.

Isso nos mostra que o comando “git fetch” vai trazer os novos commits do repositório remoto, mas não vai fundir eles com os branches locais.


#### Cheque o README

Nós podemos mostrar que o arquivo README clonado não foi modificado.

```
cat README.md
```

{{< asciinema key="git-basico68" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

## 43. Merging as modificações baixadas

Faça merge das modificações baixadas no branch master local e agora você deve ver as modificações no arquivo `README.md`

```
git merge origin/master
cat README.md
```

{{< asciinema key="git-basico69" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

## 44. Fazendo pull e merge de modificações

Não iremos passar por todo o processo de fazer e dar pull em uma mudança, mas queremos que vocês saibam que:

```
git pull
```

é, na verdade, equivalente a fazer os seguintes passos:

```
git fetch
git merge origin/master
```
## 45. Adicionando um branch de rastreamento

Branches que começam com remotes/origin pertencem ao repositório original. Perceba que, mesmo que você não tenha mais o branch styles, ele sabe que o branch está no repositório original.

#### Adicione um branch local que rastreia um branch remoto.

```
git branch --track style origin/style
git branch -a
git hist --max-count=2
```

{{< asciinema key="git-basico70" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

## 46. Repositórios bare

Repositórios bare (sem o diretório de trabalho) são tipicamente usados para compartilhamento.

#### Criando um repositório bare

```
cd ..
git clone --bare hello hello.git
ls -ld hello.git
cd hello.git/
ls -l
```

Tipicamente, repositórios terminados em `.git` são bare. Como você pode ver, não existe nenhum diretório de trabalho no repositório `hello.git`. Na verdade, ele não é nada mais que o diretório .git de um repositório que não é bare.

{{< asciinema key="git-basico71" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

## 47. Adicionando um repositório remoto

Vamos adicionar o repositório `hello.git` ao nosso repositório original.

```
cd ..
cd hello
git remote add shared ../hello.git
```

{{< asciinema key="git-basico72" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}


NOTA: Nós estamos agora no repositório hello.

## 48. Submetendo modificações

A partir de um repositório limpo, geralmente compartilhado em qualquer servidor de rede, precisamos enviar nossas modificações a outros repositórios. Comece criando uma modificação para ser enviada. Edite o arquivo `README.md` e faça um commit

```md
## Tutorial

This is the Hello World example from the git tutorial.
(Changed in original and pushed to shared)
```

```
git checkout master
git add README
git commit -m "Added shared comment to readme"
```

{{< asciinema key="git-basico73" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

Agora envie as modificações para o repositório compartilhado.

```
git push shared master
```

O repositório comum está recebendo nossas modificações enviadas. (Lembre-se, nós adicionamos ele como um repositório remoto na lição anterior).

{{< asciinema key="git-basico74" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

Nota: Tivemos que explicitamente especificar o branch master para submeter as mudanças. Isso pode ser configurado automaticamente, mas eu sempre esqueço o comando. Para fácil administração de seus branches remotos mude para «Git Remote Branch».

## 49. Removendo modificações comuns

Rapidamente mude para o repositório clonado e extraia as modificações recém enviadas ao repositório comum.

```
cd ../cloned_hello
```

Nota: Estamos agora no repositório cloned_hello.

Continue com ...

```
git remote add shared ../hello.git
git branch --track shared master
git pull shared master
cat README.md
```

{{< asciinema key="git-basico75" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

## 50. Divulgando o seu repositório

Existem maneiras diferentes de compartilhar um repositório git na rede. Essa é a mais rápida.

#### Execute git server

```
cd .. 
git daemon --verbose --export-all --base-path=.
```

{{< asciinema key="git-basico76" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

Agora, vá ao seu diretório de trabalho num terminal separado.

```
git clone git://localhost/hello.git network_hello
cd network_hello
ls
```
{{< asciinema key="git-basico77" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}} 

Confira se seu vizinho usa git daemon. Troquem seus endereços IP e depois confiram se vocês podem pegar as alterações dos repositórios um do outro.
