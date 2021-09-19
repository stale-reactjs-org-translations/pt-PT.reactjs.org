---
id: code-splitting
title: Divisão de Código
permalink: docs/code-splitting.html
---

## Empacotamento {#bundling}

<<<<<<< HEAD
A maioria das aplicações em React têm os seus ficheiros empacotados através de ferramentas como [Webpack](https://webpack.js.org/), [Rollup](https://rollupjs.org/) ou [Browserify](http://browserify.org/).
Empacotamento é o processo que converte vários ficheiros importados num único ficheiro: um pacote. Este ficheiro empacotado pode ser depois incluído numa página web para carregar uma aplicação inteira de uma só vez.
=======
Most React apps will have their files "bundled" using tools like [Webpack](https://webpack.js.org/), [Rollup](https://rollupjs.org/) or [Browserify](http://browserify.org/). Bundling is the process of following imported files and merging them into a single file: a "bundle". This bundle can then be included on a webpage to load an entire app at once.
>>>>>>> 0bb0303fb704147452a568472e968993f0729c28

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

<<<<<<< HEAD
Se estás a usar [Create React App](https://github.com/facebookincubator/create-react-app), [Next.js](https://github.com/zeit/next.js/), [Gatsby](https://www.gatsbyjs.org/), ou uma ferramenta semelhante, terás uma configuração predefinida do Webpack para empacotar a tua aplicação.

Senão, terás de configurar o empacotamento tu mesmo. Para referência, podes ver os guias de [Instalação](https://webpack.js.org/guides/installation/) e [Introdução](https://webpack.js.org/guides/getting-started/) na documentação do Webpack.
=======
If you're using [Create React App](https://create-react-app.dev/), [Next.js](https://nextjs.org/), [Gatsby](https://www.gatsbyjs.org/), or a similar tool, you will have a Webpack setup out of the box to bundle your app.

If you aren't, you'll need to set up bundling yourself. For example, see the [Installation](https://webpack.js.org/guides/installation/) and [Getting Started](https://webpack.js.org/guides/getting-started/) guides on the Webpack docs.
>>>>>>> 0bb0303fb704147452a568472e968993f0729c28

## Divisão de Código {#code-splitting}

<<<<<<< HEAD
Empacotamento é ótimo, mas, à medida que a tua aplicação cresce, o pacote cresce também. Especialmente se estiveres a incluir bibliotecas de terceiros de grandes dimensões. Tens de ficar atento ao código que estás a incluir no teu pacote, para que não o faças tão grande ao ponto que torne a aplicação muito lenta a carregar.

Para evitar acabar com um pacote grande, é bom antecipar o problema e começar a "dividir" o pacote. A divisão de código é um recurso suportado por empacotadores como o Webpack, Rollup e Browserify (através do coeficiente de empacotamento ([factor-bundle](https://github.com/browserify/factor-bundle))) que podem criar múltiplos pacotes, que são posteriormente carregados dinamicamente em tempo de execução.

Dividir o código da aplicação pode ajudar-te a carregar somente o necessário ao utilizador, o que pode melhorar dramaticamente o desempenho da aplicação. Apesar de não reduzir a quantidade total de código da aplicação, acaba por evitar o carregamento de código que o utilizador talvez nunca precise, e reduz o código inicial necessário para carregar a aplicação.

## `import()` {#import}

A melhor forma de introduzir divisão de código na aplicação é através da sintaxe dinâmica `import()`.
=======
Bundling is great, but as your app grows, your bundle will grow too. Especially if you are including large third-party libraries. You need to keep an eye on the code you are including in your bundle so that you don't accidentally make it so large that your app takes a long time to load.

To avoid winding up with a large bundle, it's good to get ahead of the problem and start "splitting" your bundle. Code-Splitting is a feature
supported by bundlers like [Webpack](https://webpack.js.org/guides/code-splitting/), [Rollup](https://rollupjs.org/guide/en/#code-splitting) and Browserify (via [factor-bundle](https://github.com/browserify/factor-bundle)) which can create multiple bundles that can be dynamically loaded at runtime.

Code-splitting your app can help you "lazy-load" just the things that are currently needed by the user, which can dramatically improve the performance of your app. While you haven't reduced the overall amount of code in your app, you've avoided loading code that the user may never need, and reduced the amount of code needed during the initial load.

## `import()` {#import}

The best way to introduce code-splitting into your app is through the dynamic `import()` syntax.
>>>>>>> 0bb0303fb704147452a568472e968993f0729c28

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

<<<<<<< HEAD
> Nota:
> 
>A sintaxe dinâmica `import()` é uma [proposta](https://github.com/tc39/proposal-dynamic-import) da ECMAScript (JavaScript) que ainda não faz parte da linguagem. Espera-se que seja aceite em breve.

Quando o Webpack encontra esta sintaxe, a divisão do código da aplicação é automaticamente feita. Se estás a usar _Create React App_, isto já está configurado e podes [começar a usar](https://facebook.github.io/create-react-app/docs/code-splitting) imediatamente. Também é suportado por predefinição no [Next.js](https://github.com/zeit/next.js/#dynamic-import).

Se estiveres a configurar o Webpack manualmente, provavelmente vais querer ler o [guia em como dividir código](https://webpack.js.org/guides/code-splitting/) do Webpack. A tua configuração deverá ser algo parecida a [isto](https://gist.github.com/gaearon/ca6e803f5c604d37468b0091d9959269).

Ao usar [Babel](https://babeljs.io/), terás de ter a certeza que o Babel consegue analisar a sintaxe de importação dinâmica e que não a esteja a transformar. Para tal, vais precisar do [babel-plugin-syntax-dynamic-import](https://yarnpkg.com/en/package/babel-plugin-syntax-dynamic-import).
=======
When Webpack comes across this syntax, it automatically starts code-splitting your app. If you're using Create React App, this is already configured for you and you can [start using it](https://create-react-app.dev/docs/code-splitting/) immediately. It's also supported out of the box in [Next.js](https://nextjs.org/docs/advanced-features/dynamic-import).

If you're setting up Webpack yourself, you'll probably want to read Webpack's [guide on code splitting](https://webpack.js.org/guides/code-splitting/). Your Webpack config should look vaguely [like this](https://gist.github.com/gaearon/ca6e803f5c604d37468b0091d9959269).

When using [Babel](https://babeljs.io/), you'll need to make sure that Babel can parse the dynamic import syntax but is not transforming it. For that you will need [@babel/plugin-syntax-dynamic-import](https://classic.yarnpkg.com/en/package/@babel/plugin-syntax-dynamic-import).
>>>>>>> 0bb0303fb704147452a568472e968993f0729c28

## `React.lazy` {#reactlazy}

> Nota:
>
> `React.lazy` e Suspense não estão ainda disponíveis para renderização no lado do servidor. Se quiseres fazer divisão de código neste sentido, recomendamos usar o pacote [Loadable Components](https://github.com/gregberge/loadable-components). Este tem um ótimo [guia de como fazer divisão de código no lado do servidor](https://loadable-components.com/docs/server-side-rendering/).

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
import React, { Suspense } from 'react';

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
import React, { Suspense } from 'react';

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
import React, { Suspense } from 'react';
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

<<<<<<< HEAD
Decidir onde introduzir a divisão de código na aplicação pode ser complicado. Tens de ter a certeza que escolhes os lugares que irão dividir os pacotes de igual modo, mas que não afetem a experiência do utilizador.

Um bom lugar para começar é nas rotas. A maioria das pessoas na web estão familiarizadas com transições entre páginas que levam algum tempo para carregar. Muito provavelmente estás a re-renderizar toda a página de uma só vez para que os utilizadores não interajam com outros elementos na página ao mesmo tempo.

Aqui está um exemplo de como configurar a divisão de código baseada nas rotas da aplicação através de bibliotecas como o [React Router](https://reacttraining.com/react-router/) com `React.lazy`.
=======
Deciding where in your app to introduce code splitting can be a bit tricky. You want to make sure you choose places that will split bundles evenly, but won't disrupt the user experience.

A good place to start is with routes. Most people on the web are used to page transitions taking some amount of time to load. You also tend to be re-rendering the entire page at once so your users are unlikely to be interacting with other elements on the page at the same time.

Here's an example of how to setup route-based code splitting into your app using libraries like [React Router](https://reacttraining.com/react-router/) with `React.lazy`.
>>>>>>> 0bb0303fb704147452a568472e968993f0729c28

```js
import React, { Suspense, lazy } from 'react';
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';

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

O `React.lazy`, atualmente, apenas suporta `export default`. Se o módulo que queres importar tem uma exportação de nome, então podes criar um módulo intermediário que tenha um export default. Assim garantes que o _tree shaking_ continua a funcionar e não importa componentes não utilizados.

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
