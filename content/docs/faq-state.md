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

### Como atualizar o `state` com valores que dependem do `state` atual? {#how-do-i-update-state-with-values-that-depend-on-the-current-state}

Passa uma função ao invés de um objeto para `setState` para garantir que a chamada sempre use o valor mais recente do state (vê abaixo).

### Qual é a diferença entre passar um objeto e uma função em `setState`? {#what-is-the-difference-between-passing-an-object-or-a-function-in-setstate}

Passar uma função de atualização permite que acesses o valor atual do `state` dentro dela. Como as chamadas de `setState` são feitas em lotes, isso permite que você encadeie atualizações e garanta que elas se componham ao invés de entrar em conflito:

```jsx
incrementCount() {
  this.setState((state) => {
    // Importante: use `state` em vez de `this.state` quando estiveres a atualizar.
    return {count: state.count + 1}
  });
}

handleSomething() {
  // Digamos que `this.state.count` começa em 0.
  this.incrementCount();
  this.incrementCount();
  this.incrementCount();

  // Se fores a ler `this.state.count` agora, ele ainda seria 0.
  // Mas quando o React renderizar novamente o componente, ele será 3.
}
```

[Lê mais sobre setState](/docs/react-component.html#setstate)

### Quando é que `setState` é assíncrono? {#when-is-setstate-asynchronous}

Atualmente, `setState` é assíncrono dentro de manipuladores de evento.

Isto garante que, por exemplo, caso tanto `Parent` quanto `Child` chamem `setState` após um evento de clique, `Child` não seja renderizado duas vezes. Em vez disso, React executa todas as atualizações de estado ao final do evento do navegador. Isto resulta numa melhoria de performance significativa para aplicativos maiores.

Isto é um detalhe de implementação, então evita depender disso diretamente. Em versões futuras, o React fará atualizações em lotes em mais casos.

### Porquê é que o React não atualiza `this.state` de forma síncrona? {#why-doesnt-react-update-thisstate-synchronously}

Como explicado na seção anterior, React intencionalmente "espera" até todos os componentes terem chamado `setState()` em seus manipuladores de evento antes de começar a renderizar novamente. Isso aumenta performance por evitar renderizações desnecessárias.

No entanto, podes ainda questionar porquê o React simplesmente não atualiza `this.state` imediatamente, sem renderizar novamente.

There are two main reasons:

* This would break the consistency between `props` and `state`, causing issues that are very hard to debug.
* This would make some of the new features we're working on impossible to implement.

This [GitHub comment](https://github.com/facebook/react/issues/11527#issuecomment-360199710) dives deep into the specific examples.

### Should I use a state management library like Redux or MobX? {#should-i-use-a-state-management-library-like-redux-or-mobx}

[Maybe.](https://redux.js.org/faq/general#when-should-i-use-redux)

It's a good idea to get to know React first, before adding in additional libraries. You can build quite complex applications using only React.
