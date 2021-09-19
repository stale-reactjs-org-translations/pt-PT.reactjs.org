---
id: handling-events
title: Manipulando Eventos
permalink: docs/handling-events.html
prev: state-and-lifecycle.html
next: conditional-rendering.html
redirect_from:
  - "docs/events-ko-KR.html"
---

<<<<<<< HEAD
Manipular eventos em elementos React é muito semelhante à manipular eventos em elementos do DOM. Existem algumas diferenças sintáticas:
=======
Handling events with React elements is very similar to handling events on DOM elements. There are some syntax differences:
>>>>>>> 0bb0303fb704147452a568472e968993f0729c28

* Eventos em React são nomeados usando camelCase ao invés de letras minúsculas.
* Com o JSX tu passas uma função como manipulador de eventos ao invés de um texto.

Por exemplo, com HTML:

```html
<button onclick="activateLasers()">
  Ativar Lasers
</button>
```

é ligeiramente diferente em React:

```js{1}
<button onClick={activateLasers}>
  Ativar Lasers
</button>
```

<<<<<<< HEAD
Outra diferença é que não podes retornar `false` para evitar o comportamento padrão no React. Deves chamar `preventDefault` explícitamente. Por exemplo, com HTML simples, para evitar que um link abra uma nova página, podes escrever:

```html
<a href="#" onclick="console.log('O link foi clicado.'); return false">
  Clica-me
</a>
=======
Another difference is that you cannot return `false` to prevent default behavior in React. You must call `preventDefault` explicitly. For example, with plain HTML, to prevent the default form behavior of submitting, you can write:

```html
<form onsubmit="console.log('You clicked submit.'); return false">
  <button type="submit">Submit</button>
</form>
>>>>>>> 0bb0303fb704147452a568472e968993f0729c28
```

Em React, isto poderia ser:

```js{3}
function Form() {
  function handleSubmit(e) {
    e.preventDefault();
<<<<<<< HEAD
    console.log('O link foi clicado.');
  }

  return (
    <a href="#" onClick={handleClick}>
      Clica-me
    </a>
=======
    console.log('You clicked submit.');
  }

  return (
    <form onSubmit={handleSubmit}>
      <button type="submit">Submit</button>
    </form>
>>>>>>> 0bb0303fb704147452a568472e968993f0729c28
  );
}
```

<<<<<<< HEAD
Aqui, "`e`" é um synthetic event. O React define esses eventos sintéticos de acordo com a [especificação W3C](https://www.w3.org/TR/DOM-Level-3-Events/). Então, não precisamos nos preocupar com a compatibilidade entre navegadores. Vê a página [`SyntheticEvent`](/docs/events.html) para saberes mais.
=======
Here, `e` is a synthetic event. React defines these synthetic events according to the [W3C spec](https://www.w3.org/TR/DOM-Level-3-Events/), so you don't need to worry about cross-browser compatibility. React events do not work exactly the same as native events. See the [`SyntheticEvent`](/docs/events.html) reference guide to learn more.
>>>>>>> 0bb0303fb704147452a568472e968993f0729c28

Ao usar o React geralmente não precisas chamar `addEventListener` para adicionar _listeneres_ à um elemento no DOM depois que ele é criado. Ao invés disso podes apenas definir um _listener_ quando o elemento é inicialmente renderizado.

Quando defines um componente usando uma [classe do ES6](https://developer.mozilla.org/pt-pt/docs/Web/JavaScript/Reference/Classes), um padrão comum é que um manipulador de eventos seja um método na classe. Por exemplo, este componente `Toggle` renderiza um botão que permite ao utilizador alternar entre os estados "ON" e "OFF":

```js{6,7,10-14,18}
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // Aqui utilizamos o `bind` para que o `this` funcione dentro do nosso callback
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState(prevState => ({
      isToggleOn: !prevState.isToggleOn
    }));
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}

ReactDOM.render(
  <Toggle />,
  document.getElementById('root')
);
```

[**Experimenta no CodePen**](https://codepen.io/gaearon/pen/xEmzGg?editors=0010)

Precisas ter cuidado com o significado do `this` nos callbacks do JSX. Em JavaScript, os métodos de classe não são [vinculados](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_objects/Function/bind) por padrão. Se esqueceres de fazer o _bind_ de `this.handleClick` e passá-lo para um `onClick`, o `this` será `undefined` quando a função for realmente chamada.

Este não é um comportamento específico do React. É [como funcionam as funções em JavaScript](https://www.smashingmagazine.com/2014/01/understanding-javascript-function-prototype-bind/). Geralmente, se referires à um método sem `()` depois dele, como `onClick={this.handleClick}`, deves fazer o _bind_ desse método.

Se chamar `bind` incomoda-te, há duas maneiras de contornar isto. Se estiveres a usar a [sintaxe experimental de campos de classe pública](https://babeljs.io/docs/plugins/transform-class-properties/), podes usar campos de classe para vincular callbacks corretamente:

```js{2-6}
class LoggingButton extends React.Component {
  // Esta sintaxe garante que o `this` seja vinculado ao handleClick.
  // Atenção: esta é uma sintaxe *experimental*.
  handleClick = () => {
    console.log('this é:', this);
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        Clica-me
      </button>
    );
  }
}
```

Esta sintaxe é habilitada por padrão em [Create React App](https://github.com/facebookincubator/create-react-app).

Se não estiveres a usar a sintaxe de campos de classe, poderás usar uma [função arrow](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Functions/Arrow_functions) como callback:

```js{7-9}
class LoggingButton extends React.Component {
  handleClick() {
    console.log('this é:', this);
  }

  render() {
    // Esta sintaxe garante que o `this` seja vinculado ao handleClick.
    return (
<<<<<<< HEAD
      <button onClick={(e) => this.handleClick(e)}>
        Clica-me
=======
      <button onClick={() => this.handleClick()}>
        Click me
>>>>>>> 0bb0303fb704147452a568472e968993f0729c28
      </button>
    );
  }
}
```

O problema com esta sintaxe é que um callback diferente é criado toda vez que o `LoggingButton` é renderizado. Na maioria dos casos não há problema. No entanto, se este callback for passado para componentes inferiores através de props, estes componentes poderão fazer uma renderização adicional. Geralmente recomendamos a vinculação no construtor ou a sintaxe dos campos de classe para evitar este tipo de problema de desempenho.

## Passar argumentos para manipuladores de eventos {#passing-arguments-to-event-handlers}

Dentro de uma estrutura de repetição é comum desejar passar um parâmetro extra para um manipulador de evento. Por exemplo, se `id` é o ID de identificação da linha, qualquer um dos dois a seguir funcionará:

```js
<button onClick={(e) => this.deleteRow(id, e)}>Apagar Linha</button>
<button onClick={this.deleteRow.bind(this, id)}>Apagar Linha</button>
```

As duas linhas acima são equivalentes e usam [função arrow](https://developer.mozilla.org/pt-PT/docs/Web/JavaScript/Reference/Functions/Arrow_functions) e [`Function.prototype.bind`](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_objects/Function/bind) respectivamente.

Em ambos os casos, o argumento `e` que representa o evento de React será passado como segundo argumento após o ID. Com uma função _arrow_, nós temos que passá-lo explicitamente. Mas com o `bind` outros argumentos adicionais serão automaticamente encaminhados.
