---
id: test-renderer
title: Renderizador de Teste
permalink: docs/test-renderer.html
layout: docs
category: Reference
---

**Importação**

```javascript
import TestRenderer from 'react-test-renderer'; // ES6
const TestRenderer = require('react-test-renderer'); // ES5 com npm
```

## Visão Geral {#overview}

Este pacote fornece um renderizador React que pode ser usado para renderizar componentes React para objectos JavaScript puros, sem depender do DOM ou de um ambiente móvel nativo.

Essencialmente, este pacote facilita a captura de um snapshot da hierarquia de visualização da plataforma (semelhante a uma árvore DOM) processada por um componente React DOM ou React Native sem usar um navegador ou [jsdom](https://github.com/tmpvar/jsdom).

Exemplo:

```javascript
import TestRenderer from 'react-test-renderer';

function Link(props) {
  return <a href={props.page}>{props.children}</a>;
}

const testRenderer = TestRenderer.create(
  <Link page="https://www.facebook.com/">Facebook</Link>
);

console.log(testRenderer.toJSON());
// { type: 'a',
//   props: { href: 'https://www.facebook.com/' },
//   children: [ 'Facebook' ] }
```

Podes usar o recurso de teste de snapshot do Jest para guardar automaticamente uma cópia da árvore JSON em um arquivo e verificar em seus testes que ela não foi alterada: [Aprende mais sobre isto](https://jestjs.io/docs/en/snapshot-testing).

Podes também percorrer o resultado para encontrar nós específicos e fazer verificações sobre eles.

```javascript
import TestRenderer from 'react-test-renderer';

function MyComponent() {
  return (
    <div>
      <SubComponent foo="bar" />
      <p className="my">Olá</p>
    </div>
  )
}

function SubComponent() {
  return (
    <p className="sub">Sub</p>
  );
}

const testRenderer = TestRenderer.create(<MyComponent />);
const testInstance = testRenderer.root;

expect(testInstance.findByType(SubComponent).props.foo).toBe('bar');
expect(testInstance.findByProps({className: "sub"}).children).toEqual(['Sub']);
```

### TestRenderer {#testrenderer}

* [`TestRenderer.create()`](#testrenderercreate)
* [`TestRenderer.act()`](#testrendereract)

### Instância de TestRenderer {#testrenderer-instance}

* [`testRenderer.toJSON()`](#testrenderertojson)
* [`testRenderer.toTree()`](#testrenderertotree)
* [`testRenderer.update()`](#testrendererupdate)
* [`testRenderer.unmount()`](#testrendererunmount)
* [`testRenderer.getInstance()`](#testrenderergetinstance)
* [`testRenderer.root`](#testrendererroot)

### TestInstance {#testinstance}

* [`testInstance.find()`](#testinstancefind)
* [`testInstance.findByType()`](#testinstancefindbytype)
* [`testInstance.findByProps()`](#testinstancefindbyprops)
* [`testInstance.findAll()`](#testinstancefindall)
* [`testInstance.findAllByType()`](#testinstancefindallbytype)
* [`testInstance.findAllByProps()`](#testinstancefindallbyprops)
* [`testInstance.instance`](#testinstanceinstance)
* [`testInstance.type`](#testinstancetype)
* [`testInstance.props`](#testinstanceprops)
* [`testInstance.parent`](#testinstanceparent)
* [`testInstance.children`](#testinstancechildren)

## Referência {#reference}

### `TestRenderer.create()` {#testrenderercreate}

```javascript
TestRenderer.create(element, options);
```

Cria uma instância do `TestRenderer` com o elemento React fornecido. Este método não usa o DOM real, mas ainda renderiza completamente a árvore de componentes na memória para que possas fazer verificações sobre ela. Retorna uma [instância de TestRenderer](#testrenderer-instance).

### `TestRenderer.act()` {#testrendereract}

```javascript
TestRenderer.act(callback);
```

Semelhante ao método auxiliar [`act()` de `react-dom/test-utils`](/docs/test-utils.html#act), `TestRenderer.act` prepara um componente para verificações. Usa esta versão do `act()` para encapsulares invocações para `TestRenderer.create` e `testRenderer.update`.

```javascript
import {create, act} from 'react-test-renderer';
import App from './app.js'; // The component being tested

// render the component
let root; 
act(() => {
  root = create(<App value={1}/>)
});

// make assertions on root 
expect(root.toJSON()).toMatchSnapshot();

// update with some different props
act(() => {
  root.update(<App value={2}/>);
})

// make assertions on root 
expect(root.toJSON()).toMatchSnapshot();
```

### `testRenderer.toJSON()` {#testrenderertojson}

```javascript
testRenderer.toJSON()
```

Retorna um objecto representando a árvore renderizada. Esta árvore contém apenas os nós específicos da plataforma como `<div>` ou `<View>` e suas props, mas não contém nenhum componente criado pelo utilizador. Isto é útil para [testes de snapshot](https://facebook.github.io/jest/docs/en/snapshot-testing.html#snapshot-testing-with-jest).

### `testRenderer.toTree()` {#testrenderertotree}

```javascript
testRenderer.toTree()
```

Retorna um objecto representando a árvore renderizada. A representação é mais detalhada que a fornecida por `toJSON()`, e inclui os componentes criados pelo utilizador. Provavelmente não precisas desse método, a menos que estejas a escrever a tua própria biblioteca de asserções sobre o renderizador de teste.

### `testRenderer.update()` {#testrendererupdate}

```javascript
testRenderer.update(element)
```

Re-renderiza a árvore na memória com um novo elemento raiz. Isto simula uma actualização do React na raiz. Se o novo elemento tiver o mesmo tipo e chave do elemento anterior, a árvore será atualizada; caso contrário, irá montar novamente uma nova árvore.

### `testRenderer.unmount()` {#testrendererunmount}

```javascript
testRenderer.unmount()
```

Desmonta a árvore na memória, acionando os eventos de ciclo de vida apropriados.

### `testRenderer.getInstance()` {#testrenderergetinstance}

```javascript
testRenderer.getInstance()
```

Retorna a instância correspondente ao elemento raiz, se disponível. Isto não funcionará se o elemento raiz for um componente de função, porque eles não possuem instâncias.

### `testRenderer.root` {#testrendererroot}

```javascript
testRenderer.root
```

Retorna o objecto raiz "instância de teste" que é útil para fazer asserções sobre nós específicos na árvore. Podes usá-lo para encontrar outras "instâncias de teste" mais abaixo.

### `testInstance.find()` {#testinstancefind}

```javascript
testInstance.find(test)
```

Encontra uma única instância de teste descendente para a qual `test(testInstance)` retorne `true`. Se `test(testInstance)` não retornar `true` para exactamente uma instância de teste, isto causará um erro.

### `testInstance.findByType()` {#testinstancefindbytype}

```javascript
testInstance.findByType(type)
```

Encontra uma única instância de teste descendente com o `type` fornecido. Se não houver exatamente uma instância de teste com o `type` fornecido, isto causará um erro.

### `testInstance.findByProps()` {#testinstancefindbyprops}

```javascript
testInstance.findByProps(props)
```

Encontra uma única instância de teste descendente com os `props` fornecidos. Se não houver exactamente uma instância de teste com os `props` fornecidos, isto causará um erro.

### `testInstance.findAll()` {#testinstancefindall}

```javascript
testInstance.findAll(test)
```

Encontra todas as instâncias de teste descendentes para as quais `test(testInstance)` retorne `true`.

### `testInstance.findAllByType()` {#testinstancefindallbytype}

```javascript
testInstance.findAllByType(type)
```

Encontra todas as instâncias de teste descendentes com o `type` fornecido.

### `testInstance.findAllByProps()` {#testinstancefindallbyprops}

```javascript
testInstance.findAllByProps(props)
```

Encontra todas as instâncias de teste descendentes com os `props` fornecidos.

### `testInstance.instance` {#testinstanceinstance}

```javascript
testInstance.instance
```

A instância do componente correspondente a essa instância de teste. Está disponível apenas para componentes de classe, pois os componentes de função não possuem instâncias. Ele corresponde ao valor `this` dentro do componente fornecido.

### `testInstance.type` {#testinstancetype}

```javascript
testInstance.type
```

O tipo de componente correspondente a essa instância de teste. Por exemplo, um componente `<Button />` tem o tipo `Button`.

### `testInstance.props` {#testinstanceprops}

```javascript
testInstance.props
```

Os props correspondentes a essa instância de teste. Por exemplo, um componente `<Button size="small"/>` possui `{size: 'small'}` como props.

### `testInstance.parent` {#testinstanceparent}

```javascript
testInstance.parent
```

A instância de teste pai desta instância de teste.

### `testInstance.children` {#testinstancechildren}

```javascript
testInstance.children
```

As instâncias de testes descendentes desta instância de teste.

## Ideias {#ideas}

Podes passar a função `createNodeMock` para `TestRenderer.create` como opção, que permite mocks personalizados de refs.
`createNodeMock` aceita o elemento actual e deve retornar um objecto mock de ref.
Isto é útil quando testas um componente que depende de refs.

```javascript
import TestRenderer from 'react-test-renderer';

class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.input = null;
  }
  componentDidMount() {
    this.input.focus();
  }
  render() {
    return <input type="text" ref={el => this.input = el} />
  }
}

let focused = false;
TestRenderer.create(
  <MyComponent />,
  {
    createNodeMock: (element) => {
      if (element.type === 'input') {
        // mock de uma função de focus
        return {
          focus: () => {
            focused = true;
          }
        };
      }
      return null;
    }
  }
);
expect(focused).toBe(true);
```
