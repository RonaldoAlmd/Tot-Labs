# TOT LAB IA — site

Site institucional da **TOT LAB IA**, laboratório de inteligência artificial em Macapá/AP.

Site estático de página única: HTML, CSS e JavaScript puros, sem build, sem dependências para instalar.
As bibliotecas de animação (GSAP, ScrollTrigger, Three.js e Lenis) são carregadas por CDN.

---

## Estrutura

```
.
├── index.html               # o site inteiro (HTML + CSS + JS)
├── favicon.ico
├── img/
│   ├── logo-dark.png        # logo completa (cabeçalho e rodapé)
│   ├── logo-mark.png        # só a marca circular (chip do card Fluênc.IA e ícones)
│   ├── og-cover.png         # imagem de compartilhamento (WhatsApp, Instagram, LinkedIn)
│   ├── favicon.png
│   └── apple-touch-icon.png
├── robots.txt
├── sitemap.xml
├── vercel.json              # cache dos assets + cabeçalhos de segurança
├── .gitignore
└── .editorconfig
```

---

## Publicar na Vercel

### 1. Subir para o GitHub

Dentro desta pasta:

```bash
git init
git add .
git commit -m "site TOT LAB IA"
git branch -M main
git remote add origin https://github.com/SEU-USUARIO/tot-lab-ia.git
git push -u origin main
```

### 2. Importar na Vercel

1. Acesse [vercel.com/new](https://vercel.com/new) e escolha **Import Git Repository**.
2. Selecione o repositório `tot-lab-ia`.
3. Nas configurações do projeto:
   - **Framework Preset:** `Other`
   - **Build Command:** deixe **vazio**
   - **Output Directory:** deixe **vazio** (ou `.`)
   - **Install Command:** deixe **vazio**
4. Clique em **Deploy**.

Não há etapa de build — a Vercel só serve os arquivos. Todo `git push` na branch `main` publica automaticamente.

### Alternativa: sem GitHub

```bash
npm i -g vercel
vercel          # preview
vercel --prod   # produção
```

---

## Trocar o domínio

O projeto está configurado para `https://tot-lab-ia.vercel.app`.
Quando o domínio definitivo estiver ativo (ex.: `https://totlabia.com.br`), substitua o endereço em **3 arquivos**:

| Arquivo       | O que trocar                                                                 |
|---------------|------------------------------------------------------------------------------|
| `index.html`  | `link rel="canonical"`, `og:url`, `og:image`, `twitter:image` e o bloco `application/ld+json` |
| `robots.txt`  | a linha `Sitemap:`                                                            |
| `sitemap.xml` | a tag `<loc>`                                                                 |

Atalho no terminal (macOS/Linux), a partir da pasta do projeto:

```bash
grep -rl "tot-lab-ia.vercel.app" . | xargs sed -i '' "s|https://tot-lab-ia.vercel.app|https://totlabia.com.br|g"
# no Linux, use: sed -i (sem as aspas vazias)
```

Depois, na Vercel: **Settings → Domains → Add** e aponte o DNS conforme as instruções que aparecem lá.

---

## Editar o conteúdo

Está tudo em `index.html`, na ordem em que aparece na tela. As seções estão marcadas por comentários:

| Comentário          | Seção                                                        |
|---------------------|--------------------------------------------------------------|
| `01 HERO`           | Título de abertura                                            |
| `02 SINAL 95 → 5`   | Contador animado de 95% para 5%                               |
| `03 MANIFESTO`      | Texto que acende palavra por palavra                          |
| `04 ORQUESTRA`      | Os três movimentos do método                                  |
| `05 DADOS`          | Percentuais e a matriz de 100 pontos                          |
| `06 FLUENC.IA`      | Programa + card do ingresso                                   |
| `07 EM CAMPO`       | Carrossel 3D (os cards vêm do array `cards` no JavaScript)     |
| `08 AMAZONIA`       | Latitude 0° e a lista de parceiros                            |
| `09 LIDERES`        | Diretoria                                                     |
| `10 RECURSOS`       | E-book, podcast e entrevista                                  |
| `11 CTA`            | Chamada final                                                 |

**Cores da marca** ficam no topo do CSS, no bloco `:root` (`--violet`, `--lilac`, `--void`, `--bone`).

**Cards do "Em campo"** ficam no array `cards` dentro do `<script>` — cada item tem `tag`, `n` (número em destaque), `t` (título), `p` (texto) e `h` (matiz do degradê, 0–360).

**Trocar a logo:** substitua `img/logo-dark.png` mantendo o mesmo nome e o fundo transparente. Se trocar também a marca circular, gere um recorte quadrado da marca em `img/logo-mark.png`.

---

## Rodar localmente

Abrir o `index.html` direto pelo Finder/Explorer funciona, mas alguns navegadores bloqueiam recursos em `file://`. O mais seguro é subir um servidor:

```bash
npx serve .
# ou
python3 -m http.server 8000
```

E acessar `http://localhost:8000`.

---

## Notas técnicas

- **Responsivo:** três faixas de ajuste (≤819px, ≤560px, ≤400px). No celular o método ORQUESTRA empilha na vertical, o tilt 3D dos cards passa a ser guiado pelo scroll e há menu hamburguer com overlay.
- **Performance no celular:** menos partículas no WebGL, `pixelRatio` reduzido, menos blur e `backdrop-filter` desligado onde pesava.
- **Acessibilidade:** respeita `prefers-reduced-motion` — com a opção "reduzir movimento" ligada no sistema, o loader e as animações são desativados e o conteúdo aparece direto.
- **Sem WebGL:** se o dispositivo não suportar, o fundo de partículas simplesmente não é desenhado; o site continua funcionando.
