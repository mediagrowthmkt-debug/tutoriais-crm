# Skill: add-tutorial

Você é um assistente especializado em gerenciar tutoriais do projeto Tutoriais-CRM da Mediagrowth.

## Contexto do projeto

- `index.html` — menu principal com cards (grid 2 colunas) para cada seção de tutoriais
- `loom-[seção].html` — página de playlist de vídeos para cada seção (ex: loom-conversas.html, loom-oportunidades.html)
- `fotos menu/` — imagens de capa usadas nos cards do index (formato 16:9)
- Vídeos hospedados no Loom. O ID fica no final da URL: `https://www.loom.com/share/LOOM_ID`

## Dois modos de operação

### MODO 1 — Nova seção (usuário enviou foto de capa)

O usuário mandou uma imagem + contexto + link(s). Isso significa criar uma nova seção completa.

**O que fazer:**

1. **Salvar a imagem de capa** em `fotos menu/[nome-da-secao].png`
   - Use o nome em minúsculas sem acentos (ex: `agendamentos.png`, `relatorios.png`)

2. **Adicionar card no `index.html`** — inserir antes do fechamento `</div>` da `.grid`, seguindo exatamente este padrão:
```html
    <a class="card" href="loom-[nome].html">
      <div class="card-preview">
        <img src="fotos menu/[nome].png" alt="Preview [Nome]">
      </div>
      <div class="card-label">[Nome com acento]</div>
      <div class="card-sub">Ver tutorial →</div>
    </a>
```

3. **Criar o arquivo `loom-[nome].html`** — copiar a estrutura de `loom-oportunidades.html` e adaptar:
   - `<title>` → `Treinamento CRM — [Nome]`
   - `<meta name="description">` → descrição relevante
   - `<meta property="og:url">` → URL com o novo arquivo
   - `<meta property="og:title">` → `Treinamento CRM — [Nome]`
   - `<meta property="og:image">` → `https://treinamento.mediagrowth.com.br/fotos%20menu/[nome].png`
   - `<p>` no `.header` → `[Nome com acento]`
   - `<iframe id="frame" src>` → primeiro vídeo da seção
   - `id="ntitle"` e `id="nsub"` → título e tag do primeiro vídeo
   - Array `const vids` → todos os vídeos passados pelo usuário

4. **Atualizar o `index.html` se o grid ficar com número ímpar de cards** — verificar se precisa ajustar o layout.

---

### MODO 2 — Adicionar vídeo a seção existente (usuário NÃO enviou foto)

O usuário mandou apenas o link. Não precisa indicar onde colocar — você descobre sozinho.

**O que fazer:**

1. **Extrair o ID do Loom** da URL fornecida

2. **Buscar o título real do vídeo via oEmbed** — fazer fetch para:
   ```
   https://www.loom.com/v1/oembed?url=https://www.loom.com/share/LOOM_ID
   ```
   Usar o campo `title` da resposta como nome do vídeo. Nunca inventar título.

3. **Determinar a seção automaticamente** — ler o `index.html` para ver as seções existentes e comparar o título do vídeo com os nomes das seções e o contexto dos vídeos já existentes em cada `loom-*.html`. Escolher a seção cujo tema mais se aproxima do título puxado.
   - Se não for possível determinar com confiança, perguntar ao usuário antes de salvar.

4. **Abrir o arquivo HTML da seção escolhida** e adicionar no final do array `vids`, antes do `];`:
```js
  { id: "LOOM_ID", title: "Título exato puxado do oEmbed", sub: "Seção · Celular/Computador" },
```
   - Inferir `sub` (Celular ou Computador) pelo título do vídeo — se mencionar "aplicativo", "app", "celular", use `Celular`; se mencionar "desktop", "computador", "web", use `Computador`. Se não for claro, perguntar.

5. **Confirmar para o usuário** qual seção foi escolhida, o título puxado e o que foi alterado.

---

## Regras importantes

- **Nunca inventar IDs de Loom** — usar somente os fornecidos pelo usuário
- **Manter o padrão visual** — não alterar CSS, apenas HTML/JS
- **Nome dos arquivos** em minúsculas, sem espaços, sem acentos
- **Tag `sub`** segue o padrão `[Seção] · Celular` ou `[Seção] · Computador`
- Após cada alteração, confirmar para o usuário o que foi feito

---

## Como identificar o modo correto

| Usuário enviou | Modo |
|---|---|
| Imagem de capa + link (com ou sem contexto extra) | MODO 1 (Nova seção) |
| Só link (sem foto) | MODO 2 — buscar título via oEmbed e inferir seção automaticamente |

> **Regra confirmada pelo usuário:** sempre que vier uma imagem junto com o link, é MODO 1 — criar nova seção. Não perguntar.
