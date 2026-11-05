# Cockpit BLM

Painel único com acessos (GA4 da Brando), status dos sites, ciclo de vendas da Brando e atalhos para os formulários e workflows do n8n.

- **Abrir:** `https://brandoguitarras.app.n8n.cloud/webhook/cockpit?key=SUA_CHAVE` (salve nos favoritos / tela inicial do celular).
- A página também funciona aberta direto do arquivo `index.html`: ela pede a chave uma vez e guarda no navegador.

## Como funciona

O workflow **Cockpit — Dados** no n8n tem duas rotas:

| Rota | O que faz |
|---|---|
| `GET /webhook/cockpit` | Serve esta página (baixada deste repositório, branch `main`). |
| `POST /webhook/daily-posts-agora?key=…` | Gatilho extra do workflow **Daily Posts** (o das 8h continua). Dispara o mesmo caminho: texto, e-mail de aprovação e publicação. |
| `GET /webhook/cockpit-dados?key=…` | Devolve o JSON: GA4 (30 dias + origens/páginas/eventos de 7 dias), status de 5 sites, abas Vendas e Ações do BLM Scorecard e a lista de atalhos. Sem a chave certa, responde vazio. |

Os links dos formulários ficam no nó **Atalhos** do n8n (e não neste repositório público), porque alguns formulários não pedem código.

## Para mudar

- **Adicionar/remover um atalho:** editar o nó *Atalhos* no n8n. Use `href` para abrir um link, ou `action` (caminho de um webhook POST que aceite `?key=`) + `confirm` para um botão que dispara com confirmação. Ícones disponíveis: play, image, story, linkedin, mic, video, upload, wand, flow, chart, sheet, mail.
- **Visual da página:** editar `index.html` e dar push na `main`. A mudança entra no ar na hora.
- **Outro site no GA4:** duplicar os nós GA4 com o ID da propriedade.
