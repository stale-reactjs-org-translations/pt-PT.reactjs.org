---
title: Don't Call PropTypes Warning
layout: single
permalink: warnings/dont-call-proptypes.html
---

> Note:
>
> `React.PropTypes` oi movido para um pacote diferente desde a versão v15.5. do React. Por favor usa [antes a biblioteca `prop-types`](https://www.npmjs.com/package/prop-types).
>
>Nós fornecemos [um script de migração](/blog/2017/04/07/react-v15.5.0.html#migrating-from-react.proptypes) para automatizar o processo.

No proximo lançamento do React, o código que implementa funções de validações PropType  functions será removido. Quando acontecer, qualquer código que chama estas funções manualmente erão emitir um erro.

### Declarar PropTypes ainda é uma boa ideia {#declaring-proptypes-is-still-fine}

O uso normal de PropTypes ainda é suportado:

```javascript
Button.propTypes = {
  highlighted: PropTypes.bool
};
```

Nada mudou.

### Não usar diretamente {#dont-call-proptypes-directly}

Usar PropTypes de uma forma diferente que não seja a anotação dos componentes React não é mais suportado:

```javascript
var apiShape = PropTypes.shape({
  body: PropTypes.object,
  statusCode: PropTypes.number.isRequired
}).isRequired;

// Not supported!
var error = apiShape(json, 'response');
```

Se estiver dependente de usar PropTypes desta forma, encorajamos a usar ou a criar um fork dos PropTypes (por exemplo [estes](https://github.com/aackerman/PropTypes) [dois](https://github.com/developit/proptypes) packages).

Se não corregir o alerta, este codigo vai falhar em produção com React 16.

### Se não usar PropTypes diretamente e continuar a ver esse alerta {#if-you-dont-call-proptypes-directly-but-still-get-the-warning}

Inspecione o stack trace produzido pelo alerta. Encontrará a definição do componente responsável pela chamada direta do PropTypes. Provavelmente, o problema é causado por um PropTypes de terceiros que encapsula o PropTypes do React, por exemplo:

```js
Button.propTypes = {
  highlighted: ThirdPartyPropTypes.deprecated(
    PropTypes.bool,
    'Use `active` prop instead'
  )
}
```

Nesse caso, `ThirdPartyPropTypes.deprecated` é invólucro chamado `PropTypes.bool`. Esse padrão por si só já é o suficiente, mas dispara um falso positivo pelo fato do React pensar que está chamando diretamente o PropTypes. Na proxima secção, , iremos explicar como resolver esse problema para a implementação de uma biblioteca, algo como `ThirdPartyPropTypes`. Se não escreveu uma biblioteca, pode abir um issue por isso.

### Resolvendo o falso positivo em uma PropTypes de terceiro{#fixing-the-false-positive-in-third-party-proptypes}

Se for o autor de uma biblioteca de terceiro do PropTypese permite que seus usuários façam wrap de uma React PropTypes existente, provavelmente eles começarão a receber esse warning da sua biblioteca. Isso acontece porque o React não enxerga um último parâmetro "secreto" que [passa a](https://github.com/facebook/react/pull/7132) detectar manualmente as chamadas da PropTypes.

Aqui está como corrigir isso. Usaremos `deprecated` daqui [react-bootstrap/react-prop-types](https://github.com/react-bootstrap/react-prop-types/blob/0d1cd3a49a93e513325e3258b28a82ce7d38e690/src/deprecated.js) como exemplo. A actual implementação só passa argumentos `props`, `propName`, e `componentName`:

```javascript
export default function deprecated(propType, explanation) {
  return function validate(props, propName, componentName) {
    if (props[propName] != null) {
      const message = `"${propName}" property of "${componentName}" has been deprecated.\n${explanation}`;
      if (!warned[message]) {
        warning(false, message);
        warned[message] = true;
      }
    }

    return propType(props, propName, componentName);
  };
}
```

Para resolver o falso positivo, tenha certeza que está passando todos os parâmetro para o wrapped da PropType. Um jeito fácil de fazer isso com ES6 é usando a notação `...rest`:

```javascript
export default function deprecated(propType, explanation) {
  return function validate(props, propName, componentName, ...rest) { // Note ...rest here
    if (props[propName] != null) {
      const message = `"${propName}" property of "${componentName}" has been deprecated.\n${explanation}`;
      if (!warned[message]) {
        warning(false, message);
        warned[message] = true;
      }
    }

    return propType(props, propName, componentName, ...rest); // and here
  };
}
```

Isso fará com que o alerta pare de ser lançado.