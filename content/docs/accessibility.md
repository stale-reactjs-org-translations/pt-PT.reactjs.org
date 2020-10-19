---
id: accessibility
title: Accessibility
permalink: docs/accessibility.html
---

## Porquê Acessibilidade? {#why-accessibility}

A acessibilidade na web (também conhecida por [** a11y **](https://en.wiktionary.org/wiki/a11y)) é o design e a criação de websites que podem ser usados por todos. O suporte à acessibilidade é necessário para permitir que as tecnologias de acessibilidade interpretem as páginas da web.

O React suporta totalmente a construção de websites acessíveis, muitas vezes com apenas técnicas de HTML padrão.

## Padrões e Diretrizes {#standards-and-guidelines}

### WCAG {#wcag}

O [Web Content Accessibility Guidelines](https://www.w3.org/WAI/intro/wcag) fornece as diretrizes necessárias para a criação de sites acessíveis.

As seguintes checklists das WCAG fornecem uma visão geral:

- [WCAG checklist from Wuhcag](https://www.wuhcag.com/wcag-checklist/)
- [WCAG checklist from WebAIM](https://webaim.org/standards/wcag/checklist)
- [Checklist from The A11Y Project](https://a11yproject.com/checklist.html)

### WAI-ARIA {#wai-aria}

O documento [Web Accessibility Initiative - Accessible Rich Internet Applications](https://www.w3.org/WAI/intro/aria) contém as técnicas necessárias para criar widgets totalmente acessíveis.

Nota que todos os atributos HTML `aria-*` são totalmente suportados em JSX. Apesar da maioria das propriedades e atributos do DOM no React serem em camelCase, estes atributos devem ser em hyphen-case (também conhecido como kebab-case, lisp-case, etc) pois estão em HTML:

```javascript{3,4}
<input
  type="text"
  aria-label={labelText}
  aria-required="true"
  onChange={onchangeHandler}
  value={inputValue}
  name="name"
/>
```

## Semântica HTML {#semantic-html}

A semântica HTML é a base de uma aplicação web acessível. Se usarmos corretamente os elementos HTML para reforçar o significado da informação colocada nos nossos websites, a acessibilidade pode vir de forma gratuita.

- [MDN HTML elements reference](https://developer.mozilla.org/en-US/docs/Web/HTML/Element)

Às vezes, quebramos a semântica HTML para adicionar elementos `<div>` ao nosso JSX apenas para fazer funcionar no React, especialmente quando trabalharmos com listas (`<ol>`, `<ul>` e `<dl>`) e com `<table>`.
Nestes casos devemos usar [React Fragments](/docs/fragments.html) para agrupar vários elementos.

Por exemplo,

```javascript{1,5,8}
import React, { Fragment } from 'react';

function ListItem({ item }) {
  return (
    <Fragment>
      <dt>{item.term}</dt>
      <dd>{item.description}</dd>
    </Fragment>
  );
}

function Glossary(props) {
  return (
    <dl>
      {props.items.map(item => (
        <ListItem item={item} key={item.id} />
      ))}
    </dl>
  );
}
```

Podemos mapear uma coleção de itens para um array de fragments tal como faríamos para qualquer outro tipo de elemento:

```javascript{6,9}
function Glossary(props) {
  return (
    <dl>
      {props.items.map(item => (
        // Quando estamos a iterar um array, os Fragments devem ter a propriedade `key` adicionada
        <Fragment key={item.id}>
          <dt>{item.term}</dt>
          <dd>{item.description}</dd>
        </Fragment>
      ))}
    </dl>
  );
}
```

Quando não é necessário passar props para a tag Fragment podemos usar a [síntaxe curta](/docs/fragments.html#short-syntax), se a configuração suportar:

```javascript{3,6}
function ListItem({ item }) {
  return (
    <>
      <dt>{item.term}</dt>
      <dd>{item.description}</dd>
    </>
  );
}
```

Para mais informação, vê a [documentação sobre Fragments](/docs/fragments.html).

## Formulários Acessíveis {#accessible-forms}

### Rótulos {#labeling}
Todos os elementos de um formulário HTML como `<input>` e `<textarea>`, precisam de ser rotulados. É necessário fornecer rótulos descritivos pois estes são expostos aos leitores de ecrã.

Os seguintes artigos mostram como os devemos aplicar:

- [The W3C shows us how to label elements](https://www.w3.org/WAI/tutorials/forms/labels/)
- [WebAIM shows us how to label elements](https://webaim.org/techniques/forms/controls)
- [The Paciello Group explains accessible names](https://www.paciellogroup.com/blog/2017/04/what-is-an-accessible-name/)

Embora estas práticas HTML padrão possam ser usadas diretamente no React, observa que o atributo `for` está escrito como `htmlFor` em JSX:

```javascript{1}
<label htmlFor="inputNome">Nome:</label>
<input id="inputNome" type="text" name="nome"/>
```

### Notificar erros ao utilizador {#notifying-the-user-of-errors}

Situações de erro precisam de ser entendidas por todos os utilizadores. Os artigos seguintes mostram como exportar os erros aos leitores de ecrã:

- [The W3C demonstrates user notifications](https://www.w3.org/WAI/tutorials/forms/notifications/)
- [WebAIM looks at form validation](https://webaim.org/techniques/formvalidation/)

## Controlo de focus {#focus-control}

Certifica-te que a aplicação web possa ser utilizada apenas com o teclado:

- [WebAIM talks about keyboard accessibility](https://webaim.org/techniques/keyboard/)

### Focus no teclado e focus no contorno {#keyboard-focus-and-focus-outline}

Focus no teclado refere-se ao elemento DOM que foi selecionado e que aceita ações do teclado. Podemos ver o contorno na imagem a seguir:

<img src="../images/docs/keyboard-focus.png" alt="Contorno azul à volta da ligação selecionada." />

Usa apenas CSS que elimine este contorno, por exemplo definindo `outline: 0`, se for substituir por outra implementação de focus.

### Mecanismos para avançar para o conteúdo desejado {#mechanisms-to-skip-to-desired-content}

Providencia mecanismos para permitir que os utilizadores consigam ignorar as secções de navegação, acelerando assim a navegação com o teclado.

Skiplinks ou Skip Navigation Links são ligações escondidos que só ficam visíveis quando os utilizadores de teclado interagem com a página. São muito fáceis de implementar com alguns estilos e âncoras:

- [WebAIM - Skip Navigation Links](https://webaim.org/techniques/skipnav/)

Usa também elementos e pontos de referência como `<main>` e `<aside>`, para demarcar zonas da página, sendo que as tecnologias de acessibilidade permitem assim que o utilizador navegue rapidamente nessas secções.

Saiba mais sobre o uso destes elementos para melhorar a acessibilidade aqui:

- [Accessible Landmarks](https://www.scottohara.me/blog/2018/03/03/landmarks.html)

### Controlar programaticamente o focus {#programmatically-managing-focus}

As nossas aplicações React modificam continuamente o HTML DOM durante o tempo de execução, às vezes perdendo o focus do teclado ou adicionado o focus a um elemento inesperado. Para corrigir isso, é necessário programar o focus do teclado na direção certo. Por exemplo, colocar o focus do teclado no botão que faz abrir uma modal depois dessa mesma modal ser fechada.

Podes encontrar mais informações de como implementar no MDN Web Docs [keyboard-navigable JavaScript widgets](https://developer.mozilla.org/en-US/docs/Web/Accessibility/Keyboard-navigable_JavaScript_widgets).

Para colocar o focus no React, podemos usar [Refs para elementos no DOM](/docs/refs-and-the-dom.html).

Desta maneira, em primeiro lugar no componente de JSX criamos a referência:

```javascript{4-5,8-9,13}
class CustomTextInput extends React.Component {
  constructor(props) {
    super(props);
    // Criamos a ref para gravar o elemento DOM textInput
    this.textInput = React.createRef();
  }
  render() {
  // Use o callback `ref` para gravar a referência do elemento
  // text input nesta instância (por exemplo, this.textInput).
    return (
      <input
        type="text"
        ref={this.textInput}
      />
    );
  }
}
```

Depois podemos colocar o focus quando for necessário:

 ```javascript
 focus() {
   // Explicitamente devemos usar a DOM API nativa para colocar o focus
   // Nota: estamos a aceder ao "current" para obter o elemento DOM
   this.textInput.current.focus();
 }
 ```

Às vezes um componente pai precisa de colocar o focus a um elemento no componente filho. Podemos fazer isso [expondo as referências DOM aos componentes pais](/docs/refs-and-the-dom.html#exposing-dom-refs-to-parent-components) através de uma `prop` especial no componente filho que encaminhe o componente pai ao elemento DOM do componente filho.

```javascript{4,12,16}
function CustomTextInput(props) {
  return (
    <div>
      <input ref={props.inputRef} />
    </div>
  );
}

class Parent extends React.Component {
  constructor(props) {
    super(props);
    this.inputElement = React.createRef();
  }
  render() {
    return (
      <CustomTextInput inputRef={this.inputElement} />
    );
  }
}

// Agora podemos colocar o focus quando for necessário.
this.inputElement.current.focus();
```

Ao usar um HOC para estender componentes, é recomendado [encaminhar a ref](/docs/forwarding-refs.html) para o elemento embrulhado usando a função de React `forwardRef`. Se um HOC de terceiros não implementar o `forwardRef`, o padrão acima pode ser usado como fallback.

Um ótimo exemplo de controlar o focus é o [react-aria-modal](https://github.com/davidtheclark/react-aria-modal). Este é um exemplo relativamente raro de uma janela modal totalmente acessível. Não só coloca o focus inicial no botão de cancelar (prevenindo assim que o utilizador ative acidentalmente a ação de sucesso) mas também bloqueia o focus do teclado dentro da modal, e ainda reinicia o focus ao elemento que primeiramente acionou a modal.

>Nota:
>
>Embora seja um recurso de acessibilidade muito importante, esta técnica deve ser usada de maneira criteriosa. Use-o para corrigir o comportamento do focus quando este está distorcido, e não para tentar
>antecipar como os utilizadores querem usar a aplicação.

## Eventos do rato e cursor {#mouse-and-pointer-events}

Certifique-se de que todas as funcionalidades expostas através dos eventos do rato ou cursor também possam ser acedidas usando apenas o teclado. Se depender apenas no rato, vai haver muitos casos em que os utilizadores não consigam utilizar a tua aplicação.

Para ilustrar isto, abaixa pode ver um exemplo clássico de quebra da acessibilidade causada por um click. Este é o padrão de click externo, em que o utilizador pode desativar uma popover aberto ao carregar fora do elemento.

<img src="../images/docs/outerclick-with-mouse.gif" alt="Um botão que abre uma lista popover implementada com um padrão de click externo e operado por um rato a mostrar que a ação de fechar funciona." />

Isto geralmente é implementado ao anexar um evento `click` ao objeto `window` que fecha a popover:

```javascript{12-14,26-30}
class OuterClickExample extends React.Component {
  constructor(props) {
    super(props);

    this.state = { isOpen: false };
    this.toggleContainer = React.createRef();

    this.onClickHandler = this.onClickHandler.bind(this);
    this.onClickOutsideHandler = this.onClickOutsideHandler.bind(this);
  }

  componentDidMount() {
    window.addEventListener('click', this.onClickOutsideHandler);
  }

  componentWillUnmount() {
    window.removeEventListener('click', this.onClickOutsideHandler);
  }

  onClickHandler() {
    this.setState(currentState => ({
      isOpen: !currentState.isOpen
    }));
  }

  onClickOutsideHandler(event) {
    if (this.state.isOpen && !this.toggleContainer.current.contains(event.target)) {
      this.setState({ isOpen: false });
    }
  }

  render() {
    return (
      <div ref={this.toggleContainer}>
        <button onClick={this.onClickHandler}>Select an option</button>
        {this.state.isOpen && (
          <ul>
            <li>Option 1</li>
            <li>Option 2</li>
            <li>Option 3</li>
          </ul>
        )}
      </div>
    );
  }
}
```

Isso pode funcionar bem para os utilizadores com dispositivos com ponteiro como, por exemplo um rato, mas se for apenas com um teclado pode quebrar a funcionalidade ao utilizar o `tab` para o próximo elemento, sendo que o objecto `window` não recebe o evento `click`. Pode também levar a uma funcionalidade escondida que impede os utilizadores de utilizar a tua aplicação. 

<img src="../images/docs/outerclick-with-keyboard.gif" alt="Um botão que abre uma lista popover implementada com um padrão de click externo e também com a possibilidade de ser operada apenas com o teclado." />

A mesma funcionalidade pode ser obtida com os eventos apropriados, como `onBlur` e `onFocus`:

```javascript{19-29,31-34,37-38,40-41}
class BlurExample extends React.Component {
  constructor(props) {
    super(props);

    this.state = { isOpen: false };
    this.timeOutId = null;

    this.onClickHandler = this.onClickHandler.bind(this);
    this.onBlurHandler = this.onBlurHandler.bind(this);
    this.onFocusHandler = this.onFocusHandler.bind(this);
  }

  onClickHandler() {
    this.setState(currentState => ({
      isOpen: !currentState.isOpen
    }));
  }

  // Fechamos a popover no próximo tick usando o setTimeout.
  // É necessário porque precisamos primeiro de verificar o
  // outro filho do elemento que recebeu o focus
  // visto que o evento blur foi acionado antes do novo evento de focus.
  onBlurHandler() {
    this.timeOutId = setTimeout(() => {
      this.setState({
        isOpen: false
      });
    });
  }

  // Se o elemento filho receber focus, não fechamos a popover.
  onFocusHandler() {
    clearTimeout(this.timeOutId);
  }

  render() {
    // O React ajuda-nos a cancelar o evento blur
    // e a colocar o focus no elemento pai.
    return (
      <div onBlur={this.onBlurHandler}
           onFocus={this.onFocusHandler}>
        <button onClick={this.onClickHandler}
                aria-haspopup="true"
                aria-expanded={this.state.isOpen}>
          Select an option
        </button>
        {this.state.isOpen && (
          <ul>
            <li>Option 1</li>
            <li>Option 2</li>
            <li>Option 3</li>
          </ul>
        )}
      </div>
    );
  }
}
```

Este código expõe a funcionalidade para os utilizadores com dispositivos de cursor (rato) e teclado. Observe também as props `aria-*` adicionadas para suportar utilizadores com leitores de ecrã. Por motivos de simplificação as interações das `arrow keys` nas opções da popover não foram implementadas. 

<img src="../images/docs/blur-popover-close.gif" alt="Uma lista popover corretamente fechada com o rato e com o teclado." />

Este é um exemplo de muitos casos em que depender apenas dos eventos de cursor, pode quebrar a funcionalidade para os utilizadores de teclado. Deve-se ir testando sempre com o teclado para realçar imediatamente as áreas problemáticas que podem ser corrigidas usando os eventos de reconhecimento de teclado.

## Widgets mais complexos {#more-complex-widgets}

Uma experiência de utilização mais complexa não significa que seja menos acessível. Considerando que a acessibilidade é alcançada mais facilmente se programado o mais próximo possível do HTML, até mesmo o widget mais complexo pode ser programado de forma acessível.

Aqui é necessário conhecimento de [ARIA Roles](https://www.w3.org/TR/wai-aria/#roles) bem como [ARIA States and Properties](https://www.w3.org/TR/wai-aria/#states_and_properties).
Estas são ferramentas com atributos HTML que são totalmente suportadas em JSX e permitem construir componentes em React totalmente funcionais e totalmente acessíveis.

Cada tipo de widget tem um design específico e espera-se que funcione de uma certa forma:

- [WAI-ARIA Authoring Practices - Design Patterns and Widgets](https://www.w3.org/TR/wai-aria-practices/#aria_ex)
- [Heydon Pickering - ARIA Examples](https://heydonworks.com/article/practical-aria-examples/)
- [Inclusive Components](https://inclusive-components.design/)

## Outros pontos a ter em consideração {#other-points-for-consideration}

### Definir o idioma {#setting-the-language}

Indique o idioma dos textos da página, pois o software de leitura de ecrã usa essa definição para escolher a voz correta:

- [WebAIM - Document Language](https://webaim.org/techniques/screenreader/#language)

### Definir o titulo do documento {#setting-the-document-title}

Defina o `<title>` para descrever corretamente o conteúdo atual da página, pois isso garante que o utilizador tenha noção do contexto da página atual:

- [WCAG - Understanding the Document Title Requirement](https://www.w3.org/TR/UNDERSTANDING-WCAG20/navigation-mechanisms-title.html)

Podemos definir o título em React usando o [React Document Title Component](https://github.com/gaearon/react-document-title).

### Contraste de Cor {#color-contrast}

Certifica-te de que todo o texto no site tem o contraste suficiente para ser legível por utilizadores com baixa visão:

- [WCAG - Understanding the Color Contrast Requirement](https://www.w3.org/TR/UNDERSTANDING-WCAG20/visual-audio-contrast-contrast.html)
- [Everything About Color Contrast And Why You Should Rethink It](https://www.smashingmagazine.com/2014/10/color-contrast-tips-and-tools-for-accessibility/)
- [A11yProject - What is Color Contrast](https://a11yproject.com/posts/what-is-color-contrast/)

Pode ser um trabalho árduo calcular adequadamente todas as combinações de cores no website. Usa ferramentas como o [Colorable](https://jxnblk.com/colorable/).

As ferramentas aXe e WAVE mencionadas abaixo também incluem testes de contraste.

Se tu quiseres estender as suas habilidades de teste de contraste, podes usar estas ferramentas: 

- [WebAIM - Color Contrast Checker](https://webaim.org/resources/contrastchecker/)
- [The Paciello Group - Color Contrast Analyzer](https://www.paciellogroup.com/resources/contrastanalyser/)

## Ferramentas de Desenvolvimento e Testes {#development-and-testing-tools}

Existe várias ferramentas que pode usar para o ajudar na criação de aplicações web acessíveis.

### Teclado {#the-keyboard}

A verificação mais fácil e também mais importante é testar todo o teu website para verificar se pode ser utilizado apenas com o teclado, fazendo o seguinte:

1. Desconecta o rato.
1. Usa o `Tab` e o `Shift+Tab` para navegar.
1. Usa o `Enter` para clicar em elementos.
1. Se necessário, usa as setas do teclado e interaja com alguns elementos como, por exemplo menus e dropdowns.

### Assistência ao desenvolvimento {#development-assistance}

Podemos verificar algumas funcionalidades de acessibilidade diretamente no código JSX. Na maioria dos casos, os intellisense utilizados nos IDE's já fornecem verificações para os ARIA roles, estados e propriedades. Podemos também usar as seguintes ferramentas:

#### eslint-plugin-jsx-a11y {#eslint-plugin-jsx-a11y}

O plugin [eslint-plugin-jsx-a11y](https://github.com/evcohen/eslint-plugin-jsx-a11y) para o ESLint fornece feedback sobre o linting da AST em relação a problemas de acessibilidade no JSX. A maioria dos IDE's permite a integração dos resultados diretamente nas janelas de código.

[Create React App](https://github.com/facebookincubator/create-react-app) tem este plugin com um subconjunto de regras já ativadas. Se quiser pode ainda adicionar mais regras de acessibilidade, adicionando o ficheiro `.eslintrc` na pasta raíz do seu projeto com o seguinte conteúdo:

  ```json
  {
    "extends": ["react-app", "plugin:jsx-a11y/recommended"],
    "plugins": ["jsx-a11y"]
  }
  ```

### Testar acessibilidade no browser {#testing-accessibility-in-the-browser}

Existem várias ferramentas que podem executar auditorias de acessibilidade nas páginas web no browser. Use-as em combinação com outros testes de acessibilidade, uma vez que o browser só irá testar as acessibilidades do conteúdo HTML.

#### aXe, aXe-core e react-axe {#axe-axe-core-and-react-axe}

Deque Systems oferece [aXe-core](https://github.com/dequelabs/axe-core) para testes de acessibilidade automatizados e testes end-to-end de acessibilidade nas suas aplicações. Este modúlo inclui integrações com o Selenium.

[O Motor de Acessibilidade](https://www.deque.com/products/axe/) ou aXe, é uma extensão de browser desenvolvida com o `aXe-core`.

Podes também usar o módulo [react-axe](https://github.com/dylanb/react-axe) para reportar todos os erros de acessibilidade encontrados na consola durante o desenvolvimento e a depuração.

#### WebAIM WAVE {#webaim-wave}

O [Web Accessibility Evaluation Tool](https://wave.webaim.org/extension/) é outra extensão de acessibilidade para o browser.

#### Inspetores de Acessibilidade e Árvore de Acessibilidade {#accessibility-inspectors-and-the-accessibility-tree}

[The Accessibility Tree](https://www.paciellogroup.com/blog/2015/01/the-browser-accessibility-tree/) é um subconjunto da árvore DOM que contém objetos de acessibilidade para cada elemento DOM que deve ser disponibilidade para a tecnologia assistida como os leitores de ecrã.

Em alguns browsers podemos facilmente visualizar as informações de acessibilidade em cada elemento na árvore de acessibilidade:

- [Using the Accessibility Inspector in Firefox](https://developer.mozilla.org/en-US/docs/Tools/Accessibility_inspector)
- [Using the Accessibility Inspector in Chrome](https://developers.google.com/web/tools/chrome-devtools/accessibility/reference#pane)
- [Using the Accessibility Inspector in OS X Safari](https://developer.apple.com/library/content/documentation/Accessibility/Conceptual/AccessibilityMacOSX/OSXAXTestingApps.html)

### Leitores de ecrã {#screen-readers}

Testar com um leitor de ecrã deve fazer parte dos testes de acessibilidade.

Note que a combinação entre os browsers e os leitores de ecrã tem importância. É recomendado que testes a tua aplicação no browser mais adequando ao teu leitor de ecrã.

### Leitores de ecrã mais comuns {#commonly-used-screen-readers}

#### NVDA no Firefox {#nvda-in-firefox}

[NonVisual Desktop Access](https://www.nvaccess.org/) ou NVDA é um leitor de ecrã open source que é bastante utilizado.

Consulte os seguintes guias de como usar o NVDA:

- [WebAIM - Using NVDA to Evaluate Web Accessibility](https://webaim.org/articles/nvda/)
- [Deque - NVDA Keyboard Shortcuts](https://dequeuniversity.com/screenreaders/nvda-keyboard-shortcuts)

#### VoiceOver no Safari {#voiceover-in-safari}

VoiceOver é um leitor de ecrã integrado nos dispositivos da Apple.

Consulta os seguintes guias sobre como ativar e utilizar o VoiceOver:

- [WebAIM - Using VoiceOver to Evaluate Web Accessibility](https://webaim.org/articles/voiceover/)
- [Deque - VoiceOver for OS X Keyboard Shortcuts](https://dequeuniversity.com/screenreaders/voiceover-keyboard-shortcuts)
- [Deque - VoiceOver for iOS Shortcuts](https://dequeuniversity.com/screenreaders/voiceover-ios-shortcuts)

#### JAWS no Internet Explorer {#jaws-in-internet-explorer}

[Job Access With Speech](https://www.freedomscientific.com/Products/software/JAWS/) ou JAWS, é um leitor de ecrã muito popular no sistema operativo Windows.

Consulta os seguintes guias sobre como usar o JAWS:

- [WebAIM - Using JAWS to Evaluate Web Accessibility](https://webaim.org/articles/jaws/)
- [Deque - JAWS Keyboard Shortcuts](https://dequeuniversity.com/screenreaders/jaws-keyboard-shortcuts)

### Outros Leitores de Ecrã {#other-screen-readers}

#### ChromeVox no Google Chrome {#chromevox-in-google-chrome}

[ChromeVox](https://www.chromevox.com/) é um leitor de ecrã integrado nos Chromebooks e está disponível como [extensão](https://chrome.google.com/webstore/detail/chromevox/kgejglhpjiefppelpmljglcjbhoiplfn?hl=en) para o Google Chrome.

Consulte os seguintes guias de como usar o ChromeVox:

- [Google Chromebook Help - Use the Built-in Screen Reader](https://support.google.com/chromebook/answer/7031755?hl=en)
- [ChromeVox Classic Keyboard Shortcuts Reference](https://www.chromevox.com/keyboard_shortcuts.html)
