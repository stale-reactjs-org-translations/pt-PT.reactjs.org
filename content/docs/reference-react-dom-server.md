---
id: react-dom-server
title: ReactDOMServer
layout: docs
category: Reference
permalink: docs/react-dom-server.html
---

O objecto `ReactDOMServer` permite que renderizes os teus componentes em _markup_ estático. Geralmente, é usado em um servidor Node:

```js
<<<<<<< HEAD
// Módulos ES
import ReactDOMServer from 'react-dom/server';
=======
// ES modules
import * as ReactDOMServer from 'react-dom/server';
>>>>>>> d522a5f4a9faaf6fd314f4d15f1be65ca997760f
// CommonJS
var ReactDOMServer = require('react-dom/server');
```

## Visão Geral {#overview}

<<<<<<< HEAD
Os métodos a seguir podem ser usados tanto em ambiente de servidor como de navegador:
=======
These methods are only available in the **environments with [Node.js Streams](https://nodejs.dev/learn/nodejs-streams):**

- [`renderToPipeableStream()`](#rendertopipeablestream)
- [`renderToNodeStream()`](#rendertonodestream) (Deprecated)
- [`renderToStaticNodeStream()`](#rendertostaticnodestream)

These methods are only available in the **environments with [Web Streams](https://developer.mozilla.org/en-US/docs/Web/API/Streams_API)** (this includes browsers, Deno, and some modern edge runtimes):

- [`renderToReadableStream()`](#rendertoreadablestream)

The following methods can be used in the environments that don't support streams:
>>>>>>> d522a5f4a9faaf6fd314f4d15f1be65ca997760f

- [`renderToString()`](#rendertostring)
- [`renderToStaticMarkup()`](#rendertostaticmarkup)

<<<<<<< HEAD
Estes métodos adicionais dependem do pacote (`stream`) que **só está disponível no servidor** e não irão funcionar no navegador.

- [`renderToNodeStream()`](#rendertonodestream)
- [`renderToStaticNodeStream()`](#rendertostaticnodestream)

* * *

## Referência {#reference}
=======
## Reference {#reference}
>>>>>>> d522a5f4a9faaf6fd314f4d15f1be65ca997760f

### `renderToPipeableStream()` {#rendertopipeablestream}

```javascript
ReactDOMServer.renderToPipeableStream(element, options)
```

<<<<<<< HEAD
Renderiza um elemento React para o seu HTML inicial. O React retornará uma string HTML. Podes usar este método para gerar HTML no servidor e enviar o _markup_ no request inicial para ter carregamentos de páginas mais rápidos e para permitir que motores de pesquisa indexem as tuas páginas para fins de _SEO_.

Se invocares o método [`ReactDOM.hydrate()`](/docs/react-dom.html#hydrate) em um nó (_node_) que já tem o seu _markup_ processado pelo servidor, o React vai preservá-lo e apenas atribuir manipuladores de eventos (_event handlers_), permitindo que tenhas uma experiência muito eficiente no primeiro carregamento.
=======
Render a React element to its initial HTML. Returns a stream with a `pipe(res)` method to pipe the output and `abort()` to abort the request. Fully supports Suspense and streaming of HTML with "delayed" content blocks "popping in" via inline `<script>` tags later. [Read more](https://github.com/reactwg/react-18/discussions/37)

If you call [`ReactDOM.hydrateRoot()`](/docs/react-dom-client.html#hydrateroot) on a node that already has this server-rendered markup, React will preserve it and only attach event handlers, allowing you to have a very performant first-load experience.

```javascript
let didError = false;
const stream = renderToPipeableStream(
  <App />,
  {
    onShellReady() {
      // The content above all Suspense boundaries is ready.
      // If something errored before we started streaming, we set the error code appropriately.
      res.statusCode = didError ? 500 : 200;
      res.setHeader('Content-type', 'text/html');
      stream.pipe(res);
    },
    onShellError(error) {
      // Something errored before we could complete the shell so we emit an alternative shell.
      res.statusCode = 500;
      res.send(
        '<!doctype html><p>Loading...</p><script src="clientrender.js"></script>'
      );
    },
    onAllReady() {
      // If you don't want streaming, use this instead of onShellReady.
      // This will fire after the entire page content is ready.
      // You can use this for crawlers or static generation.

      // res.statusCode = didError ? 500 : 200;
      // res.setHeader('Content-type', 'text/html');
      // stream.pipe(res);
    },
    onError(err) {
      didError = true;
      console.error(err);
    },
  }
);
```

See the [full list of options](https://github.com/facebook/react/blob/14c2be8dac2d5482fda8a0906a31d239df8551fc/packages/react-dom/src/server/ReactDOMFizzServerNode.js#L36-L46).

> Note:
>
> This is a Node.js-specific API. Environments with [Web Streams](https://developer.mozilla.org/en-US/docs/Web/API/Streams_API), like Deno and modern edge runtimes, should use [`renderToReadableStream`](#rendertoreadablestream) instead.
>
>>>>>>> d522a5f4a9faaf6fd314f4d15f1be65ca997760f

* * *

### `renderToReadableStream()` {#rendertoreadablestream}

```javascript
ReactDOMServer.renderToReadableStream(element, options);
```

<<<<<<< HEAD
Semelhante à [`renderToString`](#rendertostring), excepto que este não cria atributos DOM adicionais que o React usa internamente, como `data-reactroot`. Isto é útil se quiseres usar o React como um simples gerador de páginas estáticas, já que remover os atributos adicionais pode economizar alguns bytes.

Se tens planos de usar o React no cliente para tornar o _markup_ interactivo, não uses este método. Em vez disso, usa [`renderToString`](#rendertostring) no servidor e [`ReactDOM.hydrate()`](/docs/react-dom.html#hydrate) no cliente.
=======
Streams a React element to its initial HTML. Returns a Promise that resolves to a [Readable Stream](https://developer.mozilla.org/en-US/docs/Web/API/ReadableStream). Fully supports Suspense and streaming of HTML. [Read more](https://github.com/reactwg/react-18/discussions/127)

If you call [`ReactDOM.hydrateRoot()`](/docs/react-dom-client.html#hydrateroot) on a node that already has this server-rendered markup, React will preserve it and only attach event handlers, allowing you to have a very performant first-load experience.

```javascript
let controller = new AbortController();
let didError = false;
try {
  let stream = await renderToReadableStream(
    <html>
      <body>Success</body>
    </html>,
    {
      signal: controller.signal,
      onError(error) {
        didError = true;
        console.error(error);
      }
    }
  );
  
  // This is to wait for all Suspense boundaries to be ready. You can uncomment
  // this line if you want to buffer the entire HTML instead of streaming it.
  // You can use this for crawlers or static generation:

  // await stream.allReady;

  return new Response(stream, {
    status: didError ? 500 : 200,
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

See the [full list of options](https://github.com/facebook/react/blob/14c2be8dac2d5482fda8a0906a31d239df8551fc/packages/react-dom/src/server/ReactDOMFizzServerBrowser.js#L27-L35).

> Note:
>
> This API depends on [Web Streams](https://developer.mozilla.org/en-US/docs/Web/API/Streams_API). For Node.js, use [`renderToPipeableStream`](#rendertopipeablestream) instead.
>
>>>>>>> d522a5f4a9faaf6fd314f4d15f1be65ca997760f

* * *

### `renderToNodeStream()`  (Deprecated) {#rendertonodestream}

```javascript
ReactDOMServer.renderToNodeStream(element)
```

<<<<<<< HEAD
Renderiza um elemento React para o teu HTML inicial. Retorna um [_Readable Stream_](https://nodejs.org/api/stream.html#stream_readable_streams) que gera uma string HTML. A saída HTML deste _stream_ é exactamente igual à que [`ReactDOMServer.renderToString`](#rendertostring) retornaria. Podes usar este método para gerar HTML no servidor e enviar o _markup_ no _request_ inicial para ter carregamentos de página mais rápidos e para permitir que motores de busca indexem as tuas páginas para fins de _SEO_.

Se invocares o método [`ReactDOM.hydrate()`](/docs/react-dom.html#hydrate) em um nó (_node_) que já tem o seu _markup_ processado pelo servidor, o React vai preservá-lo e apenas atribuir manipuladores de eventos (_event handlers_), permitindo que tenhas uma experiência muito eficiente no primeiro carregamento.
=======
Render a React element to its initial HTML. Returns a [Node.js Readable stream](https://nodejs.org/api/stream.html#stream_readable_streams) that outputs an HTML string. The HTML output by this stream is exactly equal to what [`ReactDOMServer.renderToString`](#rendertostring) would return. You can use this method to generate HTML on the server and send the markup down on the initial request for faster page loads and to allow search engines to crawl your pages for SEO purposes.

If you call [`ReactDOM.hydrateRoot()`](/docs/react-dom-client.html#hydrateroot) on a node that already has this server-rendered markup, React will preserve it and only attach event handlers, allowing you to have a very performant first-load experience.
>>>>>>> d522a5f4a9faaf6fd314f4d15f1be65ca997760f

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
>>>>>>> d522a5f4a9faaf6fd314f4d15f1be65ca997760f

> Nota:
>
> Apenas para servidor. Esta API não está disponível no navegador.
>
<<<<<<< HEAD
> O _stream_ retornado por este método retornará um _stream_ de bytes (_byte stream_) codificado em utf-8. Se precisares de um _stream_ em outra codificação, vê projectos como o [iconv-lite](https://www.npmjs.com/package/iconv-lite), que fornecem _streams_ de transformação para transcodificação de texto.
=======
> The stream returned from this method will return a byte stream encoded in utf-8. If you need a stream in another encoding, take a look at a project like [iconv-lite](https://www.npmjs.com/package/iconv-lite), which provides transform streams for transcoding text.

* * *

### `renderToString()` {#rendertostring}

```javascript
ReactDOMServer.renderToString(element)
```

Render a React element to its initial HTML. React will return an HTML string. You can use this method to generate HTML on the server and send the markup down on the initial request for faster page loads and to allow search engines to crawl your pages for SEO purposes.

If you call [`ReactDOM.hydrateRoot()`](/docs/react-dom-client.html#hydrateroot) on a node that already has this server-rendered markup, React will preserve it and only attach event handlers, allowing you to have a very performant first-load experience.

> Note
>
> This API has limited Suspense support and does not support streaming.
>
> On the server, it is recommended to use either [`renderToPipeableStream`](#rendertopipeablestream) (for Node.js) or [`renderToReadableStream`](#rendertoreadablestream) (for Web Streams) instead.

* * *

### `renderToStaticMarkup()` {#rendertostaticmarkup}

```javascript
ReactDOMServer.renderToStaticMarkup(element)
```

Similar to [`renderToString`](#rendertostring), except this doesn't create extra DOM attributes that React uses internally, such as `data-reactroot`. This is useful if you want to use React as a simple static page generator, as stripping away the extra attributes can save some bytes.

If you plan to use React on the client to make the markup interactive, do not use this method. Instead, use [`renderToString`](#rendertostring) on the server and [`ReactDOM.hydrateRoot()`](/docs/react-dom-client.html#hydrateroot) on the client.
>>>>>>> d522a5f4a9faaf6fd314f4d15f1be65ca997760f
