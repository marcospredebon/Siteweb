================================================================================
SITEWEB - Hugo + GitHub Actions + GitHub Pages
================================================================================

Este arquivo resume o fluxo. O guia completo está em README.md.

STACK:
  Hugo Extended (não é React/Vite/Next.js; não há package.json)

RODAR LOCAL:
  hugo server --baseURL http://localhost:1313/Siteweb/ --appendPort=false
  Abrir: http://localhost:1313/Siteweb/

PUBLICAR:
  1. GitHub → Settings → Pages
     Source: Deploy from a branch
     Branch: gh-pages
     Folder: / (root)
  2. git push origin main
  3. O workflow gera public/ e atualiza a branch gh-pages

IMPORTANTE:
  - Edite content/, layouts/ e static/ (fonte)
  - Nunca edite a branch gh-pages
  - Pages NÃO pode apontar para main: lá não existe index.html do site
  - baseURL do site: https://marcospredebon.github.io/Siteweb/

ESTRUTURA REAL:
  hugo.toml
  content/          ← Markdown
  layouts/          ← templates
  static/css/       ← CSS
  .github/workflows/hugo-deploy.yml

================================================================================
