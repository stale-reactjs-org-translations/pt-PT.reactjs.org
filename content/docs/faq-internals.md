---
id: faq-internals
title: Virtual DOM e Objetos Internos
permalink: docs/faq-internals.html
layout: docs
category: FAQ
---

### O que é o Virtual DOM? {#what-is-the-virtual-dom}

O virtual DOM (VDOM) é um conceito de programação onde uma representação ideal, ou "virtual", da interface do usuário é mantida em memória e sincronizada com o DOM "real" por uma biblioteca como o ReactDOM. Esse processo é chamado de [reconciliação](/docs/reconciliation.html).

Esta abordagem permite a API declarativa do React: dizes ao React em que estado queres que a interface do usuário esteja, e ele garante que o DOM seja igual à esse estado. Isto abstrai a manipulação de atributos, manipulação de eventos e atualização manual do DOM que, caso ao contrário, terias que usar para construir a tua aplicação.

Since "virtual DOM" is more of a pattern than a specific technology, people sometimes say it to mean different things. In React world, the term "virtual DOM" is usually associated with [React elements](/docs/rendering-elements.html) since they are the objects representing the user interface. React, however, also uses internal objects called "fibers" to hold additional information about the component tree. They may also be considered a part of "virtual DOM" implementation in React.

### Is the Shadow DOM the same as the Virtual DOM? {#is-the-shadow-dom-the-same-as-the-virtual-dom}

No, they are different. The Shadow DOM is a browser technology designed primarily for scoping variables and CSS in web components. The virtual DOM is a concept implemented by libraries in JavaScript on top of browser APIs.

### What is "React Fiber"? {#what-is-react-fiber}

Fiber is the new reconciliation engine in React 16. Its main goal is to enable incremental rendering of the virtual DOM. [Read more](https://github.com/acdlite/react-fiber-architecture).
