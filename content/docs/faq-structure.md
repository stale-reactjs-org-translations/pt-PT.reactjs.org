---
id: faq-structure
title: Estrutura de Ficheiros
permalink: docs/faq-structure.html
layout: docs
category: FAQ
---

### Existe uma maneira recomendada para estruturar os projetos em React? {#is-there-a-recommended-way-to-structure-react-projects}

O React não opina sobre como deves estruturar o projeto. No entanto, existem algumas abordagens populares que podes experimentar.

#### Agrupar por funcionalidades ou rotas {#grouping-by-features-or-routes}

Uma maneira bem comum para estruturar os projetos é colocar CSS, JS e testes juntos dentro de pastas agrupadas por funcionalidades ou rotas, por exemplo:

```
common/
  Avatar.js
  Avatar.css
  APIUtils.js
  APIUtils.test.js
feed/
  index.js
  Feed.js
  Feed.css
  FeedStory.js
  FeedStory.test.js
  FeedAPI.js
profile/
  index.js
  Profile.js
  ProfileHeader.js
  ProfileHeader.css
  ProfileAPI.js
```

A definição de "funcionalidade" não é universal e cabe a ti decidir. Se não conseguires criar uma lista de pastas de alto nível, podes perguntar para os utilizadores do teu produto quais são as partes principais que ele contém e usar o modelo mental como estrutura.

#### Agrupar por tipo de arquivo {#grouping-by-file-type}

Outra maneira popular de estruturar projetos é agrupar arquivos semelhantes por tipo, por exemplo:

```
api/
  APIUtils.js
  APIUtils.test.js
  ProfileAPI.js
  UserAPI.js
components/
  Avatar.js
  Avatar.css
  Feed.js
  Feed.css
  FeedStory.js
  FeedStory.test.js
  Profile.js
  ProfileHeader.js
  ProfileHeader.css
```

Algumas pessoas preferem ir mais além, e separar os componentes em pastas diferentes, dependendo do papel que desempenham na aplicação. Por exemplo o [Atomic Design](http://bradfrost.com/blog/post/atomic-web-design/) que é uma metodologia de design construída sobre este princípio. Lembra-te de que é mais produtivo tratar essas metodologias como exemplos úteis, ao invés de regras estritas que devem ser seguidas.

#### Avoid too much nesting {#avoid-too-much-nesting}

There are many pain points associated with deep directory nesting in JavaScript projects. It becomes harder to write relative imports between them, or to update those imports when the files are moved. Unless you have a very compelling reason to use a deep folder structure, consider limiting yourself to a maximum of three or four nested folders within a single project. Of course, this is only a recommendation, and it may not be relevant to your project.

#### Don't overthink it {#dont-overthink-it}

If you're just starting a project, [don't spend more than five minutes](https://en.wikipedia.org/wiki/Analysis_paralysis) on choosing a file structure. Pick any of the above approaches (or come up with your own) and start writing code! You'll likely want to rethink it anyway after you've written some real code.

If you feel completely stuck, start by keeping all files in a single folder. Eventually it will grow large enough that you will want to separate some files from the rest. By that time you'll have enough knowledge to tell which files you edit together most often. In general, it is a good idea to keep files that often change together close to each other. This principle is called "colocation".

As projects grow larger, they often use a mix of both of the above approaches in practice. So choosing the "right" one in the beginning isn't very important.
