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
index.html                  → landing inteira (CSS 100% inline no <head>, sem CSS externo)
og-image.jpg                → preview redes sociais 1200x630 (símbolo + headline)
.cpanel.yml                 → deploy cPanel (DEPLOYPATH pages/metodo-remiyah/)
.gitignore                  → ignora .DS_Store, .claude/settings.local.json e fontes .jpg/.png/.webp
favicon/                    → favicon.ico, *-96x96.png, apple-touch-icon.png,
                              web-app-manifest-{192,512}.png, site.webmanifest
assets/                     → versionados em .avif; as fontes .jpg/.png ficam locais (gitignored)
  logo-remiyah.avif         → logo (hero topbar)
  logo-remiyah-simbolo.avif → símbolo da marca (base do favicon e da og-image)
  foto-criador.avif         → foto do autor (seção autoridade)
  depo-*.avif               → prints de depoimentos (chef-bruno, lucas, everton,
                              clerinton, gabriela, emocionado, pedro, pablo, camila)
  furadeira.avif            → ilustração/apoio
```
> ✅ Repo git ativo: **github.com/Bravo-Holding/metodo-remiyah** (público, branch
> `master`). favicon/, og-image.jpg, .cpanel.yml e .gitignore já existem.

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

### DADOS DE CAMPANHA (já implementados)
- ✅ **Checkout Hotmart:** `https://pay.hotmart.com/P106391383D?off=2srrhbv4`
  (DIFERENTE do d21d). Está nos 4 CTAs com `target="_blank" rel="noopener"`.
- ✅ **VSL Vimeo (hero):** reutiliza a do d21d — ID `1201278227`, duração `1:31 min`,
  implementada com facade lazy-load (inviolável #7). ⚠️ confirmar se haverá VSL própria.
- ✅ **og:url / og:image — DEFINIDO.** Deploy em
  `ebravoholding.com/pages/metodo-remiyah/`. As meta tags og/twitter e o
  `og-image.jpg` (1200x630) já estão na página. `<title>`/`description` foram
  preenchidos (não são mais placeholder) — revisar a copy se necessário.

## PERFORMANCE — INVIOLÁVEIS
1. **CSS crítico inline.** Todo o CSS vive no `<style>` do `<head>` (já está
   assim). Não extrair pra arquivo externo. Se crescer, split crítico-inline vs
   não-crítico — nunca um `<link>` bloqueante.
2. **Pixel adiado.** ✅ Implementado: o Meta Pixel carrega depois do LCP via
   `requestIdleCallback(loadPixel, {timeout:3000})` com fallback no `window load`
   (`PageView` + `ViewContent {value:397.00, currency:'BRL'}`). **Não** colocar
   `fbq('init')` síncrono no `<head>` (mataria o LCP).
3. **Imagens — AVIF.** Padrão do projeto. ✅ As 13 imagens já foram convertidas pra
   `.avif` e o HTML referencia os `.avif`; as fontes `.jpg`/`.png` ficam locais e
   gitignoradas. Imagem nova: converter antes de subir e ignorar a fonte. Todo
   print/foto entra com `loading="lazy"`. Comando:
   ```
   ffmpeg -y -i ORIGEM -c:v libaom-av1 -still-picture 1 -crf 30 -cpu-used 6 SAIDA.avif
   ```
   **Exceções:** `og-image.jpg` continua JPG (redes sociais não renderizam AVIF no
   preview). Logo pode ficar PNG se precisar de transparência. A imagem do
   hero/LCP, se houver, **não** deve ser lazy.
4. **Fonts.** `preconnect` pra `fonts.googleapis.com`/`fonts.gstatic.com` +
   `display=swap`, sem render-blocking (carregar com `media="print"`+`onload` e
   `<noscript>` de fallback). Não adicionar pesos não usados. ✅ Consolidado: os ~8
   `<link>` duplicados viraram 1 request variável não-bloqueante
   (`Archivo:ital,wght@0,300..900;1,300..900&Inter:wght@300..900`, cobre os pesos
   300–900 usados + o Archivo 900 itálico da citação).
5. **GTM async** no topo do `<head>`; não duplicar nem consolidar com o Pixel.
   ✅ Implementado (`GTM-PCKSDBV4` + noscript).
6. **Mobile-first.** Validar cada seção no celular antes de fechar.
7. **VSL Vimeo lazy-load (facade).** Quando a VSL entrar: o iframe do Vimeo e o
   `player.js` **só** carregam no clique, via `loadVimeo()`. Até lá, facade (play +
   label + duração) — zero request de terceiro no critical path. `preconnect` pra
   `player.vimeo.com`/`f.vimeocdn.com` + `dns-prefetch` `i.vimeocdn.com`. **Não**
   usar iframe direto no HTML (mataria o LCP). `autoplay=1&muted=0` é ok porque o
   clique é gesto do usuário.

## RASTREAMENTO DE VENDAS (UTM + sck) — ✅ IMPLEMENTADO (padrão do d21d)
> ✅ Já está no fim do `<body>` (launcher → origem das vendas → modal).

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

## MODAL DE CAPTURA DE LEAD (antes do checkout) — ✅ IMPLEMENTADO
> ✅ Já está na página. Envia `product: "metodo-remiyah"` no payload (não cai no
> default `desafio-21-dias`). Lead `value:397`. Replicou o padrão do d21d.

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

**JÁ MONTADO:**
1. ✅ `git init` + commits (branch `master`).
2. ✅ Repo GitHub PRÓPRIO público: **github.com/Bravo-Holding/metodo-remiyah**
   (separado das landings irmãs; cPanel clona via HTTPS sem deploy key).
3. ✅ `.cpanel.yml` com `DEPLOYPATH="$HOME/ebravoholding.com/pages/metodo-remiyah/"`
   copiando `index.html` + `og-image.jpg` + `favicon` + `assets/*.avif`.
4. ✅ `.gitignore` (ignora `.DS_Store`, `.claude/settings.local.json` e as fontes
   `.jpg`/`.png`/`.webp` das imagens convertidas pra AVIF).

**FALTA MONTAR:**
5. No cPanel: clonar o repo em `/home1/ewertt82/repositories/metodo-remiyah` e
   configurar o Version Control (passo manual no painel; SSH desligado).

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
- [ ] No cPanel: clonar o repo em `/home1/ewertt82/repositories/metodo-remiyah` e
      configurar o Version Control (deploy). Passo manual no painel.
- [ ] Validar o `sck` real num clique de tráfego pago (risco de truncamento Hotmart).
- [ ] Confirmar a account do launcher Hotmart (hoje reusa a do d21d
      `be7aeb20-…`) e se haverá VSL própria (hoje reusa a do d21d).
- [x] Repo + commit inicial; favicon/, og-image.jpg, .cpanel.yml, .gitignore.
- [x] Converter imagens pra AVIF + referências no HTML.
- [x] Pasta de deploy / og:url definida: `pages/metodo-remiyah/`.
- [x] Checkout Hotmart (`P106391383D`) nos 4 CTAs.
- [x] VSL Vimeo (facade lazy-load, ID `1201278227`).
- [x] Tracking: GTM, Pixel adiado, FB domain, UTM+sck, modal de lead.
- [x] Fontes Google consolidadas (1 request variável, não-bloqueante).
