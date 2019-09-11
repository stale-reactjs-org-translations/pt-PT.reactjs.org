---
id: handling-events
title: Manipulando Eventos
permalink: docs/handling-events.html
prev: state-and-lifecycle.html
next: conditional-rendering.html
redirect_from:
  - "docs/events-ko-KR.html"
---

Manipular eventos em elementos React é muito semelhante à manipular eventos em elementos do DOM. Existem algumas diferenças sintáticas:

* Eventos em React são nomeados usando camelCase ao invés de letras minúsculas.
* Com o JSX você passa uma função como manipulador de eventos ao invés de um texto.

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

Outra diferença é que não podes retornar `false` para evitar o comportamento padrão no React. Deves chamar `preventDefault` explícitamente. Por exemplo, com HTML simples, para evitar que um link abra uma nova página, podes escrever:

```html
<a href="#" onclick="console.log('O link foi clicado.'); return false">
  Clica-me
</a>
```

Em React, isto poderia ser:

```js{2-5,8}
function ActionLink() {
  function handleClick(e) {
    e.preventDefault();
    console.log('O link foi clicado.');
  }

  return (
    <a href="#" onClick={handleClick}>
      Clica-me
    </a>
  );
}
```

Aqui, "`e`" é um synthetic event. O React define esses eventos sintéticos de acordo com a [especificação W3C](https://www.w3.org/TR/DOM-Level-3-Events/). Então, não precisamos nos preocupar com a compatibilidade entre navegadores. Vê a página [`SyntheticEvent`](/docs/events.html) para saberes mais.

Ao usar o React geralmente não precisas chamar `addEventListener` para adicionar _listeneres_ à um elemento no DOM depois que ele é criado. Ao invés disso podes apenas definir um _listener_ quando o elemento é inicialmente renderizado.

Quando defines um componente usando uma [classe do ES6](https://developer.mozilla.org/pt-pt/docs/Web/JavaScript/Reference/Classes), um padrão comum é que um manipulador de eventos seja um método na classe. Por exemplo, este componente `Toggle` renderiza um botão que permite ao utilizador alternar entre os estados "ON" e "OFF":

```js{6,7,10-14,18}
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // Aqui utilizamos o `bind` para que o `this` funcione dentro da nossa callback
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState(state => ({
      isToggleOn: !state.isToggleOn
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

You have to be careful about the meaning of `this` in JSX callbacks. In JavaScript, class methods are not [bound](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_objects/Function/bind) by default. If you forget to bind `this.handleClick` and pass it to `onClick`, `this` will be `undefined` when the function is actually called.

This is not React-specific behavior; it is a part of [how functions work in JavaScript](https://www.smashingmagazine.com/2014/01/understanding-javascript-function-prototype-bind/). Generally, if you refer to a method without `()` after it, such as `onClick={this.handleClick}`, you should bind that method.

If calling `bind` annoys you, there are two ways you can get around this. If you are using the experimental [public class fields syntax](https://babeljs.io/docs/plugins/transform-class-properties/), you can use class fields to correctly bind callbacks:

```js{2-6}
class LoggingButton extends React.Component {
  // This syntax ensures `this` is bound within handleClick.
  // Warning: this is *experimental* syntax.
  handleClick = () => {
    console.log('this is:', this);
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        Click me
      </button>
    );
  }
}
```

This syntax is enabled by default in [Create React App](https://github.com/facebookincubator/create-react-app).

If you aren't using class fields syntax, you can use an [arrow function](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions) in the callback:

```js{7-9}
class LoggingButton extends React.Component {
  handleClick() {
    console.log('this is:', this);
  }

  render() {
    // This syntax ensures `this` is bound within handleClick
    return (
      <button onClick={(e) => this.handleClick(e)}>
        Click me
      </button>
    );
  }
}
```

The problem with this syntax is that a different callback is created each time the `LoggingButton` renders. In most cases, this is fine. However, if this callback is passed as a prop to lower components, those components might do an extra re-rendering. We generally recommend binding in the constructor or using the class fields syntax, to avoid this sort of performance problem.

## Passing Arguments to Event Handlers {#passing-arguments-to-event-handlers}

Inside a loop it is common to want to pass an extra parameter to an event handler. For example, if `id` is the row ID, either of the following would work:

```js
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```

The above two lines are equivalent, and use [arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions) and [`Function.prototype.bind`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_objects/Function/bind) respectively.

In both cases, the `e` argument representing the React event will be passed as a second argument after the ID. With an arrow function, we have to pass it explicitly, but with `bind` any further arguments are automatically forwarded.
