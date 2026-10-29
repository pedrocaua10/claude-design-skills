---
name: "web-performance"
description: "Audita e melhora o desempenho de sites e landing pages animadas: Core Web Vitals, imagens, fontes, JavaScript, scroll sem travar e orçamento de 3D. Use ao pedir otimização, página lenta, nota do Lighthouse ou antes de publicar."
---

# Web Performance

Mede, aponta e corrige o que deixa uma página lenta ou travada, com atenção especial a animações, scroll e 3D. Entregue o relatório e as correções aplicadas, sem explicar o que é desempenho web. Para acessibilidade e usabilidade, use `design-uiux`.

## Modos de entrada

**Modo Auditar**: o usuário enviou código, arquivo, link ou métricas. Leia tudo, aponte problemas por severidade com `arquivo:linha` e proponha a correção.

**Modo Corrigir**: o usuário aprovou achados. Aplique só o aprovado, sem mexer no resto.

Se o pedido for ambíguo, escolha o mais provável e diga qual em uma linha. Se faltar medição, não invente número: marque `[PENDENTE: medir ...]` e diga como medir.

## Metas (Core Web Vitals, nível "bom")

| Métrica | Meta |
|---|---|
| LCP (maior conteúdo visível) | até 2,5 s |
| INP (resposta a interações) | até 200 ms |
| CLS (deslocamento de layout) | até 0,1 |

Meça em celular simulado com rede lenta, e confirme com ferramenta real (Lighthouse, PageSpeed Insights, aba Performance do DevTools).

## Template: Relatório

```
Resumo
<estado geral e os 3 achados que mais pesam>

Achados
| Severidade | Onde | Problema | Correção |
| Alto | arquivo:linha | <efeito no usuário> | <ajuste concreto> |

Medições
<valores medidos e a ferramenta; sem medição, [PENDENTE: medir ...]>

Não verificado
<o que depende de rodar a página em celular real>
```

Severidade: **Crítico** (página inutilizável ou travada), **Alto** (afeta LCP, INP ou CLS de forma clara), **Médio** (peso evitável), **Baixo** (acabamento).

## O que verificar

**Imagens e mídia**
- Formatos modernos (AVIF ou WebP), tamanho adequado ao uso, `width` e `height` declarados.
- Imagem principal com `fetchpriority="high"` e sem `loading="lazy"`; as demais com `loading="lazy"`.
- Vídeo curto, comprimido, sem autoplay com som, com poster.

**Fontes**
- Poucas famílias e pesos, `font-display: swap`, subconjunto de caracteres quando possível, pré-carregamento só da fonte crítica.

**JavaScript e CSS**
- `defer` em scripts, divisão de código em projetos com build, nada de biblioteca grande para um efeito pequeno.
- CSS crítico enxuto, sem regras mortas.
- Terceiros (analytics, widgets) carregados depois e em número mínimo.

**Scroll e animação**
- Ouvintes de scroll com `{ passive: true }` e `requestAnimationFrame`.
- Animação só de `transform` e `opacity`. Sem leitura e escrita de layout alternadas.
- Efeitos ligados ao scroll desligados ou simplificados em celular.
- `ScrollTrigger.refresh()` chamado uma vez após o carregamento, não a cada quadro.

**3D e canvas**
- Densidade de pixels limitada a 2, render pausado fora da tela, recursos liberados.
- Modelos comprimidos, texturas no tamanho certo, meta de poucos MB.
- Fallback estático para quem não tem WebGL.

**Layout estável**
- Espaço reservado para imagens, fontes e embeds, sem salto ao carregar.

## Checklist antes de entregar

- Cada achado tem severidade, local, efeito e correção concreta.
- Métricas vêm de medição, nunca de estimativa.
- O que não foi medido está listado.
- Correções aplicadas não mudam o comportamento além do pedido.
