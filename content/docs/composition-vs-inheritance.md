---
id: composition-vs-inheritance
title: Composição vs. Herança
permalink: docs/composition-vs-inheritance.html
redirect_from:
  - "docs/multiple-components.html"
prev: lifting-state-up.html
next: thinking-in-react.html
---

O React tem um poderoso modelo de composição, e por isso recomendamos o uso de composição ao invés de herança para reutilizar código entre componentes.

Nesta seção, iremos demonstrar alguns problemas encontrados pelos desenvolvedores que estão a iniciar com o React e deparam-swe em situações com herança, e mostraremos como podemos resolver utilizando composição.

## Contenção {#containment}

Alguns componentes não tem como saber quem serão seus elementos filhos. Isto é muito comum para componentes como o `SideBar` ou `Dialog` que representam "caixas" genéricas.

Recomendamos que esses componentes utilizem a prop especial `children` para passar os elementos filhos diretos para sua respectiva saída:

```js{4}
function FancyBorder(props) {
  return (
    <div className={'FancyBorder FancyBorder-' + props.color}>
      {props.children}
    </div>
  );
}
```

Isto permite outros componentes passar elementos filhos no próprio JSX:

```js{4-9}
function WelcomeDialog() {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        Bem-vindo
      </h1>
      <p className="Dialog-message">
        Obrigado por visitar a nossa nave-espacial!
      </p>
    </FancyBorder>
  );
}
```

**[Experimente no CodePen](https://codepen.io/gaearon/pen/ozqNOV?editors=0010)**

Qualquer conteúdo dentro da tag JSX do componente `<FancyBorder>` vai ser passado ao componente `FancyBorder` como prop `children`. Desde que `FancyBorder` renderize a `{props.children}` dentro de uma `<div>`, os elementos serão renderizados no resultado final.

Mesmo que seja não seja comum, às vezes pode ser que precises de diversos "buracos" no componente. Em alguns casos podes criar a tua própria convenção e não utilizar `children`:

```js{5,8,18,21}
function SplitPane(props) {
  return (
    <div className="SplitPane">
      <div className="SplitPane-left">
        {props.left}
      </div>
      <div className="SplitPane-right">
        {props.right}
      </div>
    </div>
  );
}

function App() {
  return (
    <SplitPane
      left={
        <Contacts />
      }
      right={
        <Chat />
      } />
  );
}
```

[**Experimente no CodePen**](https://codepen.io/gaearon/pen/gwZOJp?editors=0010)

React elements like `<Contacts />` and `<Chat />` are just objects, so you can pass them as props like any other data. This approach may remind you of "slots" in other libraries but there are no limitations on what you can pass as props in React.

## Specialization {#specialization}

Sometimes we think about components as being "special cases" of other components. For example, we might say that a `WelcomeDialog` is a special case of `Dialog`.

In React, this is also achieved by composition, where a more "specific" component renders a more "generic" one and configures it with props:

```js{5,8,16-18}
function Dialog(props) {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        {props.title}
      </h1>
      <p className="Dialog-message">
        {props.message}
      </p>
    </FancyBorder>
  );
}

function WelcomeDialog() {
  return (
    <Dialog
      title="Welcome"
      message="Thank you for visiting our spacecraft!" />
  );
}
```

[**Experimente no CodePen**](https://codepen.io/gaearon/pen/kkEaOZ?editors=0010)

Composition works equally well for components defined as classes:

```js{10,27-31}
function Dialog(props) {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        {props.title}
      </h1>
      <p className="Dialog-message">
        {props.message}
      </p>
      {props.children}
    </FancyBorder>
  );
}

class SignUpDialog extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.handleSignUp = this.handleSignUp.bind(this);
    this.state = {login: ''};
  }

  render() {
    return (
      <Dialog title="Mars Exploration Program"
              message="How should we refer to you?">
        <input value={this.state.login}
               onChange={this.handleChange} />
        <button onClick={this.handleSignUp}>
          Sign Me Up!
        </button>
      </Dialog>
    );
  }

  handleChange(e) {
    this.setState({login: e.target.value});
  }

  handleSignUp() {
    alert(`Welcome aboard, ${this.state.login}!`);
  }
}
```

[**Experimente no CodePen**](https://codepen.io/gaearon/pen/gwZbYa?editors=0010)

## So What About Inheritance? {#so-what-about-inheritance}

At Facebook, we use React in thousands of components, and we haven't found any use cases where we would recommend creating component inheritance hierarchies.

Props and composition give you all the flexibility you need to customize a component's look and behavior in an explicit and safe way. Remember that components may accept arbitrary props, including primitive values, React elements, or functions.

If you want to reuse non-UI functionality between components, we suggest extracting it into a separate JavaScript module. The components may import it and use that function, object, or a class, without extending it.
