---
id: react-dom-server
title: ReactDOMServer
layout: docs
category: Reference
permalink: docs/react-dom-server.html
---

O objecto `ReactDOMServer` permite que renderizes os teus componentes em _markup_ estático. Geralmente, é usado em um servidor Node:

```js
// Módulos ES
import ReactDOMServer from 'react-dom/server';
// CommonJS
var ReactDOMServer = require('react-dom/server');
```

## Visão Geral {#overview}

Os métodos a seguir podem ser usados tanto em ambiente de servidor como de navegador:

- [`renderToString()`](#rendertostring)
- [`renderToStaticMarkup()`](#rendertostaticmarkup)

Estes métodos adicionais dependem do pacote (`stream`) que **só está disponível no servidor** e não irão funcionar no navegador.

- [`renderToNodeStream()`](#rendertonodestream)
- [`renderToStaticNodeStream()`](#rendertostaticnodestream)

* * *

## Referência {#reference}

### `renderToString()` {#rendertostring}

```javascript
ReactDOMServer.renderToString(element)
```

Renderiza um elemento React para o seu HTML inicial. O React retornará uma string HTML. Podes usar este método para gerar HTML no servidor e enviar o _markup_ no request inicial para ter carregamentos de páginas mais rápidos e para permitir que motores de pesquisa rastreiem as tuas páginas para fins de _SEO_.

Se invocares [`ReactDOM.hydrate()`](/docs/react-dom.html#hydrate) em um nó que já tem o seu _markup_ processado pelo servidor, o React vai preservá-lo e apenas atribuir manipuladores de eventos (_event handlers_), permitindo que tenhas uma experiência no primeiro carregamento muito eficiente.

* * *

### `renderToStaticMarkup()` {#rendertostaticmarkup}

```javascript
ReactDOMServer.renderToStaticMarkup(element)
```

Semelhante à [`renderToString`](#rendertostring), excepto que este não cria atributos DOM adicionais que o React usa internamente, como `data-reactroot`. Isto é útil se quiseres usar o React como um simples gerador de páginas estáticas, já que remover os atributos adicionais pode economizar alguns bytes.

Se tens planos de usar o React no cliente para tornar o _markup_ interactivo, não use este método. Em vez disso, use [`renderToString`](#rendertostring) no servidor e [`ReactDOM.hydrate()`](/docs/react-dom.html#hydrate) no cliente.

* * *

### `renderToNodeStream()` {#rendertonodestream}

```javascript
ReactDOMServer.renderToNodeStream(element)
```

Renderiza um elemento React para o teu HTML inicial. Retorna um [_Readable Stream_](https://nodejs.org/api/stream.html#stream_readable_streams) que gera uma string HTML. A saída HTML deste _stream_ é exactamente igual à que [`ReactDOMServer.renderToString`](#rendertostring) retornaria. Podes usar este método para gerar HTML no servidor e enviar o _markup_ no _request_ inicial para ter carregamentos de página mais rápidos e para permitir que motores de busca rastreiem as tuas páginas para fins de _SEO_.

Se invocares [`ReactDOM.hydrate()`](/docs/react-dom.html#hydrate) em um nó que já tem o seu _markup_ processado pelo servidor, o React vai preservá-lo e apenas atribuir manipuladores de eventos (_event handlers_), permitindo que tenhas uma experiência no primeiro carregamento muito eficiente.

> Nota:
>
> Apenas para servidor. Esta API não está disponível no navegador.
>
> O _stream_ retornado por este método retornará um _stream_ de bytes (_byte stream_) codificado em utf-8. Se precisares de um _stream_ em outra codificação, veja projectos como o [iconv-lite](https://www.npmjs.com/package/iconv-lite), que fornecem _streams_ de transformação para transcodificação de texto.

* * *

### `renderToStaticNodeStream()` {#rendertostaticnodestream}

```javascript
ReactDOMServer.renderToStaticNodeStream(element)
```

Semelhante à [`renderToNodeStream`](#rendertonodestream), excepto que este não cria atributos DOM adicionais que o React usa internamente, como `data-reactroot`. Isto é útil se quiseres usar o React como um simples gerador de páginas estáticas, já que remover os atributos adicionais pode economizar alguns bytes.

A saída HTML deste _stream_ é exactamente igual à que [`ReactDOMServer.renderToStaticMarkup`](#rendertostaticmarkup) retornaria.

Se tens planos de usar o React no cliente para tornar o _markup_ interactivo, não use este método. Em vez disso, use [`renderToNodeStream`](#rendertonodestream) no servidor e [`ReactDOM.hydrate()`](/docs/react-dom.html#hydrate) no cliente.

> Nota:
>
> Apenas para servidor. Esta API não está disponível no navegador.
>
> O _stream_ retornado por este método retornará um _stream_ de bytes (_byte stream_) codificado em utf-8. Se precisares de um _stream_ em outra codificação, veja projectos como o [iconv-lite](https://www.npmjs.com/package/iconv-lite), que fornecem _streams_ de transformação para transcodificação de texto.
