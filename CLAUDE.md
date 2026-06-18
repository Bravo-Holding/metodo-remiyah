# CLAUDE.md — Landing: Método Remiyah (metodo-remiyah)

> **Isolamento.** Este projeto é INDEPENDENTE das landings irmãs em
> `~/Dev/sites/d21d` (Desafio 21 Dias) e
> `~/Dev/sites/desafio-da-procrastinacao-a-promessa`. **Produto diferente, copy
> diferente, arquivos/repo/branch/DEPLOYPATH separados.** O CLAUDE.md aqui foi
> derivado do d21d porque o *padrão técnico* (Pixel adiado, modal de lead, UTM+sck,
> AVIF, deploy cPanel) é o mesmo — mas **nunca** copie copy, seções, depoimentos
> ou commits entre os projetos.

## O QUE É ESTE PROJETO
Landing page de venda (estática, single page, sem build) do produto **Método
Remiyah**, do Ewertton Bravo. Produto de propósito/direção de vida com base
bíblica (jornada Gênesis → Êxodo → Terra Prometida; tema "fazer o que Deus colocou
no coração"). Página única, todo o HTML + CSS num só arquivo.

Oferta atual: de **R$ 697 por R$ 397** (em até 12x de R$ 33,08, "para os 25
primeiros"). Valor total empilhado R$ 4.635. Garantia 30 dias. Acesso 3 anos.

## ESTRUTURA
```
vendas-metodo-remiyah.html  → landing inteira (CSS 100% inline no <head>, sem CSS externo)
assets/
  logo-remiyah.png          → logo (hero topbar)
  logo-remiyah-simbolo.png  → símbolo da marca
  foto-criador.jpg          → foto do autor (seção autoridade)
  depo-*.jpg                → prints de depoimentos (chef-bruno, lucas, everton,
                              clerinton, gabriela, emocionado, pedro, pablo, camila)
  furadeira.png             → ilustração/apoio
```
> ⚠️ **Sem favicon/, sem og-image, sem .cpanel.yml, sem .gitignore ainda.** Esta
> pasta ainda NÃO é repo git. Ver "FALTA MONTAR".

## SEÇÕES (ordem no HTML — classes `secao-*`)
hero · dor · paliativo · prova-social-1 · cta-intermediario · metodo · para-quem ·
entregaveis · bonus · stack-valor · prova-social-2 · suporte · garantia ·
autoridade · faq · oferta-final

## DADOS FIXOS — NÃO ALTERAR
- **Meta Pixel:** `25723610987317406` (compartilhado com o d21d; decisão do dono —
  não trocar). Os eventos seguem o padrão: `PageView`, `ViewContent`
  (`value: 397.00, currency: 'BRL'`), `Lead` (`value: 397, currency: 'BRL'`).
- **Google Tag Manager:** `GTM-PCKSDBV4` (mesmo container; é de domínio/conta, não
  de produto — reusar). Vai no topo do `<head>`.
- **Facebook domain verification:** `x3mdskvask209m6cuxpnd6964lbzp1` (é do domínio
  `ebravoholding.com`, então é o MESMO de todas as landings).
- **Paleta:** fundo `#0A0A0A` · texto `#FAFAFA` (sec. `rgba(250,250,250,0.62)`) ·
  acento laranja `#FF6A00`. (Identidade própria — NÃO usar o dourado do d21d.)
- **theme-color:** `#FF6A00`
- **Tipografia:** Archivo (display/títulos, 700–900) + Inter (corpo). NÃO trocar
  pelas do d21d (Plus Jakarta).
- **Autor:** Ewertton Bravo ("Criador do Método Remiyah").

### A DEFINIR (preencher antes de ir ao ar — hoje são placeholder/TODO)
- **Checkout Hotmart:** os CTAs ainda apontam pra `#oferta` / `#checkout` / `#`.
  O link real é DIFERENTE do d21d (`W105539649V`). Assim que o dono informar o
  link do Método Remiyah, substituir em TODOS os botões.
- **VSL Vimeo (hero):** vai ter VSL, mas o **ID do vídeo e a duração ainda não
  foram informados**. Quando vierem, implementar com o lazy-load facade (ver
  inviolável #7).
- **og:url / og:image:** deploy esperado em `ebravoholding.com/pages/...` —
  **pasta exata a definir** (sugestão: `metodo-remiyah/`, não é um "desafio").
  Criar `og-image.jpg` (1200x630).

## PERFORMANCE — INVIOLÁVEIS
1. **CSS crítico inline.** Todo o CSS vive no `<style>` do `<head>` (já está
   assim). Não extrair pra arquivo externo. Se crescer, split crítico-inline vs
   não-crítico — nunca um `<link>` bloqueante.
2. **Pixel adiado.** Quando for adicionado, o Meta Pixel só carrega depois do LCP,
   via `requestIdleCallback(loadPixel, {timeout:3000})` com fallback no
   `window load`. **Não** colocar `fbq('init')` síncrono no `<head>` (mataria o
   LCP). ⚠️ Hoje o Pixel ainda NÃO está na página.
3. **Imagens — AVIF.** Padrão do projeto. ⚠️ **As imagens atuais estão em `.jpg`/
   `.png` — converter pra `.avif` antes de subir** (fonte fica local e
   gitignorada). Todo print/foto entra com `loading="lazy"`. Comando:
   ```
   ffmpeg -y -i ORIGEM -c:v libaom-av1 -still-picture 1 -crf 30 -cpu-used 6 SAIDA.avif
   ```
   **Exceções:** `og-image.jpg` continua JPG (redes sociais não renderizam AVIF no
   preview). Logo pode ficar PNG se precisar de transparência. A imagem do
   hero/LCP, se houver, **não** deve ser lazy.
4. **Fonts.** `preconnect` pra `fonts.googleapis.com`/`fonts.gstatic.com` +
   `display=swap`, sem render-blocking (carregar com `media="print"`+`onload` e
   `<noscript>` de fallback). Não adicionar pesos não usados. ⚠️ **Hoje há ~7
   `<link>` de Google Fonts duplicados e render-blocking no `<head>` — consolidar
   num só (Archivo 700/800/900 + Inter 400/500/600/700) e tirar do critical path.**
5. **GTM async** no topo do `<head>`; não duplicar nem consolidar com o Pixel.
6. **Mobile-first.** Validar cada seção no celular antes de fechar.
7. **VSL Vimeo lazy-load (facade).** Quando a VSL entrar: o iframe do Vimeo e o
   `player.js` **só** carregam no clique, via `loadVimeo()`. Até lá, facade (play +
   label + duração) — zero request de terceiro no critical path. `preconnect` pra
   `player.vimeo.com`/`f.vimeocdn.com` + `dns-prefetch` `i.vimeocdn.com`. **Não**
   usar iframe direto no HTML (mataria o LCP). `autoplay=1&muted=0` é ok porque o
   clique é gesto do usuário.

## RASTREAMENTO DE VENDAS (UTM + sck) — A ADICIONAR (padrão obrigatório)
> ⚠️ Ainda NÃO está na página. Replicar o padrão do d21d.

Script "Origem das Vendas" no fim do `<body>` (após o launcher Hotmart). No load,
reescreve o `href` de todos os `<a>` injetando
`utm_source/medium/campaign/content/term` (fallback: referrer limpo → `Direto` /
`Organico` / path → `Home`); **só** em links `pay.hotmart`/`pay.kiwify` anexa
`&sck=<source-medium-campaign-content-term>`. O `sck` é o que a Hotmart mostra como
**origem da venda**.
- Pula links com `#` (âncoras) e os que já têm UTM.
- **Não** mover pro `<head>` (precisa do DOM montado). Não consolidar com GTM/Pixel.
- **Risco conhecido:** a Hotmart trunca `sck` (~limite de caracteres). Validar o
  `sck` real num clique de tráfego pago antes de escalar.

## MODAL DE CAPTURA DE LEAD (antes do checkout) — A ADICIONAR
> ⚠️ Ainda NÃO está na página. É o item que o dono pediu pra priorizar: **form
> antes do botão de checkout**. Replicar o padrão do d21d.

Markup + JS no fim do `<body>`, **depois** da origem das vendas (precisa do href já
decorado com UTM+sck). CSS inline no `<style>` (modal nasce `display:none`, zero
render até o clique — respeita o inviolável #1).
Fluxo: listener delegado no `document` casa `a[href*="pay.hotmart"]` via `closest()`
→ `preventDefault` → abre modal (nome/email/whatsapp obrigatórios; submit `disabled`
até nome 2+ palavras, e-mail por regex e WhatsApp 10–11 dígitos). No submit:
- **Pixel:** `fbq('init', PIXEL_ID, {em,ph,fn,ln})` (Advanced Matching, valores
  crus) + `fbq('track','Lead',{value:397,currency:'BRL'},{eventID})`.
- **CAPI + Supabase:** `fetch(EDGE_URL, {keepalive:true})` pra Edge Function
  `capture-lead` no projeto **`desafios-app`** (`iodseuinjheacuoptaau`),
  `https://iodseuinjheacuoptaau.supabase.co/functions/v1/capture-lead`. A função
  grava na tabela `leads` (service_role, RLS sem policy pública) **e** chama a Meta
  CAPI com o **mesmo** `eventID` (dedup).
- Redireciona pro `checkoutHref` guardado (UTM+sck preservados). Prefill Hotmart
  desligado por privacidade.

**Backend é COMPARTILHADO** com as landings irmãs — tabela/RLS/Edge/CAPI já existem
e foram testados. Não recriar. Os leads do Método Remiyah se distinguem por
`page_path` (o caminho de deploy desta landing). ⚠️ **Atenção:** como reusa o
mesmo Supabase do d21d, o campo `product` cai no default `desafio-21-dias` se não
for sobrescrito — **enviar `product: "metodo-remiyah"` explicitamente no payload**
pra não misturar com as vendas do outro produto no relatório.
**Depende de secrets na Edge Function:** `META_CAPI_TOKEN` + `META_PIXEL_ID` (e
`META_TEST_EVENT_CODE` opcional). CORS da função já libera `ebravoholding.com`.

## CHECKOUT — invariável
Todos os CTAs apontam pro MESMO link Hotmart (a definir, ver "DADOS FIXOS"),
`target="_blank" rel="noopener"`. O launcher Hotmart
(`launcher.hotmart.com/launcher.js`, id `hot`) fica logo antes do script de origem
das vendas. Ordem fixa: launcher → origem das vendas → modal de lead. Os CTAs
abrem o modal de captura antes de seguir pro checkout.

## DEPLOY (HostGator / cPanel Git — mesma conta das landings irmãs)
Site estático, sem build. Publicação via **cPanel Git Version Control** (cPanel
puxa do GitHub). **SSH/shell da conta está DESLIGADO** — deploy é sempre via *pull*
do cPanel.

**Conta cPanel/HostGator (compartilhada):** usuário `ewertt82`, servidor
`br238.hostgator.com.br` (cPanel :2083, FTP/SSH :2222), home `/home1/ewertt82`.
Domínio `ebravoholding.com` → `/home1/ewertt82/ebravoholding.com`.

**FALTA MONTAR (esta pasta ainda não está pronta pra deploy):**
1. **`git init`** + primeiro commit (a pasta ainda não é repo).
2. **Repo GitHub PRÓPRIO** (novo, separado das landings irmãs). Público, pro cPanel
   clonar via HTTPS sem deploy key.
3. **`.cpanel.yml`** com `DEPLOYPATH` (pasta de deploy a definir, ex.:
   `pages/metodo-remiyah/`) copiando `vendas-metodo-remiyah.html` (ou renomear pra
   `index.html`) + `og-image.jpg` + `favicon` + `assets/**/*.avif`.
4. **`.gitignore`** (ignorar internos e as fontes `.jpg`/`.png`/`.webp` das imagens
   convertidas pra AVIF).
5. No cPanel: clonar o repo em `/home1/ewertt82/repositories/<nome>` e configurar
   o Version Control.

**Fluxo de update (depois de montado):** editar local → `git push` → no cPanel:
"Update from Remote" + "Deploy HEAD Commit".

**Limitação:** o cPanel só faz deploy do HEAD de uma branch (não puxa tag pela
interface). A/B = uma branch por variante, cada uma com `DEPLOYPATH` próprio.

## REGRAS DE EDIÇÃO
1. **Não redesenhar** sem pedido. Preservar estrutura, CSS inline e componentes.
2. **Não misturar com as landings irmãs** (copy, seções, commits, repo, paleta).
3. **Mobile-first**; validar no celular.
4. **Sem traço "-"** em texto público (usar ponto, vírgula ou quebra de linha).
5. **Versículos sempre na ARC.**
6. **Depoimentos são reais** (prints em `assets/depo-*`): texto VERBATIM, nunca
   corrigir, normalizar nem inventar nome/idade/cidade/dia.
7. Ao tocar em performance, conferir os 7 invioláveis acima antes de fechar.

## PENDÊNCIAS ABERTAS (resumo)
- [ ] Link de checkout Hotmart do Método Remiyah.
- [ ] ID + duração da VSL Vimeo do hero.
- [ ] Pasta de deploy / og:url (sugestão `pages/metodo-remiyah/`).
- [ ] Adicionar a camada de tracking: GTM, Pixel adiado, UTM+sck, modal de lead
      (enviando `product: "metodo-remiyah"`).
- [ ] Converter imagens pra AVIF; consolidar/desbloquear as fontes.
- [ ] Criar favicon/, og-image.jpg, .cpanel.yml, .gitignore; `git init` + repo.
