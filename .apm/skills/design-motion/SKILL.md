---
name: "design-motion"
description: "Define e implementa animações de interface: microinterações, sequência de entrada, estados de hover e foco, transições e tokens de movimento, com CSS, Framer Motion ou GSAP. Use ao pedir animação, microinteração, transição ou 'dar vida' a uma interface."
---

# Design Motion

Cria movimento que explica, orienta ou confirma, nunca só enfeita. Entregue o código completo e os tokens de movimento usados, sem explicar teoria de animação. Para animação ligada ao scroll, use `design-scroll`.

## Modos de entrada

**Modo Pedido direto**: o usuário indicou o elemento e o efeito. Implemente e entregue; pergunte depois.

**Modo Briefing**: o pedido é vago ("anima isso"). Perguntas em **uma única mensagem**, numeradas e curtas:
1. Qual elemento ou fluxo precisa de movimento?
2. O que o movimento deve comunicar (confirmação, hierarquia, progresso, mudança de estado)?
3. Qual é a stack (CSS puro, React com Motion, GSAP)?
4. Há tokens de movimento no projeto?

## Fluxo de entrega

1. **Propósito.** Escreva em uma frase o que o movimento comunica. Sem propósito, não anime.
2. **Tokens.** Defina durações e curvas em variáveis.
3. **Implementação** com a ferramenta mais simples que resolve.
4. **Estados.** Hover, foco, ativo, desabilitado e carregando.
5. **Redução de movimento.** Versão sem animação que mantém o significado.
6. **Checklist e entrega.**

## Tokens de movimento

```css
:root {
  --dur-fast: 120ms;     /* feedback: hover, pressionar */
  --dur-base: 240ms;     /* transições de estado */
  --dur-slow: 480ms;     /* entrada de blocos */
  --ease-out: cubic-bezier(.22, 1, .36, 1);   /* entrada */
  --ease-in: cubic-bezier(.64, 0, .78, 0);    /* saída */
  --ease-inout: cubic-bezier(.65, 0, .35, 1); /* deslocamento */
}
@media (prefers-reduced-motion: reduce) {
  :root { --dur-fast: 0ms; --dur-base: 0ms; --dur-slow: 0ms; }
}
```

## Regras

- Feedback de toque e hover em 100 a 200 ms. Transições de estado em 200 a 400 ms. Entradas de bloco até cerca de 600 ms.
- Entrada usa curva de saída suave (desacelera no fim). Saída é mais rápida que a entrada.
- Sequência (stagger) de 40 a 80 ms entre itens, com limite de itens animados.
- Anime `transform` e `opacity`. Para altura de acordeão, use `grid-template-rows` de `0fr` para `1fr`.
- O estado inicial é legível. Nada fica invisível esperando um gatilho.
- Um movimento principal por tela. Os demais são discretos.
- Movimento nunca atrasa uma ação: a interface responde antes da animação terminar.
- `prefers-reduced-motion`: troque deslocamento por mudança de cor ou opacidade, ou mostre o estado final direto.

## Catálogo de padrões

| Elemento | Padrão |
|---|---|
| Botão | Eleva 1 a 2 px no hover, afunda no `:active`, anel de foco visível |
| Link | Sublinhado que cresce da esquerda com `background-size` |
| Cartão | Leve elevação e mudança de borda, sem mexer no layout |
| Acordeão | `grid-template-rows` com transição, ícone gira |
| Alternador de tema | Transição de cor em 240 ms, sem piscar |
| Entrada da página | Título, subtítulo e CTA em sequência curta |
| Contador | Anima até o valor final e respeita `prefers-reduced-motion` |
| Carregamento | Esqueleto com pulso suave, não spinner infinito |
| Troca de página | Opacidade e leve deslocamento, mantendo o foco gerenciado |

## Escolha da ferramenta

| Necessidade | Ferramenta |
|---|---|
| Hover, foco, estados simples | CSS (`transition`) |
| Entrada simples e repetível | CSS `@keyframes` |
| Componentes React com entrada e saída | Motion (Framer Motion) |
| Linha do tempo com várias etapas | GSAP |

## Exemplo: botão com feedback

```css
.btn {
  transition: transform var(--dur-fast) var(--ease-out), background var(--dur-base) var(--ease-out);
}
.btn:hover { transform: translateY(-2px); }
.btn:active { transform: translateY(0) scale(.98); }
.btn:focus-visible { outline: 2px solid var(--accent); outline-offset: 3px; }
```

## Checklist antes de entregar

- Cada animação tem propósito declarado.
- Durações e curvas vêm de tokens.
- Só `transform` e `opacity` animam, salvo exceções justificadas.
- Estados de hover, foco, ativo e desabilitado existem.
- `prefers-reduced-motion` tem versão equivalente.
- Nada invisível por padrão e nada atrasa uma ação.
