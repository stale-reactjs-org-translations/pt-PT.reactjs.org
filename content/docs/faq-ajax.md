---
id: faq-ajax
title: AJAX e APIs
permalink: docs/faq-ajax.html
layout: docs
category: FAQ
---

### Como posso fazer uma chamada de AJAX? {#how-can-i-make-an-ajax-call}

Podes usar qualquer biblioteca AJAX que quiseres no React. Algumas populares são [Axios](https://github.com/axios/axios), [jQuery AJAX](https://api.jquery.com/jQuery.ajax/), e o navegador embutido [window.fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API).

### Onde no ciclo da vida da componente devo fazer uma chamada de AJAX? {#where-in-the-component-lifecycle-should-i-make-an-ajax-call}

Deves preencher dados com chamadas de AJAX no ciclo da vida do método [`componentDidMount`](/docs/react-component.html#mounting) . Isto é para que possas usar `setState` para atualizar o teu componente quando os dados são recuperados.

### Exemplo: Usando resultados de AJAX para definir estado local{#example-using-ajax-results-to-set-local-state}

O componente abaixo, demonstra como fazer uma chamada de AJAX em `componentDidMount` para preencher o estado do componente local.

O exemplo API retorna um objeto JSON como este:

```
{
  "items": [
    { "id": 1, "name": "Apples",  "price": "$2" },
    { "id": 2, "name": "Peaches", "price": "$5" }
  ] 
}
```

```jsx
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      error: null,
      isLoaded: false,
      items: []
    };
  }

  componentDidMount() {
    fetch("https://api.example.com/items")
      .then(res => res.json())
      .then(
        (result) => {
          this.setState({
            isLoaded: true,
            items: result.items
          });
        },
        // Nota: é importante lidar com erros aqui
        // em vez de um bloco catch() para não "engolirmos"
        // exceções de bugs atuais em componentes.
        (error) => {
          this.setState({
            isLoaded: true,
            error
          });
        }
      )
  }

  render() {
    const { error, isLoaded, items } = this.state;
    if (error) {
      return <div>Erro: {error.message}</div>;
    } else if (!isLoaded) {
      return <div>Carregando...</div>;
    } else {
      return (
        <ul>
          {items.map(item => (
            <li key={item.name}>
              {item.name} {item.price}
            </li>
          ))}
        </ul>
      );
    }
  }
}
```
