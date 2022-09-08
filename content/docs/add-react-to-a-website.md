---
id: add-react-to-a-website
title: Add React to a Website
permalink: docs/add-react-to-a-website.html
redirect_from:
  - "docs/add-react-to-an-existing-app.html"
prev: getting-started.html
next: create-a-new-react-app.html
---

Use o mínimo ou o máximo de React que precisar.

O React foi desenvolvido desde o início para uma adoção gradual, e **podes usar o mínimo ou o máximo de React que precisares**
Talves só precisas de adicionar uma "pitada de interactividade" a um projeto existente. Os componente de React são uma excelente maneira para fazer isso.

A maior parte das páginas web não são, e não precisam de ser aplicações single-page. Com **algumas linhas de código e sem build tools**, experimente React numa pequena parte da sua página web. Depois poderás expandir a sua presença, ou manter-la contida em alguns widgets.

---

- [Adiciona React num minuto](#add-react-in-one-minute)
- [Opcional: Experimente React com JSX](#optional-try-react-with-jsx) (não precisa de bundler!)

## Adiciona React num minuto {#add-react-in-one-minute}

Nesta secção, vamos mostrar como adicionar um componente de React a uma existente página web. Poderás seguir com a tua página web ou criar um projeto HTML vazio para praticar.

Não serão necessarias ferramentas complicadas ou instalações -- **para completares esta secção, só precisas de uma conecção à internet e um minuto do teu tempo.**

Opcional: [Faça Download do exemplo completo (2KB zip)](https://gist.github.com/gaearon/6668a1f6986742109c00a581ce704605/archive/f6c882b6ae18bde42dcf6fdb751aae93495a2275.zip)

### Passo 1: Adiciona o contentor DOM ao HTML {#step-1-add-a-dom-container-to-the-html}

Primeiro abre uma página HTML que queira editar. Adiciona uma tag `<div>` que será usada para mostrar algo usando React. Por exemplo:

```html{3}
<!-- ... existing HTML ... -->

<div id="like_button_container"></div>

<!-- ... existing HTML ... -->
```

Adicionamos a esta tag `<div>` um atributo `id` único. Que fará com que o código JavaScript o encontre mais tarde onde irá mostrar dentro o componente React.

>Dica
>
>Poderá colocar um "contentor" como esta tag `<div>` em **qualquer lado** dentro da tag `<body>`. Poderá ter a quantidade de contentores DOM que precisar em uma só página web. Normalmente estão vazios -- React irá substituir qualquer conteudo existente.

### Passo 2: Adicionar as Tags Script {#step-2-add-the-script-tags}

A seguir, adicione três tags `<script>` em sua página HTML logo antes do fechamento da tag `</body>`:

```html{5,6,9}
  <!-- ...  HTMLHTML qualquer ... -->

  <!-- Adicionar React. -->
  <!-- Nota: ao fazer o deploy, substitua "development.js" por "production.min.js". -->
  <script src="https://unpkg.com/react@16/umd/react.development.js" crossorigin></script>
  <script src="https://unpkg.com/react-dom@16/umd/react-dom.development.js" crossorigin></script>

  <!-- Adicione nosso componente React. -->
  <script src="like_button.js"></script>

</body>
```

As duas primeiras tags adicionam React. A terceira irá adicionar o código do seu componente.

### Passo 3: Criar um Componente React {#step-3-create-a-react-component}

Crie um arquivo chamado `like_button.js` próximo da sua página HTML.

Abra **[este código inicial](https://gist.github.com/gaearon/0b180827c190fe4fd98b4c7f570ea4a8/raw/b9157ce933c79a4559d2aa9ff3372668cce48de7/LikeButton.js)** e copie o conteúdo no arquivo que você criou.

>Dica
>
>Este código cria um componente React chamado `LikeButton`. Não se preocupe se não perceber -- mais tarde iremos cobrir os blocos de construção do React [tutorial](/tutorial/tutorial.html) e no [guia dos conceitos principais](/docs/hello-world.html). Por enquanto, vamos apenas fazer funcionar!

Depois **[do código inicial](https://gist.github.com/gaearon/0b180827c190fe4fd98b4c7f570ea4a8/raw/b9157ce933c79a4559d2aa9ff3372668cce48de7/LikeButton.js)**, adiciona estas duas linhas no fim do `like_button.js`:

```js{3,4}
// ... código inicial copiado ...

const domContainer = document.querySelector('#like_button_container');
ReactDOM.render(e(LikeButton), domContainer);
```

Essas duas linhas de código encontram a `<div>` que foi adiciona ao HTML no primeiro passo e então mostrará o componente "Like" React dentro dele.

### É isso! {#thats-it}

Não exite quartp passo. **Acabste de adicionar o primeiro componente React a tua página web.**

Veja as proximas secções para ver mais dicas de como integrar React.

**[Veja o código fonte completo do exemplo](https://gist.github.com/gaearon/6668a1f6986742109c00a581ce704605)**

**[Faça o download do exemplo completo (2KB zip)](https://gist.github.com/gaearon/6668a1f6986742109c00a581ce704605/archive/f6c882b6ae18bde42dcf6fdb751aae93495a2275.zip)**

### Tip: Reutilizar um Componente {#tip-reuse-a-component}

Normalmente, quer mostrar um componente React em multiplos sitios na página HTML. Aqui esta um exemplo que mostar o botão "Like" três vezes e passa alguns dados nele:

[Veja o código fonte completo do exemplo](https://gist.github.com/gaearon/faa67b76a6c47adbab04f739cba7ceda)

[Faça o download do exemplo completo (2KB zip)](https://gist.github.com/gaearon/faa67b76a6c47adbab04f739cba7ceda/archive/9d0dd0ee941fea05fd1357502e5aa348abb84c12.zip)

>Nota
>
>Esta estratégia é mais útil quando as partes da página com Reactestão isoladas. Dentro do código React, é facil de usar [composição de componentes](/docs/components-and-props.html#composing-components) instead.

### Nota: Minifique o JavaScript para Produção {#tip-minify-javascript-for-production}

Antes de fazer deploy da sua página web para produção, lembre-se que o código JavaScript não minificado pode deixar sua página significativamente mais lenta para seus utilizadores.

Se já minificou os scripts da sua aplicação, seu site estará pronto para produção se garantir que o HTML carregue a versão do React terminando em `production.min.js`:

```js
<script src="https://unpkg.com/react@16/umd/react.production.min.js" crossorigin></script>
<script src="https://unpkg.com/react-dom@16/umd/react-dom.production.min.js" crossorigin></script>
```

Se não tiver uma versão minificada para os seus scripts, [aqui tem uma forma de o configurar](https://gist.github.com/gaearon/42a2ffa41b8319948f9be4076286e1f3).

## Opcional: Experimente React com JSX {#optional-try-react-with-jsx}

Nos exemplos acima, contamos apenas com recursos que são suportados nativamente pelos navegadores. E é por isso que usamos uma chamada de função JavaScript para informar ao React o que exibir:

```js
const e = React.createElement;

// Exibe um <button> "Like"
return e(
  'button',
  { onClick: () => this.setState({ liked: true }) },
  'Like'
);
```

No entanto, como alternativa React tambem oferece uma opção de usar [JSX](/docs/introducing-jsx.html):

```js
// Exibe um <button> "Like"
return (
  <button onClick={() => this.setState({ liked: true })}>
    Like
  </button>
);
```

Esses dois blocos de código são equivalentes. Enquanto o **JSX é [completamente opcional(/docs/react-without-jsx.html)**, muitas pessoas o acham útil para escrever código de UI — junto com React e com outras bibliotecas.

Poderá brincar com JSX usando [este conversor online](https://babeljs.io/en/repl#?babili=false&browsers=&build=&builtIns=false&spec=false&loose=false&code_lz=DwIwrgLhD2B2AEcDCAbAlgYwNYF4DeAFAJTw4B88EAFmgM4B0tAphAMoQCGETBe86WJgBMAXJQBOYJvAC-RGWQBQ8FfAAyaQYuAB6cFDhkgA&debug=false&forceAllTransforms=false&shippedProposals=false&circleciRepo=&evaluate=false&fileSize=false&timeTravel=false&sourceType=module&lineWrap=true&presets=es2015%2Creact%2Cstage-2&prettier=false&targets=&version=7.4.3).

### Experimente Rapidamente JSX {#quickly-try-jsx}

A maneira mais rápida de experimentar o JSX em seu projeto é adicionando essa tag `<script>` em sua página:

```html
<script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>
```
Agora poderá usar o JSX em qualquer tag <script> somente adicionando o atributo type="text/babel" nele. Aqui está um exemplo de arquivo HTML com JSX em que poderá efetuar o download e testar.

Essa abordagem é boa para aprender e criar demostrações simples. No entanto, coloca a página web lenta, não ficando **adequada para produção**. Quando estiver pronto para seguir em frente, remova essa nova tag `<script>` e os atributos `type="text/babel"` que adicionou. Em vez disso, na seção a seguir ira configurar um pré-processador JSX para converter todas suas tags `<script>` automaticamente.

### Adicionar JSX a um Projeto {#add-jsx-to-a-project}

Adicionar JSX a um projeto não requer ferramentas complicadas, como um empacotador ou um servidor de desenvolvimento. Basicamente, adicionar JSX **é como adicionar um pré-processador CSS.** O único requisito é possuir o [Node.js](https://nodejs.org/) instalado em seu computador.

No terminal, vá até a pasta do seu projeto e cole esses dois comandos:

1. **Passo 1:** Corra `npm init -y` (se falhar, [aqui esta um fix](https://gist.github.com/gaearon/246f6380610e262f8a648e3e51cad40d))
2. **Passo 2:** Corra `npm install babel-cli@6 babel-preset-react-app@3`

>Dica
>
>Estamos **a usar npm para isntalar o processador JSX;** não irá precisar dele para mais nada. Tanto o React e o código da aplicação podem ficar com as tags `<script>`.

Parabéns! Acabou de adicionar uma **configuração JSX pronta para produção** em seu projeto.


### Execute o Pré-processador JSX {#run-jsx-preprocessor}

Crie uma pasta chamada `src` e execute no terminal esse comando:

```
npx babel --watch src --out-dir . --presets react-app/prod
```

>Nota
>
>`npx` não é um erro de digitação -- é uma [ferramenta de executar pacotes que vem com npm 5.2+](https://medium.com/@maybekatz/introducing-npx-an-npm-package-runner-55f7d4bd282b).
>
>Se vir uma mensagem de erro dizendo “You have mistakenly installed the babel package”, poderá ter perdido o [passo anterior](#add-jsx-to-a-project). Execute o passo anterior na mesma pasta e tente novamente.

Não espere o comando finalizar — esse comando inicia um watcher automatizado para o JSX.

Se criar um arquivo chamado `src/like_button.js` com este **[código JSX inicial](https://gist.github.com/gaearon/c8e112dc74ac44aac4f673f2c39d19d1/raw/09b951c86c1bf1116af741fa4664511f2f179f0a/like_button.js)**, o watcher criará um  `like_button.js` pré-processado com o código JavaScript adequado ao navegador. Quando editae o arquivo com JSX, a transpilação será executada automaticamente.

Como um bônus, isso também permite que use recursos modernos de JavaScript, como classes, sem se preocupar com a incompatibilidade de navegadores antigos. A ferramenta que acabamos de usar é chamada de Babel e pode aprender mais sobre ele [na sua documentação](https://babeljs.io/docs/en/babel-cli/).

Se se sentir confortável com ferramentas de build e deseja que eles façam mais por você, a [próxima seção](/docs/create-a-new-react-app.html) descreve alguma das mais populares e acessíveis ferramentas. Caso contrário, essas tags scripts funcionarão perfeitamente.
