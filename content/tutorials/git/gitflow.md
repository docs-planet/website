---
title: "Gitflow"
description: "Tutorial de Gitflow"
asciinema: true
comments: true
tags:
  - git
  - gitflow
---

![](https://bugfender.com/wp-content/uploads/2019/11/Feature-image.jpg)

## Intro

Gitflow é um modelo de organização de branches criado por Vincent Driessen, Não é o único modelo de organização de branches, mas sem dúvida é um dos mais usados.

## Nomenclatura

Gitflow estabelece algumas regras de nomenclaturas para tipos de branches enquanto, ao mesmo tempo, define o que cada tipo de branch faz. Para referência, segue uma lista dos tipos de branches definidos pelo Git Flow e suas respectivas descrições:

* **Branch master** - É a branch que contém código em nível de produção, ou seja, o código mais maduro existente na sua aplicação. Todo o código novo produzido eventualmente é juntado com a branch master, em algum momento do desenvolvimento;

* **Branch develop** - É a branch que contém código em nível preparatório para o próximo deploy. Ou seja, quando features são terminadas, elas são juntadas com a branch develop, testadas (em conjunto, no caso de mais de uma feature), e somente depois as atualizações da branch develop passam por mais um processo para então ser juntadas com a branch master;

* **Branches feature/[*]** - São branches no qual são desenvolvidos recursos novos para o projeto em questão. Essas branches tem por convenção nome começando com feature/ (exemplo: feature/new-layout) e são criadas a partir da branch develop (pois um recurso pode depender diretamente de outro recurso em algumas situações), e, ao final, são juntadas com a branch develop;

* **Branches hotfix/[*]** - São branches no qual são realizadas correções de bugs críticos encontrados em ambiente de produção, e que por isso são criadas a partir da branch master, e são juntadas diretamente com a branch master e com a branch develop (pois os próximos deploys também devem receber correções de bugs críticos, certo?). Por convenção, essas branches tem o nome começando com hotfix/ e terminando com o próximo sub-número de versão (exemplo: hotfix/2.1.1), normalmente seguindo as regras de algum padrão de versionamento, como semversion.

* **Branches release/[*]** - São branches com um nível de confiança maior do que a branch develop, e que se encontram em nível de preparação para ser juntada com a branch master e com a branch develop (para caso tenha ocorrido alguma correção de bug na branch release/* em questão). Note que, nessas branches, bugs encontrados durante os testes das features que vão para produção podem ser corrigidos mais tranquilamente, antes de irem efetivamente para produção. Por convenção, essas branches tem o nome começando com release/ e terminando com o número da próxima versão do software (seguindo o exemplo do hotfix, dado acima, seria algo como release/2.2.0), normalmente seguindo as regras do versionamento semântico.

Um aspecto interessante do Git Flow é que, quando você mistura uma branch release/ ou hotfix/ com a branch master, ele automaticamente cria git tags correspondentes aos merge commits da mistura, facilitando o trabalho de, por exemplo, mudar para uma versão mais antiga, e organizando todo o trabalho.

## Tutorial básico de git flow

Para esse tutorial básico de git flow, vou assumir que você está com o repositório criado no [tutorial basico de git](../git-basico)

### 1. Instalacão

Gitflow é uma extensão ao Git, para instalar ele, simplesmente execute `sudo apt-get install git-flow`

{{< asciinema key="tmppv3s_6c4-ascii" rows="28" cols="110" preload="1" poster="npt:0:01" speed="1.5" >}}

### 2. Inicializando git-flow

Agora, na pasta do repositório criado no tutorial básico de git, execute os comandos para deixar o repositorio somente com a branch `master`

```shell
git branch -a
git branch -d nova-branch
```

Logo inicializamos o git flow:

```shell
git flow init -d
```
> A opcão -d, permite configurar o git flow com os nomes default

{{< asciinema key="tmp6vglj6iw-ascii" rows="28" cols="110" preload="1" poster="npt:0:21" speed="1.5" >}}

Note que, durante a execução do comando, o Git Flow criará a branch develop e fará git checkout automático para esta branch.

### 3. Criando uma feature branch

Agora, vamos criar uma nova feature branch? Para fazer isso, execute o comando:

```shell
git flow feature start recurso-milionario
```

Aquilo irá criar uma nova feature branch chamada **recurso-milionario**, com o nome **feature/recurso-milionario**. Antes vamos explorar quais sao as branch q foram criadas:

{{< asciinema key="tmp5cfxashy-ascii" rows="28" cols="110" preload="1" poster="npt:0:18" speed="1.5" >}}

### 4. Trabalhando na feature branch

Nesta nova branch criaremos um arquivo recurso.txt com o conteúdo "Este é o melhor recurso criado desde sempre!". Logo executaremos os comandos "git add recurso.txt" e "git commit -m 'Finished feature'" para adicionar e commitar o arquivo em questão na feature branch recurso-milionario

```shell
echo 'Este é o melhor recurso criado desde sempre!' > recurso.txt
git add recurso.txt
git commit -m 'Finished feature'
```

{{< asciinema key="tmp3znpy8_2-ascii" rows="28" cols="110" preload="1" poster="npt:0:18" speed="1.5" >}}

### 5. Finalizando os trabalhos na feature branch

Com o commit feito, podemos finalmente juntá-lo a branch develop. Para fazer isso, execute o comando 

```shell
git flow feature finish recurso-milionario
```

{{< asciinema key="tmpk97y4ewl-ascii" rows="28" cols="110" preload="1" poster="npt:0:18" speed="1.5" >}}

Como você pode ver, a branch feature/recurso-milionario foi correspondentemente integrada à branch develop e o git flow fez checkout automático para a branch develop, te mostrando todos os passos feitos.

### 6. Criando uma release branch

Agora que temos na branch develop os cambios que queriamos ter, vamos criar uma release branch para poder enfim publicar a atualização na branch master. Para fazer isso, execute o comando 

```shell
git flow release start 0.1.0
```

{{< asciinema key="tmphna20n5o-ascii" rows="28" cols="110" preload="1" poster="npt:0:18" speed="1.5" >}}

### 7. Alterando a release branch

Agora, com a release branch criada, vamos apenas fazer uma pequena alteração no arquivo recurso.txt, modificando a frase:

De: "Este é o melhor recurso criado desde sempre!"

Para: "Este talvez seja o melhor recurso criado desde sempre!"

Logo commitaremos a mudança

```shell
git add recurso.txt
git commit -m 'Little bug-fix in feature'
```

{{< asciinema key="tmpu3svigg1-ascii" rows="28" cols="110" preload="1" poster="npt:0:18" speed="1.5" >}}

Mudanças podem ser feitas antes da branch ser juntada com a branch master

### 8. Finalizando a release

Com a mudança registrada, vamos enfim juntar a release branch 0.1.0 com a branch master e a branch develop, para isso, vamos usar o comando

```shell
git flow release finish 0.1.0
```

Aquilo vai integrar a mudança feita à release branch 0.1.0 com as branches master e develop

{{< asciinema key="356547" rows="28" cols="110" preload="1" poster="npt:0:18" speed="1.5" >}}

Como você pode ver acima, o Git Flow abre o editor de texto três vezes:

* Uma para você editar o texto do merge commit relacionado ao merge entre a release branch 0.1.0 e a branch master

* Um para a descrição da tag 0.1.0, que será criada pelo Git Flow para facilitar mudanças de versão no software

* Uma para você editar o texto do merge commit relacionado ao merge entre a branch master e a branch develop

### 9. Criando um hotfix

Agora suponhamos que foi encontrado um bug hiper-critico na aplicação (coincidentemente, nesse exemplo, suponhamos que foi no mesmo recurso recém adicionado) e que ele é tão grave que está afetando o uso por todos os usuários da aplicação e por isso precisa ser corrigido com urgência máxima.

Para corrigir esse bug critico, vamos criar um hotfix usando o comando

```shell
git flow hotfix start 0.1.1
```

Aquilo criará uma hotfix branch chamada 0.1.1 que resolve um problema encontrado no release 0.1.0

{{< asciinema key="356548" rows="28" cols="110" preload="1" poster="npt:0:18" speed="1.5" >}}

### 10. Corrigindo na branch hotfix

Agora que temos a nossa hotfix branch 0.1.1 criada, vamos editar o arquivo recurso.txt com a correção que queremos aplicar, substituindo a frase:

"Este talvez seja o melhor recurso criado desde sempre!", Para:

"Este talvez não seja o melhor recurso criado desde sempre! Mas é um dos mais legais!"

no arquivo recurso.txt. Logo comitaremos este cambio:

{{< asciinema key="tmpjck4ihiu-ascii" rows="28" cols="110" preload="1" poster="npt:0:22" speed="1.5" >}}

### 11. Finalizando o hotfix

Com o "bug" corrigido e comittado, podemos agora finalizar nossa hotfix branch e com isso juntá-la à branch master e à branch develop. Para fazer isso, basta usar o comando

```shell
git flow hotfix finish 0.1.1
```
Aquilo fará todas essas tarefas por nós e ainda criará uma tag para marcar a correção

{{< asciinema key="tmpo5eyt2vi-ascii" rows="28" cols="110" preload="1" poster="npt:0:22" speed="1.5" >}}

Novamente, como no caso do git flow release finish, estudado acima, o git flow abre o editor três vezes: Uma para editar o merge commit para o merge com a branch master, outra para editar a descrição da tag que será criada pelo Git Flow, e outra para o merge commit para o merge da branch master com a branch develop.

## Conclusões

O Gitflow é um modelo de organização de branches é ideal para trabalhar com projetos em equipe, pois permite que cada membro da equipe trabalhe em cada feature branch com maestria e ainda resolva bugs importantes quando eles forem encontrados.

## Referências

- http://github.com/nvie/gitflow
