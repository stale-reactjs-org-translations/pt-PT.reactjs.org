---
id: pure-render-mixin
título: PureRenderMixin
layout: docs
categoria: Reference
permalink: docs/pure-render-mixin-old.html
---

> Nota

> O mixin `PureRenderMixin` é anterior ao ` React.PureComponent`. Este documento de referência é fornecido para fins de legado, e você deve considerar o uso do [`React.PureComponent`] (/ docs / react-api.html # reactpurecomponent).

If your React component's render function renders the same result given the same props and state, you can use this mixin for a performance boost in some cases.

Se o seu componente de função renderizador do React renderizar o mesmo resultado dados os mesmos prop e state, você pode usar este mixin para um aumento de desempenho em alguns casos.

Exemplo:

```js
var PureRenderMixin = require('react-addons-pure-render-mixin');
var createReactClass = require('create-react-class');
createReactClass({
  mixins: [PureRenderMixin],

  render: function() {
    return <div className={this.props.className}>foo</div>;
  }
});
```

Exemplo usando a classe em sintaxe ES6:

```js
import PureRenderMixin from 'react-addons-pure-render-mixin';
class FooComponent extends React.Component {
  constructor(props) {
    super(props);
    this.shouldComponentUpdate = PureRenderMixin.shouldComponentUpdate.bind(this);
  }

  render() {
    return <div className={this.props.className}>foo</div>;
  }
}
```

Nos bastidores, o mixin implementa [shouldComponentUpdate] (/ docs / component-specs.html # update-shouldcomponentupdate), no qual compara os props e state atuais com os próximos e retorna `false` se as equalizações forem aprovadas.

> Nota:
>

> Isto apenas compara os objetos superficialmente. Se eles contiverem estruturas de dados complexas, poderão produzir falsos negativos para diferenças mais profundas. Misture apenas em componentes que tenham props e state simples, ou use o `forceUpdate ()` quando souber que estruturas de dados profundas foram alteradas. Ou considere o uso dos [objetos imutáveis] (https://facebook.github.io/immutable-js/) para facilitar comparações rápidas de dados aninhados.
>

> Além disso, `shouldComponentUpdate` pula as atualizações para a subárvore inteira do componente. Certifique-se de que todos os componentes filhos também sejam "puros".
