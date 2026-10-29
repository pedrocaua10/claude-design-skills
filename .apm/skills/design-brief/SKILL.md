---
name: "design-brief"
description: "Transforma o assunto de um site em direção de arte e gera o DESIGN.md do projeto: paleta, tipografia, espaçamento, movimento, componentes e tom de voz. Use antes de construir uma página, ao definir identidade visual ou ao pedir um DESIGN.md."
---

# Design Brief e DESIGN.md

Define a identidade visual de um site a partir do assunto e a registra em um `DESIGN.md` que as outras skills de design leem. Entregue o arquivo pronto, sem explicar o que é direção de arte. Quem constrói a página é `design-frontend`.

## Modos de entrada

**Modo Rascunho**: o usuário trouxe referências, cores, marca ou um texto solto. Reescreva no template, preservando o que foi dito, e marque lacunas com `[PENDENTE: ...]`.

**Modo Entrevista**: o usuário tem só uma ideia. Perguntas em **uma única mensagem**, numeradas e curtas:
1. Qual é o assunto e quem vai ver a página?
2. Que sensação a página deve causar (3 palavras)?
3. Há cores, fontes, logotipo ou marca já definidos?
4. Que referências o usuário admira, e o que exatamente nelas (cor, tipografia, movimento, layout)?
5. O que a página deve evitar?

Se o usuário responder só parte, siga com o que tem. Não faça segunda rodada, a menos que falte algo que impeça escrever.

Nunca invente marca, logotipo ou fato sobre o usuário.

## Fluxo de entrega

1. **Tese visual.** Uma frase: o que este site é, para quem, e o que o diferencia.
2. **Detalhe do assunto.** Um elemento que só este tema teria (unidade, termo, convenção, material).
3. **Paleta** com 4 a 6 tokens, cada um com papel e contraste verificado.
4. **Tipografia** com 2 a 3 papéis e uma escala.
5. **Movimento, componentes e voz.**
6. **Revisão contra o padrão genérico** (abaixo). Ajuste o que parecer padrão de qualquer site parecido.
7. **Entrega** do `DESIGN.md` completo.

## Template: DESIGN.md

```
# DESIGN.md
## Tese visual
<uma frase>
## Detalhe do assunto
<elemento próprio do tema e onde aparece>

## Cores
| Token | Valor claro | Valor escuro | Uso |
| --bg | ... | ... | fundo da página |
| --fg | ... | ... | texto principal |
| --muted | ... | ... | texto de apoio |
| --line | ... | ... | bordas e divisórias |
| --accent | ... | ... | ação principal e destaques |
Contrastes: texto normal 4,5:1, texto grande e componentes 3:1.

## Tipografia
| Papel | Fonte | Uso | Escala |
| Título | ... | h1 a h3 | ... |
| Texto | ... | parágrafos | ... |
| Dados | ... | rótulos e números | ... |
Fallbacks reais para cada fonte.

## Espaçamento e forma
Escala de espaçamento, gutter lateral, largura máxima de leitura, raios, sombras.

## Movimento
Durações, curvas, o movimento principal da página e o que não anima.

## Componentes
Botão, cartão, selo, formulário, navegação: aparência e estados.

## Voz
Tom, tratamento, palavras que usa e que evita.

## Evitar
<padrões genéricos e coisas específicas que o usuário rejeita>
```

## Como escolher

- **Cor:** parta do assunto (materiais, lugares, instrumentos), não de uma paleta da moda. Neutros com leve inclinação de matiz para o acento. Acento escolhido para funcionar nos dois temas.
- **Tipografia:** um título com caráter, um texto legível e, se precisar, uma fonte para dados. Use Google Fonts quando a restrição de hospedagem pedir, com fallback.
- **Ousadia em um lugar só.** O resto fica quieto.
- **Tema escuro** reajustado, não invertido.

## Padrão genérico a evitar

Fundo creme com serifada e terracota, preto com um único acento neon, degradê roxo para azul sobre branco, Inter ou Space Grotesk por reflexo, emoji como marcador, tudo centralizado, mesmo raio e sombra em todos os cartões. Se a proposta lembra algum, revise.

## Checklist antes de entregar

- Tese visual de uma frase e detalhe próprio do assunto.
- Todos os tokens com valor e uso, nos dois temas.
- Contrastes verificados.
- Fontes com fallback.
- Nada inventado: lacunas marcadas com `[PENDENTE: ...]`.
