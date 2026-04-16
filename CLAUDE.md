# Tutoriais CRM — Mediagrowth

Site de tutoriais em vídeo do CRM para a equipe da Mediagrowth.
Deploy no Netlify em: https://treinamento.mediagrowth.com.br

## Estrutura

- `index.html` — menu principal (grid de cards 2 colunas)
- `loom-[seção].html` — playlist de vídeos de cada seção
- `fotos menu/` — imagens de capa dos cards (16:9, ex: conversas.png, oportunidades.png)
- `_headers` — headers do Netlify (permite iframe)

## Seções existentes

| Arquivo | Card no index | Foto de capa |
|---|---|---|
| loom-conversas.html | Conversas | fotos menu/conversas.png |
| loom-oportunidades.html | Oportunidades | fotos menu/oportunidades.png |
| loom-acesso.html | Acesso | fotos menu/acesso.png |

## Regra obrigatória: todo novo HTML deve ter card no index

**Sempre que criar um novo `loom-[nome].html`, é obrigatório adicionar o card correspondente no `index.html` na mesma operação.** Nunca criar um arquivo de playlist sem atualizar o index.

## Como adicionar tutoriais

### Com foto de capa → Nova seção

O usuário manda uma imagem + link(s). Sempre que vier foto junto, é para criar nova seção — não perguntar.

Fazer:
1. Salvar imagem em `fotos menu/[nome].png`
2. Criar `loom-[nome].html` baseado na estrutura de `loom-oportunidades.html`
3. **Obrigatório:** adicionar card no `index.html` (padrão existente) — nunca pular este passo

### Sem foto, só link → Adicionar a seção existente

O usuário manda apenas o link do Loom. Fazer:
1. Extrair o ID do Loom da URL
2. Buscar o título real via oEmbed: `https://www.loom.com/v1/oembed?url=https://www.loom.com/share/LOOM_ID`
3. Usar o campo `title` da resposta como nome do vídeo — nunca inventar
4. Ler o `index.html` e os arquivos `loom-*.html` existentes para inferir em qual seção o vídeo se encaixa pelo contexto do título
5. Adicionar no array `vids` do arquivo correto
6. Perguntar ao usuário somente se não for possível determinar a seção com confiança

Regras completas em `.claude/skills/add-tutorial.md`.

## Padrão dos vídeos (Loom)

- URL de embed: `https://www.loom.com/embed/LOOM_ID`
- Thumbnails carregadas automaticamente via oEmbed pelo JS da página
- Tag `sub`: `[Seção] · Celular` ou `[Seção] · Computador`
  - Celular: título menciona "aplicativo", "app", "celular"
  - Computador: título menciona "desktop", "computador", "web"
  - Se não for claro, perguntar ao usuário
- Nomes de arquivo: minúsculas, sem acentos, sem espaços
