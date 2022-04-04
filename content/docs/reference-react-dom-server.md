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

- [`renderToPipeableStream()`](#rendertopipeablestream)
- [`renderToReadableStream()`](#rendertoreadablestream)
- [`renderToNodeStream()`](#rendertonodestream) (Deprecated)
- [`renderToStaticNodeStream()`](#rendertostaticnodestream)

* * *

## Referência {#reference}

### `renderToString()` {#rendertostring}

```javascript
ReactDOMServer.renderToString(element)
```

Renderiza um elemento React para o seu HTML inicial. O React retornará uma string HTML. Podes usar este método para gerar HTML no servidor e enviar o _markup_ no request inicial para ter carregamentos de páginas mais rápidos e para permitir que motores de pesquisa indexem as tuas páginas para fins de _SEO_.

<<<<<<< HEAD
Se invocares o método [`ReactDOM.hydrate()`](/docs/react-dom.html#hydrate) em um nó (_node_) que já tem o seu _markup_ processado pelo servidor, o React vai preservá-lo e apenas atribuir manipuladores de eventos (_event handlers_), permitindo que tenhas uma experiência muito eficiente no primeiro carregamento.
=======
If you call [`ReactDOM.hydrateRoot()`](/docs/react-dom-client.html#hydrateroot) on a node that already has this server-rendered markup, React will preserve it and only attach event handlers, allowing you to have a very performant first-load experience.
>>>>>>> 707f22d25f5b343a2e5e063877f1fc97cb1f48a1

* * *

### `renderToStaticMarkup()` {#rendertostaticmarkup}

```javascript
ReactDOMServer.renderToStaticMarkup(element)
```

Semelhante à [`renderToString`](#rendertostring), excepto que este não cria atributos DOM adicionais que o React usa internamente, como `data-reactroot`. Isto é útil se quiseres usar o React como um simples gerador de páginas estáticas, já que remover os atributos adicionais pode economizar alguns bytes.

<<<<<<< HEAD
Se tens planos de usar o React no cliente para tornar o _markup_ interactivo, não uses este método. Em vez disso, usa [`renderToString`](#rendertostring) no servidor e [`ReactDOM.hydrate()`](/docs/react-dom.html#hydrate) no cliente.
=======
If you plan to use React on the client to make the markup interactive, do not use this method. Instead, use [`renderToString`](#rendertostring) on the server and [`ReactDOM.hydrateRoot()`](/docs/react-dom-client.html#hydrateroot) on the client.
>>>>>>> 707f22d25f5b343a2e5e063877f1fc97cb1f48a1

* * *

### `renderToPipeableStream()` {#rendertopipeablestream}

```javascript
ReactDOMServer.renderToPipeableStream(element, options)
```

Render a React element to its initial HTML. Returns a [Control object](https://github.com/facebook/react/blob/3f8990898309c61c817fbf663f5221d9a00d0eaa/packages/react-dom/src/server/ReactDOMFizzServerNode.js#L49-L54) that allows you to pipe the output or abort the request. Fully supports Suspense and streaming of HTML with "delayed" content blocks "popping in" later through javascript execution. [Read more](https://github.com/reactwg/react-18/discussions/37)

If you call [`ReactDOM.hydrateRoot()`](/docs/react-dom-client.html#hydrateroot) on a node that already has this server-rendered markup, React will preserve it and only attach event handlers, allowing you to have a very performant first-load experience.

> Note:
>
> This is a Node.js specific API and modern server environments should use renderToReadableStream instead.
>

```
const {pipe, abort} = renderToPipeableStream(
  <App />,
  {
    onAllReady() {
      res.statusCode = 200;
      res.setHeader('Content-type', 'text/html');
      pipe(res);
    },
    onShellError(x) {
      res.statusCode = 500;
      res.send(
        '<!doctype html><p>Loading...</p><script src="clientrender.js"></script>'
      );
    }
  }
);
```

* * *

### `renderToReadableStream()` {#rendertoreadablestream}

```javascript
    ReactDOMServer.renderToReadableStream(element, options);
```

Streams a React element to its initial HTML. Returns a [Readable Stream](https://developer.mozilla.org/en-US/docs/Web/API/ReadableStream). Fully supports Suspense and streaming of HTML. [Read more](https://github.com/reactwg/react-18/discussions/127)

If you call [`ReactDOM.hydrateRoot()`](/docs/react-dom-client.html#hydrateroot) on a node that already has this server-rendered markup, React will preserve it and only attach event handlers, allowing you to have a very performant first-load experience.

```
let controller = new AbortController();
try {
  let stream = await renderToReadableStream(
    <html>
      <body>Success</body>
    </html>,
    {
      signal: controller.signal,
    }
  );
  
  // This is to wait for all suspense boundaries to be ready. You can uncomment
  // this line if you don't want to stream to the client
  // await stream.allReady;

  return new Response(stream, {
    headers: {'Content-Type': 'text/html'},
  });
} catch (error) {
  return new Response(
    '<!doctype html><p>Loading...</p><script src="clientrender.js"></script>',
    {
      status: 500,
      headers: {'Content-Type': 'text/html'},
    }
  );
}
```
* * *

### `renderToNodeStream()` {#rendertonodestream} (Deprecated)

```javascript
ReactDOMServer.renderToNodeStream(element)
```

Renderiza um elemento React para o teu HTML inicial. Retorna um [_Readable Stream_](https://nodejs.org/api/stream.html#stream_readable_streams) que gera uma string HTML. A saída HTML deste _stream_ é exactamente igual à que [`ReactDOMServer.renderToString`](#rendertostring) retornaria. Podes usar este método para gerar HTML no servidor e enviar o _markup_ no _request_ inicial para ter carregamentos de página mais rápidos e para permitir que motores de busca indexem as tuas páginas para fins de _SEO_.

<<<<<<< HEAD
Se invocares o método [`ReactDOM.hydrate()`](/docs/react-dom.html#hydrate) em um nó (_node_) que já tem o seu _markup_ processado pelo servidor, o React vai preservá-lo e apenas atribuir manipuladores de eventos (_event handlers_), permitindo que tenhas uma experiência muito eficiente no primeiro carregamento.
=======
If you call [`ReactDOM.hydrateRoot()`](/docs/react-dom-client.html#hydrateroot) on a node that already has this server-rendered markup, React will preserve it and only attach event handlers, allowing you to have a very performant first-load experience.
>>>>>>> 707f22d25f5b343a2e5e063877f1fc97cb1f48a1

> Nota:
>
> Apenas para servidor. Esta API não está disponível no navegador.
>
> O _stream_ retornado por este método retornará um _stream_ de bytes (_byte stream_) codificado em utf-8. Se precisares de um _stream_ em outra codificação, vê projectos como o [iconv-lite](https://www.npmjs.com/package/iconv-lite), que fornecem _streams_ de transformação para transcodificação de texto.

* * *

### `renderToStaticNodeStream()` {#rendertostaticnodestream}

```javascript
ReactDOMServer.renderToStaticNodeStream(element)
```

Semelhante à [`renderToNodeStream`](#rendertonodestream), excepto que este não cria atributos DOM adicionais que o React usa internamente, como `data-reactroot`. Isto é útil se quiseres usar o React como um simples gerador de páginas estáticas, já que remover os atributos adicionais pode economizar alguns bytes.

A saída HTML deste _stream_ é exactamente igual à que [`ReactDOMServer.renderToStaticMarkup`](#rendertostaticmarkup) retornaria.

<<<<<<< HEAD
Se tens planos de usar o React no cliente para tornar o _markup_ interactivo, não uses este método. Em vez disso, usa [`renderToNodeStream`](#rendertonodestream) no servidor e [`ReactDOM.hydrate()`](/docs/react-dom.html#hydrate) no cliente.
=======
If you plan to use React on the client to make the markup interactive, do not use this method. Instead, use [`renderToNodeStream`](#rendertonodestream) on the server and [`ReactDOM.hydrateRoot()`](/docs/react-dom-client.html#hydrateroot) on the client.
>>>>>>> 707f22d25f5b343a2e5e063877f1fc97cb1f48a1

> Nota:
>
> Apenas para servidor. Esta API não está disponível no navegador.
>
> O _stream_ retornado por este método retornará um _stream_ de bytes (_byte stream_) codificado em utf-8. Se precisares de um _stream_ em outra codificação, vê projectos como o [iconv-lite](https://www.npmjs.com/package/iconv-lite), que fornecem _streams_ de transformação para transcodificação de texto.
