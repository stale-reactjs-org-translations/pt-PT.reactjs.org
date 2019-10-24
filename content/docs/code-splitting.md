---
id: code-splitting
title: Divisão de Código
permalink: docs/code-splitting.html
---

## Empacotamento {#bundling}

A maioria das aplicações em React têm os seus ficheiros empacotados através de ferramentas como [Webpack](https://webpack.js.org/), [Rollup](https://rollupjs.org/) ou [Browserify](http://browserify.org/).
Empacotamento é o processo que converte vários ficheiros importados num único ficheiro: um pacote. Este ficheiro empacotado pode ser depois incluído numa página web para carregar uma aplicação inteira de uma só vez.

#### Exemplo {#example}

**Aplicação:**

```js
// app.js
import { add } from './math.js';

console.log(add(16, 26)); // 42
```

```js
// math.js
export function add(a, b) {
  return a + b;
}
```

**Pacote:**

```js
function add(a, b) {
  return a + b;
}

console.log(add(16, 26)); // 42
```

> Nota:
>
> Os teus pacotes vão ser bastante diferentes do exemplo anterior.

Se estás a usar [Create React App](https://github.com/facebookincubator/create-react-app), [Next.js](https://github.com/zeit/next.js/), [Gatsby](https://www.gatsbyjs.org/), ou uma ferramenta semelhante, terás uma configuração predefinida do Webpack para empacotar a tua aplicação.

Senão, terás de configurar o empacotamento tu mesmo. Para referência, podes ver os guias de [Instalação](https://webpack.js.org/guides/installation/) e [Introdução](https://webpack.js.org/guides/getting-started/) na documentação do Webpack.

## Divisão de Código {#code-splitting}

Empacotamento é ótimo, mas, à medida que a tua aplicação cresce, o pacote cresce também. Especialmente se estiveres a incluir bibliotecas de terceiros de grandes dimensões. Tens de ficar atento ao código que estás a incluir no teu pacote, para que não o faças tão grande ao ponto que torne a aplicação muito lenta a carregar.

Para evitar acabar com um pacote grande, é bom antecipar o problema e começar a “dividir” o pacote. A divisão de código é um recurso suportado por empacotadores como o Webpack, Rollup e Browserify (através do coeficiente de empacotamento (factor-bundle)) no qual pode-se criar múltiplos pacotes que podem ser carregados dinamicamente em tempo de execução.

Dividir o código da aplicação pode-te ajudar a carregar somente o necessário ao utilizador, o que pode melhorar dramaticamente o desempenho da aplicação. Apesar de não ter reduzido a quantidade total de código da aplicação, acabou por evitar carregar código que o utilizador talvez nunca precise e reduziu o código inicial necessário para carregar a aplicação.

## `import()` {#import}

A melhor forma de introduzir divisão de código na aplicação é através da sintaxe dinâmica `import()`.

**Antes:**

```js
import { add } from './math';

console.log(add(16, 26));
```

**Depois:**

```js
import("./math").then(math => {
  console.log(math.add(16, 26));
});
```

> Nota:
> 
>A sintaxe dinâmica `import()` é uma [proposta](https://github.com/tc39/proposal-dynamic-import) da ECMAScript (JavaScript) que ainda não faz parte da linguagem. Espera-se que seja aceite em breve.

Quando o Webpack encontra esta sintaxe, a divisão do código da aplicação é automaticamente feita. Se estás a usar Create React App, isto já está configurado e podes [começar a usar](https://facebook.github.io/create-react-app/docs/code-splitting) imediatamente. Também é suportado por predefinição no [Next.js](https://github.com/zeit/next.js/#dynamic-import).

Se estiveres a configurar o Webpack manualmente, provavelmente vais querer ler o [guia em como dividir código](https://webpack.js.org/guides/code-splitting/) do Webpack. A tua configuração deverá ser algo parecida a [isto](https://gist.github.com/gaearon/ca6e803f5c604d37468b0091d9959269).

Ao usar [Babel](https://babeljs.io/), terás de ter a certeza que o Babel consegue analisar a sintaxe de importação dinâmica e que não a esteja a transformar. Para tal, vais precisar do [babel-plugin-syntax-dynamic-import](https://yarnpkg.com/en/package/babel-plugin-syntax-dynamic-import).

## `React.lazy` {#reactlazy}

> Nota:
>
> `React.lazy` e Suspense não estão ainda disponíveis para renderização no lado do servidor. Se quiseres fazer divisão de código neste sentido, recomendamos usar o pacote [Loadable Components](https://github.com/smooth-code/loadable-components). Este tem um ótimo [guia de como fazer divisão de código no lado do servidor](https://github.com/smooth-code/loadable-components/blob/master/packages/server/README.md).

A função `React.lazy` permite-te renderizar uma importação dinâmica como se fosse um componente comum.

**Antes:**

```js
import OtherComponent from './OtherComponent';
```

**Depois:**

```js
const OtherComponent = React.lazy(() => import('./OtherComponent'));
```

Isto vai automaticamente carregar o pacote que contém o `OtherComponent` quando este componente é renderizado pela primeira vez.

O `React.lazy` recebe uma função que deve chamar um `import()` dinâmico. Este último retorna uma Promise que é resolvida para um módulo com um export default que contém um componente React.

O componente dinâmico (ou lazy) pode ser renderizado dentro de um componente `Suspense`, o que nos permite mostrar algum conteúdo como fallback (um indicador de carregamento, por exemplo) enquanto esperamos pelo carregamento do componente dinâmico.

```js
const OtherComponent = React.lazy(() => import('./OtherComponent'));

function MyComponent() {
  return (
    <div>
      <Suspense fallback={<div>Loading...</div>}>
        <OtherComponent />
      </Suspense>
    </div>
  );
}
```

A propriedade `fallback` aceita qualquer elemento React que queiras que seja renderizado enquanto esperas que o componente seja carregado completamente. Podes colocar o componente `Suspense` em qualquer lugar acima do componente dinâmico. Podes até ter vários componentes dinâmicos dentro de apenas um componente `Suspense`.

```js
const OtherComponent = React.lazy(() => import('./OtherComponent'));
const AnotherComponent = React.lazy(() => import('./AnotherComponent'));

function MyComponent() {
  return (
    <div>
      <Suspense fallback={<div>Loading...</div>}>
        <section>
          <OtherComponent />
          <AnotherComponent />
        </section>
      </Suspense>
    </div>
  );
}
```

### Limites de Erro {#error-boundaries}

Se um outro módulo não for carregado (por exemplo, devido a uma falha na conexão), será disparado um erro. Estes erros podem ser manipulados para dar ao utilizador uma boa experiência de utilização e gerir a recuperação atráves de [Limites de Erro](/docs/error-boundaries.html). Quando um Limite de Erro é criado, podes usá-lo em qualquer lugar acima dos teus componentes dinâmicos para exibir uma mensagem de erro quando houver uma falha de conexão.

```js
import MyErrorBoundary from './MyErrorBoundary';
const OtherComponent = React.lazy(() => import('./OtherComponent'));
const AnotherComponent = React.lazy(() => import('./AnotherComponent'));

const MyComponent = () => (
  <div>
    <MyErrorBoundary>
      <Suspense fallback={<div>Loading...</div>}>
        <section>
          <OtherComponent />
          <AnotherComponent />
        </section>
      </Suspense>
    </MyErrorBoundary>
  </div>
);
```

## Divisão de código baseado em rotas {#route-based-code-splitting}

Decidir onde introduzir a divisão de código na aplicação pode ser complicado. Tens de ter a certeza que escolhes os lugares que irão dividir os pacotes de igual modo, mas que não afetem a experiência do utilizador.

Um bom lugar para começar é nas rotas. A maioria das pessoas na web estão familiarizadas com transições entre páginas que levam algum tempo para carregar. Muito provavelmente estás a re-renderizar toda a página de uma só vez para que os utilizadores não interajam com outros elementos na página ao mesmo tempo.

Aqui está um exemplo de como configurar a divisão de código baseada nas rotas da aplicação através de bibliotecas como o [React Router](https://reacttraining.com/react-router/) com `React.lazy`.

```js
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
import React, { Suspense, lazy } from 'react';

const Home = lazy(() => import('./routes/Home'));
const About = lazy(() => import('./routes/About'));

const App = () => (
  <Router>
    <Suspense fallback={<div>Loading...</div>}>
      <Switch>
        <Route exact path="/" component={Home}/>
        <Route path="/about" component={About}/>
      </Switch>
    </Suspense>
  </Router>
);
```

## Exportações de nome {#named-exports}

O `React.lazy`, atualmente, apenas suporta export default. Se o módulo que queres importar tem uma exportação de nome, então podes criar um módulo intermediário que tenha um export default. Assim garantes que o tree shaking continua a funcionar e não importa componentes não utilizados.

```js
// ManyComponents.js
export const MyComponent = /* ... */;
export const MyUnusedComponent = /* ... */;
```

```js
// MyComponent.js
export { MyComponent as default } from "./ManyComponents.js";
```

```js
// MyApp.js
import React, { lazy } from 'react';
const MyComponent = lazy(() => import("./MyComponent.js"));
```
