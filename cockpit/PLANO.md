# Plano: visão consolidada dos projetos no Cockpit

Objetivo: um painel único com sensores de navegação, funil e anúncios (Meta e Google) para cada projeto, com indicadores que levam a uma decisão.

## Estado em 06/11/2026 (fim do dia)

| Site | GA4 | Pixel Meta | Google Ads (vinculado ao GA4) | Projeto Lovable (id) |
|---|---|---|---|---|
| icaoenglishlab.com | ✅ G-SXYD6J422H (552215235) | ✅ 1161928832935894 | ✅ 477-634-6878 | Aviation Voice Coach (23a3b870-2b0f-494b-810d-3fad468aea20) |
| brandoguitarras.com.br | ✅ G-SCS97F8BKE (551751519) | ✅ 1039457254007997 | ✅ 477-634-6878 | Brando Access Hub (14d1f0ef-2663-444a-9ecc-9956b14a3e32) |
| brandoguitarcoach.app | ✅ G-Z1LFJGSR2P (562899210) | ✅ 1039457254007997 (mesmo da Brando) | ✅ 477-634-6878 | Brando Solo Coach (e546292a-8ca1-49ce-a865-37dde1b759ed) |
| dreamstickers.app | ✅ G-LMFNWF1FBF (562873753) | ✅ 1299289488995836 | ✅ 477-634-6878 | Dream Stickers (ed7c0f5f-69c2-4479-9573-a7a6d868010e) |
| blmventurecapital.app | ✅ G-4XWLK35725 (562931006) | ✅ 1112989978007783 | ✅ 477-634-6878 | BLM Token Gate (1c120109-1b70-4191-8f78-fedde2a2908f) |

- Conta GA4: "Icao" (accounts/406100109), login italianobrando2026. A credencial "Google Analytics account" do n8n só lê.
- Em cada site, todo evento do funil passa por `src/lib/track.ts`, que dispara GA4 e Pixel no mesmo ponto. Nas trocas de rota, só o Pixel recebe PageView (o GA4 conta pela métrica otimizada).
- Dream Stickers não carrega tags para visitantes da Europa, Reino Unido, Suíça, região desconhecida ou com GPC (não há banner de cookies).
- Credencial "Facebook Graph account" do n8n: token do usuário do sistema **n8n Cockpit** (61596351208606, sem expiração, só leitura), gerado pelo app BrandoStalk (API de Marketing adicionada em 06/11).
- Meta: contas de anúncio 392566346553930 (principal) e 315416931488508 (Brando Guitarras). Páginas com Instagram: ICAO English Lab (@crm_icao) e Brando Guitarras (@brando_guitarras). Os tokens do Instagram (ICAO, Brando, BLM, Dream) seguem renovados pelo workflow "Tokens Instagram — Checagem e Renovação".

## Funil padrão por projeto

| Projeto | Interesse | Lead | Conversão |
|---|---|---|---|
| ICAO | diag_start | diag_submit → generate_lead | begin_checkout → purchase |
| Brando | view_item (página de guitarra) | guitar_request / contact (WhatsApp) | venda registrada no Scorecard |
| Guitar Coach | sign_up | solo_enviado | purchase (créditos) |
| Dream Stickers | sonho_enviado | sign_up | purchase (créditos) |
| BLM | wallet_conectada | projeto_submetido | alocacao |

No Pixel da Meta, os mesmos momentos usam os eventos padrão: `ViewContent` (interesse), `Lead` (lead), `CompleteRegistration` (cadastro), `InitiateCheckout` e `Purchase` (compra).

## Fases

### Fase 1: sensores (navegador + Lovable)
1. [x] **No navegador:** criar 3 propriedades GA4 na conta "Icao" (fuso São Paulo, moeda BRL) com fluxo Web:
   Dream Stickers (dreamstickers.app), Brando Guitar Coach (brandoguitarcoach.app), BLM Venture Capital (blmventurecapital.app). Anotar os G-XXXX.
2. [x] **No navegador:** criar no Gerenciador de Eventos da Meta um conjunto de dados (Pixel) por negócio: ICAO, Brando (site + Coach), Dream Stickers, BLM. Anotar os IDs.
3. [x] **Lovable (um site por vez, começando pelo ICAO):** instalar a tag GA4 que faltar, o Pixel e os eventos do funil acima, disparando GA4 e Pixel no mesmo ponto do código.
4. [x] (Conferido no navegador, site a site.) Rodar o workflow n8n "Cockpit — Diagnóstico de rastreamento (temp)" para conferir as tags. Validar no DebugView do GA4 e no Testar Eventos da Meta.

### Fase 2: conexões com as plataformas de anúncio
1. [x] **No navegador (n8n):** reconectar a credencial do Facebook Graph com `ads_read`, `read_insights`, `pages_read_engagement`, `instagram_basic`, `instagram_manage_insights`, `business_management`. De preferência com um token de Usuário do Sistema do Business Manager, que não expira.
2. [x] **No navegador:** em GA4 → Administrador → Links de produtos → Google Ads, vincular a conta de anúncios a cada propriedade. Assim gasto, cliques e campanha aparecem no GA4.
3. [ ] **No navegador:** dar à credencial do Google no n8n o escopo de edição do Analytics, ou criar uma credencial nova com ele, para não precisar mais do navegador para criar propriedades.

### Fase 3: coleta diária (n8n)
- [ ] Workflow diário que grava na data table `metricas_diarias` (data, projeto, fonte, métrica, valor):
  GA4 de todas as propriedades (sessões, eventos do funil, origem), Meta Insights por campanha (gasto, impressões, cliques, leads), Instagram (alcance, seguidores, visitas ao perfil) e GA4 de Google Ads (`advertiserAdCost`, `advertiserAdClicks`).

### Fase 4: cockpit
- [x] Seção "Projetos" com sinal, sensores, sessões e funil (Brando e ICAO pelo GA4).
- [x] Gasto e custo por lead por projeto e por campanha, Meta e Google lado a lado (ao vivo, 7 dias vs 7 anteriores). Campanhas da Meta entram no projeto pelo nome: incluir ICAO, Brando, Coach, Dream ou BLM no nome da campanha.
- [ ] Instagram por conta.
- [ ] Comparação semana contra semana a partir de `metricas_diarias`.

### Fase 5: regras de decisão e alertas
- [x] (Ativo em 7 dias, a partir de 13/11.) Campanha com gasto acima de R$ 50 e 0 leads em 3 dias → "pausar ou trocar criativo".
- [ ] Custo por lead acima da meta do projeto por 3 dias → alerta.
- [ ] Conversão visita → interesse abaixo de 1% com mais de 200 visitas → revisar origem e chamada principal (já ativo no cockpit).
- [ ] Etapa do funil caindo mais de 50% contra a semana anterior → alerta.
- [ ] Site fora do ar → alerta imediato.

## Workflows envolvidos (n8n)
- Cockpit — Dados (PRQMdbOeZ5D3U4ho): página e JSON do cockpit.
- Cockpit — Diagnóstico de rastreamento (temp) (cvgXNXKH4iWmVS3l): confere tags, GA4 e Meta.
- Prospects LinkedIn — Busca semanal (09ey0F2FpjgzlJln).
