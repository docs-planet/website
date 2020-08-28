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
> a opcão -d, permite configurar o git flow com os valores default.

{{< asciinema key="tmp6vglj6iw-ascii" rows="28" cols="110" preload="1" poster="npt:0:21" speed="1.5" >}}
