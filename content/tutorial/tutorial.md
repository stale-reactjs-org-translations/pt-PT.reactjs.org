---
id: tutorial
title: "Tutorial: Intro to React"
layout: tutorial
sectionid: tutorial
permalink: tutorial/tutorial.html
redirect_from:
  - "docs/tutorial.html"
  - "docs/why-react.html"
  - "docs/tutorial-ja-JP.html"
  - "docs/tutorial-ko-KR.html"
  - "docs/tutorial-zh-CN.html"
---

Este tutorial não assume nenhum conhecimento existente de React.

## Antes de começarmos o tutorial {#before-we-start-the-tutorial}

Vamos construir um pequeno jogo durante este tutorial. **Poderá estar tentado a ignorar-lo porque não vai construir jogos -- Dê uma chance.** TAs técnicas que você aprenderá no tutorial são fundamentais para criar qualquer aplicativo React e o domínio delas lhe dará um profundo entendimento de React.

>Dica
>
>Este tutorial é feito para pessoas que preferem **aprender fazendo**. Se preferir aprender conceitos do zero, confira o nosso [step-by-step guia](/docs/hello-world.html). Poderá achar que este tutorial e guia complementam-se um ao outro.

O tutorial está dividido em várias seções:

* [Configuração do Tutorial](#setup-for-the-tutorial) vai lhe dar **um ponto de partida** to follow the tutorial.
* [Visão geral](#overview) Vai lhe ensinar **os fundamentais** de React: components, props, e state.
* [Completar o jogo](#completing-the-game) vai lhe ensinar **as têcnicas mais comuns** do desenvolvimento React.
* [Adicionando Time Travel (viagem no tempo)](#adding-time-travel) vai lhe dar **uma visão mais aprofundada** dos pontos fortes de React.

Não precisa completar todas as seções de uma vez para entender tudo que o tutorial tem a oferecer. Tente chegar o mais longe possível, mesmo que seja uma ou duas seções.

### O que vamos construir? {#what-are-we-building}

Neste tutorial, mostraremos como criar um jogo interativo de jogo-da-velha com React.

Pode ver o que vamos construir aqui:: **[Resultado Final](https://codepen.io/gaearon/pen/gWWZgR?editors=0010)**. Se o código não fizer sentido ou se não estiver familiarizado com a sintaxe do código, não se preocupe! O objetivo deste tutorial é ajudar a entender o React e sua sintaxe.

Recomendamos que confira o jogo tic-tac-toe (jogo da velha) antes de continuar com o tutorial. Uma das características que notará é que existe uma lista numerada à direita do tabuleiro do jogo. Esta lista fornece um histórico de todas as jogadas que ocorreram no jogo e é atualizada à medida que o jogo avança.

Pode fechar o jogo da velha assim que estiver familiarizado com ele. Começaremos a partir de um modelo mais simples neste tutorial. Nosso próximo passo é prepará-lo para que possa começar a desenvolver o jogo.

### Pré-requisitos {#prerequisites}

Vamos presumir que você já tenha alguma familiaridade com HTML e JavaScript. Porém, deve ser capaz de acompanhá-lo mesmo que esteja vindo de uma linguagem de programação diferente. Também vamos presumir que já esteja familiarizado com conceitos de programação. Tais como: funções, objetos, matrizes em menor escala e classes.

Se precisa rever JavaScript, recomendamos ler [this guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript). Observe que também estamos usando alguns recursos do ES6 - uma versão recente do JavaScript. Neste tutorial, estamos usando [arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions), [classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes), [`let`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let), e declarações [`const`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const). Pode usar o [Babel REPL](babel://es5-syntax-example) para verificar o código ES6 compilado.

## Configuração para o tutorial {#setup-for-the-tutorial}

Há duas maneiras de concluir este tutorial: você pode escrever o código em seu navegador ou configurar um ambiente de desenvolvimento local em seu computador.

### Opção de Configuração 1: Desenvolver o Código no Navegador {#setup-option-1-write-code-in-the-browser}

Esta é a maneira mais rápida de começar!

Primeiro, abre este **[Código Inicial](https://codepen.io/gaearon/pen/oWWQNa?editors=0010)** numa nova  tab (aba). A nova tab (aba) deve exibir um tabuleiro de tic-tac-toe (jogo da velha) vazio e o código React. Nós iremos editar o código React durante o tutorial.

Pode ignorar a segunda opção de configuração e ir para a seção [Visão Geral](#overview) para obter uma visão geral do React.

### Opção de instalação 2: Ambiente de desenvolvimento local {#setup-option-2-local-development-environment}

Isto é completamente opcional e não é necessário para este tutorial!

<br>

<details>

<summary><b>Opcional: Instruções para serem seguidas na sua máquina usando seu editor de texto preferido</b></summary>

Essa opção de configuração requer mais trabalho mas permite que você complete o tutorial utilizando um editor de sua preferência. Aqui estão os passos a serem seguidos:

1. Certifique-se de ter uma versão recente do [Node.js](https://nodejs.org/en/) instalada.
2. Siga [as instruções de instalação do Create React App](/docs/create-a-new-react-app.html#create-react-app) para criar um novo projeto.

```bash
npx create-react-app my-app
```

3. Apague todos os arquivos na pasta src/ do novo projeto

> Nota:
>
>**Não exclua a pasta `src` inteira, Apenas os arquivos originais dentro dela.** Substituiremos os arquivos de origem padrão por exemplos deste projeto na próxima etapa..

```bash
cd my-app
cd src

# Se estiver a usar um Mac ou Linux:
rm -f *

# Ou, ise estuver a usar um Windows:
del *

# De seguida, volte para a pasta do projeto
cd ..
```

4. Adicione um arquivo chamado `index.css` na pasta `src/` com [este código de CSS](https://codepen.io/gaearon/pen/oWWQNa?editors=0100).

5. Adicione um arquivo chamado `index.js` na pasta `src/` com [este código JS](https://codepen.io/gaearon/pen/oWWQNa?editors=0010).

6. Adicione estas três linhas ao topo do `index.js` na pasta `src/`:

```js
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
```

Agora, se executar o `npm start` na pasta do projeto e abrir `http://localhost:3000` ino navegador, verá um campo vazio de jogo da velha.

Recomendamos seguir [estas instruções](https://babeljs.io/docs/editors/) para configurar o realce de sintaxe para seu editor.

</details>

### Ajudem, estou com Dificuldades! {#help-im-stuck}

Se não conseguir proseguir no tutorial por algum motivo, confira os [recursos de suporte da comunidade](/community/support.html). Em particular, o [Chat Reactiflux](https://discord.gg/reactiflux) é uma ótima maneira de obter ajuda rapidamente. Se não receber uma resposta ou se permanecer preso, registre um problema e nós o ajudaremos.

## Vista Geral {#overview}

Agora que tudo está configurado, vamos obter uma visão geral do React!

### O que é React? {#what-is-react}

React é uma biblioteca JavaScript declarativa, eficiente e flexível para criar interfaces de usuário. Permite compor UIs complexas a partir de pequenos e isolados códigos chamados "componentes".

React possui alguns tipos diferentes de componentes, mas começaremos com subclasses `React.Component`:

```javascript
class ShoppingList extends React.Component {
  render() {
    return (
      <div className="shopping-list">
        <h1>Shopping List for {this.props.name}</h1>
        <ul>
          <li>Instagram</li>
          <li>WhatsApp</li>
          <li>Oculus</li>
        </ul>
      </div>
    );
  }
}

// Exemplo de uso:: <ShoppingList name="Marco" />
```
Falaremos sobre formatos engraçados de tags que possuem formato de XML em breve. Usaremos componentes para dizer ao React o que queremos ver na tela. Quando os nossos dados são alterados, o React atualizará e renderizará novamente com eficiência os nossos componentes.

Aqui, ShoppingList é um **componente React de class**, ou **componente React do tipo class**. O componente recebe parâmetros, chamados `props` (abreviação de "propriedades"), e retorna uma hierarquia de elementos para exibir através do método `render`.

O método `render` retorna uma *descrição* do que você deseja ver na ecrã. React pega na descrição e mostra o resultado. Em particular, `render` retorna um **elemento React**, que é uma descrição simplificada do que renderizar. A maioria dos desenvolvedores de React usa uma sintaxe especial chamada "JSX", que facilita a escrita desses elementos. A sintaxe `<div />` é transformada em tempo de compilação para `React.createElement('div')`. O exemplo acima é equivalente a:

```javascript
return React.createElement('div', {className: 'shopping-list'},
  React.createElement('h1', /* ... h1 children ... */),
  React.createElement('ul', /* ... ul children ... */)
);
```

[Veja a versão completa.](babel://tutorial-expanded-version)

Se estiver curioso, `createElement()` é descrito em mais detalhes na [referência da API](/docs/react-api.html#createelement), mas não iremos usá-lo neste tutorial. Em vez disso, continuaremos usando o JSX.

JSX vem com todo o poder do JavaScript. Você pode colocar *qualquer* expressões JavaScript dentro de chaves no JSX. Cada elemento React é um objeto JavaScript que você pode armazenar em uma variável ou passar em seu código.

O componente `ShoppingList` acima apenas renderiza componentes internos do DOM como `<div />` e `<li />`. Mas também pode compor e renderizar componentes React personalizados. Por exemplo, agora podemos nos referir a toda a lista de compras escrevendo `<ShoppingList />`. Cada componente React é encapsulado e pode operar de forma independente; Isso permite que você construa interfaces complexas a partir de componentes simples.

## Inspecionando o Código Inicial {#inspecting-the-starter-code}

Se for trabalhar no tutorial **em seu navegador**, abra esse código em uma nova tab (aba): **[Código Inicial](https://codepen.io/gaearon/pen/oWWQNa?editors=0010)**. Se for trabalhar no tutorial **localmente,** abra a pasta `src/index.js` do projeto (já editou este arquivo durante o [setup](#setup-option-2-local-development-environment)).

Este Código Inicial é a base do que estamos construindo. Fornecemos o estilo CSS para que só precise de se concentrar em aprender React e na programação do jogo da velha.

Ao inspecionar o código, notará que temos três componentes React:

* Square (Quadrado)
* Board (Tabuleiro)
* Game (Jogo)

O componente Square renderiza um único `<button>` ae o Board renderiza 9 squares. O componente Game renderiza um Board com valores que modificaremos mais tarde. Atualmente não há componentes interativos.

### Passando dados através de props {#passing-data-through-props}

Para aquecer, vamos tentar passar alguns dados do nosso componente Board para o nosso componente Square.

É altamente recomendável digitar o código manualmente, enquanto você está trabalhando no tutorial e não usando copiar/colar. Isso ajudará a desenvolver a memória muscular e um melhor entendimento.

No método Board's `renderSquare`, altere o código para passar um prop chamado `value` para o Square:

```js{3}
class Board extends React.Component {
  renderSquare(i) {
    return <Square value={i} />;
  }
}
```

Altere o método Square's `render` para mostrar o valor substituindo `{/* TODO */}` com `{this.props.value}`:

```js{5}
class Square extends React.Component {
  render() {
    return (
      <button className="square">
        {this.props.value}
      </button>
    );
  }
}
```

Antes:

![React Devtools](../images/tutorial/tictac-empty.png)

Depois: Deve ver um número em cada quadrado.

![React Devtools](../images/tutorial/tictac-numbers.png)

**[Ver o código completo até este ponto](https://codepen.io/gaearon/pen/aWWQOG?editors=0010)**

Parabéns! Você acabou de "passar um prop" de um componente pai Board para um componente filho Square. Passar props é a forma como os dados fluem em aplicações React, de pais para filhos.

### Fazendo um componente interativo {#making-an-interactive-component}

Vamos preencher o componente Square com um "X" quando clicamos nele. Primeiro, altere a tag
`button` que é retornada na função `render()` do componente Square para isto:

```javascript{4}
class Square extends React.Component {
  render() {
    return (
      <button className="square" onClick={function() { alert('click'); }}>
        {this.props.value}
      </button>
    );
  }
}
```

Se clicar em um quadrado agora, deverá ver um alerta no seu navegador.

>Nota
>
>TPara salvar a digitação e evitar o [comportamento confuso de `this`](https://yehudakatz.com/2011/08/11/understanding-javascript-function-invocation-and-this/), vamos usar a [sintaxe arrow function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions) para manipuladores de eventos:
>
>```javascript{4}
>class Square extends React.Component {
>  render() {
>    return (
>      <button className="square" onClick={() => alert('click')}>
>        {this.props.value}
>      </button>
>    );
>  }
>}
>```
>
>Note que com `onClick={() => alert('click')}`, estamos passando *uma função* como prop `onClick`. React só chamará essa função depois de um clique. Esquecendo `() =>` e escrevendo somente `onClick={alert('click')}` é um erro comum, e dispararia o alerta toda vez que o componente fosse renderizado novamente.

Como próximo passo, queremos que o componente Square "se lembre" que foi clicado e preencha com um "X". Para "lembrar" as coisas, os componentes usam o **state** (estado).

Os componentes React podem ter estado (state) configurando `this.state` em seus construtores. `this.state` deve ser considerado como privado para o componente React que o definiu. Vamos armazenar o valor atual do Square em `this.state`, e alterá-lo quando o Square for clicado.

Primeiro, adicionaremos um construtor à classe para inicializar o estado:

```javascript{2-7}
class Square extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: null,
    };
  }

  render() {
    return (
      <button className="square" onClick={() => alert('click')}>
        {this.props.value}
      </button>
    );
  }
}
```

>Nota
>
>Em [classes JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes), precisa sempre chamar `super` o definir o construtor de uma subclasse. ATodas os componentes de classe React que possuem um método `constructor` devem iniciá-lo com uma chamada `super(props)`.

Agora vamos mudar o método `render` do componente Square para exibir o valor do estado (state) atual quando clicado:

* Substitua `this.props.value` com `this.state.value` dentro da tag `<button>`.
* Substitua o evento `onClick={...}` com `onClick={() => this.setState({value: 'X'})}`.
* Coloque `className` e `onClick` props em linhas separadas para melhor leitura.

Após estas mudanças, a tag `<button>` tque é retornada pelo método `render` do Square deve se parecer com o seguinte:

```javascript{12-13,15}
class Square extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: null,
    };
  }

  render() {
    return (
      <button
        className="square"
        onClick={() => this.setState({value: 'X'})}
      >
        {this.state.value}
      </button>
    );
  }
}
```

Ao chamar `this.setState` a partir de um evento `onClick` no método `render` do componente Square, dizemos ao React para renderizar novamente aquele Square sempre que seu `<button>` for clicado. Após a atualização, o `this.state.value` do Square será `'X'`, então vamos ver o `X` no tabuleiro do jogo. Se você clicar em qualquer quadrado, um `X` deve aparecer.

Quando você chama `setState` em um componente, o também React atualiza automaticamente os componentes filhos dentro dele.

**[Ver código completo até este momento](https://codepen.io/gaearon/pen/VbbVLg?editors=0010)**

### Developer Tools {#developer-tools}

A extensão React Devtools para [Chrome](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en) e [Firefox](https://addons.mozilla.org/en-US/firefox/addon/react-devtools/) permite inspecionar uma árvore de componentes React com as ferramentas de desenvolvedor do seu navegador.

<img src="../images/tutorial/devtools.png" alt="React Devtools" style="max-width: 100%">

O React DevTools permite que vereficar as props e o estado (state) de seus componentes React.

Depois de instalar o React DevTools, você pode clicar com o botão direito do mouse em qualquer elemento da página, clicar em "Inspecionar" para abrir as ferramentas de desenvolvedor, e as guias React ("⚛️ Components" e "⚛️ Profiler") aparecerá como a última guia à direita. Use "⚛️ Components" para inspecionar a árvore de componentes.

**No entanto, observe que há algumas etapas extras para a extensão funcionar com o CodePen:**

1. Faça o login ou registre-se e confirme seu e-mail (necessário para evitar spam).
2. Clique no botão "Fork".
3. Clique em "Change View" e escolha "Debug mode".
4. Na nova aba que se abre, o devtools deve agora ter uma aba React.

## Completando o jogo {#completing-the-game}

Agora temos os blocos básicos para contrução do nosso jogo da velha. Para completar o jogo, precisamos preencher os "X"s e os "O"s no tabuleiro e de alguma maneira necessitamos definir o vencedor.

### Movendo o state para cima {#lifting-state-up}

Atualmente, cada componente Quadrado (Square) mantém o estado do jogo. Para verificar o vencedor, vamos manter o valor de cada um dos 9 quadrados em uma posição.

Podemos pensar que o Tabuleiro (Board) poderia apenas perguntar para cada Quadrado pelo seu estado. Apesar desse modelo ser possível no React, nós o desencorajamos, pois, o código se torna difícil de ser compreendido, suscetível à erros e difícil de refatorar. Ao invés disso, a melhor opção é guardar o estado do jogo no componente pai (Tabuleiro) ao invés de cada Quadrado. O componente do tabuleiro pode dizer para cada Quadrado o que pode ser exibido via prop, [assim como fizemos quando passamos o número de cada Quadrado](#passing-data-through-props).

**To collect data from multiple children, or to have two child components communicate with each other, you need to declare the shared state in their parent component instead. The parent component can pass the state back down to the children by using props; this keeps the child components in sync with each other and with the parent component.**

Lifting state into a parent component is common when React components are refactored -- let's take this opportunity to try it out.

Add a constructor to the Board and set the Board's initial state to contain an array of 9 nulls corresponding to the 9 squares:

```javascript{2-7}
class Board extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      squares: Array(9).fill(null),
    };
  }

  renderSquare(i) {
    return <Square value={i} />;
  }
```

When we fill the board in later, the `this.state.squares` array will look something like this:

```javascript
[
  'O', null, 'X',
  'X', 'X', 'O',
  'O', null, null,
]
```

The Board's `renderSquare` method currently looks like this:

```javascript
  renderSquare(i) {
    return <Square value={i} />;
  }
```

In the beginning, we [passed the `value` prop down](#passing-data-through-props) from the Board to show numbers from 0 to 8 in every Square. In a different previous step, we replaced the numbers with an "X" mark [determined by Square's own state](#making-an-interactive-component). This is why Square currently ignores the `value` prop passed to it by the Board.

We will now use the prop passing mechanism again. We will modify the Board to instruct each individual Square about its current value (`'X'`, `'O'`, or `null`). We have already defined the `squares` array in the Board's constructor, and we will modify the Board's `renderSquare` method to read from it:

```javascript{2}
  renderSquare(i) {
    return <Square value={this.state.squares[i]} />;
  }
```

**[View the full code at this point](https://codepen.io/gaearon/pen/gWWQPY?editors=0010)**

Each Square will now receive a `value` prop that will either be `'X'`, `'O'`, or `null` for empty squares.

Next, we need to change what happens when a Square is clicked. The Board component now maintains which squares are filled. We need to create a way for the Square to update the Board's state. Since state is considered to be private to a component that defines it, we cannot update the Board's state directly from Square.

Instead, we'll pass down a function from the Board to the Square, and we'll have Square call that function when a square is clicked. We'll change the `renderSquare` method in Board to:

```javascript{5}
  renderSquare(i) {
    return (
      <Square
        value={this.state.squares[i]}
        onClick={() => this.handleClick(i)}
      />
    );
  }
```

>Note
>
>We split the returned element into multiple lines for readability, and added parentheses so that JavaScript doesn't insert a semicolon after `return` and break our code.

Now we're passing down two props from Board to Square: `value` and `onClick`. The `onClick` prop is a function that Square can call when clicked. We'll make the following changes to Square:

* Replace `this.state.value` with `this.props.value` in Square's `render` method
* Replace `this.setState()` with `this.props.onClick()` in Square's `render` method
* Delete the `constructor` from Square because Square no longer keeps track of the game's state

After these changes, the Square component looks like this:

```javascript{1,2,6,8}
class Square extends React.Component {
  render() {
    return (
      <button
        className="square"
        onClick={() => this.props.onClick()}
      >
        {this.props.value}
      </button>
    );
  }
}
```

When a Square is clicked, the `onClick` function provided by the Board is called. Here's a review of how this is achieved:

1. The `onClick` prop on the built-in DOM `<button>` component tells React to set up a click event listener.
2. When the button is clicked, React will call the `onClick` event handler that is defined in Square's `render()` method.
3. This event handler calls `this.props.onClick()`. The Square's `onClick` prop was specified by the Board.
4. Since the Board passed `onClick={() => this.handleClick(i)}` to Square, the Square calls `this.handleClick(i)` when clicked.
5. We have not defined the `handleClick()` method yet, so our code crashes. If you click a square now, you should see a red error screen saying something like "this.handleClick is not a function".

>Note
>
>The DOM `<button>` element's `onClick` attribute has a special meaning to React because it is a built-in component. For custom components like Square, the naming is up to you. We could give any name to the Square's `onClick` prop or Board's `handleClick` method, and the code would work the same. In React, it's conventional to use `on[Event]` names for props which represent events and `handle[Event]` for the methods which handle the events.

When we try to click a Square, we should get an error because we haven't defined `handleClick` yet. We'll now add `handleClick` to the Board class:

```javascript{9-13}
class Board extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      squares: Array(9).fill(null),
    };
  }

  handleClick(i) {
    const squares = this.state.squares.slice();
    squares[i] = 'X';
    this.setState({squares: squares});
  }

  renderSquare(i) {
    return (
      <Square
        value={this.state.squares[i]}
        onClick={() => this.handleClick(i)}
      />
    );
  }

  render() {
    const status = 'Next player: X';

    return (
      <div>
        <div className="status">{status}</div>
        <div className="board-row">
          {this.renderSquare(0)}
          {this.renderSquare(1)}
          {this.renderSquare(2)}
        </div>
        <div className="board-row">
          {this.renderSquare(3)}
          {this.renderSquare(4)}
          {this.renderSquare(5)}
        </div>
        <div className="board-row">
          {this.renderSquare(6)}
          {this.renderSquare(7)}
          {this.renderSquare(8)}
        </div>
      </div>
    );
  }
}
```

**[View the full code at this point](https://codepen.io/gaearon/pen/ybbQJX?editors=0010)**

After these changes, we're again able to click on the Squares to fill them, the same as we had before. However, now the state is stored in the Board component instead of the individual Square components. When the Board's state changes, the Square components re-render automatically. Keeping the state of all squares in the Board component will allow it to determine the winner in the future.

Since the Square components no longer maintain state, the Square components receive values from the Board component and inform the Board component when they're clicked. In React terms, the Square components are now **controlled components**. The Board has full control over them.

Note how in `handleClick`, we call `.slice()` to create a copy of the `squares` array to modify instead of modifying the existing array. We will explain why we create a copy of the `squares` array in the next section.

### Why Immutability Is Important {#why-immutability-is-important}

In the previous code example, we suggested that you use the `.slice()` method to create a copy of the `squares` array to modify instead of modifying the existing array. We'll now discuss immutability and why immutability is important to learn.

There are generally two approaches to changing data. The first approach is to *mutate* the data by directly changing the data's values. The second approach is to replace the data with a new copy which has the desired changes.

#### Data Change with Mutation {#data-change-with-mutation}
```javascript
var player = {score: 1, name: 'Jeff'};
player.score = 2;
// Now player is {score: 2, name: 'Jeff'}
```

#### Data Change without Mutation {#data-change-without-mutation}
```javascript
var player = {score: 1, name: 'Jeff'};

var newPlayer = Object.assign({}, player, {score: 2});
// Now player is unchanged, but newPlayer is {score: 2, name: 'Jeff'}

// Or if you are using object spread syntax proposal, you can write:
// var newPlayer = {...player, score: 2};
```

The end result is the same but by not mutating (or changing the underlying data) directly, we gain several benefits described below.

#### Complex Features Become Simple {#complex-features-become-simple}

Immutability makes complex features much easier to implement. Later in this tutorial, we will implement a "time travel" feature that allows us to review the tic-tac-toe game's history and "jump back" to previous moves. This functionality isn't specific to games -- an ability to undo and redo certain actions is a common requirement in applications. Avoiding direct data mutation lets us keep previous versions of the game's history intact, and reuse them later.

#### Detecting Changes {#detecting-changes}

Detecting changes in mutable objects is difficult because they are modified directly. This detection requires the mutable object to be compared to previous copies of itself and the entire object tree to be traversed.

Detecting changes in immutable objects is considerably easier. If the immutable object that is being referenced is different than the previous one, then the object has changed.

#### Determining When to Re-Render in React {#determining-when-to-re-render-in-react}

The main benefit of immutability is that it helps you build _pure components_ in React. Immutable data can easily determine if changes have been made which helps to determine when a component requires re-rendering.

You can learn more about `shouldComponentUpdate()` and how you can build *pure components* by reading [Optimizing Performance](/docs/optimizing-performance.html#examples).

### Function Components {#function-components}

We'll now change the Square to be a **function component**.

In React, **function components** are a simpler way to write components that only contain a `render` method and don't have their own state. Instead of defining a class which extends `React.Component`, we can write a function that takes `props` as input and returns what should be rendered. Function components are less tedious to write than classes, and many components can be expressed this way.

Replace the Square class with this function:

```javascript
function Square(props) {
  return (
    <button className="square" onClick={props.onClick}>
      {props.value}
    </button>
  );
}
```

We have changed `this.props` to `props` both times it appears.

**[View the full code at this point](https://codepen.io/gaearon/pen/QvvJOv?editors=0010)**

>Note
>
>When we modified the Square to be a function component, we also changed `onClick={() => this.props.onClick()}` to a shorter `onClick={props.onClick}` (note the lack of parentheses on *both* sides).

### Taking Turns {#taking-turns}

We now need to fix an obvious defect in our tic-tac-toe game: the "O"s cannot be marked on the board.

We'll set the first move to be "X" by default. We can set this default by modifying the initial state in our Board constructor:

```javascript{6}
class Board extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      squares: Array(9).fill(null),
      xIsNext: true,
    };
  }
```

Each time a player moves, `xIsNext` (a boolean) will be flipped to determine which player goes next and the game's state will be saved. We'll update the Board's `handleClick` function to flip the value of `xIsNext`:

```javascript{3,6}
  handleClick(i) {
    const squares = this.state.squares.slice();
    squares[i] = this.state.xIsNext ? 'X' : 'O';
    this.setState({
      squares: squares,
      xIsNext: !this.state.xIsNext,
    });
  }
```

With this change, "X"s and "O"s can take turns. Try it!

Let's also change the "status" text in Board's `render` so that it displays which player has the next turn:

```javascript{2}
  render() {
    const status = 'Next player: ' + (this.state.xIsNext ? 'X' : 'O');

    return (
      // the rest has not changed
```

After applying these changes, you should have this Board component:

```javascript{6,11-16,29}
class Board extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      squares: Array(9).fill(null),
      xIsNext: true,
    };
  }

  handleClick(i) {
    const squares = this.state.squares.slice();
    squares[i] = this.state.xIsNext ? 'X' : 'O';
    this.setState({
      squares: squares,
      xIsNext: !this.state.xIsNext,
    });
  }

  renderSquare(i) {
    return (
      <Square
        value={this.state.squares[i]}
        onClick={() => this.handleClick(i)}
      />
    );
  }

  render() {
    const status = 'Next player: ' + (this.state.xIsNext ? 'X' : 'O');

    return (
      <div>
        <div className="status">{status}</div>
        <div className="board-row">
          {this.renderSquare(0)}
          {this.renderSquare(1)}
          {this.renderSquare(2)}
        </div>
        <div className="board-row">
          {this.renderSquare(3)}
          {this.renderSquare(4)}
          {this.renderSquare(5)}
        </div>
        <div className="board-row">
          {this.renderSquare(6)}
          {this.renderSquare(7)}
          {this.renderSquare(8)}
        </div>
      </div>
    );
  }
}
```

**[View the full code at this point](https://codepen.io/gaearon/pen/KmmrBy?editors=0010)**

### Declaring a Winner {#declaring-a-winner}

Now that we show which player's turn is next, we should also show when the game is won and there are no more turns to make. Copy this helper function and paste it at the end of the file:

```javascript
function calculateWinner(squares) {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return null;
}
```

Given an array of 9 squares, this function will check for a winner and return `'X'`, `'O'`, or `null` as appropriate.

We will call `calculateWinner(squares)` in the Board's `render` function to check if a player has won. If a player has won, we can display text such as "Winner: X" or "Winner: O". We'll replace the `status` declaration in Board's `render` function with this code:

```javascript{2-8}
  render() {
    const winner = calculateWinner(this.state.squares);
    let status;
    if (winner) {
      status = 'Winner: ' + winner;
    } else {
      status = 'Next player: ' + (this.state.xIsNext ? 'X' : 'O');
    }

    return (
      // the rest has not changed
```

We can now change the Board's `handleClick` function to return early by ignoring a click if someone has won the game or if a Square is already filled:

```javascript{3-5}
  handleClick(i) {
    const squares = this.state.squares.slice();
    if (calculateWinner(squares) || squares[i]) {
      return;
    }
    squares[i] = this.state.xIsNext ? 'X' : 'O';
    this.setState({
      squares: squares,
      xIsNext: !this.state.xIsNext,
    });
  }
```

**[View the full code at this point](https://codepen.io/gaearon/pen/LyyXgK?editors=0010)**

Congratulations! You now have a working tic-tac-toe game. And you've just learned the basics of React too. So *you're* probably the real winner here.

## Adding Time Travel {#adding-time-travel}

As a final exercise, let's make it possible to "go back in time" to the previous moves in the game.

### Storing a History of Moves {#storing-a-history-of-moves}

If we mutated the `squares` array, implementing time travel would be very difficult.

However, we used `slice()` to create a new copy of the `squares` array after every move, and [treated it as immutable](#why-immutability-is-important). This will allow us to store every past version of the `squares` array, and navigate between the turns that have already happened.

We'll store the past `squares` arrays in another array called `history`. The `history` array represents all board states, from the first to the last move, and has a shape like this:

```javascript
history = [
  // Before first move
  {
    squares: [
      null, null, null,
      null, null, null,
      null, null, null,
    ]
  },
  // After first move
  {
    squares: [
      null, null, null,
      null, 'X', null,
      null, null, null,
    ]
  },
  // After second move
  {
    squares: [
      null, null, null,
      null, 'X', null,
      null, null, 'O',
    ]
  },
  // ...
]
```

Now we need to decide which component should own the `history` state.

### Lifting State Up, Again {#lifting-state-up-again}

We'll want the top-level Game component to display a list of past moves. It will need access to the `history` to do that, so we will place the `history` state in the top-level Game component.

Placing the `history` state into the Game component lets us remove the `squares` state from its child Board component. Just like we ["lifted state up"](#lifting-state-up) from the Square component into the Board component, we are now lifting it up from the Board into the top-level Game component. This gives the Game component full control over the Board's data, and lets it instruct the Board to render previous turns from the `history`.

First, we'll set up the initial state for the Game component within its constructor:

```javascript{2-10}
class Game extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      history: [{
        squares: Array(9).fill(null),
      }],
      xIsNext: true,
    };
  }

  render() {
    return (
      <div className="game">
        <div className="game-board">
          <Board />
        </div>
        <div className="game-info">
          <div>{/* status */}</div>
          <ol>{/* TODO */}</ol>
        </div>
      </div>
    );
  }
}
```

Next, we'll have the Board component receive `squares` and `onClick` props from the Game component. Since we now have a single click handler in Board for many Squares, we'll need to pass the location of each Square into the `onClick` handler to indicate which Square was clicked. Here are the required steps to transform the Board component:

* Delete the `constructor` in Board.
* Replace `this.state.squares[i]` with `this.props.squares[i]` in Board's `renderSquare`.
* Replace `this.handleClick(i)` with `this.props.onClick(i)` in Board's `renderSquare`.

The Board component now looks like this:

```javascript{17,18}
class Board extends React.Component {
  handleClick(i) {
    const squares = this.state.squares.slice();
    if (calculateWinner(squares) || squares[i]) {
      return;
    }
    squares[i] = this.state.xIsNext ? 'X' : 'O';
    this.setState({
      squares: squares,
      xIsNext: !this.state.xIsNext,
    });
  }

  renderSquare(i) {
    return (
      <Square
        value={this.props.squares[i]}
        onClick={() => this.props.onClick(i)}
      />
    );
  }

  render() {
    const winner = calculateWinner(this.state.squares);
    let status;
    if (winner) {
      status = 'Winner: ' + winner;
    } else {
      status = 'Next player: ' + (this.state.xIsNext ? 'X' : 'O');
    }

    return (
      <div>
        <div className="status">{status}</div>
        <div className="board-row">
          {this.renderSquare(0)}
          {this.renderSquare(1)}
          {this.renderSquare(2)}
        </div>
        <div className="board-row">
          {this.renderSquare(3)}
          {this.renderSquare(4)}
          {this.renderSquare(5)}
        </div>
        <div className="board-row">
          {this.renderSquare(6)}
          {this.renderSquare(7)}
          {this.renderSquare(8)}
        </div>
      </div>
    );
  }
}
```

We'll update the Game component's `render` function to use the most recent history entry to determine and display the game's status:

```javascript{2-11,16-19,22}
  render() {
    const history = this.state.history;
    const current = history[history.length - 1];
    const winner = calculateWinner(current.squares);

    let status;
    if (winner) {
      status = 'Winner: ' + winner;
    } else {
      status = 'Next player: ' + (this.state.xIsNext ? 'X' : 'O');
    }

    return (
      <div className="game">
        <div className="game-board">
          <Board
            squares={current.squares}
            onClick={(i) => this.handleClick(i)}
          />
        </div>
        <div className="game-info">
          <div>{status}</div>
          <ol>{/* TODO */}</ol>
        </div>
      </div>
    );
  }
```

Since the Game component is now rendering the game's status, we can remove the corresponding code from the Board's `render` method. After refactoring, the Board's `render` function looks like this:

```js{1-4}
  render() {
    return (
      <div>
        <div className="board-row">
          {this.renderSquare(0)}
          {this.renderSquare(1)}
          {this.renderSquare(2)}
        </div>
        <div className="board-row">
          {this.renderSquare(3)}
          {this.renderSquare(4)}
          {this.renderSquare(5)}
        </div>
        <div className="board-row">
          {this.renderSquare(6)}
          {this.renderSquare(7)}
          {this.renderSquare(8)}
        </div>
      </div>
    );
  }
```

Finally, we need to move the `handleClick` method from the Board component to the Game component. We also need to modify `handleClick` because the Game component's state is structured differently. Within the Game's `handleClick` method, we concatenate new history entries onto `history`.

```javascript{2-4,10-12}
  handleClick(i) {
    const history = this.state.history;
    const current = history[history.length - 1];
    const squares = current.squares.slice();
    if (calculateWinner(squares) || squares[i]) {
      return;
    }
    squares[i] = this.state.xIsNext ? 'X' : 'O';
    this.setState({
      history: history.concat([{
        squares: squares,
      }]),
      xIsNext: !this.state.xIsNext,
    });
  }
```

>Note
>
>Unlike the array `push()` method you might be more familiar with, the `concat()` method doesn't mutate the original array, so we prefer it.

At this point, the Board component only needs the `renderSquare` and `render` methods. The game's state and the `handleClick` method should be in the Game component.

**[View the full code at this point](https://codepen.io/gaearon/pen/EmmOqJ?editors=0010)**

### Showing the Past Moves {#showing-the-past-moves}

Since we are recording the tic-tac-toe game's history, we can now display it to the player as a list of past moves.

We learned earlier that React elements are first-class JavaScript objects; we can pass them around in our applications. To render multiple items in React, we can use an array of React elements.

In JavaScript, arrays have a [`map()` method](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) that is commonly used for mapping data to other data, for example:

```js
const numbers = [1, 2, 3];
const doubled = numbers.map(x => x * 2); // [2, 4, 6]
```

Using the `map` method, we can map our history of moves to React elements representing buttons on the screen, and display a list of buttons to "jump" to past moves.

Let's `map` over the `history` in the Game's `render` method:

```javascript{6-15,34}
  render() {
    const history = this.state.history;
    const current = history[history.length - 1];
    const winner = calculateWinner(current.squares);

    const moves = history.map((step, move) => {
      const desc = move ?
        'Go to move #' + move :
        'Go to game start';
      return (
        <li>
          <button onClick={() => this.jumpTo(move)}>{desc}</button>
        </li>
      );
    });

    let status;
    if (winner) {
      status = 'Winner: ' + winner;
    } else {
      status = 'Next player: ' + (this.state.xIsNext ? 'X' : 'O');
    }

    return (
      <div className="game">
        <div className="game-board">
          <Board
            squares={current.squares}
            onClick={(i) => this.handleClick(i)}
          />
        </div>
        <div className="game-info">
          <div>{status}</div>
          <ol>{moves}</ol>
        </div>
      </div>
    );
  }
```

**[View the full code at this point](https://codepen.io/gaearon/pen/EmmGEa?editors=0010)**

For each move in the tic-tac-toes's game's history, we create a list item `<li>` which contains a button `<button>`. The button has a `onClick` handler which calls a method called `this.jumpTo()`. We haven't implemented the `jumpTo()` method yet. For now, we should see a list of the moves that have occurred in the game and a warning in the developer tools console that says:

>  Warning:
>  Each child in an array or iterator should have a unique "key" prop. Check the render method of "Game".

Let's discuss what the above warning means.

### Picking a Key {#picking-a-key}

When we render a list, React stores some information about each rendered list item. When we update a list, React needs to determine what has changed. We could have added, removed, re-arranged, or updated the list's items.

Imagine transitioning from

```html
<li>Alexa: 7 tasks left</li>
<li>Ben: 5 tasks left</li>
```

to

```html
<li>Ben: 9 tasks left</li>
<li>Claudia: 8 tasks left</li>
<li>Alexa: 5 tasks left</li>
```

In addition to the updated counts, a human reading this would probably say that we swapped Alexa and Ben's ordering and inserted Claudia between Alexa and Ben. However, React is a computer program and does not know what we intended. Because React cannot know our intentions, we need to specify a *key* property for each list item to differentiate each list item from its siblings. One option would be to use the strings `alexa`, `ben`, `claudia`. If we were displaying data from a database, Alexa, Ben, and Claudia's database IDs could be used as keys.

```html
<li key={user.id}>{user.name}: {user.taskCount} tasks left</li>
```

When a list is re-rendered, React takes each list item's key and searches the previous list's items for a matching key. If the current list has a key that didn't exist before, React creates a component. If the current list is missing a key that existed in the previous list, React destroys the previous component. If two keys match, the corresponding component is moved. Keys tell React about the identity of each component which allows React to maintain state between re-renders. If a component's key changes, the component will be destroyed and re-created with a new state.

`key` is a special and reserved property in React (along with `ref`, a more advanced feature). When an element is created, React extracts the `key` property and stores the key directly on the returned element. Even though `key` may look like it belongs in `props`, `key` cannot be referenced using `this.props.key`. React automatically uses `key` to decide which components to update. A component cannot inquire about its `key`.

**It's strongly recommended that you assign proper keys whenever you build dynamic lists.** If you don't have an appropriate key, you may want to consider restructuring your data so that you do.

If no key is specified, React will present a warning and use the array index as a key by default. Using the array index as a key is problematic when trying to re-order a list's items or inserting/removing list items. Explicitly passing `key={i}` silences the warning but has the same problems as array indices and is not recommended in most cases.

Keys do not need to be globally unique; they only need to be unique between components and their siblings.


### Implementing Time Travel {#implementing-time-travel}

In the tic-tac-toe game's history, each past move has a unique ID associated with it: it's the sequential number of the move. The moves are never re-ordered, deleted, or inserted in the middle, so it's safe to use the move index as a key.

In the Game component's `render` method, we can add the key as `<li key={move}>` and React's warning about keys should disappear:

```js{6}
    const moves = history.map((step, move) => {
      const desc = move ?
        'Go to move #' + move :
        'Go to game start';
      return (
        <li key={move}>
          <button onClick={() => this.jumpTo(move)}>{desc}</button>
        </li>
      );
    });
```

**[View the full code at this point](https://codepen.io/gaearon/pen/PmmXRE?editors=0010)**

Clicking any of the list item's buttons throws an error because the `jumpTo` method is undefined. Before we implement `jumpTo`, we'll add `stepNumber` to the Game component's state to indicate which step we're currently viewing.

First, add `stepNumber: 0` to the initial state in Game's `constructor`:

```js{8}
class Game extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      history: [{
        squares: Array(9).fill(null),
      }],
      stepNumber: 0,
      xIsNext: true,
    };
  }
```

Next, we'll define the `jumpTo` method in Game to update that `stepNumber`. We also set `xIsNext` to true if the number that we're changing `stepNumber` to is even:

```javascript{5-10}
  handleClick(i) {
    // this method has not changed
  }

  jumpTo(step) {
    this.setState({
      stepNumber: step,
      xIsNext: (step % 2) === 0,
    });
  }

  render() {
    // this method has not changed
  }
```

We will now make a few changes to the Game's `handleClick` method which fires when you click on a square.

The `stepNumber` state we've added reflects the move displayed to the user now. After we make a new move, we need to update `stepNumber` by adding `stepNumber: history.length` as part of the `this.setState` argument. This ensures we don't get stuck showing the same move after a new one has been made.

We will also replace reading `this.state.history` with `this.state.history.slice(0, this.state.stepNumber + 1)`. This ensures that if we "go back in time" and then make a new move from that point, we throw away all the "future" history that would now become incorrect.

```javascript{2,13}
  handleClick(i) {
    const history = this.state.history.slice(0, this.state.stepNumber + 1);
    const current = history[history.length - 1];
    const squares = current.squares.slice();
    if (calculateWinner(squares) || squares[i]) {
      return;
    }
    squares[i] = this.state.xIsNext ? 'X' : 'O';
    this.setState({
      history: history.concat([{
        squares: squares
      }]),
      stepNumber: history.length,
      xIsNext: !this.state.xIsNext,
    });
  }
```

Finally, we will modify the Game component's `render` method from always rendering the last move to rendering the currently selected move according to `stepNumber`:

```javascript{3}
  render() {
    const history = this.state.history;
    const current = history[this.state.stepNumber];
    const winner = calculateWinner(current.squares);

    // the rest has not changed
```

If we click on any step in the game's history, the tic-tac-toe board should immediately update to show what the board looked like after that step occurred.

**[View the full code at this point](https://codepen.io/gaearon/pen/gWWZgR?editors=0010)**

### Wrapping Up {#wrapping-up}

Congratulations! You've created a tic-tac-toe game that:

* Lets you play tic-tac-toe,
* Indicates when a player has won the game,
* Stores a game's history as a game progresses,
* Allows players to review a game's history and see previous versions of a game's board.

Nice work! We hope you now feel like you have a decent grasp on how React works.

Check out the final result here: **[Final Result](https://codepen.io/gaearon/pen/gWWZgR?editors=0010)**.

If you have extra time or want to practice your new React skills, here are some ideas for improvements that you could make to the tic-tac-toe game which are listed in order of increasing difficulty:

1. Display the location for each move in the format (col, row) in the move history list.
2. Bold the currently selected item in the move list.
3. Rewrite Board to use two loops to make the squares instead of hardcoding them.
4. Add a toggle button that lets you sort the moves in either ascending or descending order.
5. When someone wins, highlight the three squares that caused the win.
6. When no one wins, display a message about the result being a draw.

Throughout this tutorial, we touched on React concepts including elements, components, props, and state. For a more detailed explanation of each of these topics, check out [the rest of the documentation](/docs/hello-world.html). To learn more about defining components, check out the [`React.Component` API reference](/docs/react-component.html).
