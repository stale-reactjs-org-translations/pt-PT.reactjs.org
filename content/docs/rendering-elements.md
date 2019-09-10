---
id: rendering-elements
title: Renderizando Elementos
permalink: docs/rendering-elements.html
redirect_from:
  - "docs/displaying-data.html"
prev: introducing-jsx.html
next: components-and-props.html
---

Elementos são os menores blocos para a construção de aplicações em React.

Um elemento descreve o que queres ver no ecrã:

```js
const element = <h1>Olá, mundo</h1>;
```

Ao contrário de elementos DOM do navegador, elementos React são objectos simples e utilizam menos recursos ao serem criados. O React DOM é responsável por actualizar o DOM para igualar os elementos React.

>**Nota:**
>
>Poderás confundir elementos com o mais amplo e conhecido conceito de "componentes". Iremos introduzir componentes na [próxima seccção](/docs/components-and-props.html). Elementos são o que compõem um componente, e recomendamos que leias esta secção antes de prosseguires.

## Renderizando um Elemento no DOM {#rendering-an-element-into-the-dom}

Suponhamos que exista um `<div>` algures no teu ficheiro HTML:

```html
<div id="root"></div>
```

Chamamos a isto um nó "raiz" do DOM porque tudo dentro dele será controlado pelo React DOM.

Aplicações construídas apenas com React geralmente têm apenas um único nó raiz no DOM. Se desejas integrar o React numa aplicação já existente, podes ter os nós raiz isolados que quiseres.

Para renderizar um elemento React num nó raiz, passa ambos para `ReactDOM.render()`:

`embed:rendering-elements/render-an-element.js`

[](codepen://rendering-elements/render-an-element)

Exibe "Olá, mundo" na página.

## Actualizando o Elemento Renderizado {#updating-the-rendered-element}

Elementos React são [imutáveis](https://pt.wikipedia.org/wiki/Objeto_imutável). Uma vez criado o elemento, não poderás modificar os seus filhos ou atributos. Um elemento é como um _frame_ num filme: representa a interface gráfica (_UI_) num certo momento.

Com o que aprendemos até agora, a única forma de actualizar a interface gráfica (_UI_) é criando um novo elemento e passando-o ao `ReactDOM.render()`.

Vê o seguinte exemplo de um relógio:

`embed:rendering-elements/update-rendered-element.js`

[](codepen://rendering-elements/update-rendered-element)

O exemplo invoca o método `ReactDOM.render()` a cada segundo a partir de um _callback_ do [`setInterval()`](https://developer.mozilla.org/pt-PT/docs/Web/API/WindowOrWorkerGlobalScope/setInterval).

>**Nota:**
>
>Na prática, a maioria das aplicações em React apenas invocam o método `ReactDOM.render()` uma vez. Nas secções seguintes, aprenderemos como este código pode ser encapsulado em [componentes com estado](/docs/state-and-lifecycle.html).
>
>Recomendamos que não saltes os tópicos porque eles se complementam.

## O React Apenas Actualiza O Que É Necessário {#react-only-updates-whats-necessary}

O React DOM compara o novo elemento e seus filhos com o anterior e apenas aplica as modificações necessárias no DOM para que fique no estado desejado.

Podes verificar isto inspeccionando o [último exemplo](codepen://rendering-elements/update-rendered-element) com as ferramentas do navegador:

![Actualizações granulares no inspector do DOM](../images/docs/granular-dom-updates.gif)

Embora nós criemos um elemento descrevendo toda a estrutura da interface gráfica (_UI_) a cada instante, somente o nó de texto cujo conteúdo muda é actualizado pelo React DOM.

Com base na nossa experiência, pensar em como a interface gráfica (_UI_) deve estar num determinado momento, ao invés de pensar em como modificá-la com o tempo, evita uma série de _bugs_.
