---
id: faq-functions
title: Passando Funções para Componentes
permalink: docs/faq-functions.html
layout: docs
category: FAQ
---

### Como eu passo um manipulador de eventos (como onClick) para uma componente? {#how-do-i-pass-an-event-handler-like-onclick-to-a-component}

Passa manipuladores de evento e outras funções como propriedades para componentes filho:

```jsx
<button onClick={this.handleClick}>
```

Se, precisas de ter acesso para o componente pai no manipulador, também, precisas de vincular a função à instância do componente (ver abaixo).
.
### Como eu posso vincular uma função a uma instância do componente? {#how-do-i-bind-a-function-to-a-component-instance}

Existem imensas maneiras de ter a certeza de que as funções têm acesso aos atributos do componente, tais como, `this.props` e `this.state`, dependendo de qual síntaxe e passos de construção que estás a usar.

#### Ligar num Construtor (ES2015) {#bind-in-constructor-es2015}

```jsx
class Foo extends Component {
  constructor(props) {
    super(props);
    this.handleClick = this.handleClick.bind(this);
  }
  handleClick() {
    console.log('Clique aconteceu');
  }
  render() {
    return <button onClick={this.handleClick}>Clica-me</button>;
  }
}
```

#### Propriedades da Classe (Estágio 3 Proposta) {#class-properties-stage-3-proposal}

```jsx
class Foo extends Component {
  // Nota: esta sintaxe é experimental e ainda não padronizada.
  handleClick = () => {
    console.log('Clique aconteceu');
  }
  render() {
    return <button onClick={this.handleClick}>Clica-me</button>;
  }
}
```

#### Vincular no Render{#bind-in-render}

```jsx
class Foo extends Component {
  handleClick() {
    console.log('Clique aconteceu');
  }
  render() {
    return <button onClick={this.handleClick.bind(this)}>Clica-me</button>;
  }
}
```

>**Nota:**
>
>Usando `Function.prototype.bind` no render cria uma nova função, de cada vez que, o componente renderiza, o que pode ter implicações no desempenho (vê em baixo).

#### Arrow Function no Render {#arrow-function-in-render}

```jsx
class Foo extends Component {
  handleClick() {
    console.log('Clique aconteceu');
  }
  render() {
    return <button onClick={() => this.handleClick()}>Clica-me</button>;
  }
}
```

>**Nota:**
>
>Usando uma arrow function no render cria uma nova função, de cada vez que, o componente renderiza, o que pode ter implicações no desempenho (vê em baixo).

### Há algum problema em usar as arrow functions em métodos de renderização? {#is-it-ok-to-use-arrow-functions-in-render-methods}

De um modo geral, sim,está tudo bem, e, muitas vezes, a maneira mais fácil de passar parâmetros para as funções de retorno de chamada.

Se, tens problemas de desempenho, por todos os meios, otimiza-o!

### Por que a vinculação é necessária? {#why-is-binding-necessary-at-all}

Em JavaScript, estes dois pedaços de código **não** são equivalentes:

```js
obj.method();
```

```js
var method = obj.method;
method();
```

Métodos de vinculação ajuda a certificar que, o segundo pedaço funciona da mesma forma, que o primeiro.

Com React, tipicamente, só precisas de vincular os métodos que *passas* para outros componentes. Por exemplo, `<button onClick={this.handleClick}>` passa `this.handleClick` para que possas vinculá-lo. Contudo, é  desnecessário vincular o método `render`  ou o ciclo de vida do método: não passamo-los para outros componentes.

[Esta postagem de Yehuda Katz](https://yehudakatz.com/2011/08/11/understanding-javascript-function-invocation-and-this/) explica o que vincular é, e como funções funcionam em JavaScript, em detalhe.

### Porque é que a minha função está a ser chamada, cada vez que, o componente é renderizado? {#why-is-my-function-being-called-every-time-the-component-renders}

Certifica-te de que não estás a chamar a função quando o passas para o componente:

```jsx
render() {
  // Errado: handleClick é chamado em vez de ser passado como uma referência!
  return <button onClick={this.handleClick()}>Clica-me</button>
}
```

Em vez disso, *passa a função em si* (sem parênteses):

```jsx
render() {
  // Correto: handleClick é passado como uma referência!
  return <button onClick={this.handleClick}>Clica-me</button>
}
```

### Como eu passo um parâmetro para um manipulador de evento ou retorno de chamada? {#how-do-i-pass-a-parameter-to-an-event-handler-or-callback}

Podes usar um arrow function para encapsular à volta um manipulador de evento e passar parâmetros:

```jsx
<button onClick={() => this.handleClick(id)} />
```

Isto é o equivalente a chamar `.bind`:

```jsx
<button onClick={this.handleClick.bind(this, id)} />
```

#### Exemplo: Passando parâmetros usando arrow functions {#example-passing-params-using-arrow-functions}

```jsx
const A = 65 // código de caractere ASCII

class Alphabet extends React.Component {
  constructor(props) {
    super(props);
    this.handleClick = this.handleClick.bind(this);
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
        Apenas clicado: {this.state.justClicked}
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

#### Exemplo: Passando parâmetros usando atributos de dados {#example-passing-params-using-data-attributes}

Alternadamente, podes usar DOM APIs para armazenar dados necessários para manipuladores de eventos. Considera esta aproximação, se precisares de optimizar um grande número de elementos ou ter uma árvore de renderização que depende das verificações de igualdade no React.PureComponent.

```jsx
const A = 65 // código de caractere ASCII

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
        Apenas clicado: {this.state.justClicked}
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

### Como posso prevenir uma função ser chamada demasiado depressa ou demasiadas vezes duma só vez? {#how-can-i-prevent-a-function-from-being-called-too-quickly-or-too-many-times-in-a-row}

Se, tens um manipulador de evento, tal como, `onClick` ou `onScroll` e queres prevenir o retorno de chamada de ser disparado demasiado depressa então, podes limitar a taxa em que, o retorno de chamada é executado. Isto pode ser feito ao usar:

- **throttling**: mudanças de amostras baseadas numa frequência baseada no tempo (por exemplo, [`_.throttle`](https://lodash.com/docs#throttle))
- **debouncing**: publica mudanças depois, de um periode de inatividade (por exemplo, [`_.debounce`](https://lodash.com/docs#debounce))
- **`requestAnimationFrame` throttling**: mudança de amostas baseado em [`requestAnimationFrame`](https://developer.mozilla.org/en-US/docs/Web/API/window/requestAnimationFrame) (por exemplo, [`raf-schd`](https://github.com/alexreardon/raf-schd))

Vê [esta visualização](http://demo.nimius.net/debounce_throttle/) para uma comparação das funções `throttle` e `debounce`.

> Nota:
>
> `_.debounce`, `_.throttle` e `raf-schd` fornecem um método `cancel` para cancelar os retornos de chamada atrasados. Tanto deves chamar este método de `componentWillUnmount` _ou_ verificar, se o componente ainda está montado dentro da função atrasada.

#### Throttle {#throttle}

A limitação (_Throttling_) impede que uma função seja chamada mais do que uma vez numa determinada janela de tempo. O exemplo abaixo limita um manipulador de "click" para evitar chamá-lo mais do que uma vez por segundo.

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

Disfarçar (_Debouncing_) assegura que uma função não será executada até que um certo período de tempo tenha passado desde que foi chamado pela última vez. Isto pode ser útil quando precisas de executar algum cálculo caro em resposta a um evento que pode ser despachado rapidamente (por exemplo, eventos de rolagem ou de teclado). O exemplo abaixo disfarça a entrada de texto com um atraso de 250ms.

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
        placeholder="Search..."
        defaultValue={this.props.value}
      />
    );
  }

  handleChange(e) {
    // Eventos React pools então, lemos o valor antes de disfarçar.
    // Alternadamente, podemos chamar `event.persist()` e passar o evento inteiro.
    // Para mais informações vê reactjs.org/docs/events.html#event-pooling
    this.emitChangeDebounced(e.target.value);
  }

  emitChange(value) {
    this.props.onChange(value);
  }
}
```

#### `requestAnimationFrame` throttling {#requestanimationframe-throttling}

[`requestAnimationFrame`](https://developer.mozilla.org/en-US/docs/Web/API/window/requestAnimationFrame) é uma maneira de enfileirar uma função para ser executada no navegador no momento ideal para renderizar o desempenho. Uma função que está enfileirada com `requestAnimationFrame` irá disparar no próximo frame. O navegador irá trabalhar arduamente para garantir que exista 60 frames por segundo (60 fps). Contudo, se o navegador está indisponível, para isso, irá, naturalmente, *limitar* a quantidade de frames num segundo. Por exemplo, um dispositivo que, só pode ser capaz de lidar com 30 fps e, portanto, só obterás 30 frames nesse segundo. Usando `requestAnimationFrame` para limitar, é uma técnica única que, previne-te de estar a mais do que 60 atualizações num segundo. Se, estás a fazer 100 atualizações num segundo isto cria trabalho adicional para o navegador onde, o utilizador, não irá ver de qualquer forma.

>**Nota:**
>
>Usando esta técnica irá apenas capturar o último valor publicado num frame. Podes ver um exemplo de como esta otimização trabalha no [`MDN`](https://developer.mozilla.org/en-US/docs/Web/Events/scroll)

```jsx
import rafSchedule from 'raf-schd';

class ScrollListener extends React.Component {
  constructor(props) {
    super(props);

    this.handleScroll = this.handleScroll.bind(this);

    // Cria uma nova funçãp para agendar atualizações.
    this.scheduleUpdate = rafSchedule(
      point => this.props.onScroll(point)
    );
  }

  handleScroll(e) {
    //Quando recebemos um evento de rolagem, agenda uma atualização.
    // Se recebermos muitas atualizações num frame, publicaremos apenas, o valor mais recente.
    this.scheduleUpdate({ x: e.clientX, y: e.clientY });
  }

  componentWillUnmount() {
    // Cancela quaisquer atualizações pendentes desde que, estamos desmontando.
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

#### Testando a tua taxa de limite  {#testing-your-rate-limiting}

Ao testar, corretamente, o teu código de limtação de axa, é útil ter a capacidade de avançar o tempo. Se, estiveres a usar[`jest`](https://facebook.github.io/jest/), então podes usar [`mock timers`](https://facebook.github.io/jest/docs/en/timer-mocks.html) para avançar o tempo. Se, estiveres a usar a limitação de `requestAnimationFrame`, poderás achar o [`raf-stub`](https://github.com/alexreardon/raf-stub) uma ferramenta útil para controlar a marcação de frames de animação.
