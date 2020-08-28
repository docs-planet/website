---
title: "Tutorial - Git Básico"
menuTitle: "Git Básico"
description: "Tutorial de GIT Básico"
weight: 1
date: "2020-08-27"
LastModifierDisplayName: Juan Enciso
LastModifierEmail: juan.enciso@gmail.com
asciinema: true
tags:
  - git
---

![](https://www.hostinger.com/tutorials/wp-content/uploads/sites/2/2017/03/git-basics-big-768x478.png)

## Intro

Nesse tutorial, vamos fazer operações básicas relacionadas ao Git. como initializar um repositório, adicionar arquivos, comittar as mudanças, depois modificar os arquivos, adicionar os arquivos modificados, comittar tudo de novo, e por fim algumas noções básicas relacionadas a branch e merges.

## Prerequisitos

* Ter o git instalado, senão rodar: `sudo apt-get install git`
* Instalar git-bash-prompt seguindo este [tutorial](/tutorials/devopstools/git-bash-prompt/)
* Ter uma estacao de trabalho Linux


## 1. Inicializando o repositorio

Crie uma pasta chamada `tutorial-git`. Entre nela e rode o comando `git init`

```shell
cd ~
mkdir projetos
cd projetos
mkdir tutorial-git
cd tutorial-git
git init
```

{{< asciinema key="git01" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

## 2. Primeiro commit

Agora, experimente criar um arquivo chamado `test.txt`, com uma única linha contendo  `Hello World`. Em seguida, execute o comando `git add test.txt` seguido de `git commit -m 'First commit'`.

Como esse é o seu primeiro commit e o Git não sabe nada sobre você, é necessário adicionar algumas informações (o Git emitirá um erro sempre que você tentar comittar sem essas informações devidamente registradas), como nome e e-mail, para que o Git saiba quem está fazendo o commit e poder registrar essa informação no repositório.

Para ajustar isso, execute o comando `git config --global user.name '[seu nome]'` para configurar o seu nome (substitua [seu nome] pelo seu nome nesse comando) e `git config --global user.email '[seu e-mail]'` para configurar o seu e-mail (substitua [seu e-mail] pelo seu e-mail nesse comando)

Após executar esses dois comandos, execute novamente o comando `git commit -m 'First commit'`. Agora sim, seu primeiro commit estará registrado!

{{< asciinema key="git02" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

Note que o Git registrará, junto ao commit, o arquivo recém criado. Se você quiser ver como está o log da branch atual, execute o comando `git log`.

{{< asciinema key="git03" rows="28" cols="110" preload="1" poster="npt:0:12" speed="1.5" >}}

## 3. Fazendo modificações

Em seguida, modifique o arquivo, adicionando uma nova linha com o conteúdo "How are you?". Novamente, execute o comando `git add test.txt`, mas agora seguido do comando `git commit -m 'Second commit'` (note que agora o Git não pede mais seu e-mail e nome), e com isso a modificação feita no arquivo será salva pelo git.

{{< asciinema key="git04" rows="28" cols="110" preload="1" poster="npt:0:41" speed="1.5" >}}

Agora, se você rodar o "git log" novamente, verá que há um novo registro na lista, correspondente ao segundo commit. Como você pode ver abaixo:

{{< asciinema key="git05" rows="28" cols="110" preload="1" poster="npt:0:07" speed="1.5" >}}

## 4. Navegando pela branch master

Verifique o conteúdo do arquivo `test.txt`. experimente agora rodar o comando `git checkout master^` (sim, com o acento circunflexo no final), agora, abra novamente o arquivo `test.txt` no editor. Você verá que o conteúdo original do primeiro commit foi restaurado.

{{< asciinema key="git06" rows="28" cols="110" preload="1" poster="npt:0:30" speed="1.5" >}}

Agora execute `git diff master` para verificar as diferenças. Após fazer esses testes, use `git checkout master` para restaurar a visualização para o segundo commit.

{{< asciinema key="git07" rows="28" cols="110" preload="1" poster="npt:0:24" speed="1.5" >}}

## 5. Criando uma branch

Agora, vamos experimentar criar uma branch. Para fazer isso, execute o comando `git branch nova-branch` e após isso execute o comando `git checkout nova-branch`. Para constatar a mudança de branches, execute o comando `git status`, que mostra um status geral do repositório

{{< asciinema key="git08" rows="28" cols="110" preload="1" poster="npt:0:18" speed="1.5" >}}

Nessa nova branch, chamada _nova-branch_, você ainda está com uma cópia dos commits da branch original _master_, e ambas as branchs são iguais, pois você ainda não fez nenhum commit na nova branch. Experimente alterar o arquivo, adicionando uma nova linha com o conteúdo "I'm fine" e execute os comandos `git add test.txt` e `git commit -m 'Third commit'`. Agora, rode o comando `git log` para ver os commits registrados na branch.

{{< asciinema key="git09" rows="28" cols="110" preload="1" poster="npt:0:44" speed="1.5" >}}

## 6. Mudando de branch

Agora, vamos experimentar executar o comando `git checkout master` para voltar a branch master e ver como estão as coisas por lá. Estando na branch master, execute novamente o comando `git log`, e constate que o commit "Third commit" não está presente no log (pois afinal esse commit está presente apenas na branch nova-branch.

{{< asciinema key="git10" rows="28" cols="110" preload="1" poster="npt:0:12" speed="1.5" >}}

Ainda na branch master, experimente criar um arquivo chamado `test2.txt` com o conteúdo "Hello World 2", adicioná-lo ao Git executando `git add test2.txt` e commite a mudança executando `git commit -m 'Fourth commit'`

{{< asciinema key="git11" rows="28" cols="110" preload="1" poster="npt:0:32" speed="1.5" >}}

Se você executar o comando "git log" agora, verá que estão cadastrados na branch os commits "First commit", "Second commit" e...."Fourth commit"

{{< asciinema key="git12" rows="28" cols="110" preload="1" poster="npt:0:32" speed="1.5" >}}

## 7. Fazendo um merge

Agora, vamos juntar o "Third Commit", que adiciona mais uma alteração no arquivo test.txt e que existe apenas na branch nova-branch - com a branch master, aplicando assim a alteração correspondente feita pelo test.txt e mantendo o arquivo test2.txt inalterado.

Para isso, precisamos fazer o "merge" das duas branches. Ainda na branch master, execute o comando `git merge nova-branch` (nesse procedimento, o git vai abrir o merge commit em um editor de texto para você poder alterá-lo, apenas salve o arquivo e feche o editor de texto para continuar) e logo em seguida execute o comando `git log`

> (note que, agora, você precisa apertar a tecla "q" para sair da visualização do comando git log)

{{< asciinema key="git13" rows="28" cols="110" preload="1" poster="npt:0:19" speed="1" >}}

Um aspecto interessante para você notar é que o arquivo test2.txt, criado pelo commit "Fourth Commit", foi mantido inalterado pelo merge, pois nenhum commit existente na branch nova-branch altera o arquivo. Além disso, se você observar bem a ordem dos commits no comando `git log`, verá que o "Third Commit" aparece antes do "Fourth Commit", pois o Git sabe a ordem em que os commits devem ser aplicados.

## 8. Visualizando as branch

Para terminar o tutorial, execute o comando `git log --graph`. Ele vai mostrar como o Git entende a ordem dos commits e inclusive uma visualização bem intuitiva feita em ASCII com a branch nova-branch.

{{< asciinema key="git14" rows="28" cols="110" preload="1" poster="npt:0:18" speed="1" >}}

## Resumo dos comandos praticados

* `git init` - Inicializa um repositório git, criando uma pasta oculta chamada ".git" com os arquivos usados internamente pelo Git;
* `git add [arquivo]` - Adiciona as modificações no arquivo (ou pasta) [arquivo] à "staged area" do Git, no qual os arquivos que vão ser comittados ficam registrados temporariamente. Note que se você fizer posterior modificações no arquivo, você terá que executar o comando novamente para adicionar as novas modificações à "staged area" do Git;
* `git commit -m "[mensagem]"` - Registra um commit com as modificações registradas na "staged area" e com a mensagem "[mensagem]", armazenando também informações como nome e e-mail do autor do commit a partir das configurações salvas anteriormente na base de configurações do Git;
* `git config --global user.name "[seu nome]"` - Salva o nome "[seu nome]" na base de configurações global do Git;
* `git config --global user.email "[seu e-mail]"` - Salva o e-mail "[seu e-mail]" na base de configurações global do Git;
* `git log` - Mostra os commits registrados na branch atual;
* `git checkout [alvo]` - Faz o Git mudar o ponteiro HEAD (ou seja, o que se refere ao estado atual do repositório, que você vê) para [alvo], que pode ser o nome de uma branch, o hash de um commit, ou pode ser algumas referências especiais, como:
   - master^ - Muda para o penúltimo commit da branch master;
   - [branch] - Muda para a branch [branch];
* `git diff [alvo]` - Faz o git mostrar as diferenças entre o commit do ponteiro HEAD (que você está vendo no momento) e [alvo], que pode ser uma branch, o hash de um commit, ou pode ser algumas referências especiais (como no caso do git checkout);
* `git branch [branch]` - Cria uma nova branch com o nome [branch], copiando todos os commits da branch no qual você está até o commit referido pelo ponteiro HEAD;
* `git merge [branch]` - "Junta" a branch atual no qual você está com a branch de nome [branch], criando um merge commit ao final do processo;
* `git log --graph` - Mostra os commits registrados na branch atual, mostrando ao lado esquerdo a conexão entre um commit e outro.

Referências:

- http://fjorgemota.com/git-sistema-de-controle-de-versoes-distribuido/
