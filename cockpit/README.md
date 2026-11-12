# Cockpit BLM

Painel único, de cima para baixo: **O que fazer agora** (prioridades calculadas a cada atualização), **Dinheiro** (Stripe: saldo, receita, assinantes ICAO; gasto em anúncios), **ICAO English Lab** em destaque (funil até a assinatura no Stripe, anúncios, origens com alerta de robô, assinantes recentes), cartões dos outros projetos (o Ciclo Brando fica recolhido no cartão da Brando), atalhos, status dos sites e prospects.

O Stripe é lido com uma chave restrita só de leitura (credencial "Stripe Leitura (Cockpit)" no n8n). A conta tem o Premium ICAO (assinatura) e o Crédito de Sonhos (Dream, avulso); pagamentos de teste do próprio Felipe não contam.

- **Abrir:** `https://brandoguitarras.app.n8n.cloud/webhook/cockpit?key=SUA_CHAVE` (salve nos favoritos / tela inicial do celular).
- A página também funciona aberta direto do arquivo `index.html`: ela pede a chave uma vez e guarda no navegador.

## Como funciona

O workflow **Cockpit — Dados** no n8n tem duas rotas:

| Rota | O que faz |
|---|---|
| `GET /webhook/cockpit` | Serve esta página (baixada deste repositório, branch `main`). |
| `POST /webhook/daily-posts-agora?key=…` | Gatilho extra do workflow **Daily Posts** (o das 8h continua). Dispara o mesmo caminho: texto, e-mail de aprovação e publicação. |
| `POST /webhook/prospect-status?key=…&id=…&status=…` | Muda o status de um prospect (novo, convite, conectado, conversa, descartado). Prospects da Brando com convite/conexão/conversa entram também na aba **Ações** do Scorecard. |
| `POST /webhook/prospects-buscar?key=…` | Roda agora a busca semanal de prospects. |
| `GET /webhook/cockpit-dados?key=…` | Devolve o JSON: GA4 (30 dias + origens/páginas/eventos de 7 dias), status de 5 sites, abas Vendas e Ações do BLM Scorecard e a lista de atalhos. Sem a chave certa, responde vazio. |

Os links dos formulários ficam no nó **Atalhos** do n8n (e não neste repositório público), porque alguns formulários não pedem código.

## Para mudar

- **Adicionar/remover um atalho:** editar o nó *Atalhos* no n8n. Use `href` para abrir um link, ou `action` (caminho de um webhook POST que aceite `?key=`) + `confirm` para um botão que dispara com confirmação. Ícones disponíveis: play, image, story, linkedin, mic, video, upload, wand, flow, chart, sheet, mail.
- **Visual da página:** editar `index.html` e dar push na `main`. A mudança entra no ar na hora.
- **Outro site no GA4:** duplicar os nós GA4 com o ID da propriedade.

## Prospects no LinkedIn

O workflow **Prospects LinkedIn — Busca semanal** roda toda segunda às 10h (horário de Brasília):

1. Faz 14 buscas de perfis públicos (`site:linkedin.com/in …`) no Brave Search: 5 para BLM, 4 para Brando, 5 para ICAO.
2. Pega até 4 perfis novos por busca, sem repetir quem já está na tabela.
3. A IA (Claude) dá nota de 1 a 5 de relevância, explica o motivo e escreve uma nota de convite de até 200 caracteres.
4. Grava na data table `prospects_linkedin`. Nota 1–2 vai como descartado automático.

Nada acessa a sua conta do LinkedIn: você abre o perfil e conecta manualmente. Para mudar as buscas ou o objetivo de cada projeto, edite o nó **Consultas** desse workflow.
