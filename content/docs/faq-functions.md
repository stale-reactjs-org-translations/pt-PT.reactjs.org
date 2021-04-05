---
id: faq-functions
title: Passando Funções para Componentes
permalink: docs/faq-functions.html
layout: docs
category: FAQ
---

### Como passar um manipulador de eventos (como onClick) para um componente? {#how-do-i-pass-an-event-handler-like-onclick-to-a-component}

Passe manipuladores de evento e outras funções como props para componentes filhos:

```jsx
<button onClick={this.handleClick}>
```

Se precisas ter acesso ao componente pai no manipulador, também precisas fazer _bind_ de uma função para a instância do componente (Vê abaixo).

### Como fazer _bind_ de uma função para a instância de um componente? {#how-do-i-bind-a-function-to-a-component-instance}

Dependendo da sintaxe e etapas de build que estás a usar, existem diversas maneiras de garantir que as funções tenham acesso aos atributos dos componentes como `this.props` e `this.state`.

#### _Bind_ no `constructor` (ES2015) {#bind-in-constructor-es2015}

```jsx
class Foo extends Component {
  constructor(props) {
    super(props);
    this.handleClick = this.handleClick.bind(this);
  }
  handleClick() {
    console.log('Clicado');
  }
  render() {
    return <button onClick={this.handleClick}>Clica-me</button>;
  }
}
```

#### Propriedades de Classe (Proposta de Estágio 3) {#class-properties-stage-3-proposal}

```jsx
class Foo extends Component {
  // Nota: esta sintaxe é experimental e ainda não foi padronizada.
  handleClick = () => {
    console.log('Clicado');
  }
  render() {
    return <button onClick={this.handleClick}>Clica-me</button>;
  }
}
```

#### _Bind_ no `render` {#bind-in-render}

```jsx
class Foo extends Component {
  handleClick() {
    console.log('Clicado');
  }
  render() {
    return <button onClick={this.handleClick.bind(this)}>Clica-me</button>;
  }
}
```

>**Nota:**
>
>Ao usar `Function.prototype.bind` no render, uma nova função é criada cada vez que o componente é renderizado, o que pode afetar a performance (Vê abaixo).

#### _Arrow function_ no `render` {#arrow-function-in-render}

```jsx
class Foo extends Component {
  handleClick() {
    console.log('Clicado');
  }
  render() {
    return <button onClick={() => this.handleClick()}>Clica-me</button>;
  }
}
```

>**Nota:**
>
>Ao usar uma arrow function no render, uma nova função é criada cada vez que o componente é renderizado, que pode quebrar otimizações com base em comparação de identidade `on strict`.

### Não faz mal usar _arrow functions_ em métodos de `render`? {#is-it-ok-to-use-arrow-functions-in-render-methods}

De um modo geral, não, não faz mal. E muitas das vezes é a maneira mais fácil de passar parâmetros para funções de callback.

Se tiveres problemas de performance, procure sempre otimizar!

### Porquê _binding_ é necessário afinal? {#why-is-binding-necessary-at-all}

Em JavaScript, estes dois trechos de código **não** são equivalentes:

```js
obj.method();
```

```js
var method = obj.method;
method();
```

Métodos de _binding_ ajudam a garantir que o segundo trecho funcione da mesma maneira que o primeiro.

Com React, geralmente precisas fazer _bind_ apenas nos métodos que *tu passas* para outros componentes. Por exemplo, `<button onClick={this.handleClick}>` passa `this.handleCLick` logo deve fazer _bind_ nele. Entretanto, não é necessário usar _bind_ no método `render` ou nos métodos do ciclo de vida: nós não passamos ele à outros componentes.

[Este post de Yehuda Katz](https://yehudakatz.com/2011/08/11/understanding-javascript-function-invocation-and-this/) explica o que é _binding_ e como funcionam as funções do JavaScript, em detalhes.

### Porquê a minha função é invocada sempre que o componente renderiza? {#why-is-my-function-being-called-every-time-the-component-renders}

Certifique-se de não estar a _invocar a função_ quando passas para o componente:

```jsx
render() {
  // Errado: handleClick é invocado ao invés de ser passado como referência!
  return <button onClick={this.handleClick()}>Clica-me</button>
}
```

Em vez disso, *passe a própria função* (sem parentêses):

```jsx
render() {
  // Correto: handleClick é passado como referência!
  return <button onClick={this.handleClick}>Clica-me</button>
}
```

### Como passar um parâmetro para um manipulador de evento ou um callback? {#how-do-i-pass-a-parameter-to-an-event-handler-or-callback}

Podes usar uma _arrow function_ para envolver um manipulador de eventos e passar parâmetros:

```jsx
<button onClick={() => this.handleClick(id)} />
```

Isto é equivalente à chamar o `.bind`:

```jsx
<button onClick={this.handleClick.bind(this, id)} />
```

#### Exemplo: Passando parâmetros usando _arrow functions_ {#example-passing-params-using-arrow-functions}

```jsx
const A = 65 // cógido de caractere ASCII

class Alphabet extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      justClicked: null,
      letters: Array.from({length: 26}, (_, i) => String.fromCharCode(A + i))
    };
  }
  handleClick(letter) {
    this.setState({ justClicked: letter });
  }
  render() {
    return (
      <div>
        Acabaste de clicar: {this.state.justClicked}
        <ul>
          {this.state.letters.map(letter =>
            <li key={letter} onClick={() => this.handleClick(letter)}>
              {letter}
            </li>
          )}
        </ul>
      </div>
    )
  }
}
```

#### Exemplo: Passando parâmetros usando _data-attributes_ {#example-passing-params-using-data-attributes}

Em vez disso, podes usar APIs do DOM para guardar dados necessários para manipuladores de evento. Considera esta abordagem caso precises otimizar um grande número de elementos ou caso possuas uma árvore de renderização que depende de verificações de igualdade do React.PureComponent.

```jsx
const A = 65 // cógido de caractere ASCII

class Alphabet extends React.Component {
  constructor(props) {
    super(props);
    this.handleClick = this.handleClick.bind(this);
    this.state = {
      justClicked: null,
      letters: Array.from({length: 26}, (_, i) => String.fromCharCode(A + i))
    };
  }

  handleClick(e) {
    this.setState({
      justClicked: e.target.dataset.letter
    });
  }

  render() {
    return (
      <div>
        Acabaste de clicar: {this.state.justClicked}
        <ul>
          {this.state.letters.map(letter =>
            <li key={letter} data-letter={letter} onClick={this.handleClick}>
              {letter}
            </li>
          )}
        </ul>
      </div>
    )
  }
}
```

### Como evitar que uma função seja invocada muito rapidamente ou chamada muitas vezes em seguida? {#how-can-i-prevent-a-function-from-being-called-too-quickly-or-too-many-times-in-a-row}

Se tens um manipulador de eventos como `onClick` ou `onScroll` e queres evitar que o callback seja ativado muito rapidamente, então podes limitar a frequência com a qual o callback é executado. Isso pode ser feito usando:

- **throttling**: amostra de mudanças com base em uma frequência baseada no tempo (exemplo: [`_.throttle`](https://lodash.com/docs#throttle))
- **debouncing**: publica alterações após um período de inatividade (exemplo: [`_.debounce`](https://lodash.com/docs#debounce))
- **`requestAnimationFrame` throttling**: amostra de mudanças baseadas em [`requestAnimationFrame`](https://developer.mozilla.org/en-US/docs/Web/API/window/requestAnimationFrame) (exemplo: [`raf-schd`](https://github.com/alexreardon/raf-schd))

Vê [esta visualização](http://demo.nimius.net/debounce_throttle/) para uma comparação entre as funções `throttle` e `debounce`.

> Nota:
>
> `_.debounce`, `_.throttle` e `raf-schd` fornecem um método `cancel` para cancelar callbacks atrasados. Deves chamar este método a partir de `componentWillUnmount` _ou_ verificar se o componente ainda está montado dentro da função atrasada.

#### Throttle {#throttle}

_Throttling_ impede a função de ser chamada mais de uma vez em um certo intervalo de tempo. O exemplo abaixo regula o manipulador do evento "click" para impedí-lo de ser chamado mais de uma vez por segundo.

```jsx
import throttle from 'lodash.throttle';

class LoadMoreButton extends React.Component {
  constructor(props) {
    super(props);
    this.handleClick = this.handleClick.bind(this);
    this.handleClickThrottled = throttle(this.handleClick, 1000);
  }

  componentWillUnmount() {
    this.handleClickThrottled.cancel();
  }

  render() {
    return <button onClick={this.handleClickThrottled}>Carregar mais</button>;
  }

  handleClick() {
    this.props.loadMore();
  }
}
```

#### Debounce {#debounce}

_Debouncing_ garante que a função não vai ser executada até que um certo período de tempo tenha passado desde sua última chamada. Isso pode ser útil quando tens que executar algum cálculo pesado em resposta à um evento que pode despachar rapidamente (exemplo: rolagem do mouse ou evento de teclas). O exemplo abaixo atualiza o texto com um atraso de 250ms.

```jsx
import debounce from 'lodash.debounce';

class Searchbox extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.emitChangeDebounced = debounce(this.emitChange, 250);
  }

  componentWillUnmount() {
    this.emitChangeDebounced.cancel();
  }

  render() {
    return (
      <input
        type="text"
        onChange={this.handleChange}
        placeholder="A procurar..."
        defaultValue={this.props.value}
      />
    );
  }

  handleChange(e) {
    this.emitChangeDebounced(e.target.value);
  }

  emitChange(value) {
    this.props.onChange(value);
  }
}
```

#### `requestAnimationFrame` throttling {#requestanimationframe-throttling}

[`requestAnimationFrame`](https://developer.mozilla.org/en-US/docs/Web/API/window/requestAnimationFrame) é uma maneira de enfileirar uma função para ser executada no navegador no tempo ideal para a performance de renderização. A função que é enfileirada com `requestAnimationFrame` vai disparar no próximo frame. O navegador esforça-se para garantir que haja 60 frames por segundo (60 fps). Entretanto, se o navegador for incapaz disso, vai naturalmente *limitar* a quantidade de frames por segundo. Por exemplo, um dispostivo pode ser capaz de aguentar apenas 30fps e então só terás 30 frames por segundo. Usar `requestAnimationFrame` para _throttling_ é uma técnica útil para impedir que faças mais de 60 atualizações em um segundo. Se estás a fazer 100 atualizações em um segundo, isso cria trabalho extra para o navegador que o utilizador nem será capaz de ver.

>**Nota:**
>
>Usar esta técnica capturará apenas o último valor publicado em um frame. Podes ver um exemplo de como esta otimização funciona em [`MDN`](https://developer.mozilla.org/en-US/docs/Web/Events/scroll)

```jsx
import rafSchedule from 'raf-schd';

class ScrollListener extends React.Component {
  constructor(props) {
    super(props);

    this.handleScroll = this.handleScroll.bind(this);

    // Create a new function to schedule updates.
    this.scheduleUpdate = rafSchedule(
      point => this.props.onScroll(point)
    );
  }

  handleScroll(e) {
    // When we receive a scroll event, schedule an update.
    // If we receive many updates within a frame, we'll only publish the latest value.
    this.scheduleUpdate({ x: e.clientX, y: e.clientY });
  }

  componentWillUnmount() {
    // Cancel any pending updates since we're unmounting.
    this.scheduleUpdate.cancel();
  }

  render() {
    return (
      <div
        style={{ overflow: 'scroll' }}
        onScroll={this.handleScroll}
      >
        <img src="/my-huge-image.jpg" />
      </div>
    );
  }
}
```

#### Testando a frequência limite {#testing-your-rate-limiting}

Ao testar o teu código de limitação de frequência, é útil ter a capacidade de avançar o tempo. Se usas [`jest`](https://facebook.github.io/jest/) então podes usar [`mock timers`](https://facebook.github.io/jest/docs/en/timer-mocks.html) para avançar o tempo. Se usas `requestAnimationFrame` _throttling_ podes achar [`raf-stub`](https://github.com/alexreardon/raf-stub) uma ferramenta útil para controlar o pulsar dos frames das animações.
