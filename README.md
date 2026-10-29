# claude-design-skills

Skills pessoais de design para o Claude Code, distribuídas como pacote APM (Agent Package Manager). Escritas no mesmo padrão das skills do trabalho: português do Brasil, entrega pronta, modos de entrada, templates e checklist final.

## Skills

| Skill | Para que serve |
|---|---|
| `design-frontend` | Cria ou refatora interfaces (landing pages, telas, componentes) com direção visual própria, tokens, temas claro e escuro, scroll interativo e código completo. |
| `design-uiux` | Planeja a experiência (fluxo, wireframe em texto) e audita telas com relatório por severidade, incluindo acessibilidade WCAG 2.2 AA. |

As duas conversam entre si: `design-frontend` constrói e `design-uiux` revisa o resultado.

## Estrutura

```
claude-design-skills/
├── apm.yml
├── README.md
└── .apm/
    └── skills/
        ├── design-frontend/SKILL.md
        └── design-uiux/SKILL.md
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
    - pedrocaua10/claude-design-skills#v1.0.0
```

Se o repositório for privado, o APM precisa de acesso ao GitHub da mesma forma que no repositório de skills do trabalho.

## Uso

Peça direto ("cria a landing page do portfólio", "audita essa tela") ou chame pelo nome: `/design-frontend`, `/design-uiux`.

## Atualização

Depois de alterar uma skill: commit, tag nova (`v1.0.1`, `v1.1.0`) e `apm install` no projeto consumidor.

## Segurança

As skills são só texto: não têm scripts, não buscam conteúdo em URL externa e não declaram `allowed-tools`. Leia o `SKILL.md` antes de instalar qualquer skill de terceiros.

## Referências

As ideias vêm de práticas públicas de design e acessibilidade (WCAG 2.2, heurísticas de usabilidade) e de skills abertas da comunidade, como o `frontend-design` da Anthropic, as diretrizes de design web da Vercel e o AccessLint. O texto aqui foi escrito do zero para o meu fluxo.
