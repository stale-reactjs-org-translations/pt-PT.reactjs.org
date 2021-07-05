---
id: javascript-environment-requirements
title: Requisitos do Ambiente JavaScript
layout: docs
category: Reference
permalink: docs/javascript-environment-requirements.html
---

<<<<<<< HEAD
O React 16 depende dos tipos de colecção [Map](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Map) e [Set](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Set). Para suportar navegadores antigos e dispositivos que ainda não tenham estes tipos de forma nativa (ex: IE < 11) ou tenham uma implementação incompatível (ex. IE 11), considera a inclusão de um _polyfill_ global no _bundle_ da tua aplicação, tal como o pacote [core-js](https://github.com/zloirock/core-js) ou [babel-polyfill](https://babeljs.io/docs/usage/polyfill/).
=======
React 16 depends on the collection types [Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map) and [Set](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set). If you support older browsers and devices which may not yet provide these natively (e.g. IE < 11) or which have non-compliant implementations (e.g. IE 11), consider including a global polyfill in your bundled application, such as [core-js](https://github.com/zloirock/core-js).
>>>>>>> 0bb0303fb704147452a568472e968993f0729c28

Um ambiente com _polyfill_ para o React 16, que usa core-js para dar suporte a navegadores antigos é mais ou menos assim:

```js
import 'core-js/es/map';
import 'core-js/es/set';

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
