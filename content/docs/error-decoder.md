---
id: error-decoder
title: Descodificador de Erros
permalink: docs/error-decoder.html
---

Na versão minificada do _build_ de produção do React, evitamos o envio de mensagens de erro completas de forma a reduzir o número de bytes enviados pela rede.

É altamente recomendável que uses o _build_ de desenvolvimento localmente quando estiveres a depurar a tua aplicação, já que esta versão rastreia informações adicionais de depuração e fornece vários avisos úteis sobre possíveis problemas nas tuas aplicações. Mas se encontrares uma excepção ao utilizar o _build_ de produção, esta página irá reconstruir o texto original do erro.
