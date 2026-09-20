# Siteweb

Site estático gerado com **Hugo**, publicado em [https://marcospredebon.github.io/Siteweb/](https://marcospredebon.github.io/Siteweb/).

Não é um projeto Node/React/Vite/Next.js. Não existe `package.json`. A única dependência de runtime é o **Hugo Extended**.

## Stack

- Hugo Extended (gerador de site estático)
- HTML nos templates em `layouts/`
- CSS em `static/css/custom.css`
- Conteúdo em Markdown em `content/`
- GitHub Actions (`.github/workflows/hugo-deploy.yml`) gera o site e envia para a branch `gh-pages`
- GitHub Pages deve servir a branch `gh-pages`, pasta `/`

## Requisitos em outra máquina

- Git
- Hugo Extended **0.151.2** ou compatível (o CI usa exatamente essa versão)
- Navegador para validar `http://localhost:1313/Siteweb/`

## Instalação do Hugo (Windows)

```powershell
winget install Hugo.Hugo.Extended --version 0.151.2
hugo version
```

Alternativa: baixe o zip `hugo_extended_0.151.2_windows-amd64.zip` em [https://github.com/gohugoio/hugo/releases](https://github.com/gohugoio/hugo/releases), extraia e coloque `hugo.exe` no `PATH`.

macOS:

```bash
brew install hugo
hugo version
```

Linux:

```bash
# exemplo com o binário oficial extended
# ver https://gohugo.io/installation/linux/
hugo version
```

## Rodar localmente

Na raiz do repositório (onde está o `hugo.toml`):

```powershell
git clone https://github.com/marcospredebon/Siteweb.git
cd Siteweb
hugo server --baseURL http://localhost:1313/Siteweb/ --appendPort=false
```

Abra [http://localhost:1313/Siteweb/](http://localhost:1313/Siteweb/).

Build de produção (saída em `public/`, pasta ignorada pelo Git):

```powershell
hugo --minify --baseURL "https://marcospredebon.github.io/Siteweb/"
```

Não é necessário `npm install` nem qualquer outra instalação de pacotes JS.

## Publicar no GitHub Pages

1. Confirme em **Settings → Pages**:
   - Source: **Deploy from a branch**
   - Branch: **`gh-pages`**
   - Folder: **`/ (root)`**
2. Envie as alterações para `main`:

```powershell
git add .
git commit -m "Describe your change"
git push origin main
```

3. O workflow **Build and deploy Hugo site to GitHub Pages** gera `public/` e atualiza `gh-pages`.
4. Aguarde o workflow `pages-build-deployment` da branch **`gh-pages`** (não o da `main`).
5. Abra [https://marcospredebon.github.io/Siteweb/](https://marcospredebon.github.io/Siteweb/).

Se o Pages estiver apontado para `main`, o GitHub publica o código-fonte em vez do HTML gerado. A home vira o `index.xml` residual ou um 404, e rotas como `/criatividade/` não existem.

## Estrutura

```
Siteweb/
├── hugo.toml                 # configuração do site e baseURL
├── content/                  # Markdown (fonte)
├── layouts/                  # templates Hugo
├── static/css/custom.css     # CSS publicado em /css/custom.css
├── archetypes/
└── .github/workflows/hugo-deploy.yml
```

Edite sempre a fonte (`content/`, `layouts/`, `static/`). Nunca edite a branch `gh-pages` manualmente.
