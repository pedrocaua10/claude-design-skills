---
name: "web-escrita-humana"
description: "Audita e reescreve textos em português para remover marcas de texto gerado por IA: clichês, travessões, estrutura repetitiva, tom de vendedor e conclusões genéricas. Use em currículo, portfólio, LinkedIn, e-mail e landing page, ou ao pedir 'tirar cara de IA', 'humanizar' ou 'revisar o texto'."
---

# Escrita Humana

Revisa um texto para que ele soe como uma pessoa específica, com fatos, e não como um modelo de linguagem. Entregue o texto reescrito e a lista curta do que mudou, sem explicar teoria de escrita. Para escrever do zero, use `design-copy`; esta skill é a revisão final.

A ideia vem da skill aberta `avoid-ai-writing` (Conor Bronsdon, licença MIT), reescrita aqui em português e para os meus textos.

## Modos de entrada

**Modo Reescrever** (padrão): o usuário colou o texto. Corrija e entregue a versão final, seguida da lista do que mudou.

**Modo Detectar**: o usuário pediu "só apontar", "auditar" ou "o que parece IA". Liste os trechos e o motivo, sem reescrever.

Se faltar contexto, faça as perguntas em **uma única mensagem**, numeradas e curtas:
1. Onde o texto vai (currículo, LinkedIn, portfólio, e-mail, landing page)?
2. Quem lê (recrutador, cliente, colega)?
3. Há fatos, números ou nomes reais que devem entrar?

Nunca invente fato, número, cargo ou data para o texto parecer mais humano. Marque lacunas com `[PENDENTE: ...]`.

## O que cortar ou corrigir

### Formatação
- **Travessão (—)**: troque por vírgula, ponto ou parênteses. Meta: nenhum.
- **Negrito em excesso**: no máximo uma expressão em negrito por seção.
- **Emoji em título** e listas com emoji: remova. Em post de rede social, no máximo um ou dois, no fim da linha.
- **Listas demais**: converta em parágrafo o que não é lista de verdade. Currículo é exceção: marcadores curtos e factuais são o formato.

### Estrutura de frase
- **"Não é apenas X, é Y"** e **"mais do que X, é Y"**: reescreva como afirmação direta. No máximo uma vez no texto.
- **Trio por reflexo** ("rápido, simples e eficaz"): varie para dois itens, quatro ou uma frase.
- **Pergunta retórica de abertura** ("Já parou para pensar...?"): comece pelo fato.
- **Falsa concessão** ("Embora seja desafiador, também é recompensador"): diga o que é, com dado.
- **Ritmo uniforme**: frases do mesmo tamanho denunciam texto gerado. Misture curtas e longas.
- **Parágrafos que podiam trocar de lugar** sem ninguém notar: falta ligação entre as ideias.

### Palavras e expressões a substituir

| Troque | Por |
|---|---|
| mergulhar em, explorar a fundo | olhar, analisar, estudar |
| cenário (metafórico), panorama | área, mercado, situação (nomeie qual) |
| jornada | trajetória, caminho, experiência (ou cite o fato) |
| robusto, abrangente, holístico | sólido, completo (ou descreva o que ele cobre) |
| alavancar | usar |
| impulsionar, potencializar, otimizar (vago) | diga o que melhorou e quanto |
| inovador, revolucionário, de ponta | descreva o que há de novo |
| transformar, desbloquear o potencial | diga o que muda, em concreto |
| sinergia, ecossistema, protagonismo | cite quem fez o quê |
| apaixonado por, movido por desafios | mostre com um projeto ou resultado |
| é importante ressaltar que, vale destacar | corte e afirme o fato |
| no mundo atual, na era digital | corte |
| sem dúvida, certamente, naturalmente | corte |
| atuar de forma proativa e dinâmica | descreva a ação |
| cada vez mais, ao longo do tempo (sem dado) | cite o período ou o número |

Agrupamento conta: uma palavra da lista isolada pode ser aceitável; três no mesmo parágrafo é sinal de texto gerado.

### Estruturas típicas de IA
- **Abertura de assistente**: "Claro!", "Com certeza!", "Aqui está...". Remova.
- **Conclusão genérica**: "Em resumo, ...", "Portanto, conclui-se que...". Termine no último fato.
- **Elogio vazio**: "excelente pergunta", "ótima ideia".
- **Atribuição vaga**: "especialistas dizem", "estudos mostram". Cite a fonte ou corte.
- **Inflação de importância**: "marco fundamental", "papel crucial". Diga o efeito real.
- **Sinônimos em rodízio**: o mesmo objeto chamado de três nomes. Repita o termo certo.
- **Promessa de tom de vendedor**: "solução completa", "resultados incríveis". Troque por dado.

## Textos de currículo e portfólio

- Verbo de ação no começo e resultado no fim ("Reduzi o tempo de X em Y").
- Sem adjetivo sobre si mesmo (dedicado, proativo, apaixonado). A experiência prova.
- Termos da área em vez de frases amplas: "testes unitários com Jasmine e Karma" vence "garantia de qualidade".
- Primeira pessoa no portfólio, impessoal no currículo, e nunca misturar os dois no mesmo texto.
- Números só se forem reais. Sem número, descreva o escopo (dois sistemas, 6 meses, 8 cursos).

## Fluxo de entrega

1. Leia o texto inteiro e o contexto (onde vai e quem lê).
2. **Auditoria**: liste cada trecho com marca de IA, citando-o.
3. **Reescrita**: devolva a versão limpa, mantendo fatos, tom e extensão aproximada.
4. **O que mudou**: três a seis linhas, só as mudanças principais.
5. **Segunda passada**: releia a versão nova procurando o que sobrou (travessão, trios, conclusão genérica).

Formato de entrega:
```
Problemas encontrados
- "<trecho>": <motivo curto>

Versão revisada
<texto>

O que mudou
- <mudança>
```
No Modo Detectar, entregue só "Problemas encontrados" e uma avaliação: quais são problemas claros e quais podem ser intencionais.

## Checklist antes de entregar

- Nenhum fato, número ou cargo foi inventado.
- Nenhum travessão, emoji em título ou abertura de assistente.
- Nenhum trio por reflexo e nenhuma conclusão genérica.
- O ritmo varia: há frases curtas e longas.
- O texto serve ao leitor definido, não a um leitor qualquer.
- Lacunas marcadas com `[PENDENTE: ...]`.
