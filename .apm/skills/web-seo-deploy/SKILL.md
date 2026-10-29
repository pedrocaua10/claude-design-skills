---
name: "web-seo-deploy"
description: "Prepara um site para publicação: meta tags, Open Graph, SEO básico, dados estruturados, favicon, domínio e hospedagem (GitHub Pages, Vercel, Netlify, Cloudflare Pages). Use ao pedir para publicar a página, melhorar o SEO ou compartilhar o link com boa prévia."
---

# Web SEO e Deploy

Deixa uma página pronta para ser encontrada, compartilhada e publicada. Entregue os trechos de código e o passo a passo de publicação já prontos, sem explicar o que é SEO. Para desempenho, use `web-performance`. Se a página coletar dados pessoais, use também a skill de LGPD do projeto.

## Modos de entrada

**Modo Pedido direto**: o usuário indicou o que publicar e onde. Prepare e entregue.

**Modo Briefing**: faltam dados. Perguntas em **uma única mensagem**, numeradas e curtas:
1. Qual é a URL final ou o domínio?
2. Onde vai ser hospedado (GitHub Pages, Vercel, Netlify, Cloudflare Pages, outro)?
3. Qual é a stack (arquivo único ou projeto com build)?
4. A página coleta dados (formulário, analytics, cookies)?
5. Qual título, descrição e imagem de compartilhamento usar?

Nunca invente URL, domínio, perfil de rede social ou texto sobre a pessoa. Marque `[PENDENTE: ...]`.

## Fluxo de entrega

1. Revisar o `<head>` e completar o que falta.
2. Criar a imagem de compartilhamento e o favicon.
3. Adicionar dados estruturados quando fizer sentido.
4. Preparar a publicação na hospedagem escolhida.
5. Checklist antes de publicar.

## Template: head essencial

```html
<html lang="pt-BR">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Nome da página</title>
  <meta name="description" content="Resumo de 140 a 160 caracteres, com o assunto e para quem é.">
  <link rel="canonical" href="https://[PENDENTE: URL final]/">
  <meta property="og:type" content="website">
  <meta property="og:title" content="Nome da página">
  <meta property="og:description" content="Mesmo resumo, ajustado para compartilhamento.">
  <meta property="og:url" content="https://[PENDENTE: URL final]/">
  <meta property="og:image" content="https://[PENDENTE: URL final]/og.png">
  <meta name="twitter:card" content="summary_large_image">
  <link rel="icon" href="/favicon.svg" type="image/svg+xml">
  <meta name="theme-color" content="#0A1325">
</head>
```

Imagem de compartilhamento: 1200 por 630 px, texto legível em tamanho pequeno, URL absoluta.

## Dados estruturados (portfólio)

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Person",
  "name": "[PENDENTE: nome]",
  "jobTitle": "[PENDENTE: cargo]",
  "url": "https://[PENDENTE: URL final]/",
  "sameAs": ["[PENDENTE: perfis reais]"]
}
</script>
```
Inclua só dados que a pessoa autorizou publicar.

## Hospedagem

| Opção | Quando usar |
|---|---|
| GitHub Pages | Site estático em repositório, gratuito, simples para portfólio |
| Cloudflare Pages, Netlify, Vercel | Site estático ou com build, deploy automático a cada push |
| Domínio próprio | Registro de `.com.br` pelo Registro.br ou `.com` por registrador, depois apontar o DNS |

Passos comuns:
1. Repositório com `index.html` na raiz (ou saída do build).
2. Ativar a hospedagem e apontar para a branch.
3. Configurar domínio e HTTPS.
4. Abrir a URL final e conferir.

## Arquivos de apoio

- `robots.txt` liberando o rastreamento e apontando o sitemap.
- `sitemap.xml` com as URLs reais, se houver mais de uma página.
- `favicon.svg` e, se quiser, ícones para celular.

## Checklist antes de publicar

- `lang="pt-BR"`, título e descrição únicos e específicos.
- Open Graph e imagem de compartilhamento testados em uma prévia real.
- `canonical` com a URL final.
- Um único `h1`, links com texto descritivo, `alt` nas imagens.
- Sem informação pessoal que a pessoa não quis publicar.
- Se houver formulário ou analytics, aviso de privacidade e consentimento onde exigido.
- HTTPS ativo e página abre em celular.
- Nada inventado: lacunas marcadas com `[PENDENTE: ...]`.
