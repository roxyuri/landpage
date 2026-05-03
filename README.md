# Ketilly Santana — Landing Page

Site estático pronto para deploy no Vercel.

## Estrutura

```
deploy/
├── index.html        ← página única
├── assets/
│   └── ketilly.jpeg  ← foto de perfil (substitua para trocar)
├── vercel.json       ← headers de segurança e cache
├── robots.txt
└── README.md
```

## Como fazer deploy no Vercel (mais rápido — sem Git)

1. Acesse https://vercel.com/new
2. Faça login (com e-mail, GitHub, Google etc.)
3. Clique em **"Import third-party Git Repository"** → ou use o botão de **drag-and-drop**
4. Arraste a pasta `deploy/` inteira para o navegador
5. Confirme o deploy. Em ~30 segundos você terá uma URL `*.vercel.app` no ar.

### Com Vercel CLI (se preferir terminal)

```bash
npm i -g vercel
cd deploy
vercel              # primeiro deploy (preview)
vercel --prod       # deploy de produção
```

## Como trocar a foto

Substitua `assets/ketilly.jpeg` por uma nova foto com o **mesmo nome**.
Recomendo:
- proporção próxima de 4:5 (vertical),
- pelo menos 1000px de largura,
- formato `.jpeg` ou `.webp` (se usar webp, ajuste o nome no `index.html`).

## Como trocar textos / links

Edite `index.html`. Os trechos relevantes estão marcados claramente:
- número de WhatsApp: procure por `5511919694124` (3 ocorrências)
- nome: procure por `Ketilly` e `Santana`
- frase em itálico: procure por `Cuidado é uma forma de presença`

## Domínio próprio

Após o deploy, em **Project → Settings → Domains** no painel da Vercel, adicione
seu domínio (ex: `ketillysantana.com.br`). A Vercel te dará os registros DNS
para configurar no seu provedor (Registro.br, GoDaddy, etc).

## Segurança

O `vercel.json` aplica os seguintes cabeçalhos de segurança a todas as rotas:

| Cabeçalho                    | O que faz                                                  |
|------------------------------|-----------------------------------------------------------|
| Content-Security-Policy      | Restringe origens de scripts, imagens, fontes e estilos.  |
| Strict-Transport-Security    | Força HTTPS por 2 anos (HSTS preload-ready).              |
| X-Content-Type-Options       | Bloqueia MIME-sniffing.                                   |
| X-Frame-Options              | Impede que o site seja colocado em iframe (clickjacking). |
| Referrer-Policy              | Não vaza URL completa para sites externos.                |
| Permissions-Policy           | Desliga câmera, microfone, GPS, FLoC, pagamento, USB.     |
| Cross-Origin-Opener-Policy   | Isola o contexto da aba.                                  |
| Cross-Origin-Resource-Policy | Bloqueia inclusão cross-origin dos seus recursos.         |

Outras práticas aplicadas no HTML:
- Todos os links externos usam `rel="noopener noreferrer external"` (impede
  que o site de destino acesse `window.opener` e reduz vazamento de referrer).
- Sem JavaScript próprio rodando no cliente — superfície de ataque mínima.
- Sem dependências de CDN além das fontes do Google (que são whitelisted no CSP).
- Sem cookies, sem formulários, sem armazenamento local — nada de LGPD.

## Performance

- HTML único, sem JS de runtime, sem React/Babel — carrega em <1s mesmo no 3G.
- Fontes carregadas com `display=swap` (não bloqueia render).
- Imagem com `fetchpriority="high"` para LCP rápido.
- Assets cacheados por 1 ano (Vercel + cabeçalho `immutable`).
