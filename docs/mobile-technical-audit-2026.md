# Kwispelbox / Hondenknuffels.com — Technische Mobile Audit 2026

_Opgesteld: 30-09-2026 · Read-only audit tegen Shopify 2026 best practices · **Er is NIETS gewijzigd aan de theme.**_
_Theme: KB-v4 (repo `POASIFY/kwispelbox-v4`, branch `main`), gekoppeld aan zowel de dev- als de echte store._
_Methode: statische code-analyse van de daadwerkelijke theme-bestanden (4 read-only verkenners + directe verificatie van kritieke regels). Runtime-metingen (Lighthouse/veldwaarden) zijn NIET gedraaid — alles wat runtime vereist is expliciet gemarkeerd **UNKNOWN — HANDMATIGE TEST NODIG**._

---

## Managementsamenvatting

De theme is **technisch degelijk en mobile-first gebouwd**. De cart/GWP-architectuur is opvallend robuust (één autoritatieve cart-bron, idempotente GWP met lock, race-guards, drawer/cart-pariteit, gedelegeerde listeners). Responsive images, preload-strategie voor hero/PDP, per-sectie CSS en volledige JSON-LD zijn sterk.

**Geen harde code-blockers gevonden.** De echte P0's zijn vooral **verificatie-taken** (consent in Admin) en een paar **Markets-hardenings** die pas breken zodra België/een tweede markt/taal aangaat. Voor een NL-only launch achter wachtwoord is de shop dicht bij klaar; onderstaande P0/P1 punten eerst afvinken.

**Top-prioriteiten:** (1) consent-flow verifiëren in Admin [P0], (2) `viewport-fit=cover` ontbreekt terwijl safe-area wél gebruikt wordt [P1], (3) font-preload zonder `crossorigin` [P1], (4) collectie-afbeeldingen lazy+zonder srcset boven de vouw [P1], (5) hardcoded routes in cart-drawer/discount vóór Markets [P1].

---

## Hardening-batch uitgevoerd (30-09-2026) — RESOLVED (code)

De volgende auditpunten zijn in code opgelost (4 commits H1–H4). **NB:** dit zijn code-fixes; runtime-CWV (LCP/INP/CLS) blijven **onbewezen tot echte Lighthouse/veldmeting**.

| Auditpunt | Fix | Bestand(en) | Status |
|---|---|---|---|
| **N1** viewport-fit=cover | `,viewport-fit=cover` toegevoegd | `snippets/meta-tags.liquid` | ✅ RESOLVED (H1) |
| **K2** font-preload crossorigin | `type=font/woff2` + `crossorigin=anonymous` | `layout/theme.liquid` | ✅ RESOLVED (H1) |
| **A3** collectie-afbeeldingen LCP/srcset | eerste kaart eager+fetchpriority high; srcset 200/300/400/600 + fluide sizes | `sections/collection.liquid` | ✅ RESOLVED (H2) |
| **A4** vaste image `sizes` | `280px` → fluid (72vw/30vw/235px) — **in `happy-dogs.liquid`**, niet moments (moments = SVG-stickers, geen raster) | `sections/happy-dogs.liquid` | ✅ RESOLVED (H2) |
| **F2** touch targets ≥44px | drawer-qty 30→44, upsell+ 38→44, hamburger 40→44, PDP-qty (mobiel) 44, cart-steppers (mobiel) 44, extras-"+" 40→44 | `cart-drawer.liquid`, `header.liquid`, `main-product-kwispelbox.liquid`, `cart.liquid`, `product-extras.liquid` | ✅ RESOLVED (H3) |
| **N3** inputs ≥16px (iOS zoom) | `@media ≤749px` input/select/textarea 16px | `assets/critical.css` | ✅ RESOLVED (H3) |
| **P1** cart property escaping | `{{ prop.first\|escape }}` + `{{ prop.last\|escape }}` | `sections/cart.liquid` | ✅ RESOLVED (H4) |
| **Q1** hardcoded routes | drawer-checkout → `routes.checkout_url`; discount-redirect → `CART_ROOT`-based. (`a[href$="/cart"]` = selector, bewust ongewijzigd) | `cart-drawer.liquid`, `cart.liquid` | ✅ RESOLVED (H4) |

**Blijft OPEN (niet in deze codebatch):** H1 consent-Admin-verificatie · M1 Judge.me/loyalty consent · echte Lighthouse/CWV-metingen (A/K/L) · iPhone/notch device-test (N1) · Markets-specifieke delivery/cutoff/threshold vóór BE (G2/G3) · P2-restjes (O1 editor re-init, B5 cart-count centraliseren, F3 contrast-check, `loy_*.js` opruimen).

---

## A. PERFORMANCE / CORE WEB VITALS

### A1 — Homepage hero (LCP) — **PASS**
- **Ernst:** — · **Bestand:** `sections/hero.liquid:10-29, 97-109`
- **Huidige situatie:** Preload-link met `rel=preload as=image fetchpriority=high` + `imagesrcset`/`imagesizes` voor mobiel+desktop. `<img>` conditioneel `loading="eager" fetchpriority="high"` als `image_priority` aanstaat. srcset 800/1200/1600w, `sizes="(max-width:989px) 100vw, 54vw"`, intrinsieke `width=1644 height=957`.
- **Waarom:** Dit is exact de Shopify-2026 aanbeveling voor LCP-beelden.
- **Impact:** LCP ✅ · CLS ✅ (dimensies gezet).
- **Aanbevolen fix:** Geen. Verifieer wel dat `image_priority` op de live homepage aanstaat (setting).
- **Vóór launch:** n.v.t. (PASS) — wel even setting checken.

### A2 — PDP eerste galerij-afbeelding (LCP) — **PASS**
- **Bestand:** `sections/main-product-kwispelbox.liquid:44-47, 66-70`
- **Huidige situatie:** Preload matcht de eerste `<img>` (`loading="eager" fetchpriority="high" decoding="async"`, srcset 700/1200w, `sizes="(max-width:989px) 92vw, 46vw"`, `width=600 height=600`). Thumbnails lazy.
- **Impact:** LCP ✅ · geen dubbele fetch (preload==img).
- **Vóór launch:** n.v.t. (PASS).

### A3 — Collectie-pagina eerste kaart (LCP/overfetch) — **FAIL/WARN**
- **Ernst:** **P1** · **Bestand:** `sections/collection.liquid:200-210` (img ~r204)
- **Huidige situatie:** Alle kaart-afbeeldingen `loading="lazy"` (óók de eerste, boven de vouw) en **geen `srcset`/`sizes`** — alleen `image_url: width:400`. `width/height` wél aanwezig.
- **Waarom:** Boven-de-vouw lazy vertraagt LCP; vaste 400px overfetcht op smal mobiel en onderfetcht op desktop; ontbrekende `sizes` = geen responsive selectie.
- **Impact:** LCP ⚠️ (collectie-template) · UX ⚠️ · overfetch mobiel.
- **Aanbevolen fix:** Eerste kaart `loading:'eager' fetchpriority:'high'`, overige lazy; `widths:'200,300,400'` + `sizes:'(max-width:749px) 50vw, 300px'`.
- **Risico fix:** Laag. · **Complexiteit:** laag. · **Vóór launch:** aanbevolen (collectie = belangrijk instappunt vanuit menu "Boxen").

### A4 — Moment-tiles vaste `sizes:'280px'` — **WARN**
- **Ernst:** P2 · **Bestand:** `sections/moments.liquid:~35`
- **Huidige situatie:** `image_tag widths:'250,350,500' sizes:'280px'` (vast) terwijl grid 3-koloms is op desktop.
- **Impact:** Lichte overfetch mobiel / underfetch desktop.
- **Fix:** fluid `sizes` per breakpoint. · **Complexiteit:** laag. · **Vóór launch:** nee (P2).

### A5 — Render-blocking JS in `<head>` — **PASS**
- **Bestand:** `layout/theme.liquid:8-12`
- **Huidige situatie:** Enige inline script checkt `prefers-reduced-motion` + `IntersectionObserver` en zet `reveal-on`-class; staat ná de stylesheets, niet blokkerend. Geen eigen render-blocking JS. `content_for_header` (Shopify) buiten scope.
- **Impact:** LCP ✅. · **Vóór launch:** n.v.t. (PASS).

---

## B. JAVASCRIPT / AJAX / CART-ARCHITECTUUR

### B1 — Eén autoritatieve cart-state — **PASS (P0-gebied)**
- **Bestanden:** `sections/cart.liquid:400-493`, `snippets/cart-drawer.liquid:165-375`, `snippets/gwp-sync.liquid`
- **Huidige situatie:** Alle mutaties gaan via de Shopify Cart API (`/cart.js`, `/cart/add.js`, `/cart/change.js`). Cart-pagina gebruikt Section Rendering API (POST met `sections`, swap `.kbcart__inner`). Drawer rendert client-side uit `/cart.js`. GWP centraal via `window.kbGwp`.
- **Impact:** cart-reliability ✅. · **Vóór launch:** n.v.t. (PASS).

### B2 — GWP idempotentie & qualifying spend — **PASS (P0-gebied)**
- **Bestand:** `snippets/gwp-sync.liquid:36-82`
- **Huidige situatie:** `api.running`-lock; hoogstens één add/change per call; gift **niet** meegerekend in qualifying total (r41-45); threshold uit `settings.kb_gift_threshold` (fallback 89); `needsAdd/needsRemove/needsFix` sluiten elkaar uit → geen oneindige loop; `kb:cart:updated` event na sync.
- **Impact:** cart-reliability ✅ · geen gift-duplicaat.
- **Vóór launch:** n.v.t. (PASS).

### B3 — Race conditions / in-flight guards — **PASS**
- **Ernst:** P1 · **Bestanden:** `cart-drawer.liquid` (`busy`-flag r185/332/346/356), `cart.liquid` (`busy` r414/454, `aria-busy`), `gwp-sync.liquid` (`running`)
- **Huidige situatie:** Elke mutatie heeft een guard; error-recovery reset de flag in `catch`. Drawer- en cart-page-`busy` zijn onafhankelijk, maar zijn nooit tegelijk actief (aparte contexten) en delen dezelfde GWP-lock.
- **Impact:** INP ✅ · cart-reliability ✅. · **Vóór launch:** nee (al goed).

### B4 — Error handling / loading states — **PASS**
- **Ernst:** P2 · **Bestanden:** `cart.liquid:466-468`, `cart-drawer.liquid:338/350/365`, `product-extras.liquid:127`, `gwp-sync.liquid:77`
- **Huidige situatie:** `catch` overal, met fallback naar normale form-submit en foutmelding "Er ging iets mis"; loading-states via `aria-busy`/`is-busy`/`is-added`.
- **Impact:** UX ✅. · **Vóór launch:** nee.

### B5 — Cart-counter update op 3 plekken — **WARN**
- **Ernst:** P2 · **Bestanden:** `cart-drawer.liquid:199`, `product-extras.liquid:125`, `cart.liquid` (via section re-render)
- **Huidige situatie:** Drie onafhankelijke plekken updaten `.site-header__cart-count` / `.header-drawer__cart-count`. Alle drie lezen uit `cart.js` → geen echte desync, wel drie code-paden.
- **Impact:** UX ⚠️ (klein) · onderhoud.
- **Fix:** optioneel centraliseren in één `updateCartCount(count)`-helper. · **Complexiteit:** laag. · **Vóór launch:** nee.

### B6 — Line-item properties behouden — **PASS**
- **Bestanden:** `cart-drawer.liquid:223-227`, `cart.liquid` (properties uit `item.properties`)
- **Huidige situatie:** Properties (`properties[Naam op bandana]` etc.) blijven behouden bij qty-change/render (Cart API + section re-render). · **Vóór launch:** n.v.t. (PASS).

---

## C. CART DRAWER — LAZY RENDER / DOM-EFFICIËNTIE

### C1 — Drawer altijd in DOM, upsell server-side — **PASS / acceptable**
- **Ernst:** P2 · **Bestanden:** `layout/theme.liquid:28` (`render 'cart-drawer'` globaal), `snippets/cart-drawer.liquid:44-64, 85-159`
- **Huidige situatie:** Snippet wordt op **elke** pagina gerenderd; verborgen via `.kbcd{display:none}` tot `is-open`. Upsell (max 4 uit `cadeau-extras`) wordt server-side voorgebouwd (~2KB extra HTML). Item-HTML wordt runtime opgebouwd uit `/cart.js`.
- **Waarom:** Kleine, statische extra markup; drawer alleen zichtbaar ≤749px. Geen zware DOM-kosten.
- **Oordeel:** **acceptable** — geen performance-risk, geen over-engineering. Section Rendering/lazy-dialog zou hier nauwelijks winst geven.
- **Impact:** mobile-perf ✅ (verwaarloosbaar). · **Vóór launch:** nee.

---

## D. SECTION RENDERING API

### D1 — Beoordeling — **PASS (correct ingezet)**
- **Bestand:** `cart.liquid:396-493`
- **Bevinding:** Cart-**pagina** gebruikt SRA correct (server-truth voor thresholds/GWP/upsell-excludes). **Drawer** rendert bewust client-side (sneller voor kleine updates). 
- **Kandidaten met marginale winst:** header cart-count (nu 3× JS-update) zou via een mini-section kunnen — maar huidige client-render is robuust genoeg. GWP-status: al optimaal client-side.
- **Advies:** **Niet migreren.** Huidige mix is passend. · **Vóór launch:** nee.

---

## E. MOBILE / DESKTOP DUPLICATE DOM

### E1 — Beoordeling — **PASS**
- **Bestanden:** `header.liquid` (één DOM, hamburger + desktop-nav via CSS), `cart-drawer.liquid` (`display:none` >749px), `mobile-tabbar.liquid` (`display:none` >749px), `cart.liquid` (responsive grid).
- **Bevinding:** Geen grove dubbele markup (geen twee volledige nav-drawers). `site-header__cart-count` + `header-drawer__cart-count` bestaan beide bewust (top-bar vs nav-drawer). Lichte upsell-duplicatie tussen `cart.liquid` en `cart-drawer.liquid` (zelfde 4 producten) — verwaarloosbaar.
- **Impact:** DOM ✅. · **Vóór launch:** nee.

---

## F. ACCESSIBILITY

### F1 — Focus/keyboard/dialogs — **PASS**
- **Bestanden:** `focus-trap.liquid:9-35`, `cart-drawer.liquid:21/283/290/303/312-322`, `main-product-kwispelbox.liquid:792`, `header.liquid:66/194/98`
- **Huidige situatie:** `role="dialog" aria-modal="true"`, focus-trap met focus-return, ESC sluit (drawer + PDP-lightbox), scroll-lock (`overflow:hidden`), geneste dialogs afgehandeld (nav-drawer sluit vóór cart-drawer opent), `aria-expanded`/`aria-controls`/`aria-hidden` correct, `focus-visible` outlines. Predictive search keyboard-navigeerbaar (Arrow/Enter).
- **Impact:** a11y ✅. · **Vóór launch:** n.v.t. (PASS).

### F2 — Touch targets < 44px — **WARN**
- **Ernst:** P2 · **Bestanden:** cart-drawer qty-knoppen **30×30px** (`cart-drawer.liquid:~122`); PDP qty ~32px hoog (`main-product-kwispelbox.liquid:~521`); hamburger 40×40 (`header.liquid:~1380`); header cart-pill 44×44 (borderline).
- **Waarom:** WCAG 2.5.5 / mobiel comfort ≥44px (liever 48px).
- **Impact:** a11y/UX ⚠️ (mis-taps op qty).
- **Fix:** qty-knoppen → 44×44 (of padding/hit-area vergroten). · **Complexiteit:** laag. · **Vóór launch:** nee (P2), wel aanbevolen.

### F3 — Contrast (merk-roze) — **UNKNOWN**
- **Ernst:** P2 · **Bestand:** `css-variables.liquid` (`--color-pink`)
- **Bevinding:** Licht-roze op wit/crème mogelijk onder AA (4.5:1). **UNKNOWN — HANDMATIGE TEST NODIG** (contrast-checker op echte kleurwaarden). Merkregel schrijft al voor: witte tekst alleen op donkere varianten `#4d8833`/`#e75480` — dus waarschijnlijk oké mits die regel overal is gevolgd.
- **Vóór launch:** verifiëren (snel).

---

## G. SHOPIFY MARKETS / LOCALIZATION

### G1 — Localization-form aanwezig — **PASS**
- **Bestand:** `sections/header.liquid:874-890`
- **Huidige situatie:** `{% form 'localization' %}` in de mobiele drawer, land-selectie alleen bij `available_countries.size > 1`, auto-submit. Geen hardcoded landen. Geen hardcoded `/nl` `/en` URLs; routing via `routes.*`.
- **Impact:** Markets ✅ (basis). 
- **Let op / UNKNOWN:** Taal-selectie (`available_languages`) — verifieer of naast land ook taal schakelbaar is indien NL+FR (BE) gewenst. **HANDMATIGE CHECK** in Admin → Markets.
- **Vóór launch:** n.v.t. voor NL-only; verifiëren vóór BE.

### G2 — Hardcoded €-symbool + `nl-NL` formattering — **WARN**
- **Ernst:** P1 (bij niet-EUR markt) / P2 (BE=EUR) · **Bestanden:** `main-product-kwispelbox.liquid:266, 809-811` (`fmtEuro()` met `'€'` + `nl-NL`), `seo-meta.liquid:16`, `trust-bar.liquid:72`, `box-compare.liquid:177-179`
- **Huidige situatie:** Beloftes/claims tonen `€` letterlijk i.p.v. via `| money`; JS-prijsformattering hardcodeert `nl-NL`.
- **Waarom:** Bij een niet-EUR markt breekt valuta-weergave/-formattering. Voor **BE (EUR, komma-decimaal)** is de impact **laag** (zelfde valuta+format als NL).
- **Impact:** Markets ⚠️ · SEO-claim (meta) ⚠️.
- **Fix:** `| money`/`| money_with_currency` gebruiken; JS-locale uit `request.locale.iso_code`. · **Complexiteit:** middel. · **Vóór launch:** nee voor NL/BE-EUR; ja vóór niet-EUR markt.

### G3 — Beloftes niet per-markt (delivery/cutoff/threshold/free-shipping) — **WARN**
- **Ernst:** P1 (vóór BE) · **Bestanden:** `config/settings_schema.json` (`kb_free_shipping:50`, `kb_order_cutoff:16:00`, `kb_delivery:"morgen in huis"`, `kb_gift_threshold:89`), refs in `main-product-kwispelbox.liquid:265-266`, `cart.liquid:8-9/82-105`, `product-faq.liquid:130-138`
- **Huidige situatie:** Centrale settings (goed!) maar **globaal**, niet per markt.
- **Waarom:** "morgen in huis"/€50-drempel/16:00-cutoff kloppen niet automatisch voor BE (ander netwerk/price-point).
- **Impact:** Markets ⚠️ · claim-juistheid.
- **Fix:** markt-conditionele settings of tekst-varianten vóór BE-activatie. · **Complexiteit:** middel. · **Vóór launch:** nee voor NL-only; ja vóór BE.

---

## H. CONSENT / COOKIE / PRIVACY

### H1 — Geen custom banner → delegatie aan Shopify native — **UNKNOWN (P0-verificatie)**
- **Ernst:** **P0 (verificatie, geen code)** · **Bestanden:** `layout/theme.liquid:15` (`content_for_header`), `footer.liquid:137` (alleen een "Cookies"-link), `templates/page.cookiebeleid` (informatief)
- **Huidige situatie:** De theme bevat **geen** eigen cookie-banner en **geen** eigen pixel-scripts (geen `gtag`/`gtm`/`fbq`/`dataLayer` in de repo). Consent + tracking hangen dus volledig aan **Shopify's native Customer Privacy** (banner + Custom Pixels + Customer Privacy API via `content_for_header`).
- **Waarom dit géén theme-FAIL is:** Voor Shopify 2026 is native consent (Settings → Customer privacy → Cookie banner + Consent-mode) de aanbevolen route; een eigen banner is niet vereist en vaak juist risicovoller.
- **Waarom dit tóch P0 is:** Of de native banner voor NL/EU **aanstaat** en of alle pixels/apps consent-gated zijn, is **niet uit de code te verifiëren**.
- **UNKNOWN — HANDMATIGE TEST NODIG:**
  1. Admin → Instellingen → Klantprivacy: cookiebanner **aan** voor EU/NL? Regio's ingesteld?
  2. Incognito, geen consent → DevTools Network: vuren GA/Meta/andere pixels **niet** vóór toestemming?
  3. Na "Accepteren": worden ze wél actief? Voorkeuren wijzigbaar + persistent?
  4. Judge.me: GDPR-/consent-mode aan in de app-instellingen?
- **Vóór launch:** **JA** — juridisch blokkerend tot geverifieerd.

### H2 — `loy_77036486821.js` (loyalty) — **WARN / UNKNOWN**
- **Ernst:** P2 · **Bestand:** `assets/loy_77036486821.js` (zet enkel `ba_msg_active` timestamp in localStorage)
- **Huidige situatie:** Dit asset wordt **nergens in de theme gerefereerd** (dood/orphan bestand). Als het toch laadt, gebeurt dat via een **app-embed** (Admin), niet via de theme.
- **Impact:** privacy ⚠️ (localStorage zonder consent?) — **UNKNOWN, Admin-check**: welke loyalty-app, en laadt die vóór consent?
- **Fix:** identificeren; indien echt ongebruikt → los asset opruimen (housekeeping). · **Vóór launch:** verifiëren (privacy).

---

## I. SEARCH

### I1 — Predictive + full-page search — **PASS**
- **Bestanden:** `header.liquid:2237+` (predictive), `sections/search.liquid` (SSR fallback)
- **Huidige situatie:** Predictive search met keyboard-nav (Arrow/Enter, `header.liquid:2337-2339`), no-results-state (`~2307`), resultaten via `textContent` (veilig), `aria-expanded` op combobox. Full-page search responsive (2-koloms mobiel).
- **UNKNOWN:** locale-aware endpoint bij Markets — predictive respecteert normaal automatisch de markt; **verifiëren bij BE**. `aria-live` op resultaten: aanwezigheid **verifiëren** (keyboard-nav werkt wel).
- **Advies:** huidige search is **voldoende**; predictive is al aanwezig. Geen actie. · **Vóór launch:** nee.

---

## J. SEO / MOBILE CONTENT PARITY

### J1 — JSON-LD structured data — **PASS**
- **Bestand:** `snippets/structured-data.liquid`
- **Huidige situatie:** Organization (r31), WebSite+SearchAction (r47), BreadcrumbList (r86), Product met Offer/price/`availability` (InStock/OutOfStock)/brand/sku (r100-124), prijs via `cart.currency.iso_code` (locale-aware). Geen `aggregateRating` (bewust — komt van Judge.me).
- **Impact:** SEO ✅. · **Vóór launch:** n.v.t. (PASS).

### J2 — Canonical / meta / noindex — **PASS**
- **Bestanden:** `meta-tags.liquid:88-91` (canonical), `seo-meta.liquid` (per-type titles/descriptions, `noindex` op utility-pagina's).
- **Let op:** meta-description index bevat hardcoded `€{{ kb_free_shipping }}` (zie G2). · **Vóór launch:** nee (PASS, met G2-notitie).

### J3 — Mobile content-pariteit — **PASS**
- **Bevinding:** Geen SEO-content die alleen op mobiel verborgen/weggelaten wordt. `display:*` in media-queries betreft carousels/layout, niet het verbergen van tekst/FAQ/related. Alt-teksten aanwezig (`| escape`, met `default: product.title`). Server-side gerenderd → mobiel == desktop.
- **Impact:** SEO ✅. · **Vóór launch:** n.v.t. (PASS).

---

## K. FONTS

### K1 — Self-hosted Lilita One + Karla — **PASS (met K2)**
- **Bestanden:** `snippets/css-variables.liquid:4-36`, `assets/*.woff2`
- **Huidige situatie:** woff2 (latin + latin-ext), `font-display:swap`, fallbacks `cursive`/`sans-serif`, Karla variabel 200-800. **De eerder gemelde MissingAsset-errors zijn opgelost** (de 4 woff2 zijn nu in de repo, commit `a6eb0a5`) — theme check = 0 errors.
- **Impact:** LCP/CLS ✅ (geen FOIT). · **Vóór launch:** n.v.t. (PASS).

### K2 — Font-preload zonder `crossorigin` — **WARN**
- **Ernst:** P1 · **Bestand:** `layout/theme.liquid:4-5`
- **Huidige situatie:** `preload_tag: as:'font'` **zonder** `crossorigin`. Font-fetches zijn CORS-modus; een preload zonder `crossorigin` matcht de latere request niet → **font wordt dubbel gedownload** (preload verspild).
- **Impact:** LCP/perf ⚠️ (dubbele font-download op eerste load).
- **Fix:** `crossorigin: 'anonymous'` toevoegen aan beide `preload_tag`-calls. · **Risico:** verwaarloosbaar. · **Complexiteit:** laag. · **Vóór launch:** aanbevolen (goedkope winst).
- **UNKNOWN:** exacte CLS van Lilita-swap op traag netwerk — **meten** (Lighthouse).

---

## L. CSS / ASSET DELIVERY

### L1 — Critical CSS + per-sectie CSS — **PASS**
- **Bestanden:** `assets/critical.css` (193 regels / ~4,7KB), `layout/theme.liquid:7` (`stylesheet_tag preload:true`), alle secties gebruiken `{% stylesheet %}`
- **Huidige situatie:** Klein kritiek CSS-bestand; rest is **per-sectie gebundeld** (geen monoliet, geladen wanneer sectie rendert). `!important` alleen in `prefers-reduced-motion`-blok. Minimale inline styles (dynamische kaartkleur).
- **Impact:** LCP/perf ✅. · **Vóór launch:** n.v.t. (PASS).
- **UNKNOWN:** gzipped totaal-transfer + som van section-CSS op zware pagina's — **meten**.

---

## M. THIRD-PARTY APPS

### M1 — Judge.me — **PASS / UNKNOWN**
- **Bestanden:** `judgeme-product-reviews.liquid`, `judgeme-rating.liquid`, `config/settings_data.json` (app-block `shopify://apps/judge-me-reviews/...`)
- **Huidige situatie:** Rating/aantal uit `shop.metafields.judgeme.*` (server-cached, geen live-API-blocking); widget als app-embed (async, app-gecontroleerd). Nette lege staat zonder verzonnen reviews.
- **UNKNOWN — HANDMATIGE CHECK:** laadt het Judge.me-script `async/defer` (DevTools) en is het consent-aware (app-instellingen)? Cart-interferentie: onwaarschijnlijk (read-only op PDP).
- **Vóór launch:** verifiëren (DevTools/app).

### M2 — Overige embeds / pixels — **PASS (repo) / Admin-check**
- **Bevinding:** Geen chat/GA/Meta/Klaviyo-scripts in de theme-code. Alles wat er is, komt via `content_for_header` (Admin/Custom Pixels/app-embeds) → **HANDMATIGE ADMIN-CHECK** welke pixels actief zijn en of ze consent-gated zijn (zie H1).

---

## N. MOBILE LAYOUT / DEVICE-TECHNIEK

### N1 — `viewport-fit=cover` ontbreekt terwijl safe-area gebruikt wordt — **FAIL**
- **Ernst:** **P1** · **Bestanden:** `meta-tags.liquid:3` (viewport), safe-area gebruikt in `mobile-tabbar.liquid:28/34`, `cart-drawer.liquid:152`, `main-product-kwispelbox.liquid:547`
- **Huidige situatie:** `<meta name="viewport" content="width=device-width,initial-scale=1">` — **geen** `viewport-fit=cover`. Zonder dat leveren `env(safe-area-inset-*)` **0** op notch-/home-indicator-toestellen, terwijl de theme er wél op rekent.
- **Waarom:** Tabbar / sticky PDP-CTA / drawer-footer kunnen onder de home-indicator vallen op iPhone met notch/Dynamic Island.
- **Impact:** UX ⚠️ (mobiel iOS) · a11y (bereikbaarheid CTA).
- **Fix:** `,viewport-fit=cover` toevoegen aan de viewport-meta. · **Risico:** laag (moet op notch getest). · **Complexiteit:** laag. · **Vóór launch:** **ja** (goedkoop, zichtbaar effect).

### N2 — dvh/svh vs 100vh — **PASS**
- **Bestand:** `assets/critical.css:13` (`100svh`)
- **Bevinding:** `svh` gebruikt; geen problematische kale `100vh` gevonden. Dialogs gebruiken `inset:0`, overscroll `contain`, momentum-scroll. · **Vóór launch:** n.v.t. (PASS).

### N3 — Input font-size < 16px (iOS auto-zoom) — **WARN**
- **Ernst:** P2 · **Bestanden:** `header.liquid:~1546` (header-search `.92rem`), `main-product-kwispelbox.liquid:~504` (PDP-velden `.92rem`)
- **Huidige situatie:** Diverse inputs < 16px → iOS Safari zoomt bij focus. (De full-page search-input is wél 16px.)
- **Impact:** UX ⚠️ (mobiele zoom-sprong).
- **Fix:** input font-size ≥16px op mobiel (of `@media` clamp). · **Complexiteit:** laag. · **Vóór launch:** nee (P2).

### N4 — Z-index stacking — **PASS**
- **Bevinding:** Coherente hiërarchie: header 100 · tabbar 200 · nav-drawer 300 · cart-drawer 400. Geen conflicten zolang er geen nieuw hoog-z-index element bijkomt. · **Vóór launch:** n.v.t. (PASS).

---

## O. THEME EDITOR ROBUUSTHEID

### O1 — Geen `shopify:section:load` re-init — **WARN**
- **Ernst:** P2 (editor-only) · **Bestanden:** `main-product-kwispelbox.liquid:~742` (gallery `init()` IIFE), diverse carousels; **geen** `Shopify.designMode`/`shopify:section:load` listeners in de hele theme (bevestigd via grep).
- **Huidige situatie:** JS initialiseert één keer bij page-load. Na een **section-reload in de theme-editor** worden gallery-dots/sliders/GWP-bindings mogelijk niet opnieuw geïnitialiseerd.
- **Waarom:** Alleen relevant in de editor (niet voor eindgebruikers), maar kan Jasper's editor-preview "kapot" laten lijken.
- **Impact:** UX (editor) ⚠️.
- **Fix:** `document.addEventListener('shopify:section:load', init)` per interactieve sectie. · **Complexiteit:** laag-middel. · **Vóór launch:** nee (P2), handig voor comfort.

---

## P. SECURITY / DATA-HYGIËNE

### P1 — `cart.liquid:132` line-item property niet ge-escaped — **WARN**
- **Ernst:** P2 · **Bestand:** `sections/cart.liquid:132` (`{{ prop.last }}` zonder `| escape`)
- **Huidige situatie:** Op de **server-gerenderde** cart-pagina worden property-waarden (bv. "Naam op bandana", "Kaarttekst") rauw uitgevoerd. Deze section-HTML wordt via `innerHTML` geswapt (`cart.liquid:432`).
- **Waarom:** Een property met HTML/JS zou uitgevoerd worden. In de praktijk **self-XSS** (properties reflecteren wat de gebruiker zelf toevoegde; cart is niet gedeeld) → beperkt risico, maar best practice = escapen.
- **Impact:** security ⚠️ (laag/self-XSS).
- **Fix:** `{{ prop.last | escape }}`. · **Risico fix:** geen. · **Complexiteit:** laag. · **Vóór launch:** aanbevolen (triviaal).

### P2 — Cart-drawer JS `innerHTML` — **PASS**
- **Bestand:** `snippets/cart-drawer.liquid:211-242`
- **Bevinding (geverifieerd):** De drawer bouwt item-HTML via `innerHTML` maar **escapet alle dynamische waarden** met een `esc()`-helper (`esc(it.product_title)`, `esc(it.properties[k])`, `esc(it.key)` etc.); image-src is een Shopify-CDN-URL. → **veilig**. Geen `eval`/`Function`, geen URL-param→DOM, data-attributes via `.textContent`. · **Vóór launch:** n.v.t. (PASS).

---

## Q. HARD-CODED ROUTES

### Q1 — Inventaris — **WARN**
- **Ernst:** P1 (Markets-readiness) / acceptabel (NL-only nu)
- **Echte bevindingen (locale-onvriendelijk):**
  | Bestand:regel | Route | Classificatie |
  |---|---|---|
  | `snippets/cart-drawer.liquid:77` | `/checkout` (checkout-knop) | Markets-risk → `{{ routes.checkout_url }}` |
  | `snippets/cart-drawer.liquid:306` | `/cart` (link) | Markets-risk → `routes.cart_url` |
  | `sections/cart.liquid:559` | `'/discount/' + code + '?redirect=/cart'` | Markets-risk → `Shopify.routes.root` prefix |
- **Al goed (locale-aware):** header/footer/tabbar/gwp-sync/product-extras gebruiken `routes.*` / `Shopify.routes.root` (bevestigd). Theme-check `HardcodedRoutes`-warnings in `mobile-tabbar.liquid` betreffen `/collections/boxen` (interne collectie-link — **acceptabel**, geen locale-breuk voor collecties).
- **Waarom:** Bij een markt/taal met pad-prefix (`/fr/…`) verwijzen absolute paden naar de verkeerde/niet-gelokaliseerde bron.
- **Impact:** Markets ⚠️ (cart/checkout betrouwbaarheid bij tweede taal).
- **Fix:** de 3 bovenstaande via `routes`-object/`Shopify.routes.root`. · **Complexiteit:** laag. · **Vóór launch:** nee voor NL-only; **ja vóór Markets/tweede taal**.

---

## R. REAL USER METRICS / MONITORING (aanbevolen na koppeling live)

Te meten vóór/direct na launch (mobiel én desktop, per template: homepage, PDP, collectie, cart):
- **CWV veldwaarden:** LCP, INP, CLS.
- **Shopify-native:** Analytics → Reports → **Online Store Speed / Web Performance** (Shopify's ingebouwde CWV-dashboard, veldwaarden). 
- **Google Search Console → Core Web Vitals** (CrUX-veldwaarden, mobiel/desktop split) — koppelen zodra store publiek is.
- **PageSpeed Insights / CrUX** per key-URL; **Lighthouse** (lab) voor diagnose.
- **Device-matrix:** iPhone (notch: 13/14/15), oudere iPhone SE, mid-range Android (Chrome), iPad. **Netwerk:** Fast 3G / Slow 4G / kabel. **Browser-matrix:** iOS Safari, Chrome Android, Samsung Internet.
- **Handmatige a11y-pass:** keyboard-only + VoiceOver/TalkBack op drawer, PDP, cart.

---

## S. VOORGESTELD PERFORMANCE-BUDGET (aanbevelingen, geen gemeten waarden)

> Dit zijn **doelen**, geen metingen — er is geen profiler gedraaid.

| Metric (mobiel, 4G) | Doel |
|---|---|
| LCP | < 2,5 s (goed), hard budget < 3,0 s |
| INP | < 200 ms |
| CLS | < 0,10 |
| Initiële JS (theme, excl. Shopify/apps) | < 150 KB gecomprimeerd |
| DOM-nodes per pagina | < 1.500 |
| Image-transfer above-the-fold | < 300 KB (hero) |
| Totale transfer eerste view (mobiel) | < 1,2 MB |
| Third-party JS (Judge.me/loyalty/pixels) | < 150 KB, async/defer, niet render-blocking |

---

## T. PRIORITEITEN

### P0 — Moet vóór launch (max 10)
1. **Consent verifiëren** — Shopify native cookiebanner aan voor NL/EU + alle pixels/apps consent-gated (H1). _Verificatie, geen code._
2. **Judge.me + loyalty consent-check** — GDPR-mode Judge.me; identificeer `loy_*.js`-app en of het pre-consent laadt (H2/M1). _Verificatie._

### P1 — Zeer wenselijk vóór launch (max 10)
1. `viewport-fit=cover` toevoegen (safe-area werkt nu niet op notch) — N1.
2. Font-preload `crossorigin` toevoegen (dubbele font-download) — K2.
3. Collectie-afbeeldingen: eerste eager+fetchpriority, srcset/sizes toevoegen — A3.
4. Hardcoded routes cart-drawer/discount locale-aware maken **vóór Markets/tweede taal** — Q1.
5. Markt-specifieke beloftes (delivery/cutoff/threshold) + `| money` **vóór BE** — G2/G3.

### P2 — Kan na launch (max 10)
1. Cart-drawer qty-knoppen → 44×44 (touch target) — F2.
2. Input font-size ≥16px (iOS auto-zoom) — N3.
3. `cart.liquid:132` property `| escape` (self-XSS) — P1.
4. `shopify:section:load` re-init voor editor-comfort — O1.
5. Moment-tiles fluid `sizes`; cart-count centraliseren; `loy_*.js` opruimen — A4/B5/H2.
6. Contrast merk-roze verifiëren — F3.

### PASS — Al goed gebouwd
Cart/GWP-architectuur (B1-B6), hero/PDP LCP-preloads (A1-A2), responsive images meerderheid (A/J), width/height overal (geen image-CLS), per-sectie CSS + kleine critical.css (L1), JSON-LD compleet (J1), canonical/mobile-parity (J2-J3), focus-trap/ESC/scroll-lock/nested dialogs/aria (F1), z-index & dvh/safe-area-gebruik (N2/N4), predictive search (I1), localization-form (G1), drawer-JS escaping (P2), fonts opgelost (K1).

---

## U. VERPLICHTE SAMENVATTINGSTABEL

| Onderdeel | Status | Prioriteit | Risico | Complexiteit fix |
|-----------|--------|------------|--------|------------------|
| LCP (hero/PDP) | PASS | — | laag | — |
| LCP (collectie afbeeldingen) | FAIL/WARN | P1 | middel | laag |
| Responsive images (srcset/sizes) | PASS (m.u.v. collectie/moments) | P1/P2 | laag-middel | laag |
| CLS (images/badges/fonts) | PASS | P2 | laag | — |
| INP (cart/drawer/handlers) | PASS | — | laag | — |
| Cart AJAX / state | PASS | — | laag | — |
| GWP idempotentie/qualifying | PASS | — | laag | — |
| Cart drawer (DOM/lazy) | PASS/acceptable | P2 | laag | — |
| Section Rendering API | PASS (correct) | — | laag | — |
| Duplicate DOM | PASS | P2 | laag | — |
| Locale-aware routes | WARN | P1 (vóór Markets) | middel | laag |
| Shopify Markets (money/beloftes) | WARN | P1 (vóór BE) | middel | middel |
| Cookie consent | UNKNOWN | **P0** (verificatie) | hoog (juridisch) | — (Admin) |
| Accessibility (touch/inputs) | WARN | P2 | laag | laag |
| Search (predictive) | PASS | — | laag | — |
| SEO parity / JSON-LD | PASS | — | laag | — |
| Fonts (preload crossorigin) | WARN | P1 | laag | laag |
| Third-party (Judge.me/loyalty) | UNKNOWN | P1/P2 | middel | — (Admin) |
| Mobile safe-area (viewport-fit) | FAIL | P1 | laag | laag |
| Theme editor re-init | WARN | P2 | laag | laag-middel |
| Hardcoded routes | WARN | P1 (vóór Markets) | middel | laag |
| Security (cart escaping) | WARN (server) / PASS (drawer) | P2 | laag | laag |

---

## Openstaande handmatige tests (UNKNOWN — niet uit code te verifiëren)
1. Consent: incognito → pixels vuren pas ná toestemming? (H1) — **P0**
2. Judge.me GDPR-mode + async/defer laadgedrag (M1).
3. `loy_77036486821.js`: welke app, laadt pre-consent? (H2)
4. Notch-toestellen: tabbar/sticky-CTA onder home-indicator zonder `viewport-fit=cover`? (N1)
5. iOS auto-zoom op <16px inputs (N3).
6. Contrast merk-roze vs AA (F3).
7. Theme-editor: gallery/carousel na section-reload (O1).
8. Lighthouse/veld: werkelijke LCP-element homepage, Lilita font-swap CLS, gzipped transfer (A/K/L).
9. Markets: land- én taalkeuze + prijs/claim-formattering bij BE (G1-G3).

---

_Einde audit. Er is uitsluitend dit document toegevoegd; geen theme-code gewijzigd._
