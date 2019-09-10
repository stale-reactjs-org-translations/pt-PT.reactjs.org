---
id: faq-ajax
title: AJAX e APIs
permalink: docs/faq-ajax.html
layout: docs
category: FAQ
---

### Como é que faço uma requisição AJAX? {#how-can-i-make-an-ajax-call}

Podes usar qualquer biblioteca AJAX que desejas com React. Algumas populares são [Axios](https://github.com/axios/axios), [jQuery AJAX](https://api.jquery.com/jQuery.ajax/), e o método nativo do navegador [window.fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API).

### Onde devo fazer uma requisição AJAX no ciclo de vida do componente? {#where-in-the-component-lifecycle-should-i-make-an-ajax-call}

Deves preencher dados com requisições AJAX no método [`componentDidMount`](/docs/react-component.html#mounting) do ciclo de vida. Isto é necessário para que consigas usar `setState` para atualizar o teu componente quando os dados forem recebidos.

### Example: Using AJAX results to set local state {#example-using-ajax-results-to-set-local-state}

The component below demonstrates how to make an AJAX call in `componentDidMount` to populate local component state. 

The example API returns a JSON object like this:

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
        // Note: it's important to handle errors here
        // instead of a catch() block so that we don't swallow
        // exceptions from actual bugs in components.
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
      return <div>Error: {error.message}</div>;
    } else if (!isLoaded) {
      return <div>Loading...</div>;
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
