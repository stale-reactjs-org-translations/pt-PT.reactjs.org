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

Dado que "virtual DOM" é mais um padrão do que uma tecnologia específica, as pessoas às vezes o citam querendo dizer coisas diferentes. No mundo do React, o termo "virtual DOM" é geralmente associado aos [Elementos do React](/docs/rendering-elements.html) uma vez que eles são objetos que representam a interface do usuário. O React, contudo, também usa objetos internos chamados "fibers" para guardar informações adicionais sobre a árvore de componentes. Eles também podem ser considerados parte da implementação do "virtual DOM" no React.

### Is the Shadow DOM the same as the Virtual DOM? {#is-the-shadow-dom-the-same-as-the-virtual-dom}

No, they are different. The Shadow DOM is a browser technology designed primarily for scoping variables and CSS in web components. The virtual DOM is a concept implemented by libraries in JavaScript on top of browser APIs.

### What is "React Fiber"? {#what-is-react-fiber}

Fiber is the new reconciliation engine in React 16. Its main goal is to enable incremental rendering of the virtual DOM. [Read more](https://github.com/acdlite/react-fiber-architecture).
