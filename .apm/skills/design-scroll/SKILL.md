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
| Objeto de produto que atravessa as seções | Camada fixa com `pointer-events: none`, pose por seção via ScrollTrigger |
| Texto grande que se revela ao rolar | SplitType por palavra e ScrollTrigger com `scrub` |
| Rolagem suave | Só se o projeto pedir, com Lenis integrado ao ScrollTrigger, nunca sequestrando teclado e âncoras |

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

**Objeto persistente entre cenas**

Um elemento (frasco, dispositivo, logotipo) fica em uma camada fixa acima das seções e muda de posição, rotação e escala conforme a seção ativa. Cada seção tem fundo e tipografia próprios, e o objeto viaja entre elas.
```js
const layer = document.querySelector(".object-layer"); // position: fixed; pointer-events: none
const poses = {
  hero:    { x: "0vw",   rotation: 0,   scale: 1 },
  detalhe: { x: "-18vw", rotation: -24, scale: 0.9 },
  final:   { x: "18vw",  rotation: 12,  scale: 1.05 }
};
Object.entries(poses).forEach(([id, pose]) => {
  ScrollTrigger.create({
    trigger: `#${id}`, start: "top 60%", end: "bottom 40%",
    onToggle: self => self.isActive && gsap.to(layer, { ...pose, duration: 1, ease: "power3.out", overwrite: true })
  });
});
```
O texto das seções continua legível por baixo do objeto. Em telas estreitas, reduza o objeto ou fixe-o em uma posição que não cubra o conteúdo.

**Texto grande que se revela pelo scroll**

Divida o título em palavras e leve cada uma de um contraste reduzido ao pleno, em uma faixa curta de rolagem (cerca de 30 a 40% da altura da tela).
```js
const split = new SplitType(".reveal-text", { types: "words" });
gsap.fromTo(split.words, { opacity: 0.35 }, {
  opacity: 1, ease: "power1.out", stagger: 0.1,
  scrollTrigger: { trigger: ".reveal-text", start: "top 90%", end: "top 50%", scrub: true }
});
```
Faixa de referência: do topo do elemento a 90% da tela até a 50% (cerca de 40% da altura da tela, sem `pin`). Um trigger por título. Comece o contraste em 0,2 a 0,35, conforme o fundo. O texto já aparece legível, só mais fraco. Mantenha o `aria-label` no contêiner, porque a divisão em `span` pode atrapalhar leitores de tela.

**Rolagem suave com Lenis integrada ao ScrollTrigger**
```js
const lenis = new Lenis();
lenis.on("scroll", ScrollTrigger.update);
gsap.ticker.add(time => lenis.raf(time * 1000));
gsap.ticker.lagSmoothing(0);
```
Desligue com `prefers-reduced-motion`, preserve as âncoras (`lenis.scrollTo`) e o uso do teclado.

**Fundos chapados por seção**

Cada seção tem uma cor de fundo forte e própria (token por seção), e a troca acontece pela posição de rolagem, sem fade longo. Textura de grão ou ilustração de baixo contraste dá profundidade sem competir com o texto.

## Padrões de referência

Observados em um site de produto com scroll narrativo (análise de rede e estrutura), descritos só no nível da técnica:
- Fundo texturizado com grão e ilustração de baixo contraste.
- Título gigante ocupando a largura da tela, parcialmente coberto pelo objeto do produto.
- Objeto do produto em uma camada `position: fixed` dentro de uma abertura longa (cerca de 8 alturas de tela). Sem `pin`: o hero é só uma seção alta com a camada fixa dentro.
- As "poses" não são modelos diferentes. São faixas de quadros da mesma sequência pré-renderizada (em pé, tampa para a câmera, tombando), mais escala, rotação e deslocamento em keyframes ligados ao progresso da seção (ver `design-3d`, seção de sequência de quadros).
- Saída do objeto: ao entrar a seção seguinte, ele reduz até escala 0 e opacidade 0 com leve rotação, e as seções seguintes têm `z-index` maior e cobrem a camada.
- Mobile usa outra lista de keyframes, não a mesma encolhida.
- Seção dividida ao meio: texto em um lado, foto em cor chapada no outro.
- Navegação em pílulas fixas e ações (carrinho) sempre visíveis. O cabeçalho troca para modo de contraste (`mix-blend-mode: difference` ou classe) quando certas seções cruzam o meio da tela.
- Linhas pontilhadas como divisórias.
- Página de cerca de 18 alturas de tela, das quais 8 são só o hero. As demais seções têm de 1 a 2,8 alturas.
- Troca de cor entre seções por corte seco na borda, sem transição. Cada seção tem cor chapada ou imagem própria, e o movimento vem de parallax, rotação e escala dos cartões e imagens.
- Revelação de títulos: um `ScrollTrigger` com `scrub: true` por bloco, faixa curta de rolagem (do topo do elemento a 90% da tela até a 50%), palavras de opacidade 0,2 a 1 com `power1.out` e `stagger`.
- Pilha de bibliotecas: GSAP com ScrollTrigger, Lenis e SplitType, mais Lottie (canvas) para a sequência. O Lenis estava com duração de 1,2 s e `smoothTouch` desligado, aparentemente sem ligação ao ScrollTrigger. Ligue os dois (ver acima) para evitar descompasso. Use versões atuais e fixas, não as antigas.

Observados em um portfólio de desenvolvedor com GSAP e ScrollTrigger (sem Lenis), só no nível da técnica:
- Blocos de cor chapada por seção (vermelho, preto, creme), com corte seco entre eles e poucas cores no total.
- Nome em fonte condensada gigante no início, com dois quadrados sobrepostos com as iniciais e um círculo de destaque.
- Faixa de rolagem contínua com as tecnologias, com a lista duplicada para o laço.
- Projetos em cartões que correm na horizontal enquanto o scroll vertical avança, com a dica "continue rolando para ver todos os projetos".
- Painel de abas por área (full stack, frontend, backend, dados, deploy, UX), trocando o conteúdo no lugar.
- Navegação flutuante com links de seção e botões redondos de redes sociais.
- Chamada final em letras gigantes com um único botão.
- Página curta, de cerca de 4 alturas de tela, com poucos efeitos bem escolhidos.

Parâmetros medidos nesse portfólio, para adaptar com identidade própria:
- Trilho horizontal: trigger no contêiner de 100vh, `start: "top top"`, `pin: true`, `scrub: true`, `ease: "none"`, `invalidateOnRefresh: true`. A distância é `trilho.scrollWidth - window.innerWidth + 80`, usada em funções para `end` e `x`, o que refaz o cálculo depois de `ScrollTrigger.refresh()`. Cartão com `clamp(300px, 62vw, 520px)` e espaço de 34px.
- Faixa de tecnologias só em CSS: `width: max-content`, `translateX(0)` a `translateX(-50%)` em 26s linear, conjunto duplicado no HTML (cerca de 56 px/s), levemente inclinada e sobrepondo a base do hero.
- Nome por letra: cada letra em um `span` dentro de linha com `overflow: hidden`, estado inicial `translateY(115%) rotate(6deg)`, 0,7s com `cubic-bezier(.2,1,.3,1)` e atraso de 0,045s por letra e 0,35s por linha.
- Objetos decorativos flutuando em CSS (0 a -14px, 5s e 6,5s em sentidos opostos), sem parallax.
- Abas: troca de classe com animação de 0,45s (opacidade e 10px de deslocamento).

Falhas a evitar, vistas nesse mesmo portfólio:
- Com `prefers-reduced-motion` no desktop, o pin some e o trilho vira `overflow: hidden`, deixando cartões inalcançáveis. O fallback de rolagem nativa com `scroll-snap` precisa valer também para movimento reduzido, não só para celular.
- Cabeçalho transparente sem troca de cor por seção fica ilegível sobre fundo claro.
- Abas sem `role="tab"`, `aria-selected` e navegação por setas.
- Imagens de 0,5 a 1 MB em PNG e JPEG, sem `width`, `height` nem `loading="lazy"`, somando 97% do peso da página.
- Menu móvel que não fecha por clique fora, Esc nem usa `aria-expanded`.

Use isso como repertório. Não copie código, texto, imagens, fontes nem identidade visual de outro site.

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
