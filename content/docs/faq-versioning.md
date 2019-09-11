---
id: faq-versioning
title: Política de Versão
permalink: docs/faq-versioning.html
layout: docs
category: FAQ
---

React segue os princípios de [versionamento semântico (semver)](https://semver.org/).

Isso significa que com um número de versão **x.y.z**:

* Ao lançarmos uma **atualização que quebra compatibilidade**, fazemos uma **major release** alterando o número **x** (ex: 15.6.2 para 16.0.0).
* Ao lançarmos uma **atualização com novas funcionalidades**, fazemos uma **minor release** alterando o número **y** (ex: 15.6.2 para 15.7.0).
* Ao lançarmos uma **atualização para correção de erros**, fazemos um **patch release** alterando o número **z** (ex: 15.6.2 para 15.6.3).

Atualizações que quebram compatibilidade podem também conter novas funcionalidades, e qualquer versão pode incluir correção de erros.

### Atualizações que quebram compatibilidade {#breaking-changes}

Atualizações que quebram compatibilidade são inconvenientes para todos, sendo assim, tentamos diminuir o número de major releases – por exemplo, React 15 foi lançado em Abril de 2016 e React 16 foi lançado em Setembro de 2017; React 17 não é esperado até 2019.

Todavia, lançamos novas funcionalidades em versões menores. Isso significa que atualizações menores (_minor releases_) são mais interessantes e convenientes que atualizações maiores (_major releases_), apesar de parecer o contrário.

### Compromisso com a estabilidade {#commitment-to-stability}

Conforme o React é alterado, tentamos minimizar o esforço necessário para tirar um melhor proveito das novas funcionalidades. Quando possível mantemos uma versão anterior da API em funcionamento, mesmo que isso signifique colocá-la em outro pacote. Por exemplo, [mixins são desencorajados há anos](/blog/2016/07/13/mixins-considered-harmful.html) porém atualmente continuam sendo suportados [via create-react-class](/docs/react-without-es6.html#mixins) e muitos projectos continuam a usá-los em versões estáveis e legadas.

Mais de um milhão de desenvolvedores utilizam o React, coletivamente mantendo milhões de componentes. Apenas o código base do Facebook possui mais de 50.000 componentes. Isso significa que precisamos deixá-lo o mais simples possível para atualizar para novas versões; Se nós realizarmos grandes mudanças sem um plano de migração, as pessoas poderão ficar presas em versões anteriores. As atualizações são testadas no próprio Facebook – se a nossa equipa de menos de 10 pessoas consegue atualizar 50.000+ de componentes sozinhos, nós esperamos que a atualização seja possível para qualquer um que use o React. Em muitos casos, escrevemos [scripts](https://github.com/reactjs/react-codemod) afim de atualizar a sintaxe dos componentes, nos quais incluímos na versão de código fonte para uso de todos.

### Atualizações graduais através de advertências {#gradual-upgrades-via-warnings}

Aplicações desenvolvidas em React incluem muitas advertências úteis. Sempre que possível, nós adicionamos advertências afim de preparar para grandes alterações futuras que possam quebrar a compatibilidade. Dessa forma, se a tua aplicação não possui nenhuma advertência na última versão, ela será compatível com a próxima grande atualização. Isso permitirá que atualizes os componentes da tua aplicação um de cada vez.

As advertências de desenvolvimento não afetarão o desempenho da tua aplicação. Dessa maneira, podes te sentir confiante de que sua aplicação se comportará da mesma maneira entre as versões de desenvolvimento e produção -- as únicas diferenças serão de que a versão de produção não apresentará logs de advertência, tornando-a mais eficiente. (Caso notes o contrário, por gentileza, abra uma issue).

### O quê conta como uma atualização que quebra compatibilidade? {#what-counts-as-a-breaking-change}

No geral, *não* alteramos o número de _major version_ por alterações como:

* * **Advertências de desenvolvimento.** Já que estas não afetam o comportamento de produção, nós podemos adicionar novas advertências ou modificar as existentes entre novas versões. De fato, isso é o que nos permite garantir a segurança das próximas versões. 
* **APIs començando com `unstable_`.** Estas são providas como funcionalidades experimentais das quais ainda não possuímos certa estabilidade das APIs. Ao lançarmos estas com um prefixo `unstable_`, podemos rapidamente melhorar e tornar a API estável de forma mais rápida.  
* **Versões alpha e canary do React.** Nós fornecemos versões alpha do React como uma maneira de testar novas funcionalidades de forma mais rápida, porém precisamos da flexibilidade para realizar alterações baseadas no que aprendemos com o período da versão alpha. Caso faças uso destas versões, nota que as APIs podem mudar antes do lançamento da versão estável. 
* **APIs não documentadas e estrutura de dados interna.** Se acessas propriedades com nomes internos como `__SECRET_INTERNALS_DO_NOT_USE_OR_YOU_WILL_BE_FIRED` ou `__reactInternalInstance$uk43rzhitjg`, não há garantia alguma. Estás por conta própria.

Este documento é destinado a ser pragmático: obviamente, não queremos causar-te dores de cabeça. Se nós alterarmos a _major version_ para todas essas mudanças, nós acabaríamos por lançar mais _major versions_ e consequentemente causando mais dores de versionamento para a comunidade. Isso também significaria que não poderíamos melhorar o React tão rápido quanto gostaríamos.

Dito isso, se nós percebermos que uma mudança desta lista possa causar problemas na comunidade, ainda faremos o nosso melhor para prover um plano gradual de migração.
