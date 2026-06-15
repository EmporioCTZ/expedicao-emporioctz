# Expedição & Recebimento — Empório CTZ

## O que é este projeto
Sistema web de gestão operacional para o Empório CTZ.
- **URL ao vivo**: https://sistemas.emporioctz.com.br/expedicao/
- **Servidor**: Contabo VPS — `root@sistemas.emporioctz.com.br`
- **Arquivo no servidor**: `/var/www/ctz-sistemas/public/expedicao/index.html`
- **Repositório GitHub** (backup): git@github.com:EmporioCTZ/expedicao-emporioctz.git
- **Arquivo principal**: `index.html` (single-file, tudo em um arquivo)

## Como fazer deploy (SEMPRE assim)
```bash
cd /Users/eduardocortez/Documents/projetos/expedicao-emporioctz

# 1. Enviar para o servidor — entra em produção instantaneamente
scp -i ~/.ssh/claude_ctz index.html root@sistemas.emporioctz.com.br:/var/www/ctz-sistemas/public/expedicao/index.html

# 2. Commit no GitHub (backup)
git add index.html && git commit -m "descrição" && git push origin main
```
A chave `~/.ssh/claude_ctz` já tem acesso ao servidor e ao GitHub.

## Stack
- HTML/CSS/JS puro (sem framework, sem build step)
- Firebase Realtime Database (lazy-loaded — NÃO coloque no `<head>`)
- html5-qrcode@2.3.8 — scanner de câmera
- jsPDF@2.5.1 + jspdf-autotable@3.5.28 — geração de PDF

## Módulos
1. **Expedição** — bipagem de pacotes, assinatura do motorista, PDF + WhatsApp
2. **Recebimento** — conferência de NF, catálogo de caixas, relatório
3. **Dashboard** — histórico Firebase, stats por funcionário, export CSV

## Configuração no topo do index.html
```js
const LOGO_URL = 'logo.jpg';
const EMAIL_DESTINO = 'atendimento@emporioctz.com.br';
const OWNER_PASSWORD = 'Eduardo2579';
const FIREBASE_CONFIG = { ... };
```

## Regras críticas
- **Firebase NUNCA no `<head>`** — bloqueia toda execução JS
- Firebase usa lazy loading via `ensureFirebase(cb)`
- `recScannerActive` declarado APENAS uma vez (linha ~593)
- Shopee só aceita códigos `SPXBR\d+` — barcode bloqueado para Shopee
- Envio Full Shopee: padrão `FBSINBR\d+` — obrigatório antes de bipar

## Bugs já resolvidos (não regredir)
- Firebase no head bloqueava botão "Entrar"
- `recScannerActive` declarado duas vezes causava SyntaxError
- Código ASN Shopee (`INBRFSP...`) rejeitado
- JSON do Mercado Livre parseado via `cleanCode()`

## Dono
- Eduardo Cortez — contato@emporioctz.com.br
