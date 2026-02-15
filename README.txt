================================================================================
SITEWEB - CI/CD com Hugo e GitHub Actions
================================================================================

ESTRUTURA DO REPOSITÓRIO:
========================

/Siteweb (raiz do repositório GitHub)
├── content/                    ← Fonte Hugo (config + conteúdo)
│   ├── hugo.toml              ← configuração do site
│   ├── content/               ← páginas Markdown
│   │   ├── criatividade/
│   │   │   ├── jose-predebon.md
│   │   │   └── livros/        ← diretório de livros com imagens
│   │   └── tecnologia/
│   ├── layouts/               ← templates HTML
│   ├── static/                ← CSS e assets estáticos
│   ├── themes/                ← temas (se houver)
│   └── public/                ← saída gerada (não combitar)
├── .github/workflows/
│   └── hugo-deploy.yml        ← GitHub Actions ci/cd
├── index.html                 ← página de landing (legacy)
├── css/custom.css             ← estilos (legacy)
└── README.txt                 ← este arquivo


FLUXO CI/CD (Automático):
=========================

1. Você edita arquivos em content/content/ (Markdown) ou content/static/ (CSS/assets)
2. Faz commit e push para branch 'main':
   
   git add content/
   git commit -m "Descrição da alteração"
   git push origin main

3. GitHub Actions dispara automaticamente:
   - Faz checkout do repositório
   - Instala Hugo versão 'latest'
   - Roda: hugo --source content --destination public --minify
   - Faz deploy do conteúdo de public/ para branch 'gh-pages'

4. GitHub Pages publica automaticamente o site em:
   https://marcospredebon.github.io/Siteweb/ (ou domínio customizado se configurado)


TAREFAS COMUNS:
===============

A) EDITAR CONTEÚDO (Markdown)
   ├─ Arquivo: content/content/criatividade/jose-predebon.md
   ├─ Editar no VS Code (ou editor de texto)
   ├─ Salvar
   ├─ Terminal:
   │  git add content/content/criatividade/jose-predebon.md
   │  git commit -m "Atualiza página José Predebon"
   │  git push origin main
   └─ Site atualiza em ~30-60 segundos (CI/CD roda)

B) ADICIONAR LIVROS NOVOS
   ├─ Pasta: content/content/criatividade/livros/<nome-livro>/
   ├─ Adicionar arquivos:
   │  - index.md (conteúdo do livro)
   │  - capa.jpg (imagem)
   ├─ Editar index.md com front-matter YAML:
   │  ---
   │  title: "Título do Livro"
   │  ---
   │  Descrição aqui...
   ├─ Terminal:
   │  git add content/content/criatividade/livros/<nome-livro>/
   │  git commit -m "Adiciona livro: <nome-livro>"
   │  git push origin main
   └─ Site atualiza automaticamente

C) ATUALIZAR CSS/ESTILOS
   ├─ Arquivo: content/static/css/custom.css (ou novo arquivo em static/)
   ├─ Editar
   ├─ Terminal:
   │  git add content/static/
   │  git commit -m "Atualiza estilos CSS"
   │  git push origin main
   └─ Site atualiza

D) ADICIONAR TEMPLATES/LAYOUTS
   ├─ Arquivo: content/layouts/_default/single.html (ou outro em layouts/)
   ├─ Editar
   ├─ Terminal:
   │  git add content/layouts/
   │  git commit -m "Novo layout para single page"
   │  git push origin main
   └─ Hugo regenera com novo template


VERIFICAR STATUS DO CI/CD:
==========================

1. GitHub → Repositório → Actions
   - Vejo histórico de execuções (verde = sucesso, vermelho = erro)
   - Se falhar, clico e vejo logs detalhados

2. GitHub → Settings → Pages
   - Confirma que está servindo de branch 'gh-pages' (pasta '/')
   - Mostra URL do site publicado

3. Teste rápido local (opcional):
   PowerShell:
   & 'C:\Users\mapredeb\OneDrive - Microsoft\Pessoal\hugo\hugo.exe' --source content
   Abre: http://localhost:1313


DICAS IMPORTANTES:
==================

★ Não editar /public diretamente
  → É gerado automaticamente pelo Hugo
  → Qualquer alteração aqui é sobrescrita

★ Git ignore (já configurado):
  → /public/ → não é adicionado ao git
  → Apenas source code em content/ é versionado

★ Branches:
  → 'main' → source code (o que você edita)
  → 'gh-pages' → output gerado (gerado automaticamente pelo CI)
  → Nunca edite manualmente gh-pages

★ Botão "Apresentar livros":
  → Criado em index.html
  → Link para /criatividade/livros/
  → Aponta para conteúdo em content/content/criatividade/livros/

★ Se o workflow falhar:
  → Verifique GitHub → Actions → última execução (logs em vermelho)
  → Motivos comuns:
     • Erro de sintaxe em .md
     • Arquivo quebrado em content/
     • Arquivo sem permissão
     • Nome de pasta com caracteres especiais
  → Corrija, commit e push novamente


EXEMPLO COMPLETO - ADICIONAR NOVO LIVRO:
==========================================

1. Crie pasta:
   mkdir "content/content/criatividade/livros/novo-livro"

2. Crie index.md com conteúdo:
   cat > "content/content/criatividade/livros/novo-livro/index.md" << 'EOF'
   ---
   title: "Novo Livro"
   ---
   
   Descrição e informações do livro aqui...
   EOF

3. Copie capa.jpg para a pasta

4. Commit e push:
   git add content/content/criatividade/livros/novo-livro/
   git commit -m "Adiciona livro: Novo Livro"
   git push origin main

5. Aguarde CI/CD (~1-2 min)

6. Verifique em: https://marcospredebon.github.io/Siteweb/criatividade/livros/novo-livro/


SUPORTE / TROUBLESHOOTING:
==========================

Problema: Site não atualizou depois de 5 minutos
Solução:
  → Verifique GitHub Actions (pode estar com erro)
  → Logs: Actions → última execução → ver output completo
  → Rerun workflow manualmente se necessário

Problema: Arquivo não aparece no site
Solução:
  → Verifique se está em content/content/ (não em raiz)
  → Verifique nomenclatura (case-sensitive em URLs)
  → Verifique front-matter YAML em .md
  → Rodou git push? (não só git add/commit)

Problema: Imagens não carregam
Solução:
  → Coloque em content/static/ ou pasta com index.md
  → Referencie com caminho relativo ou /static/...
  → Verifique permissões do arquivo


================================================================================
Criado: 15 de fevereiro de 2026
Última atualização: CI/CD automático com GitHub Actions
================================================================================
