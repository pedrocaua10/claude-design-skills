---
name: "design-frontend"
description: "Cria ou refatora interfaces front-end (landing pages, telas, componentes) com direção visual própria, tokens de design e código pronto para uso. Use ao pedir para criar, estilizar, redesenhar ou dar acabamento em uma página ou componente."
---

# Design Front-end

Transforma um pedido de interface em código pronto, com identidade visual específica para o assunto e sem cara de template. Entregue o arquivo final completo, sem explicar conceitos de design. Para revisar usabilidade e acessibilidade do resultado, use a skill `design-uiux`.

## Ordem de precedência

Aplique nesta ordem, sem inverter:

1. O que o usuário pediu, com as palavras dele.
2. O sistema de design que o projeto já tem (`DESIGN.md`, `CLAUDE.md`, arquivo de tokens, estilos dos componentes existentes).
3. As escolhas desta skill.

Em projeto existente, siga a stack, as convenções e a lógica que já estão lá. Não troque biblioteca, não renomeie classes e não mude comportamento que não foi pedido.

## Modos de entrada

Escolha pelo que o usuário forneceu:

**Modo Pedido direto**: o usuário descreveu o que quer com clareza suficiente (assunto, objetivo, estilo ou referência).
1. Monte o plano de design (abaixo) e construa.
2. Entregue a primeira versão antes de perguntar. Pergunte depois, só o que bloqueia.

**Modo Briefing**: o pedido é vago ("faz uma landing page bonita").
1. Faça as perguntas em **uma única mensagem**, numeradas e curtas. Use AskUserQuestion se disponível, para as de múltipla escolha.
2. Se o usuário responder só parte, siga com o que tem e marque lacunas com `[PENDENTE: ...]`.

Perguntas do briefing (use as que não foram respondidas):
- Qual é o assunto e quem vai usar ou ler a página?
- Qual é a única ação que a página precisa provocar (contato, compra, cadastro, leitura)?
- Há referência visual, marca, cores ou fontes já definidas?
- Qual é a stack e onde o código vai morar (arquivo único, Angular, React, outro)?
- Que conteúdo real existe (textos, números, imagens, certificados)?

Nunca invente conteúdo factual: cargos, métricas, clientes, depoimentos, datas. Se faltar, escreva `[PENDENTE: ...]` no ponto exato e liste ao final.

## Fluxo de entrega

1. **Plano de design.** Defina em poucas linhas: assunto, público, objetivo único da página e um detalhe que só este assunto teria (unidade, termo técnico, convenção do domínio). Esse detalhe entra como conteúdo, não como enfeite.
2. **Tokens.** Escreva primeiro o bloco de variáveis: 4 a 6 cores nomeadas, 2 a 3 papéis tipográficos, escala de espaçamento, raios, sombras e durações de movimento. Se o projeto tiver `DESIGN.md`, use-o; se não tiver e o projeto for continuar, ofereça gerá-lo ao final.
3. **Estrutura e layout.** Mobile-first, grid e flex com `gap`.
4. **Estados e temas.** Claro e escuro pelos tokens; hover, foco, ativo, desabilitado, vazio, carregando e erro quando se aplicam.
5. **Movimento.** Só onde serve ao conteúdo (abaixo).
6. **Revisão.** Rode o checklist e corrija antes de entregar.
7. **Entrega.** Arquivo completo, pronto para salvar e abrir, seguido de uma linha com o que ficou como suposição ou `[PENDENTE]`.

## Direção visual

- A página abre com a coisa mais característica do assunto: título, imagem, demonstração ou interação. Não abra com um bloco genérico de números.
- Escolha uma direção e execute com precisão. Ousadia em um único lugar, o resto quieto.
- Evite o visual padrão de IA: fundo creme com serifada e terracota, preto com um único acento neon, degradê roxo para azul sobre branco, Inter ou Space Grotesk por reflexo, emoji como marcador de seção, tudo centralizado, mesmo raio e sombra em todos os cartões, barra colorida na lateral de cartão.
- Tipografia define a personalidade. Combine uma face de título com caráter, uma de texto legível e, se precisar, uma monoespaçada para dados e rótulos. Mantenha uma escala fixa. Linhas de leitura com cerca de 65 caracteres. `text-wrap: balance` em títulos.
- Neutros escolhidos, não herdados: cinza com leve inclinação de matiz para o acento. Cinza puro parece esquecido.
- Cartão, borda, sombra e raio marcam um elemento como objeto separado. Use por papel, no que precisa ser destacado, e não em tudo.
- Estrutura carrega informação. Numeração só para sequências reais. Rótulos e eyebrows devem dizer algo verdadeiro sobre o conteúdo.

## Layout e responsividade

- Gutter lateral mínimo de 16px em qualquer largura. A página funciona em 400px e nunca rola na horizontal.
- Linhas de flex e grid quebram ou empilham em telas estreitas. Filhos com texto, código ou tabela levam `min-width: 0`.
- Tabelas, diagramas e blocos de código largos ficam dentro de um contêiner próprio com `overflow-x: auto`.
- Espaçamento entre irmãos com `gap`, não com margens por elemento.
- Elementos repetidos (cartões, linhas, selos) têm as mesmas bordas, linha de base e preenchimento, e o item recorrente fica no mesmo lugar em cada um.
- Números alinhados em coluna usam `font-variant-numeric: tabular-nums`.

## Temas

- Toda cor é um token definido em `:root`. O tema escuro redefine os tokens em `@media (prefers-color-scheme: dark)` e em `[data-theme="dark"]`, para o botão de alternância funcionar nos dois sentidos.
- O `body` define o fundo explicitamente por token.
- Não inverta o tema: reajuste contraste e acento para funcionarem no fundo escuro.

## Movimento e scroll interativo

- Anime para explicar ou orientar: progresso de leitura, destaque da seção ativa, linha que se preenche, cartões que empilham com `position: sticky`, contadores que respondem a filtros.
- Tudo que deve ser lido fica visível ao carregar. Animação parte de um estado legível e nunca deixa conteúdo em `opacity: 0` esperando um observador.
- Prefira `transform` e `opacity`. Use `IntersectionObserver` e `requestAnimationFrame`; evite ouvintes de scroll sem controle.
- Respeite `prefers-reduced-motion`: sem animação, o resultado final aparece direto.
- Um momento orquestrado vale mais que efeitos espalhados. Em dúvida, menos movimento.

## Código

- HTML semântico (`header`, `main`, `section`, `nav`, `button`), uma hierarquia de títulos coerente e `lang` correto.
- CSS com variáveis, classes com nomes de papel (`.proj`, `.step`), cuidado com especificidade para uma regra não anular a outra.
- JavaScript enxuto, sem biblioteca quando o recurso nativo resolve. Se precisar de uma, carregue uma versão fixa e explique em uma linha.
- Fontes com pilha de fallback real.
- Poucos comentários: só onde a intenção não é óbvia.
- Foco visível em todo controle interativo.
- Em framework (Angular, React), siga a estrutura do projeto: componentes, módulos e estilos escopados. Não use seletores globais para resolver um ajuste local.

## Texto da interface

- Português do Brasil, direto, na voz de quem usa a página. Frases curtas e específicas.
- Botões dizem o que acontece ("Ver projetos", "Copiar e-mail").
- Evite frases de efeito, travessões como recurso retórico, "não é X, é Y" e termos de marketing vazios.
- O título do documento é um nome curto e específico, não uma legenda.

## Checklist antes de entregar

- Segue o sistema de design do projeto, se existir.
- Tokens definidos em um só lugar, sem cor ou fonte solta no CSS.
- Funciona em 400px, sem rolagem horizontal.
- Tema claro e escuro legíveis, com contraste suficiente.
- Todo conteúdo visível ao carregar; animação respeita `prefers-reduced-motion`.
- Foco visível e navegação por teclado nos controles.
- Nada inventado: lacunas marcadas com `[PENDENTE: ...]`.
- Arquivo completo, sem trechos omitidos.
