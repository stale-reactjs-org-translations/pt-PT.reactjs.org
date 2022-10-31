---
id: react-dom
title: ReactDOM
layout: docs
category: Reference
permalink: docs/react-dom.html
---

<<<<<<< HEAD
Se usares o React através da tag `<script>`, estas APIs de alto nível estão disponíveis através do objecto global `ReactDOM`. Se usares ES6 através do npm, podes escrever `import ReactDOM from 'react-dom'`. Se usares ES5 através do npm, podes escrever `var ReactDOM = require('react-dom')`.
=======
The `react-dom` package provides DOM-specific methods that can be used at the top level of your app and as an escape hatch to get outside the React model if you need to.

```js
import * as ReactDOM from 'react-dom';
```

If you use ES5 with npm, you can write:

```js
var ReactDOM = require('react-dom');
```

The `react-dom` package also provides modules specific to client and server apps:
- [`react-dom/client`](/docs/react-dom-client.html)
- [`react-dom/server`](/docs/react-dom-server.html)
>>>>>>> e21b37c8cc8b4e308015ea87659f13aa26bd6356

## Visão Geral {#overview}

<<<<<<< HEAD
O pacote `react-dom` fornece métodos DOM específicos que podem ser usados ao mais alto nível da tua aplicação como uma escapatória para saíres do modelo React se precisares. A maioria dos teus componentes não deverão necessitar de usar este módulo.
=======
The `react-dom` package exports these methods:
- [`createPortal()`](#createportal)
- [`flushSync()`](#flushsync)
>>>>>>> e21b37c8cc8b4e308015ea87659f13aa26bd6356

These `react-dom` methods are also exported, but are considered legacy:
- [`render()`](#render)
- [`hydrate()`](#hydrate)
- [`findDOMNode()`](#finddomnode)
- [`unmountComponentAtNode()`](#unmountcomponentatnode)

> Note: 
> 
> Both `render` and `hydrate` have been replaced with new [client methods](/docs/react-dom-client.html) in React 18. These methods will warn that your app will behave as if it's running React 17 (learn more [here](https://reactjs.org/link/switch-to-createroot)).

### Suporte entre Navegadores {#browser-support}

<<<<<<< HEAD
React suporta todos os principais navegadores, incluindo o Internet Explorer 9 e superiores, embora [alguns _polyfills_ sejam necessários](/docs/javascript-environment-requirements.html) para navegadores mais antigos como o IE 9 e IE 10.
=======
React supports all modern browsers, although [some polyfills are required](/docs/javascript-environment-requirements.html) for older versions.
>>>>>>> e21b37c8cc8b4e308015ea87659f13aa26bd6356

> Nota
>
<<<<<<< HEAD
> Não suportamos navegadores antigos que não suportem métodos ES5, no entanto, a tua aplicação poderá funcionar nesses navegadores antigos se _polyfills_ como [es5-shim e es5-sham](https://github.com/es-shims/es5-shim) estiverem incluídos na tua página. Se escolheres esse caminho, estarás por tua conta.

* * *
=======
> We do not support older browsers that don't support ES5 methods or microtasks such as Internet Explorer. You may find that your apps do work in older browsers if polyfills such as [es5-shim and es5-sham](https://github.com/es-shims/es5-shim) are included in the page, but you're on your own if you choose to take this path.
>>>>>>> e21b37c8cc8b4e308015ea87659f13aa26bd6356

## Referência {#reference}

### `createPortal()` {#createportal}

```javascript
createPortal(child, container)
```

<<<<<<< HEAD
Renderiza um elemento React no DOM dentro do `container` fornecido e retorna a [referência](/docs/more-about-refs.html) para o componente, ou retorna `null` para [componentes sem estado (_stateless components_)](/docs/components-and-props.html#function-and-class-components).
=======
Creates a portal. Portals provide a way to [render children into a DOM node that exists outside the hierarchy of the DOM component](/docs/portals.html).

### `flushSync()` {#flushsync}

```javascript
flushSync(callback)
```

Force React to flush any updates inside the provided callback synchronously. This ensures that the DOM is updated immediately.

```javascript
// Force this state update to be synchronous.
flushSync(() => {
  setCount(count + 1);
});
// By this point, DOM is updated.
```

> Note:
> 
> `flushSync` can significantly hurt performance. Use sparingly.
> 
> `flushSync` may force pending Suspense boundaries to show their `fallback` state.
> 
> `flushSync` may also run pending effects and synchronously apply any updates they contain before returning.
> 
> `flushSync` may also flush updates outside the callback when necessary to flush the updates inside the callback. For example, if there are pending updates from a click, React may flush those before flushing the updates inside the callback.

## Legacy Reference {#legacy-reference}
### `render()` {#render}
```javascript
render(element, container[, callback])
```

> Note:
>
> `render` has been replaced with `createRoot` in React 18. See [createRoot](/docs/react-dom-client.html#createroot) for more info.

Render a React element into the DOM in the supplied `container` and return a [reference](/docs/more-about-refs.html) to the component (or returns `null` for [stateless components](/docs/components-and-props.html#function-and-class-components)).
>>>>>>> e21b37c8cc8b4e308015ea87659f13aa26bd6356

Se o elemento React já foi anteriormente renderizado no `container`, isto irá executar uma actualização onde apenas irá alterar o necessário no DOM para reflectir o elemento React actual.

Se o _callback_ opcional for fornecido, este irá ser executado após a renderização ou actualização do componente.

> Nota:
>
<<<<<<< HEAD
> `ReactDOM.render()` controla o conteúdo do nó (_node_) do _container_ que lhe é passado. Todos os elementos DOM que estão dentro do nó são substituídos na primeira invocação. As invocações seguintes usam um algoritmo de diferenciação do DOM (do próprio React) para actualizações eficientes.
>
> `ReactDOM.render()` não modifica o nó (_node_) do _container_ (apenas modifica os filhos do _container_). É portanto possível, inserir um componente num nó já existente no DOM sem reescrever os filhos existentes.
>
> `ReactDOM.render()` retorna a referência para a raíz da instância `ReactComponent`. No entanto, deverás evitar usá-la, pois em futuras versões de React, os componentes poderão ser renderizados assíncronamente em algums casos. Se necessitares usar uma referência para a raíz da instância `ReactComponent`, a melhor solução será anexar uma [referência callback (_callback ref_)](/docs/more-about-refs.html#the-ref-callback-attribute) no elemento raíz.
>
> Usar `ReactDOM.render()` para "hidratar" um _container_ renderizado pelo servidor está descontinuado e será removido no React 17. Usa antes o método [`hydrate()`](#hydrate).
=======
> `render()` controls the contents of the container node you pass in. Any existing DOM elements inside are replaced when first called. Later calls use React’s DOM diffing algorithm for efficient updates.
>
> `render()` does not modify the container node (only modifies the children of the container). It may be possible to insert a component to an existing DOM node without overwriting the existing children.
>
> `render()` currently returns a reference to the root `ReactComponent` instance. However, using this return value is legacy
> and should be avoided because future versions of React may render components asynchronously in some cases. If you need a reference to the root `ReactComponent` instance, the preferred solution is to attach a
> [callback ref](/docs/refs-and-the-dom.html#callback-refs) to the root element.
>
> Using `render()` to hydrate a server-rendered container is deprecated. Use [`hydrateRoot()`](/docs/react-dom-client.html#hydrateroot) instead.
>>>>>>> e21b37c8cc8b4e308015ea87659f13aa26bd6356

* * *

### `hydrate()` {#hydrate}

```javascript
hydrate(element, container[, callback])
```

<<<<<<< HEAD
O mesmo que [`render()`](#render), mas é usado para "hidratar" um _container_ cujos conteúdos HTML foram servidos por [`ReactDOMServer`](/docs/react-dom-server.html). O React irá tentar anexar _event listeners_ no código existente.
=======
> Note:
>
> `hydrate` has been replaced with `hydrateRoot` in React 18. See [hydrateRoot](/docs/react-dom-client.html#hydrateroot) for more info.

Same as [`render()`](#render), but is used to hydrate a container whose HTML contents were rendered by [`ReactDOMServer`](/docs/react-dom-server.html). React will attempt to attach event listeners to the existing markup.
>>>>>>> e21b37c8cc8b4e308015ea87659f13aa26bd6356

O React espera que o conteúdo apresentado seja idêntico entre o servidor e o cliente. Pode corrigir diferenças de texto, mas deverás tratar divergências como _bugs_ e corrigi-las. Em modo de desenvolvimento, o React alerta sobre divergências durante o processo de "hidratação". Não há garantias que diferenças de atributos sejam corrigidas em caso de divergências. Isto é importante em termos de desempenho na maioria das aplicações, divergências são raras, e portanto, validar todo o código seria muito dispendioso.

Se um único atributo ou texto de um elemento for inevitavelmente diferente entre o servidor e o cliente (por exemplo, uma data e hora), o alerta poderá ser silenciado adicionando `suppressHydrationWarning={true}` ao elemento. Apenas funcionará no primeiro nível, e é suposto funcionar como uma escapatória. Não o uses excessivamente. A menos que se trate de texto, o React nem irá tentar corrigi-lo, portanto poderá permanecer intacto até uma próxima actualização.

Se for necessário apresentar intencionalmente algo diferente entre o servidor e o cliente, poderás optar por uma renderização de dupla passagem (_two-pass rendering_). Componentes que apresentem algo diferente no cliente podem ler uma variável de estado como `this.state.isClient`, que poderá ser definida como `true` no `componentDidMount()`. Desta forma, a passagem inicial de apresentação, irá apresentar o mesmo conteúdo que o servidor, evitando divergências, mas uma passagem adicional irá acontecer sincronamente, logo a seguir à "hidratação". Nota que esta abordagem irá fazer com que os teus componentes sejam mais lentos, pois irão ser apresentados duas vezes, portanto, usa com precaução.

Toma em consideração a experiência do utilizador em conexões lentas. O código JavaScript pode ser carregado significantemente depois da apresentação inicial do HTML, portanto, se apresentares algo diferente na passagem do lado do cliente, a transição pode ser dissonante. No entanto, se bem executada, pode ser benéfico apresentar uma "_shell_" da aplicação no servidor, e apenas alguns _widgets_ extras no cliente. Para aprender como fazer isto sem ter problemas com as divergências no código, consulta a explicação no parágrafo anterior.

* * *

### `unmountComponentAtNode()` {#unmountcomponentatnode}

```javascript
unmountComponentAtNode(container)
```

<<<<<<< HEAD
Remove um componente React montado do DOM e limpa o seu estado e manipuladores de eventos (_event handlers_). Se nenhum componente foi montado no _container_, a invocação deste método não tem qualquer efeito. Retorna `true` se o component foi desmontado e `false` se não houver nenhum componente para desmontar.
=======
> Note:
>
> `unmountComponentAtNode` has been replaced with `root.unmount()` in React 18. See [createRoot](/docs/react-dom-client.html#createroot) for more info.

Remove a mounted React component from the DOM and clean up its event handlers and state. If no component was mounted in the container, calling this function does nothing. Returns `true` if a component was unmounted and `false` if there was no component to unmount.
>>>>>>> e21b37c8cc8b4e308015ea87659f13aa26bd6356

* * *

### `findDOMNode()` {#finddomnode}

> Nota:
>
> `findDOMNode` é uma escapatória usada para aceder ao nó (_node_) do DOM subjacente. Na maioria dos casos, o uso desta escapatória é desaconselhado porque trespassa a abstração do componente. [Foi descontinuado em `StrictMode`.](/docs/strict-mode.html#warning-about-deprecated-finddomnode-usage)

```javascript
findDOMNode(component)
```
Se este componente foi montado no DOM, retorna o elemento DOM nativo correspondente. Este método é util para ler valores fora do DOM, como valores de um campo de um formulário e fazer medições ao DOM. **Na maioria dos casos, pode ser anexada uma referência (ref) no nó (_node_) do DOM evitando o uso de `findDOMNode` por completo.**

Quando um componente retorna `null` ou `false`, `findDOMNode` retorna `null`. Se o componente retorna uma string, `findDOMNode` retorna um nó (_node_) do DOM de texto contendo o valor dessa string. Desde o React 16, um componente pode retornar um fragmento com múltiplos filhos, e neste caso, `findDOMNode` irá retornar o nó (_node_) do DOM correspondente ao primeiro filho não vazio.

> Nota:
>
> `findDOMNode` apenas funciona em componentes montados (isto é, componentes que tenham sido inseridos no DOM). Se tentares invocar este método num componente que ainda não tenha sido montado (por exemplo, invocando `findDOMNode` no `render()` de um componente que ainda não tenha sido criado) será lançada uma excepção.
>
> `findDOMNode` não pode ser usado em componentes funcionais (_function components_).

* * *
<<<<<<< HEAD

### `createPortal()` {#createportal}

```javascript
ReactDOM.createPortal(child, container)
```

Cria um portal. Portais fornecem uma forma de [renderizar filhos num nó (_node_) do DOM que existe fora da hierárquia do DOM do componente](/docs/portals.html).
=======
>>>>>>> e21b37c8cc8b4e308015ea87659f13aa26bd6356
