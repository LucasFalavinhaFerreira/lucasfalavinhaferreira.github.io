Calendário de Lançamentos

Web app (PWA) para consultar e escanear códigos de barras de peças de vestuário, mostrando data de lançamento, cores disponíveis e estoque por variação — direto do navegador, sem instalação de loja de app.

 Demo ao vivo: https://lucasfalavinhaferreira.github.io/lancamentos-demo/

⚠️ Este repositório usa dados 100% fictícios ("Loja Modelo", coleção DEMO26), gerados apenas para demonstrar a ferramenta. Nenhuma informação real de estoque, produto ou marca está incluída aqui.

 Funcionalidades
 Busca instantânea por código de produto, EAN ou nome da peça
 Scanner de código de barras via câmera do celular (EAN-13, EAN-8, Code128, UPC), usando Quagga.js
 Página de detalhe por produto — mostra todas as cores/variações de uma peça, com estoque individual e total
 Indicadores de urgência de lançamento (hoje, amanhã, em X dias, já lançado) com filtros rápidos
 Indicadores de estoque (disponível / estoque baixo / esgotado) por variação e por peça
 Instalável como app (PWA) — ícone na tela inicial, funciona em tela cheia, com cache offline básico via Service Worker
 Pronto para conectar a uma planilha real (Google Sheets publicado como CSV) — veja Conectando dados reais
🖼️ Estrutura do projeto
├── index.html          # aplicação completa (HTML + CSS + JS em um arquivo só)
├── manifest.json        # metadados do PWA (nome, ícones, cores)
├── service-worker.js    # cache offline básico
├── icon-192.png          # ícone do app (192×192)
└── icon-512.png          # ícone do app (512×512)

Tudo é estático — sem backend, sem build step, sem dependências de servidor. Só precisa de hospedagem com HTTPS (obrigatório para o scanner de câmera funcionar).

 Rodando localmente

Não precisa de nada além de um navegador. Duas opções:

bash
# opção 1: abrir direto
open index.html

# opção 2: servidor local simples (recomendado, evita bloqueios de CORS/câmera)
python3 -m http.server 8080
# depois acesse http://localhost:8080

O scanner de câmera só funciona em contexto seguro: https:// ou localhost. Não funciona abrindo o arquivo direto com file:// em alguns navegadores.

 Publicando:

Hospedado atualmente via GitHub Pages, sem custo:

bash
git add .
git commit -m "update"
git push

Funciona igualmente bem em Netlify, Vercel ou Cloudflare Pages — é só arrastar a pasta.

 Conectando dados reais:

Por padrão, os dados ficam fixos dentro do index.html (um "retrato" do momento em que foram gerados). Para conectar numa planilha real que atualiza sozinha:

No Google Sheets: Arquivo → Compartilhar → Publicar na Web → formato CSV
Garanta que a planilha tenha estas colunas, com cabeçalho:
   produto | cor | ean | colecao | peca | qtd | data

(uma linha por variação de cor — o app agrupa automaticamente por produto) 3. No index.html, defina a constante:

js
   const CSV_URL = 'https://docs.google.com/spreadsheets/d/e/SEU_ID/pub?output=csv';
O app busca os dados a cada carregamento da página, com cache local de fallback (localStorage) caso a planilha fique indisponível.

⚠️ Atenção com dados sensíveis: "Publicar na Web" no Google Sheets torna o CSV acessível a qualquer pessoa com o link, sem login. Isso é aceitável para dados de demonstração, mas não deve ser usado com dados reais de estoque/lançamento de uma marca sem antes avaliar controle de acesso (planilha privada + Google Sheets API com conta de serviço, app atrás de autenticação, etc.).

 Stack:
HTML/CSS/JavaScript puro (zero frameworks, zero build)
Quagga.js — leitura de código de barras via câmera
PapaParse — parsing de CSV (quando conectado a planilha real)
Fontes: Fraunces, Inter, Space Mono via Google Fonts
Web App Manifest + Service Worker para instalação como PWA

 Status:
Projeto em fase de demonstração/portfólio. Ainda não conectado a nenhum sistema ou planilha real.
