---
name: "design-scroll"
description: "Projeta e constrói páginas com narrativa pelo scroll: cenas fixas, troca de seções, parallax, elementos que reagem à rolagem, com GSAP ScrollTrigger, CSS scroll-driven ou IntersectionObserver. Use ao pedir landing page interativa, scrollytelling ou efeito ao rolar."
---

# Design Scroll

Transforma uma página em uma sequência de cenas que respondem à rolagem, sem esconder conteúdo nem quebrar a leitura. Entregue o código completo e o roteiro das cenas, sem explicar o que é scrollytelling. Para a base visual, siga `design-frontend`; para o objeto 3D, `design-3d`; para microinterações, `design-motion`.

## Ordem de precedência

1. O que o usuário pediu, com as palavras dele.
2. O sistema de design do projeto (`DESIGN.md`, tokens, stack existente).
3. As escolhas desta skill.

Em projeto existente, mantenha a stack. Não migre para outro framework sem pedido.

## Modos de entrada

**Modo Pedido direto**: o usuário descreveu a página e o efeito. Monte o roteiro de cenas, construa e entregue. Pergunte depois, só o que bloqueia.

**Modo Briefing**: o pedido é vago ("faz uma landing page animada"). Faça as perguntas em **uma única mensagem**, numeradas e curtas:
1. Qual é a história da página, do primeiro ao último bloco?
2. Qual é a ação final (contato, compra, cadastro)?
3. Existe um objeto ou imagem central que deve reagir ao scroll?
4. Qual é a stack (arquivo único, Next.js, Angular, outro) e onde o código vai morar?
5. Quais conteúdos reais existem (textos, imagens, números)?

Nunca invente conteúdo factual. Marque lacunas com `[PENDENTE: ...]`.

## Fluxo de entrega

1. **Roteiro de cenas** (template abaixo). Cada cena tem um papel na história e um comportamento ao rolar.
2. **Escolha da técnica** pela tabela. Use a mais simples que resolve.
3. **Construção.** Estrutura e conteúdo legíveis sem JavaScript primeiro; efeito depois.
4. **Mobile e teclado.** Teste mentalmente em 400px, toque e navegação por teclado.
5. **Fallback.** Sem JS, sem WebGL ou com `prefers-reduced-motion`, a página é uma sequência normal de seções.
6. **Checklist e entrega.** Arquivo completo, seguido de uma linha com suposições e `[PENDENTE]`.

## Template: Roteiro de cenas

```
Página
<nome>
História
<uma frase: do problema do visitante à ação final>

Cenas
| Cena | Papel na história | Conteúdo | Ao rolar | Técnica |
| 1 Abertura | Promessa | Título, subtítulo, CTA | Objeto entra, título sobe | Carga + ScrollTrigger |
| 2 ...      | ...       | ...      | ...      | ...     |

Ação final
<rótulo exato do botão>

Duração do scroll
<altura total aproximada, em telas>
```

Regras de montagem:
- Uma ideia por cena. Se a cena precisa de duas, divida.
- Ordem narrativa: promessa, prova, como funciona, detalhe, ação.
- Numeração só aqui, porque a ordem das cenas é informação.

## Qual técnica usar

| Necessidade | Técnica |
|---|---|
| Revelar blocos ao entrar na tela | CSS `animation-timeline: view()` com fallback, ou `IntersectionObserver` |
| Barra de progresso, seção ativa | `IntersectionObserver` e `requestAnimationFrame` |
| Cena fixa com várias etapas (pin) | GSAP ScrollTrigger com `pin: true` e `scrub` |
| Objeto 3D ligado ao scroll | ScrollTrigger ou progresso normalizado alimentando o Three.js (ver `design-3d`) |
| Troca de tema ou cor por seção | `IntersectionObserver` alterando um atributo em `body` |
| Rolagem horizontal dentro de seção vertical | ScrollTrigger com `pin` e `x` em `scrub` |
| Rolagem suave | Só se o projeto pedir, com Lenis, nunca sequestrando teclado e âncoras |

## Técnicas

**Cena fixa (pin) com etapas**
```js
gsap.registerPlugin(ScrollTrigger);
const mm = gsap.matchMedia();
mm.add("(prefers-reduced-motion: no-preference)", () => {
  const tl = gsap.timeline({
    scrollTrigger: { trigger: ".scene", start: "top top", end: "+=300%", pin: true, scrub: 1 }
  });
  tl.from(".scene .title", { yPercent: 30 })
    .to(".scene .object", { rotation: 360, scale: 1.15 }, "<")
    .to(".scene .title", { yPercent: -30 });
});
```
O `end: "+=300%"` define quanta rolagem a cena consome. Ajuste conforme o número de etapas.

**Revelação nativa, com recuo para navegadores sem suporte**
```css
@keyframes rise { from { transform: translateY(40px); } to { transform: none; } }
@supports (animation-timeline: view()) {
  .reveal { animation: rise linear both; animation-timeline: view(); animation-range: entry 0% cover 30%; }
}
```
O estado inicial fica legível; só o deslocamento anima. Confirme o suporte atual dos navegadores antes de depender disso.

**Seção ativa e progresso**
```js
const io = new IntersectionObserver(entries => {
  entries.forEach(e => e.isIntersecting && document.body.setAttribute("data-scene", e.target.id));
}, { rootMargin: "-45% 0px -50% 0px" });
document.querySelectorAll("[data-scene-id]").forEach(s => io.observe(s));
```

## Regras

- **Conteúdo sempre legível.** Nada fica em `opacity: 0` esperando o scroll. Anime deslocamento, escala e cor a partir de um estado visível.
- **Sem sequestro de scroll.** O usuário mantém roda, toque, teclado (setas, espaço, Page Down) e âncoras. Não bloqueie `overflow` nem troque a velocidade nativa sem pedido.
- **Altura sob controle.** Cada cena fixa consome rolagem. Some as alturas e evite páginas com dezenas de telas de scroll.
- **Desempenho.** Anime `transform` e `opacity`. Evite animar `width`, `height`, `top` e `box-shadow`. Use `will-change` só no que anima. Ouvintes de scroll com `{ passive: true }` e `requestAnimationFrame`.
- **Recalcular.** Chame `ScrollTrigger.refresh()` depois de fontes e imagens carregarem e após mudar o layout.
- **Responsivo.** Use `gsap.matchMedia()` para versões diferentes em desktop e celular. Em telas estreitas, simplifique: menos pins, menos parallax.
- **Acessibilidade.** A ordem do DOM é a ordem de leitura. Efeitos ligados ao scroll não são o único jeito de acessar conteúdo. Respeite `prefers-reduced-motion`.
- **Âncoras.** Links internos funcionam e levam à cena certa, com `scroll-margin-top` se houver barra fixa.
- **Bibliotecas.** Fixe a versão. GSAP e ScrollTrigger carregam de CDN em arquivo único ou entram pelo gerenciador de pacotes em projetos com build.

## Stacks

| Contexto | Stack sugerida |
|---|---|
| Arquivo único, portfólio | HTML, CSS, JS puro, GSAP e ScrollTrigger por CDN, Three.js opcional |
| Projeto React ou Next.js | Tailwind, GSAP ScrollTrigger, Framer Motion (Motion), React Three Fiber para 3D |
| Projeto Angular | Serviços para registrar ScrollTrigger, limpeza no `ngOnDestroy` |

Em React e Angular, destrua os ScrollTriggers ao desmontar o componente para evitar vazamento.

## Checklist antes de entregar

- Roteiro de cenas definido e coerente com a história.
- Sem JavaScript, a página é legível em ordem.
- Funciona em 400px e por teclado.
- `prefers-reduced-motion` desliga os efeitos e mostra o resultado final.
- Nada em `opacity: 0` aguardando o scroll.
- `ScrollTrigger.refresh()` após carregar fontes e imagens.
- Sem scroll sequestrado, âncoras funcionando.
- Nada inventado: lacunas marcadas com `[PENDENTE: ...]`.
