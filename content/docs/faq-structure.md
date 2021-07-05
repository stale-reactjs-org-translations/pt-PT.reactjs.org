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

<<<<<<< HEAD
Uma maneira bem comum para estruturar os projetos é colocar CSS, JS e testes juntos dentro de pastas agrupadas por funcionalidades ou rotas, por exemplo:
=======
One common way to structure projects is to locate CSS, JS, and tests together inside folders grouped by feature or route.
>>>>>>> 0bb0303fb704147452a568472e968993f0729c28

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

#### Agrupar por tipo de ficheiro {#grouping-by-file-type}

Outra maneira popular de estruturar projetos é agrupar ficheiros semelhantes por tipo, por exemplo:

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

#### Evite muitas ramificações {#avoid-too-much-nesting}

Existem vários problemas associados à ramificações de pastas em projetos JavaScript. Torna-se mais difícil escrever importações relativas entre elas ou atualizá-las quando os ficheiros são movidos. A menos que tenhas um motivo muito convincente para usar uma estrutura de pastas ramificadas, considera limitar-te a um máximo de três ou quatro pastas ramificadas em um único projeto. Claro, isso é apenas uma recomendação e pode não ser relevante para o seu projeto.

#### Não pense muito {#dont-overthink-it}

Se estás apenas a começar um projeto, [não perca mais do que cinco minutos](https://en.wikipedia.org/wiki/Analysis_paralysis) na escolha de uma estrutura de ficheiros. Escolhe qualquer uma das abordagens acima (ou crie as tuas próprias) e começa a escrever o código! Provavelmente sempre vais querer reestruturá-lo depois de teres escrito algum código.

Se sentires-te completamente preso, começa por manter todos os ficheiros em uma única pasta. Eventualmente ela crescerá o suficiente para que desejes separar alguns ficheiros dos demais. A essa altura, terás conhecimento suficiente para saber quais ficheiros são modificados juntos com mais frequência. Em geral é uma boa ideia manter os ficheiros que geralmente são alterados juntos uns dos outros. Este princípio é chamado de "colocation".

À medida que os projetos vão crescendo, eles costumam usar uma mistura de ambas as abordagens acima na prática. Então escolher a abordagem "certa" no começo não é muito importante.
