# Brando Guitar Coach — plano de evolução

Documento de passagem de contexto: qualquer sessão (nuvem ou desktop) deve ler isto antes de continuar.

## Projetos (Lovable, workspace "Felipe's Lovable")
- **Brando Solo Coach** — `e546292a-8ca1-49ce-a865-37dde1b759ed` — https://brandoguitarcoach.app
  TanStack Start (SSR), Supabase, Stripe, análise feita no n8n
  (webhook `guitar-coach-analyze`, retorno em `/api/public/n8n-callback`).
- **Brando Access Hub** — `14d1f0ef-2663-444a-9ecc-9956b14a3e32` — https://brandoguitarras.com.br
  Catálogo de guitarras; tem fotos (`public/guitars`, `public/oficina`), modelos 3D e API pública de catálogo.

## Decisões tomadas
- Personagem rival: **Johnny Bad Boy** — original, sem citar filmes ou músicos reais.
- Vídeos: API do **Runway** via n8n, estilo desenho, em biblioteca fixa (nunca um vídeo por análise).
  Gastar créditos só com aprovação explícita do Felipe a cada lote.
- Android: mesmo caminho do iOS do Dream Stickers (**Codemagic**).
- Mudanças no app vão pelo agente do Lovable; publicar só com aprovação do Felipe.

## Sequência
1. **Pendências (dependem do Felipe no Lovable):** ativar pagamentos reais, definir `N8N_CALLBACK_SECRET`,
   preencher Google OAuth.
2. **Aba "Palco"** — nota lúdica calculada no app (`src/lib/stage-score.ts`), patentes
   (Garagem → Bar da esquina → Abertura de show → Headliner → Lenda da Encruzilhada),
   "Duelo na Encruzilhada" contra o Johnny (nota fixa 90), falas locais com campo opcional
   `result.stage.johnnyLine` para o n8n preencher depois. *Publicado em 10/11/2026.*
3. **Fala do Johnny pela IA no n8n** — *publicado em 10/11/2026.* Fluxo "Brando Guitar Coach — Análise": nós "Nota de Palco" (espelho de `src/lib/stage-score.ts`; mudou lá, mudar aqui) → "Fala do Johnny" (gpt-4.1-mini, até 220 caracteres) → `result.stage.johnnyLine`. Se a IA falhar, o app usa as falas fixas. Falta ver uma fala real numa análise.
4. **Landing com guitarras + integração dos sites** — fotos do Hub na landing do Coach, faixa de catálogo,
   CTA "Analise seu solo grátis" no Hub, créditos para quem compra guitarra. Logins continuam separados.
5. **Johnny em imagem e vídeo** — *publicado em 10/11/2026.* Mídias aprovadas em `brando-guitar-coach/johnny/` (avatares por patente, clipes de reação, shredder sem som, vídeo falado com voz Yuri). Landing ganhou a seção "Duelo na Encruzilhada" com o vídeo falado e o "Setup de bolso" (ToneBridge + adaptador tipo iRig + backing do YouTube). Fluxos n8n: "Johnny Bad Boy — Mídias (Runway)" e "Johnny Bad Boy — Vídeo falado (HeyGen)" (credencial Heygen API). Pendente: Felipe testar o setup de bolso no celular.
6. **Android** — conta Play Console (teste fechado: 12 pessoas por 14 dias em conta pessoal nova),
   política de privacidade e exclusão de conta, ícones; v1 sem venda de créditos no app
   (conferir a regra de faturamento da Play antes); gravação pelo microfone.
