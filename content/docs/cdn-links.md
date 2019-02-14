---
id: cdn-links
title: Hiperligações CDN
permalink: docs/cdn-links.html
prev: create-a-new-react-app.html
next: hello-world.html
---

Tanto React como o ReactDOM estão disponíveis através de uma CDN (_Rede de Entrega de Conteúdos_).

```html
<script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
```

As versões acima são para uso num ambiente de desenvolvimento, não são adequadas para ambientes em produção. Versões minificadas e optimizadas para produção podem ser encontradas em:

```html
<script crossorigin src="https://unpkg.com/react@16/umd/react.production.min.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.production.min.js"></script>
```

Para carregar uma versão específica de `react` e `react-dom` substitui `16` com o número da versão que pretendes.

### O Porquê do Atributo `crossorigin`? {#why-the-crossorigin-attribute}

Caso uses React através de CDN, recomendamos que mantenhas o atributo [`crossorigin`](https://developer.mozilla.org/en-US/docs/Web/HTML/CORS_settings_attributes) definido:

```html
<script crossorigin src="..."></script>
```

Recomendamos também que verifiques se o CDN que estás a usar define `Access-Control-Allow-Origin: *` no cabeçalho HTTP:

![Access-Control-Allow-Origin: *](../images/docs/cdn-cors-header.png)

Isto vai permitir uma melhor [experiência a lidar com erros](/blog/2017/07/26/error-handling-in-react-16.html) ao usares React 16 e versões mais avançadas.
