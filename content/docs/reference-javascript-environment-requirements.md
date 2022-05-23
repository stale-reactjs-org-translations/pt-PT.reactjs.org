---
id: javascript-environment-requirements
title: Requisitos do Ambiente JavaScript
layout: docs
category: Reference
permalink: docs/javascript-environment-requirements.html
---

<<<<<<< HEAD
O React 16 depende dos tipos de colecção [Map](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Map) e [Set](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Set). Para suportar navegadores antigos e dispositivos que ainda não tenham estes tipos de forma nativa (ex: IE < 11) ou tenham uma implementação incompatível (ex. IE 11), considera a inclusão de um _polyfill_ global no _bundle_ da tua aplicação, tal como o pacote [core-js](https://github.com/zloirock/core-js) ou [babel-polyfill](https://babeljs.io/docs/usage/polyfill/).

Um ambiente com _polyfill_ para o React 16, que usa core-js para dar suporte a navegadores antigos é mais ou menos assim:
=======
React 18 supports all modern browsers (Edge, Firefox, Chrome, Safari, etc).

If you support older browsers and devices such as Internet Explorer which do not provide modern browser features natively or have non-compliant implementations, consider including a global polyfill in your bundled application.
>>>>>>> 3aac8c59848046fb427aab4373a7aadd7069a24c

Here is a list of the modern features React 18 uses:
- [`Promise`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [`Symbol`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol)
- [`Object.assign`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)

<<<<<<< HEAD
import React from 'react';
import ReactDOM from 'react-dom';

ReactDOM.render(
  <h1>Olá, mundo!</h1>,
  document.getElementById('root')
);
```

O React também depende da função `requestAnimationFrame` (mesmo em ambiente de testes).  
Podes usar o pacote [raf](https://www.npmjs.com/package/raf)  como _shim_ para a função `requestAnimationFrame`:

```js
import 'raf/polyfill';
```
=======
The correct polyfill for these features depend on your environment. For many users, you can configure your [Browserlist](https://github.com/browserslist/browserslist) settings. For others, you may need to import polyfills like [`core-js`](https://github.com/zloirock/core-js) directly.
>>>>>>> 3aac8c59848046fb427aab4373a7aadd7069a24c
