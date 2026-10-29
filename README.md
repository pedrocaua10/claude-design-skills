# claude-design-skills

Skills pessoais de design e criação de sites para o Claude Code, distribuídas como pacote APM (Agent Package Manager). Escritas no mesmo padrão das skills do trabalho: português do Brasil, entrega pronta, modos de entrada, templates e checklist final.

## Skills

| Skill | Para que serve |
|---|---|
| `design-brief` | Define a direção de arte e gera o `DESIGN.md` do projeto (paleta, tipografia, movimento, voz). Use antes de construir. |
| `design-frontend` | Cria ou refatora interfaces com direção visual própria, tokens, temas claro e escuro e código completo. |
| `design-scroll` | Páginas com narrativa pelo scroll: cenas fixas, parallax, GSAP ScrollTrigger, CSS scroll-driven, com fallback. |
| `design-3d` | Elementos 3D com Three.js ou React Three Fiber: objeto central, luz, partículas, ligação ao scroll e fallback. |
| `design-motion` | Microinterações, sequências de entrada, tokens de movimento e redução de movimento. |
| `design-copy` | Textos da página em português: hero, projetos, experiência, chamada final e microcopy. |
| `design-uiux` | Planeja a experiência (fluxo, wireframe em texto) e audita telas com acessibilidade WCAG 2.2 AA. |
| `web-performance` | Auditoria de desempenho: Core Web Vitals, imagens, fontes, scroll e orçamento de 3D. |
| `web-seo-deploy` | Meta tags, Open Graph, dados estruturados, domínio e publicação (GitHub Pages, Vercel, Netlify, Cloudflare Pages). |
| `web-escrita-humana` | Revisa textos em português (currículo, portfólio, LinkedIn, e-mail) e tira as marcas de texto gerado por IA. |
| `agentes-orquestracao` | Desenha fluxos de agentes e skills: escolhe o padrão, define o contrato entre etapas e a aprovação humana. |

## Fluxo sugerido para uma landing page

1. `design-brief`: identidade visual e `DESIGN.md`.
2. `design-copy`: textos reais.
3. `design-uiux`: estrutura e roteiro da página.
4. `design-frontend`, com `design-scroll`, `design-3d` e `design-motion`: construção.
5. `design-uiux` e `web-performance`: revisão.
6. `web-escrita-humana`: revisão final dos textos.
7. `web-seo-deploy`: publicação.

Para montar agentes e automações, use `agentes-orquestracao`.

## Estrutura

```
claude-design-skills/
├── apm.yml
├── README.md
└── .apm/
    └── skills/
        ├── design-brief/SKILL.md
        ├── design-frontend/SKILL.md
        ├── design-scroll/SKILL.md
        ├── design-3d/SKILL.md
        ├── design-motion/SKILL.md
        ├── design-copy/SKILL.md
        ├── design-uiux/SKILL.md
        ├── web-performance/SKILL.md
        ├── web-seo-deploy/SKILL.md
        ├── web-escrita-humana/SKILL.md
        └── agentes-orquestracao/SKILL.md
```

## Instalação com APM

No projeto onde as skills vão ser usadas:

```bash
apm install pedrocaua10/claude-design-skills --target claude
```

O APM registra a dependência no `apm.yml` do projeto e instala as skills na pasta `.claude/`. Para fixar uma versão, use uma tag:

```yaml
dependencies:
  apm:
    - pedrocaua10/claude-design-skills#v1.4.0
```

Se o repositório for privado, o APM precisa de acesso ao GitHub da mesma forma que no repositório de skills do trabalho.

## Skills de terceiros (instalação opcional)

Não ficam copiadas aqui. Instale no projeto que precisar, depois de ler o `SKILL.md` de cada uma.

| Repositório | Instalação | Quando usar |
|---|---|---|
| `isdiegoalves/marketingskills` | `apm install isdiegoalves/marketingskills/skills/<nome> --target claude` | Muvve e o site: `paid-ads`, `ad-creative`, `analytics-tracking`, `ab-test-setup`, `copywriting`, `copy-editing`, `page-cro`, `seo-audit`, `schema-markup`, `ai-seo`, `site-architecture`. Elas leem `.agents/product-marketing-context.md`. |
| `browser-use/video-use` | `apm install browser-use/video-use --target claude` | Vídeo de apresentação do Nodify. Roda scripts, pede `ELEVENLABS_API_KEY` e `ffmpeg`: leia antes. |
| `isdiegoalves/avoid-ai-writing` | referência da `web-escrita-humana` | Versão em inglês, com modos e perfis de contexto. |

Avaliados e não adicionados: `StarTrail-org/PixelRAG` (precisa da CLI `pixelshot` via pip), `anthropics/claude-quickstarts` e o fork `isdiegoalves/claude-quickstarts` (aplicações de exemplo, não skills), `anthropics/claude-cookbooks` (os padrões de agentes entraram reescritos em `agentes-orquestracao`) e `poloclub/transformer-explainer` (aplicação Svelte, serve só de inspiração visual).

## Uso

Peça direto ("cria a landing page do portfólio com scroll interativo", "audita essa tela") ou chame pelo nome: `/design-scroll`, `/design-brief` e assim por diante.

## Atualização

Depois de alterar uma skill: commit, tag nova (`v1.1.1`, `v1.2.0`) e `apm install` no projeto consumidor.

## Segurança

As skills são só texto: não têm scripts, não buscam conteúdo em URL externa e não declaram `allowed-tools`. Leia o `SKILL.md` antes de instalar qualquer skill de terceiros.

## Referências

`web-escrita-humana` parte do `avoid-ai-writing` de Conor Bronsdon (MIT) e `agentes-orquestracao`, do material da Anthropic sobre agentes e do `claude-cookbooks` (MIT). As ideias vêm de práticas públicas de design, acessibilidade (WCAG 2.2) e desempenho (Core Web Vitals), da documentação do GSAP e do Three.js e de skills abertas da comunidade, como o `frontend-design` da Anthropic, as diretrizes de design web da Vercel e o AccessLint. O texto aqui foi escrito do zero para o meu fluxo.
