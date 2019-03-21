---
id: faq-styling
title: Estilo e CSS
permalink: docs/faq-styling.html
layout: docs
category: FAQ
---

### Como eu adiciono classes CSS para componentes? {#how-do-i-add-css-classes-to-components}

Passa uma frase como a propriedade (props) className:

```jsx
render() {
  return <span className="menu navigation-menu">Menu</span>
}
```

É comum para as classes de CSS dependerem das propriedades (props) ou estado (state) da componente:

```jsx
render() {
  let className = 'menu';
  if (this.props.isActive) {
    className += ' menu-active';
  }
  return <span className={className}>Menu</span>
}
```

>Dica
>
>Se tu, frequentemente, encontras-te a escrever código como este, os pacotes [classnames](https://www.npmjs.com/package/classnames#usage-with-reactjs) podem simplificá-lo.

### Posso usar estilos em linha (inline styles)? {#can-i-use-inline-styles}

Sim, vê os documentos sobre estilo [aqui](/docs/dom-elements.html#style).

### Os estilos em linha são maus? {#are-inline-styles-bad}

As classes CSS são, geralmente, melhores para o desempenho do que os estilos em linha.

### O que é CSS-in-JS? {#what-is-css-in-js}

O “CSS-in-JS” refere-se a um padrão onde o CSS é composto usando JavaScript em vez de ser definido nos ficheiros externos. Lê uma comparação de bibliotecas de CSS-in-JS [aqui](https://github.com/MicheleBertoli/css-in-js).

_Nota que esta funcionalidade não é uma parte de React, mas fornecido por bilbiotecas de terceiros. React não tem uma opinião sobre como os estilos são definidos; em caso de dúvida, um bom ponto de partida é definir os teus estilos num ficheiro  `*.css` como de costume e fazer referência a eles usando [`className`](/docs/dom-elements.html#classname).

### Posso fazer animações no React? {#can-i-do-animations-in-react}

React pode ser usado para animar animações. Vê [React Transition Group](https://reactcommunity.org/react-transition-group/) e [React Motion](https://github.com/chenglou/react-motion), por exemplo.
