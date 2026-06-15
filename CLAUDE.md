# Expedição & Recebimento — Empório CTZ

## O que é este projeto
Sistema web de gestão operacional para o Empório CTZ hospedado no GitHub Pages.
- **URL ao vivo**: https://emporioctz.github.io/expedicao-emporioctz
- **Repositório**: git@github.com:EmporioCTZ/expedicao-emporioctz.git
- **Arquivo principal**: `index.html` (single-file, tudo em um arquivo)

## Como fazer deploy
```bash
git add index.html
git commit -m "descrição da mudança"
GIT_SSH_COMMAND="ssh -i ~/.ssh/claude_ctz" git push origin main
```
O GitHub Pages publica automaticamente em ~1-2 minutos após o push.

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
- `recScannerActive` declarado APENAS na linha ~593 (não declarar de novo)
- Shopee só aceita códigos `SPXBR\d+` — barcode bloqueado para Shopee
- Envio Full Shopee: padrão `FBSINBR\d+` — obrigatório antes de bipar

## Bugs já resolvidos (não regredir)
- Firebase no head bloqueava botão "Entrar"
- `recScannerActive` declarado duas vezes causava SyntaxError
- Código ASN Shopee (`INBRFSP...`) rejeitado
- JSON do Mercado Livre parseado via `cleanCode()`

## Dono
- Eduardo Cortez — contato@emporioctz.com.br
