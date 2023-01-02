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

Nesta seção, iremos demonstrar alguns problemas encontrados pelos desenvolvedores que estão a iniciar com o React e deparam-se em situações com herança, e mostraremos como podemos resolver utilizando composição.

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

**[Experimenta no CodePen](https://codepen.io/gaearon/pen/ozqNOV?editors=0010)**

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

[**Experimenta no CodePen**](https://codepen.io/gaearon/pen/gwZOJp?editors=0010)

Os elementos em React como `<Contacts/>` e `<Chat/>` são apenas objetos, e podes passá-los como props assim como fazes com outros tipos de dados. Esta abordagem pode soar familiar como "slots" em outras bibliotecas, mas em React não existe limitações sobre o que pode ser passado como props.

## Especialização {#specialization}

Algumas vezes acabamos pensando em componentes como "casos especiais" de outros componentes, por exemplo, podemos dizer que o componente `WelcomeDialog` é um caso especial de `Dialog`.

Em React, isto também pode ser obtido através do uso de composição, um componente específico renderiza um componente mais "genérico" e o configura com as suas respectivas props:

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
      title="Bem-vindo"
      message="Obrigado por visitar a nossa nave-espacial!" />
  );
}
```

[**Experimenta no CodePen**](https://codepen.io/gaearon/pen/kkEaOZ?editors=0010)

A composição também irá funcionar para componentes escritos como classes:

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
      <Dialog title="Programa de Exploração de Marte"
              message="Como gostarias de ser chamado?">
        <input value={this.state.login}
               onChange={this.handleChange} />
        <button onClick={this.handleSignUp}>
          Cadastra-te!
        </button>
      </Dialog>
    );
  }

  handleChange(e) {
    this.setState({login: e.target.value});
  }

  handleSignUp() {
    alert(`Bem-vindo à bordo, ${this.state.login}!`);
  }
}
```

[**Experimenta no CodePen**](https://codepen.io/gaearon/pen/gwZbYa?editors=0010)

## E em relação a herança? {#so-what-about-inheritance}

No Facebook, nós usamos o React em milhares de componentes, e não encontramos nenhum caso que recomendaríamos criar componentes utilizando hierarquia de herança.

O uso de props e composição irá te dar toda flexibilidade que precisas para personalizar o comportamento e aparência dos componentes, de uma maneira explícita e segura. Lembra-te de que os componentes podem aceitar um número variável de props, incluindo valores primitivos, como int, array, boolean; assim como elementos Reacts e funções.

<<<<<<< HEAD
 E se quiseres reutilizar funcionalidades (não gráficas) entre componentes, sugerimos que as extraias em módulos JavaScript. Os componentes podem importar essa função, objeto ou classe sem precisar herdá-la.
=======
If you want to reuse non-UI functionality between components, we suggest extracting it into a separate JavaScript module. The components may import it and use that function, object, or class, without extending it.
>>>>>>> e77ba1e90338ff18f965c9b94c733b034b3ac18f
