---
id: react-dom
title: ReactDOM
layout: docs
category: Reference
permalink: docs/react-dom.html
---

Se usares o React através da tag `<script>`, estas APIs de alto nível estão disponíveis através do objecto global `ReactDOM`. Se usares ES6 através do npm, podes escrever `import ReactDOM from 'react-dom'`. Se usares ES5 através do npm, podes escrever `var ReactDOM = require('react-dom')`.

## Visão Geral {#overview}

O pacote `react-dom` fornece métodos DOM específicos que podem ser usados ao mais alto nível da tua aplicação como uma escapatória para saíres do modelo React se precisares. A maioria dos teus componentes não deverão necessitar de usar este módulo.

- [`render()`](#render)
- [`hydrate()`](#hydrate)
- [`unmountComponentAtNode()`](#unmountcomponentatnode)
- [`findDOMNode()`](#finddomnode)
- [`createPortal()`](#createportal)

### Suporte entre Navegadores {#browser-support}

React suporta todos os principais navegadores, incluindo o Internet Explorer 9 e superiores, embora [alguns _polyfills_ sejam necessários](/docs/javascript-environment-requirements.html) para navegadores mais antigos como o IE 9 e IE 10.

> Nota
>
> Não suportamos navegadores antigos que não suportem métodos ES5, no entanto, a tua aplicação poderá funcionar nesses navegadores antigos se _polyfills_ como [es5-shim e es5-sham](https://github.com/es-shims/es5-shim) estiverem incluídos na tua página. Se escolheres esse caminho, estarás por tua conta.

* * *

## Referência {#reference}

### `render()` {#render}

```javascript
ReactDOM.render(element, container[, callback])
```

Renderiza um elemento React no DOM dentro do `container` fornecido e retorna a [referência](/docs/more-about-refs.html) para o componente, ou retorna `null` para [componentes sem estado (_stateless components_)](/docs/components-and-props.html#functional-and-class-components).

Se o elemento React já foi anteriormente renderizado no `container`, isto irá executar uma actualização onde apenas irá alterar o necessário no DOM para reflectir o elemento React actual.

Se o _callback_ opcional for fornecido, este irá ser executado após a renderização ou actualização do componente.

> Nota:
>
> `ReactDOM.render()` controla o conteúdo do nó (_node_) do _container_ que lhe é passado. Todos os elementos DOM que estão dentro do nó são substituídos na primeira invocação. As invocações seguintes usam um algoritmo de diferenciação do DOM (do próprio React) para actualizações eficientes.
>
> `ReactDOM.render()` não modifica o nó (_node_) do _container_ (apenas modifica os filhos do _container_). É portanto possível, inserir um componente num nó já existente no DOM sem reescrever os filhos existentes.
>
> `ReactDOM.render()` retorna a referência para a raíz da instância `ReactComponent`. No entanto, deverás evitar usá-la, pois em futuras versões de React, os componentes poderão ser renderizados assíncronamente em algums casos. Se necessitares usar uma referência para a raíz da instância `ReactComponent`, a melhor solução será anexar uma [referência callback (_callback ref_)](/docs/more-about-refs.html#the-ref-callback-attribute) no elemento raíz.
>
> Usar `ReactDOM.render()` para "hidratar" um _container_ renderizado pelo servidor está descontinuado e será removido no React 17. Usa antes o método [`hydrate()`](#hydrate).

* * *

### `hydrate()` {#hydrate}

```javascript
ReactDOM.hydrate(element, container[, callback])
```

O mesmo que [`render()`](#render), mas é usado para "hidratar" um _container_ cujos conteúdos HTML foram servidos por [`ReactDOMServer`](/docs/react-dom-server.html). O React irá tentar anexar _event listeners_ no código existente.

O React espera que o conteúdo apresentado seja idêntico entre o servidor e o cliente. Pode corrigir diferenças de texto, mas deverás tratar divergências como _bugs_ e corrigi-las. Em modo de desenvolvimento, o React alerta sobre divergências durante o processo de "hidratação". Não há garantias que diferenças de atributos sejam corrigidas em caso de divergências. Isto é importante em termos de desempenho na maioria das aplicações, divergências são raras, e portanto, validar todo o código seria muito dispendioso.

Se um único atributo ou texto de um elemento for inevitavelmente diferente entre o servidor e o cliente (por exemplo, uma data e hora), o alerta poderá ser silenciado adicionando `suppressHydrationWarning={true}` ao elemento. Apenas funcionará no primeiro nível, e é suposto funcionar como uma escapatória. Não o uses excessivamente. A menos que se trate de texto, o React nem irá tentar corrigi-lo, portanto poderá permanecer intacto até uma próxima actualização.

Se for necessário apresentar intencionalmente algo diferente entre o servidor e o cliente, poderás optar por uma renderização de dupla passagem (_two-pass rendering_). Componentes que apresentem algo diferente no cliente podem ler uma variável de estado como `this.state.isClient`, que poderá ser definida como `true` no `componentDidMount()`. Desta forma, a passagem inicial de apresentação, irá apresentar o mesmo conteúdo que o servidor, evitando divergências, mas uma passagem adicional irá acontecer sincronamente, logo a seguir à "hidratação". Nota que esta abordagem irá fazer com que os teus componentes sejam mais lentos, pois irão ser apresentados duas vezes, portanto, usa com precaução.

Toma em consideração a experiência do utilizador em conexões lentas. O código JavaScript pode ser carregado significantemente depois da apresentação inicial do HTML, portanto, se apresentares algo diferente na passagem do lado do cliente, a transição pode ser dissonante. No entanto, se bem executada, pode ser benéfico apresentar uma "_shell_" da aplicação no servidor, e apenas alguns _widgets_ extras no cliente. Para aprender como fazer isto sem ter problemas com as divergências no código, consulta a explicação no parágrafo anterior.

* * *

### `unmountComponentAtNode()` {#unmountcomponentatnode}

```javascript
ReactDOM.unmountComponentAtNode(container)
```

Remove um componente React montado do DOM e limpa o seu estado e manipuladores de eventos (_event handlers_). Se nenhum componente foi montado no _container_, a invocação deste método não tem qualquer efeito. Retorna `true` se o component foi desmontado e `false` se não houver nenhum componente para desmontar.

* * *

### `findDOMNode()` {#finddomnode}

> Nota:
>
> `findDOMNode` é uma escapatória usada para aceder ao nó (_node_) do DOM subjacente. Na maioria dos casos, o uso desta escapatória é desaconselhado porque trespassa a abstração do componente. [Foi descontinuado em `StricMode`.](/docs/strict-mode.html#warning-about-deprecated-finddomnode-usage)

```javascript
ReactDOM.findDOMNode(component)
```
Se este componente foi montado no DOM, retorna o elemento DOM nativo correspondente. Este método é util para ler valores fora do DOM, como valores de um campo de um formulário e fazer medições ao DOM. **Na maioria dos casos, pode ser anexada uma referência (ref) no nó (_node_) do DOM evitando o uso de `findDOMNode` por completo.**

Quando um componente retorna `null` ou `false`, `findDOMNode` retorna `null`. Se o componente retorna uma string, `findDOMNode` retorna um nó (_node_) do DOM de texto contendo o valor dessa string. Desde o React 16, um componente pode retornar um fragmento com múltiplos filhos, e neste caso, `findDOMNode` irá retornar o nó (_node_) do DOM correspondente ao primeiro filho não vazio.

> Nota:
>
> `findDOMNode` apenas funciona em componentes montados (isto é, componentes que tenham sido inseridos no DOM). Se tentares invocar este método num componente que ainda não tenha sido montado (por exemplo, invocando `findDOMNode` no `render()` de um componente que ainda não tenha sido criado) será lançada uma excepção.
>
> `findDOMNode` não pode ser usado em componentes funcionais (_function components_).

* * *

### `createPortal()` {#createportal}

```javascript
ReactDOM.createPortal(child, container)
```

Cria um portal. Portais fornecem uma forma de [renderizar filhos num nó (_node_) do DOM que existe fora da hierárquia do DOM do componente](/docs/portals.html).
