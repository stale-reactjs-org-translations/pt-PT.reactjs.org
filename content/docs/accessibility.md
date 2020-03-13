---
id: accessibility
title: Accessibility
permalink: docs/accessibility.html
---

## Porque Acessibilidade? {#why-accessibility}

A acessibilidade na web (também conhecida por [** a11y **](https://en.wiktionary.org/wiki/a11y)) é o design e a criação de websites que podem ser usados por todos. O suporte à acessibilidade é necessário para permitir que as tecnologias assistivas interpretem as páginas da web.

O React suporta totalmente a construção de websites acessíveis, muitas vezes com apenas técnias de HTML padrão.

## Standards and Guidelines {#standards-and-guidelines}

### WCAG {#wcag}

O [Web Content Accessibility Guidelines](https://www.w3.org/WAI/intro/wcag) fornece as diretrizes necessárias para a criação de sites acessíveis.

As seguintes checklists das WCAG fornecem uma visão geral:

- [WCAG checklist from Wuhcag](https://www.wuhcag.com/wcag-checklist/)
- [WCAG checklist from WebAIM](https://webaim.org/standards/wcag/checklist)
- [Checklist from The A11Y Project](https://a11yproject.com/checklist.html)

### WAI-ARIA {#wai-aria}

O documento [Web Accessibility Initiative - Accessible Rich Internet Applications](https://www.w3.org/WAI/intro/aria) contém as técnicas necessárias para criar widgets totalmente acessíveis.

Nota que todos os atributos HTML `aria-*` são totalmente suportados em JSX. Apesar da maioria das proriedades e atributos do DOM no React serem em camelCase, estes atributos devem ser em hyphen-cased (tambem conhecido como kebab-case, lisp-case, etc) pois estão em HTML:

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
A semântica é a base de uma aplicação web acessível. Se usarmos corretamente os elementos HTML para reforçar o significado da informação colocada nos nossos websites, muitas das vezes a acessibilidade pode ser gratuita.

- [MDN HTML elements reference](https://developer.mozilla.org/en-US/docs/Web/HTML/Element)

Às vezes, quebramos a semântica HTML para adicionar elementos `<div>` ao nosso JSX apenas para fazer funcionar no React, especialmente quando trabalharmos com listas (`<ol>`, `<ul>` and `<dl>`) e com `<table>`.
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

Podemos mapear uma coleção de itens para um array de fragments tal como fariamos para qualquer outro tipo de elemento:

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

Quando não é necessario passar propriedades para a tag Fragment podemos usar a [syntax curta](/docs/fragments.html#short-syntax), se a configuração suportar:

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

Para mais informação, veja a [documentação sobre Fragments](/docs/fragments.html).

## Formulários Acessíveis {#accessible-forms}

### Rótulos {#labeling}
Todos os elementos de um formulário HTML como `<input>` e `<textarea>`, precisam de ser rótulados. É necessário fornecer rótulos descritivos pois estes são expostos aos leitores de ecrã.

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

Situações de erro precisam de ser entendidas por todos os utilizadores. Os artigos seguintes mostam como export os erros aos leitores de ecrã:

- [The W3C demonstrates user notifications](https://www.w3.org/WAI/tutorials/forms/notifications/)
- [WebAIM looks at form validation](https://webaim.org/techniques/formvalidation/)

## Controlo de foco {#focus-control}

Certifique-se que a aplicação web possa ser utilizada apenas com o teclado:

- [WebAIM talks about keyboard accessibility](https://webaim.org/techniques/keyboard/)

### Foco no teclado e foco no contorno {#keyboard-focus-and-focus-outline}

Foco no teclado refere-se ao elemento DOM que foi selecionado e que aceita ações do teclado. Podemos ver o contorno na imagem a seguir:

<img src="../images/docs/keyboard-focus.png" alt="Contorno azul à volta da ligação selecionada." />

Apenas use CSS que elimine este contorno, por exemplo definindo `outline: 0`, se for substituir por outra implementação de foco.

### Mecanismos para avançar para o conteudo desejado {#mechanisms-to-skip-to-desired-content}

Providencie mecanismos para permitir que os utilizadores consigam ignorar as secções de navegação, acelarando assim a navegação com o teclado.

Skiplinks ou Skip Navigation Links são ligações escondidos que só ficam visiveis quando os utilizadores de teclado interagem com a página. São muito fáceis de implementar com alguns estilos e âncoras:

- [WebAIM - Skip Navigation Links](https://webaim.org/techniques/skipnav/)

Use também elementos e pontos de referência como `<main>` e `<aside>`, para demarcar zonas da página, sendo que as tecnologias de acessibilidade permitem assim que o utilizador navege rapidamente nessas seções.

Saiba mais sobre o uso destes elementos para melhorar a acessibilidade aqui:

- [Accessible Landmarks](https://www.scottohara.me/blog/2018/03/03/landmarks.html)

### Controlar programaticamente o foco {#programmatically-managing-focus}

As nossas aplicações React modificam continuamente o HTML DOM durante o tempo de execução, às vezes perdendo o foco do teclado ou adicionado o foco a um elemento inesperado. Para corrigir isso, é necessário programar o foco do teclado na direção certo. Por exemplo, colocar o foco do teclado no botão que faz abrir uma modal depois dessa mesma modal ser fechada.

Pode encontrar mais informações de como implementar no MDN Web Docs [keyboard-navigable JavaScript widgets](https://developer.mozilla.org/en-US/docs/Web/Accessibility/Keyboard-navigable_JavaScript_widgets).

Para colocar o foco no React, podemos usar [Refs para elementos no DOM](/docs/refs-and-the-dom.html).

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
  // text input nesta instancia (por exemplo, this.textInput).
    return (
      <input
        type="text"
        ref={this.textInput}
      />
    );
  }
}
```

Depois podemos colocar o foco quando for necessário:

 ```javascript
 focus() {
   // Explicitamente devemos usar a DOM API nativa para colocar o foco
   // Nota: estamos a aceder ao "current" para obter o elemento DOM
   this.textInput.current.focus();
 }
 ```

Às vezes um componente pai precisa de colocar o foco a um elemento no componente filho. Podemos fazer isso [expondo as referências DOM aos componentes pais](/docs/refs-and-the-dom.html#exposing-dom-refs-to-parent-components) através de uma `prop` especial no componente filho que encaminhe o componente pai ao elemento DOM do componente filho.

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

// Agora podemos colocar o foco quando for necessário.
this.inputElement.current.focus();
```

Ao usar um HOC para estender componentes, é recomendado [encaminhar a ref](/docs/forwarding-refs.html) para o elemento embrulhado usando a função de React `forwardRef`. se um third party HOC  não implementar o `forwardRef`, o padrão acima pode ser usado como fallback.

Um ótimo exemplo de controlar o foco é o [react-aria-modal](https://github.com/davidtheclark/react-aria-modal). Este é um exemplo relativamente raro de uma janela modal totalmente acessível. Não só coloca o foco inicial no botão de cancelar (prevenindo assim que o utilizador ative acidentalmente a ação de sucesso) mas também bloqueia o foco do teclado dentro da modal, e ainda reinicia o foco ao elemento que primeiramente acionou a modal.

>Nota:
>
>Embora seja um recurso de acessibilidade muito importante, esta técnica deve ser usada de maneira criteriosa. Use-o para corrigir o comportamento do foco quando este está distorcido, e não para tentar
>antecipar como os utilizadores querem usar a aplicação.

## Eventos do rato e cursor {#mouse-and-pointer-events}

Certifique-se de que todas as funcionalidades expostas atráves dos eventos do rato ou cursor também possam ser acedidas usando apenas o teclado. Se depender apenas no rato, vai haver muitos casos em que os utilizadores não consigam utilizar a sua aplicação.

Para ilustrar isto, abaixa pode ver um exemplo clássico de quebra da acessibilidade causada por um click. Este é o padrão de click externo, em que o utilizador pode desativar uma popover aberto ao carregar fora do elemento.

<img src="../images/docs/outerclick-with-mouse.gif" alt="Um botão que abre uma lista popover implementada com um padrão de click externo e operado por um rato a mostrar quee a ação de fechar funciona." />

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

Isso pode funcionar bem para os utilizadores com dispositivos com ponteiro, como por exemplo um rato, mas se for apenas com um teclado pode quebrar a funcionalidade ao utilizar o `tab` para o próximo elemento, sendo que o objecto `window` não recebe o evento `click`. Pode também levar a uma funcionalidade escondida que impede os utilizadores de utilizar a aplicação. 

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

  // Fechamos a popover no proximo tick usando o setTimeout.
  // É necessário porque precisamos primeiro de verificar o
  // outro filho do elemento que recebeu o foco
  // visto que o evento blue foi acionado antes do novo evento de foco.
  onBlurHandler() {
    this.timeOutId = setTimeout(() => {
      this.setState({
        isOpen: false
      });
    });
  }

  // Se o elemento filho receber foco, não fechamos a popover.
  onFocusHandler() {
    clearTimeout(this.timeOutId);
  }

  render() {
    // O React ajuda-nos a cancelar o evento blur
    // e a colocar o foco no elemento pai.
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

<img src="../images/docs/blur-popover-close.gif" alt="Uma lista popover correamente fechada com o rato e com o teclado." />

Este é um exemplo de muitos casos em que depender apenas dos eventos de cursor, pode quebrar a funcionalidade para os utilizadores de teclado. Deve ir testando sempre com o teclado porque vai realçar logo as áreas problemáticas que podem ser corrigidas usando os eventos de reconhecimento de teclado.

## Widgets mais complexos {#more-complex-widgets}

Uma experiencia de utilização mais complexa não significa que seja menos acessível. Considerando que a acessibilidade é alcançada mais facilmente se programado o mais proximo possivel do HTML, até mesmo o widget mais complexo pode ser programado de forma acessível.

Aqui é necessário conhecimento de [ARIA Roles](https://www.w3.org/TR/wai-aria/#roles) bem como [ARIA States and Properties](https://www.w3.org/TR/wai-aria/#states_and_properties).
Estas são ferramentas com atrributos HTML que são totalmente suportadas em JSX e permitem construir componentes em React totalmente funcionais e totalmente acessíveis.

Cada tipo de widget tem um design específico e espera-se que funcione de uma certa forma:

- [WAI-ARIA Authoring Practices - Design Patterns and Widgets](https://www.w3.org/TR/wai-aria-practices/#aria_ex)
- [Heydon Pickering - ARIA Examples](https://heydonworks.com/practical_aria_examples/)
- [Inclusive Components](https://inclusive-components.design/)

## Other Points for Consideration {#other-points-for-consideration}

### Setting the language {#setting-the-language}

Indicate the human language of page texts as screen reader software uses this to select the correct voice settings:

- [WebAIM - Document Language](https://webaim.org/techniques/screenreader/#language)

### Setting the document title {#setting-the-document-title}

Set the document `<title>` to correctly describe the current page content as this ensures that the user remains aware of the current page context:

- [WCAG - Understanding the Document Title Requirement](https://www.w3.org/TR/UNDERSTANDING-WCAG20/navigation-mechanisms-title.html)

We can set this in React using the [React Document Title Component](https://github.com/gaearon/react-document-title).

### Color contrast {#color-contrast}

Ensure that all readable text on your website has sufficient color contrast to remain maximally readable by users with low vision:

- [WCAG - Understanding the Color Contrast Requirement](https://www.w3.org/TR/UNDERSTANDING-WCAG20/visual-audio-contrast-contrast.html)
- [Everything About Color Contrast And Why You Should Rethink It](https://www.smashingmagazine.com/2014/10/color-contrast-tips-and-tools-for-accessibility/)
- [A11yProject - What is Color Contrast](https://a11yproject.com/posts/what-is-color-contrast/)

It can be tedious to manually calculate the proper color combinations for all cases in your website so instead, you can [calculate an entire accessible color palette with Colorable](https://jxnblk.com/colorable/).

Both the aXe and WAVE tools mentioned below also include color contrast tests and will report on contrast errors.

If you want to extend your contrast testing abilities you can use these tools:

- [WebAIM - Color Contrast Checker](https://webaim.org/resources/contrastchecker/)
- [The Paciello Group - Color Contrast Analyzer](https://www.paciellogroup.com/resources/contrastanalyser/)

## Development and Testing Tools {#development-and-testing-tools}

There are a number of tools we can use to assist in the creation of accessible web applications.

### The keyboard {#the-keyboard}

By far the easiest and also one of the most important checks is to test if your entire website can be reached and used with the keyboard alone. Do this by:

1. Disconnecting your mouse.
1. Using `Tab` and `Shift+Tab` to browse.
1. Using `Enter` to activate elements.
1. Where required, using your keyboard arrow keys to interact with some elements, such as menus and dropdowns.

### Development assistance {#development-assistance}

We can check some accessibility features directly in our JSX code. Often intellisense checks are already provided in JSX aware IDE's for the ARIA roles, states and properties. We also have access to the following tool:

#### eslint-plugin-jsx-a11y {#eslint-plugin-jsx-a11y}

The [eslint-plugin-jsx-a11y](https://github.com/evcohen/eslint-plugin-jsx-a11y) plugin for ESLint provides AST linting feedback regarding accessibility issues in your JSX. Many IDE's allow you to integrate these findings directly into code analysis and source code windows.

[Create React App](https://github.com/facebookincubator/create-react-app) has this plugin with a subset of rules activated. If you want to enable even more accessibility rules, you can create an `.eslintrc` file in the root of your project with this content:

  ```json
  {
    "extends": ["react-app", "plugin:jsx-a11y/recommended"],
    "plugins": ["jsx-a11y"]
  }
  ```

### Testing accessibility in the browser {#testing-accessibility-in-the-browser}

A number of tools exist that can run accessibility audits on web pages in your browser. Please use them in combination with other accessibility checks mentioned here as they can only
test the technical accessibility of your HTML.

#### aXe, aXe-core and react-axe {#axe-axe-core-and-react-axe}

Deque Systems offers [aXe-core](https://github.com/dequelabs/axe-core) for automated and end-to-end accessibility tests of your applications. This module includes integrations for Selenium.

[The Accessibility Engine](https://www.deque.com/products/axe/) or aXe, is an accessibility inspector browser extension built on `aXe-core`.

You can also use the [react-axe](https://github.com/dylanb/react-axe) module to report these accessibility findings directly to the console while developing and debugging.

#### WebAIM WAVE {#webaim-wave}

The [Web Accessibility Evaluation Tool](https://wave.webaim.org/extension/) is another accessibility browser extension.

#### Accessibility inspectors and the Accessibility Tree {#accessibility-inspectors-and-the-accessibility-tree}

[The Accessibility Tree](https://www.paciellogroup.com/blog/2015/01/the-browser-accessibility-tree/) is a subset of the DOM tree that contains accessible objects for every DOM element that should be exposed
to assistive technology, such as screen readers.

In some browsers we can easily view the accessibility information for each element in the accessibility tree:

- [Using the Accessibility Inspector in Firefox](https://developer.mozilla.org/en-US/docs/Tools/Accessibility_inspector)
- [Using the Accessibility Inspector in Chrome](https://developers.google.com/web/tools/chrome-devtools/accessibility/reference#pane)
- [Using the Accessibility Inspector in OS X Safari](https://developer.apple.com/library/content/documentation/Accessibility/Conceptual/AccessibilityMacOSX/OSXAXTestingApps.html)

### Screen readers {#screen-readers}

Testing with a screen reader should form part of your accessibility tests.

Please note that browser / screen reader combinations matter. It is recommended that you test your application in the browser best suited to your screen reader of choice.

### Commonly Used Screen Readers {#commonly-used-screen-readers}

#### NVDA in Firefox {#nvda-in-firefox}

[NonVisual Desktop Access](https://www.nvaccess.org/) or NVDA is an open source Windows screen reader that is widely used.

Refer to the following guides on how to best use NVDA:

- [WebAIM - Using NVDA to Evaluate Web Accessibility](https://webaim.org/articles/nvda/)
- [Deque - NVDA Keyboard Shortcuts](https://dequeuniversity.com/screenreaders/nvda-keyboard-shortcuts)

#### VoiceOver in Safari {#voiceover-in-safari}

VoiceOver is an integrated screen reader on Apple devices.

Refer to the following guides on how to activate and use VoiceOver:

- [WebAIM - Using VoiceOver to Evaluate Web Accessibility](https://webaim.org/articles/voiceover/)
- [Deque - VoiceOver for OS X Keyboard Shortcuts](https://dequeuniversity.com/screenreaders/voiceover-keyboard-shortcuts)
- [Deque - VoiceOver for iOS Shortcuts](https://dequeuniversity.com/screenreaders/voiceover-ios-shortcuts)

#### JAWS in Internet Explorer {#jaws-in-internet-explorer}

[Job Access With Speech](https://www.freedomscientific.com/Products/software/JAWS/) or JAWS, is a prolifically used screen reader on Windows.

Refer to the following guides on how to best use JAWS:

- [WebAIM - Using JAWS to Evaluate Web Accessibility](https://webaim.org/articles/jaws/)
- [Deque - JAWS Keyboard Shortcuts](https://dequeuniversity.com/screenreaders/jaws-keyboard-shortcuts)

### Other Screen Readers {#other-screen-readers}

#### ChromeVox in Google Chrome {#chromevox-in-google-chrome}

[ChromeVox](https://www.chromevox.com/) is an integrated screen reader on Chromebooks and is available [as an extension](https://chrome.google.com/webstore/detail/chromevox/kgejglhpjiefppelpmljglcjbhoiplfn?hl=en) for Google Chrome.

Refer to the following guides on how best to use ChromeVox:

- [Google Chromebook Help - Use the Built-in Screen Reader](https://support.google.com/chromebook/answer/7031755?hl=en)
- [ChromeVox Classic Keyboard Shortcuts Reference](https://www.chromevox.com/keyboard_shortcuts.html)
