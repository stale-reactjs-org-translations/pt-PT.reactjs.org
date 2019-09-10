---
id: faq-state
title: Estado de um Componente
permalink: docs/faq-state.html
layout: docs
category: FAQ
---

### O que `setState` faz? {#what-does-setstate-do}

`setState()` agenda uma atualização para o objeto `state` de um componente. Quando o state muda, o componente responde renderizando novamente.

### Qual é a diferença entre `state` e `props`? {#what-is-the-difference-between-state-and-props}

[`props`](/docs/components-and-props.html) (abreviação de "<i lang="en">properties</i>") and [`state`](/docs/state-and-lifecycle.html) são ambos objetos JavaScript. Apesar de ambos guardarem informações que influenciam no resultado da renderização, eles são diferentes por uma razão importante: `props` são *passados* para o componente (como parâmetros de funções), enquanto `state` é gerido *de dentro* do componente (como variáveis declaradas dentro de uma função).

Aqui estão alguns bons recursos para ler mais sobre quando usar `props` vs `state` (ambos em inglês):
* [Props vs State](https://github.com/uberVU/react-guide/blob/master/props-vs-state.md)
* [ReactJS: Props vs. State](https://lucybain.com/blog/2016/react-state-vs-pros/)

### Porquê `setState` está a dar-me o valor errado? {#why-is-setstate-giving-me-the-wrong-value}

Em React, tanto `this.props` quanto `this.state` representam os valores *renderizados*, ou seja, o que está atualmente na tela.

Chamadas para `setState` são assíncronas - não confie que `this.state` vá refletir o novo valor imediatamente após chamar `setState`. Usa uma função de atualização ao invés de um objeto se precisas calcular valores baseado no state atual (vê abaixo para mais detalhes).

Exemplo de código que *não* vai funcionar como esperado:

```jsx
incrementCount() {
  // Nota: isso *não* vai funcionar como esperado.
  this.setState({count: this.state.count + 1});
}

handleSomething() {
  // Digamos que `this.state.count` começa em 0.
  this.incrementCount();
  this.incrementCount();
  this.incrementCount();
  // Quando o React renderizar novamente o componente, `this.state.count` será 1, mas tu esperavas 3.

  // Isto é porque a função `incrementCount()` usa `this.state.count`,
  // mas o React não atualiza `this.state.count` até o componente ser renderizado novamente.
  // Então `incrementCount()` lê `this.state.count` como 0 todas as vezes, e muda seu valor para 1.

  // A solução é descrita abaixo!
}
```

Vê abaixo como solucionar esse problema.

### How do I update state with values that depend on the current state? {#how-do-i-update-state-with-values-that-depend-on-the-current-state}

Pass a function instead of an object to `setState` to ensure the call always uses the most updated version of state (see below). 

### What is the difference between passing an object or a function in `setState`? {#what-is-the-difference-between-passing-an-object-or-a-function-in-setstate}

Passing an update function allows you to access the current state value inside the updater. Since `setState` calls are batched, this lets you chain updates and ensure they build on top of each other instead of conflicting:

```jsx
incrementCount() {
  this.setState((state) => {
    // Important: read `state` instead of `this.state` when updating.
    return {count: state.count + 1}
  });
}

handleSomething() {
  // Let's say `this.state.count` starts at 0.
  this.incrementCount();
  this.incrementCount();
  this.incrementCount();

  // If you read `this.state.count` now, it would still be 0.
  // But when React re-renders the component, it will be 3.
}
```

[Learn more about setState](/docs/react-component.html#setstate)

### When is `setState` asynchronous? {#when-is-setstate-asynchronous}

Currently, `setState` is asynchronous inside event handlers.

This ensures, for example, that if both `Parent` and `Child` call `setState` during a click event, `Child` isn't re-rendered twice. Instead, React "flushes" the state updates at the end of the browser event. This results in significant performance improvements in larger apps.

This is an implementation detail so avoid relying on it directly. In the future versions, React will batch updates by default in more cases.

### Why doesn't React update `this.state` synchronously? {#why-doesnt-react-update-thisstate-synchronously}

As explained in the previous section, React intentionally "waits" until all components call `setState()` in their event handlers before starting to re-render. This boosts performance by avoiding unnecessary re-renders.

However, you might still be wondering why React doesn't just update `this.state` immediately without re-rendering.

There are two main reasons:

* This would break the consistency between `props` and `state`, causing issues that are very hard to debug.
* This would make some of the new features we're working on impossible to implement.

This [GitHub comment](https://github.com/facebook/react/issues/11527#issuecomment-360199710) dives deep into the specific examples.

### Should I use a state management library like Redux or MobX? {#should-i-use-a-state-management-library-like-redux-or-mobx}

[Maybe.](https://redux.js.org/faq/general#when-should-i-use-redux)

It's a good idea to get to know React first, before adding in additional libraries. You can build quite complex applications using only React.
