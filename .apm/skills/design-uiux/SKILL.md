---
name: "design-uiux"
description: "Planeja e audita experiência e interface: fluxo do usuário, estrutura de página, hierarquia, estados, microcopy e acessibilidade (WCAG 2.2 AA). Use ao pedir revisão de UX/UI, crítica de uma tela, wireframe em texto, fluxo ou checagem de acessibilidade."
---

# Design UI/UX

Planeja a experiência antes do código e audita telas e páginas depois dele. Entregue o resultado pronto (wireframe em texto ou relatório de achados), sem explicar o conceito de UX. Para construir a interface, use a skill `design-frontend`.

## Modos de entrada

Escolha pelo que o usuário forneceu:

**Modo Planejar**: o usuário tem uma ideia de página, tela ou fluxo e nada construído.
1. Identifique público, objetivo único e a ação principal.
2. Entregue o fluxo e o wireframe em texto (template abaixo).
3. Marque lacunas com `[PENDENTE: ...]`. Entregue primeiro, pergunte depois, só o que bloqueia.

**Modo Auditar**: o usuário enviou código, arquivo, print ou link de uma tela.
1. Leia tudo antes de apontar problemas. Para código, cite `arquivo:linha`.
2. Entregue o relatório no formato abaixo, ordenado por severidade.
3. Se o usuário pedir correção, aplique só os achados aprovados, sem mexer no resto.

Se o pedido for ambíguo entre planejar e auditar, escolha o mais provável e diga qual escolheu em uma linha. Se houver perguntas, faça-as em uma única mensagem, numeradas e curtas.

Nunca invente dado de usuário, métrica ou resultado de pesquisa. Hipótese é hipótese e deve estar rotulada.

## Template: Wireframe em texto

```
Página / Tela
<nome>
Público
<quem usa e em que situação>
Objetivo único
<a ação que a página precisa provocar>

Estrutura (de cima para baixo)
1. <Bloco>: <o que mostra> | <prioridade: alta, média, baixa>
2. <Bloco>: ...

Ação principal
<rótulo exato do botão e onde aparece>

Estados
 - Vazio: <o que aparece>
 - Carregando: <o que aparece>
 - Erro: <mensagem e saída>
 - Sucesso: <confirmação>

Responsivo
 - Em telas estreitas: <o que empilha, o que muda de ordem, o que some>
```

Regras de montagem:
- A ordem dos blocos segue a pergunta que o visitante faz em cada momento: quem é, o que faz, que prova existe, como falar.
- Uma ação principal por tela. Ações secundárias parecem secundárias.
- Numeração só aqui, porque a ordem dos blocos é informação.
- Para landing page: promessa clara acima da dobra, prova logo depois da promessa (projetos, resultados, certificados reais), um único caminho de contato.

## Template: Relatório de auditoria

```
Resumo
<2 a 3 frases: o estado geral e os 3 achados que mais pesam.>

Achados
| Severidade | Onde | Problema | Correção |
| Crítico    | arquivo:linha ou tela | <o que acontece com o usuário> | <ajuste concreto> |
| ...        | ...   | ...      | ...      |

O que já está bom
 - <até 3 pontos que devem ser mantidos>

Não verificado
 - <o que não deu para checar sem rodar a página, ex.: leitor de tela, contraste de imagens>
```

Severidade:
- **Crítico**: impede uma tarefa ou exclui um grupo de usuários (sem acesso por teclado, texto ilegível, formulário que não envia).
- **Alto**: atrapalha muito a tarefa principal (CTA escondido, mensagem de erro sem saída, contraste abaixo do mínimo).
- **Médio**: gera atrito ou dúvida (hierarquia confusa, rótulo ambíguo, estado faltando).
- **Baixo**: acabamento e consistência.

Cada achado descreve o efeito no usuário, não só a regra violada. Correção sempre concreta ("trocar o cinza `#9AA` por `#5A6A80` no texto de apoio"), nunca "melhorar o contraste".

## O que verificar

**Hierarquia e foco**
- Há um ponto de entrada visual claro e uma ação principal evidente.
- Títulos seguem ordem lógica (um `h1`, depois `h2`, `h3`) e descrevem o conteúdo.
- Tamanho, peso e espaço indicam importância, não só decoração.

**Acessibilidade (WCAG 2.2, nível AA)**
- Contraste: texto normal 4,5:1, texto grande e componentes de interface 3:1.
- Teclado: todo controle alcançável e operável, ordem de foco lógica, foco sempre visível.
- Alvo de toque com pelo menos 24 por 24 px; para toque, prefira 44 px.
- A informação não depende só de cor (use também ícone, texto ou forma).
- Campos de formulário com rótulo associado, erro descrito em texto e saída clara.
- Imagens com `alt` útil; decorativas com `alt` vazio.
- Links descrevem o destino ("Ver certificados"), não "clique aqui".
- Animação respeita `prefers-reduced-motion` e nada pisca de forma agressiva.
- Idioma da página declarado; zoom de 200% sem perda de conteúdo.

**Estados e feedback**
- Vazio, carregando, erro, sucesso e desabilitado existem e dizem o que fazer a seguir.
- Toda ação responde (confirmação, mudança visível ou mensagem).
- Ações destrutivas pedem confirmação dentro da própria página.

**Formulários e conteúdo**
- Só os campos necessários. Mensagem de erro explica o que houve e como corrigir, sem culpar.
- Texto escrito na voz do usuário, em português claro, com botões que dizem o que acontece.
- Linhas de leitura de cerca de 65 caracteres; parágrafos curtos.

**Responsivo e desempenho percebido**
- Funciona em 400px sem rolagem horizontal. Nada fixo cobre o conteúdo.
- O conteúdo principal aparece sem esperar animação ou script.
- Imagens e fontes não travam a primeira leitura.

**Consistência**
- Mesmo componente, mesmo comportamento e mesma aparência em toda a página.
- Rótulos, nomes e termos iguais do começo ao fim.

## Checklist antes de entregar

- Modo certo (planejar ou auditar) e template correspondente.
- Cada achado tem severidade, local, efeito no usuário e correção concreta.
- Nenhuma regra citada sem aplicar ao caso real.
- O que não foi possível verificar está listado em Não verificado.
- Nada inventado: lacunas marcadas com `[PENDENTE: ...]`.
