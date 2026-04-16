# Documentação — Tutoriais CRM

## Visão Geral

Site estático de tutoriais em vídeo (Loom) para o CRM. Hospedado via GitHub Pages.

- **Repositório:** `mediagrowthmkt-debug/tutoriais-crm`
- **Branch:** `main`
- **URL:** definida no arquivo `CNAME`

---

## Estrutura de Arquivos

```
index.html                  → Menu principal (cards de cada categoria)
loom-conversas.html         → Playlist de vídeos: Conversas
loom-oportunidades.html     → Playlist de vídeos: Oportunidades
fotos menu/                 → Imagens dos cards do menu
  conversas.png
  oportunidades.png
mediagrowth_design_system.html → Design system (referência)
```

---

## Como Adicionar / Editar Vídeos

### 1. Obtenha o ID do vídeo no Loom

O link do Loom tem este formato:
```
https://www.loom.com/share/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```
O **ID** é a parte após `/share/`. Exemplo:
```
Link:  https://www.loom.com/share/1daf511e0a6a4ee7ac9dd1832b0d2c67
ID:    1daf511e0a6a4ee7ac9dd1832b0d2c67
```

### 2. Escolha em qual página o vídeo será adicionado

| Página | Arquivo |
|--------|---------|
| Conversas | `loom-conversas.html` |
| Oportunidades | `loom-oportunidades.html` |

### 3. Edite o array `vids` no `<script>` do arquivo

Localize o array `vids` no final do HTML. Cada vídeo é um objeto com 3 campos:

```javascript
const vids = [
  {
    id: "ID_DO_LOOM",              // ID extraído do link
    title: "Título do Vídeo",      // Título exibido na tela
    sub: "Categoria · Plataforma"  // Tag exibida (ex: "Conversas · Celular")
  },
  // ... mais vídeos
];
```

**Exemplo — adicionar um vídeo em Conversas:**
```javascript
const vids = [
  { id: "1daf511e0a6a4ee7ac9dd1832b0d2c67", title: "Gerenciando Leads pelo Aplicativo CRM [Whatsapp]", sub: "Conversas · Celular" },
  { id: "2d6c7059daa24395bf535e1397d0e127", title: "Gerenciando Leads pelo WhatsApp API", sub: "Conversas · Computador" },
  { id: "SEU_NOVO_ID_AQUI", title: "Título do Novo Vídeo", sub: "Conversas · Celular" },
];
```

### 4. Atualize o iframe inicial (primeiro vídeo da lista)

No HTML, o primeiro vídeo carrega automaticamente. Se mudar o primeiro item do array, atualize também o `src` do iframe:

```html
<iframe id="frame" src="https://www.loom.com/embed/ID_DO_PRIMEIRO_VIDEO" ...></iframe>
```

E atualize o título e tag iniciais:

```html
<p class="now-title" id="ntitle">Título do Primeiro Vídeo</p>
<span class="now-tag" id="nsub">Categoria · Plataforma</span>
```

### 5. Plataformas disponíveis para o campo `sub`

| Valor | Quando usar |
|-------|-------------|
| `Celular` | Vídeo gravado no app mobile |
| `Computador` | Vídeo gravado no desktop/web |

Formato: `"NomeDaCategoria · Plataforma"`

Exemplos:
- `"Conversas · Celular"`
- `"Conversas · Computador"`
- `"Oportunidades · Celular"`
- `"Oportunidades · Computador"`

---

## Como Criar uma Nova Categoria (nova página)

1. **Duplique** `loom-conversas.html` e renomeie (ex: `loom-contatos.html`)
2. Altere o `<title>` e o `<p>` do header para o nome da categoria
3. Substitua o array `vids` com os vídeos da nova categoria
4. Atualize o `src` do iframe com o ID do primeiro vídeo
5. Atualize título e tag iniciais no HTML
6. **No `index.html`**, adicione um novo card:

```html
<a class="card" href="loom-contatos.html">
  <div class="card-preview">
    <img src="fotos menu/contatos.png" alt="">
  </div>
  <div class="card-body">
    <h2>Contatos</h2>
    <p>X tutoriais</p>
  </div>
  <span class="card-arrow">→</span>
</a>
```

7. Adicione a imagem de preview em `fotos menu/contatos.png`

---

## Configurações Visuais (padrão)

| Propriedade | Valor |
|-------------|-------|
| Fonte | DM Sans (Google Fonts) |
| Background | `#EEF2F7` |
| Cards/Sidebar | `#ffffff` |
| Texto principal | `#111` |
| Texto secundário | `#777` / `#999` |
| Border radius cards | `10px` |
| Border radius player | `14px` |
| Max-width layout | `1140px` |
| Sidebar width | `320px` |

---

## Funcionalidades Automáticas

- **Thumbnails da playlist**: Buscadas automaticamente via API oEmbed do Loom (formato `.jpg` estático)
- **Player principal**: Usa iframe nativo do Loom com preview e play original
- **Navegação**: Botões "Anterior" e "Próximo" + clique nos itens da playlist
- **Responsivo**: Em telas < 720px, layout muda para coluna única

---

## Deploy

O site é servido pelo GitHub Pages. Para publicar:

```bash
git add .
git commit -m "descrição da alteração"
git push origin main
```

As alterações ficam disponíveis em poucos minutos.
