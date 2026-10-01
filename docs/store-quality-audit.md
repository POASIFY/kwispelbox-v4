# Kwispelbox — Store Quality Audit

_Opgesteld: 01-10-2026 · Scope: storefront `kwispelbox.com` (theme KB-v4, `POASIFY/kwispelbox-v4@main`)._
_Audit-only. Er is in deze ronde **niets** gewijzigd, gepubliceerd of verwijderd. Broken/legacy-issues worden alleen gerapporteerd._
_Bronnen: Shopify Admin API (live pagina's/menu's/collecties) + theme-code (lokaal, main)._

---

## 0. Belangrijkste conclusies vooraf

1. **Geen oude "Hondenknuffels"-branding aanwezig.** Theme-breed **0 treffers**; alle 16 live pagina-bodies zeggen "Kwispelbox". De brand-migration-zorg is op deze store **niet** van toepassing (vermoedelijk relevant voor een andere/oude store). → Zie §2.
2. **De store is structureel schoon** (producten, collecties, menu's, korting, fonts, PDP). De zwakte zit in **content-diepte van service/legal/brand-pagina's**: de templates zijn deels al mooi gebouwd, maar de **pagina-bodies zijn placeholders** (1 zin elk) en een aantal belangrijke servicepagina's **ontbreekt nog**.
3. **3 concrete link/structuur-issues** gevonden (geen daarvan is een harde 500/kapotte checkout): dead link `/pages/verlanglijst`, lege mega-menu-dropdowns, en een orphan product-template. → §3.
4. Aanpak: **niet 20 pagina's los bouwen**, maar 4 herbruikbare page-families (zie `docs/page-design-system.md`) en dan gefaseerd uitrollen.

---

## 1. Volledige inventaris

### 1a. Commercieel
| Onderdeel | Status | Opmerking |
|---|---|---|
| Homepage (`index.json`) | ✅ live | Premium, recent gepolijst (hero/USP/meest-gekozen/blije honden) |
| `/collections/boxen` | ✅ 5 producten | smart TYPE=Special Editions |
| `/collections/special-editions` | ✅ 5 | |
| `/collections/cadeau-extras` | ✅ 4 | |
| `/collections/meest-gekozen` | ✅ 4 (manual) | homepage-bron |
| `/collections/maak-er-een-echt-cadeautje-van` | ✅ 4 | cart/PDP-upsell |
| `/collections/puppy-favorieten` | ⚠️ **0 producten** | gelinkt vanaf homepage ("Nieuwe puppy") → lege collectie |
| `/collections/speeltjes·snacks·knuffels` | ⚠️ **0 producten** | leeg (smart, geen matches); bewust leeg |
| `/collections/alle-producten-voor-acties` | ✅ 10 | technische verzamel-collectie (GWP-scope) |
| 5 Special-Edition PDP's | ✅ live | Verjaardag/Kerst/Halloween/Dierendag/Valentijn — template `product.box` |
| Search (`search.json`) | ✅ | predictive + full-page |
| Cart (`cart.json`) + cart-drawer | ✅ | GWP-centraal, Safari-fix gedaan |
| Account | ✅ Shopify native (customer-account menu) | |

### 1b. Service-pagina's (live)
| Gewenst (brief) | Bestaat? | Handle / template |
|---|---|---|
| Klantenservice (hub) | ❌ **ontbreekt** | — |
| Veelgestelde vragen | ✅ | `veelgestelde-vragen` / `faq` |
| Verzending & levering | ✅ | `verzenden-en-bezorgen` / `shipping` |
| Bestellen & betalen | ✅ (deels) | `betaalmogelijkheden` / `betaalmogelijkheden` |
| Retourneren | ✅ | `retourneren` / `returns` |
| Retour aanmelden / herroepen | ❌ **ontbreekt** | — |
| Annuleren of wijzigen | ❌ **ontbreekt** | — |
| Bestelling volgen | ❌ **ontbreekt** | — |
| Klachten & garantie | ✅ (klachten) | `klachten` / `complaints` |
| Contact | ✅ | `contact` / `contact` |
| Mijn account & bestellingen | ✅ Shopify native | — |

### 1c. Merk / content
| Gewenst | Bestaat? | Handle / template |
|---|---|---|
| Over ons | ✅ | `over-ons` / `about-page` |
| Waarom Kwispelbox | ❌ (deels in "hoe-werkt-het") | — |
| Veilig spelen | ❌ **ontbreekt** | — |
| Onderhoud (knuffels/speeltjes) | ❌ **ontbreekt** | — |
| Hoe werkt het | ✅ | `hoe-werkt-het` / `hoe-werkt-het` |
| Kwispelclub | ✅ | `kwispelclub` / `kwispelclub` |
| Partners / Zakelijk | ✅ | `partners` / `partners` |
| Cadeau(bon) | ✅ | `cadeau` / `cadeau` |
| Reviews | ✅ | `reviews` / `reviews` |

### 1d. Juridisch
| Gewenst | Bestaat? | Opmerking |
|---|---|---|
| Privacybeleid | ⚠️ **niet als page** | Hoort bij Shopify **Policies** (`/policies/privacy-policy`, Settings → Policies) — niet hand-bouwen. **Verifiëren in Admin.** |
| Algemene voorwaarden | ✅ page | `algemene-voorwaarden` / `voorwaarden` |
| Cookiebeleid | ✅ page | `cookiebeleid` / `cookiebeleid` |
| Toegankelijkheid | ❌ **ontbreekt** | aanbevolen (A11y-statement) |
| Bedrijfsgegevens | ❌ **ontbreekt** | KvK/btw/adres — aanbevolen (en EU-verplicht) |
| Herroepingsformulier | ❌ **ontbreekt** | wettelijk modelformulier ontbreekt |

> **Alle 16 bestaande pagina's zijn gepubliceerd** en Kwispelbox-branded, maar met **placeholder-body** (1 zin). De bijbehorende **templates** renderen wél eigen sectie-content (defaults), dus veel pagina's ogen al "gevuld" via de template — niet via de body.

---

## 2. Brand-migration audit (Hondenknuffels → Kwispelbox)

| Bron | Treffers "Hondenknuffels/hondenknuffel" | Classificatie |
|---|---|---|
| Theme-code (sections/snippets/templates/layout/config/locales) | **0** | n.v.t. |
| Live pagina-bodies (16) | **0** (allemaal "Kwispelbox") | n.v.t. |
| Menu's / collecties / producttitels | **0** | n.v.t. |

**Conclusie:** op deze store is **geen** oude branding aanwezig → geen REBRAND/LEGACY-acties nodig. 
**LEGAL REVIEW (niet wijzigen):** controleer in Admin → Settings of de **juridische entiteitsnaam/bedrijfsgegevens** (facturen, policies, e-mails) kloppen — die kunnen bewust afwijken en vallen buiten theme-scope.

> Let op het alias in de brief: een content-pagina **"Onderhoud hondenknuffels"** moet, als we 'm bouwen, **"Onderhoud speeltjes & knuffels"** heten (Kwispelbox-term), niet de oude merknaam.

---

## 3. Broken / dead / structuur-issues (alleen rapport)

| # | Issue | Bewijs | Impact | Prio |
|---|---|---|---|---|
| 1 | **Dead link `/pages/verlanglijst`** | `sections/header.liquid` verlanglijst-actie linkt naar `/pages/verlanglijst`; geen page met handle `verlanglijst` bestaat → **404** | Verlanglijst-knop in header leidt naar 404 | **P1** |
| 2 | **Lege mega-menu-dropdowns** | `header-group.json` bindt dropdowns aan menu's `menu-verrassingsboxen/speeltjes/snacks/knuffels/puppy` die **niet bestaan** → dropdowns tonen niets (theme vangt blank netjes op) | Mega-menu oogt onafgemaakt | P2 |
| 3 | **Orphan template `product.kwispelbox.json`** | Geen product gebruikt suffix `kwispelbox` (alle 5 boxen = `product.box`); restant van gearchiveerde hoofdbox | Verwarrende dode template | P2 |
| 4 | **Lege gelinkte collecties** | `puppy-favorieten` (0) gelinkt vanaf homepage; `speeltjes/snacks/knuffels` (0) | Lege collectiepagina's bij doorklikken | P2 |
| 5 | **Menu-duplicaat** | main-menu "Feestdagen" én "Momenten" → beide `/collections/special-editions` | Zwakke IA (2 items, 1 bestemming) | P2 |
| 6 | **Privacybeleid als page?** | Geen privacy-page; hoort bij Shopify Policies | Compliance — **verifiëren** dat `/policies/privacy-policy` gevuld + gelinkt is | **P1 (verify)** |

**Geen** gevonden: kapotte checkout/cart-routes, kapotte productlinks, ongepubliceerde pagina's in footer (alle gelinkte pagina's zijn gepubliceerd), oude productlinks.

---

## 4. Kwaliteitsscore per pagina/template

Designniveau: **A** = volledig Kwispelbox/premium · **B** = bruikbaar maar basic · **C** = generieke tekstpagina · **D** = legacy/verouderd/broken.

| Pagina | Gepubliceerd | Template (sectie-type) | Footer/header-link | Branding | Niveau | Actie |
|---|---|---|---|---|---|---|
| Homepage | ✅ | `index` (vele secties) | header/logo | ✅ | **A** | KEEP |
| PDP (5 boxen) | ✅ | `product.box` | menu/collectie | ✅ | **A-** | POLISH (zie §6) |
| Boxen | ✅ | `page.boxen` (box-compare, boxes, steps…) | main-menu | ✅ | **A/B** | KEEP |
| Special Editions | ✅ | `special-editions-page` (editions/groups) | menu | ✅ | **B+** | KEEP |
| Kwispelclub | ✅ | `kwispelclub-page` (benefit/member/step) | menu | ✅ | **B+** | KEEP |
| Partners | ✅ | `partners-page` (perk/ptype/step) | main-menu "Zakelijk" | ✅ | **B+** | KEEP |
| Hoe werkt het | ✅ | `product-how-it-works` + steps | menu | ✅ | **B** | KEEP/POLISH |
| Cadeau | ✅ | `product-boxes` + benefit/box | footer-shop | ✅ | **B** | KEEP |
| Over ons | ✅ | `about-page` (value blocks) | menu-over-ons | ✅ | **B** | POLISH (Brand-template) |
| Reviews | ✅ | `reviews-page` (Judge.me) | menu | ✅ | **B** | KEEP |
| FAQ | ✅ | `faq`/`product-faq` + page-intro | footer-service | ✅ | **B** | POLISH (Help-hub) |
| Betaalmogelijkheden | ✅ | `payment-page` + faq | footer-service | ✅ | **B** | POLISH (Service-template) |
| Verzenden & bezorgen | ✅ | `info-page` (4 items) | footer-service | ✅ | **C** | REDESIGN → Service-template |
| Retourneren | ✅ | `info-page` (4 items) | footer-service | ✅ | **C** | REDESIGN → Service-template |
| Klachten | ✅ | `info-page` (4 items) | footer-service | ✅ | **C** | REDESIGN → Service-template |
| Contact | ✅ | `contact-page` (tiles) | footer-service | ✅ | **B** | POLISH |
| Algemene voorwaarden | ✅ | `info-page` (8 items) | menu-over-ons | ✅ | **C** | REDESIGN → Legal-template |
| Cookiebeleid | ✅ | `info-page` (3 items) | footer/menu | ✅ | **C** | REDESIGN → Legal-template |
| _Klantenservice hub_ | ❌ | — | — | — | **—** | NIEUW (Help-hub) |
| _Bestelling volgen / annuleren / retour-aanmelden_ | ❌ | — | — | — | **—** | NIEUW (Service) |
| _Veilig spelen / Onderhoud / Waarom Kwispelbox_ | ❌ | — | — | — | **—** | NIEUW (Brand) |
| _Toegankelijkheid / Bedrijfsgegevens / Herroepingsformulier_ | ❌ | — | — | — | **—** | NIEUW (Legal) |

**Samengevat:** niveau A/B bestaat voor commercieel + merk-highlights; de **service/legal-pagina's zijn C** (generieke `info-page` met item-lijstjes) en de **content is placeholder** — dáár zit de grootste kwaliteitswinst.

---

## 5. Consolidatie — welke templates/bestanden samen kunnen

**Bestaande herbruikbare secties (goed nieuws — veel bouwstenen bestaan al):**
- `info-page` → generieke service/legal-workhorse (item-blocks). Nu voor shipping/returns/complaints/voorwaarden/cookiebeleid.
- `contact-page`, `payment-page`, `about-page` → bespoke page-secties.
- `page-intro`, `product-faq`, `product-cta`, `product-how-it-works`, `box-compare`, `product-boxes` → inzetbare blokken.

**Consolidatievoorstel:**
1. **Splits `info-page` conceptueel in 2 rollen** via de nieuwe families: een **Service-shell** (cards + accordions + support-CTA) en een **Legal-shell** (TOC + anchors + leestypografie). Nu doet één generieke sectie beide matig.
2. **`product.kwispelbox.json`** (orphan) → na akkoord verwijderen (consolideren op `product.box`).
3. **Lege collecties** (`speeltjes/snacks/knuffels`): of vullen, of uit de (toekomstige) mega-menu's houden; `puppy-favorieten` homepage-link herrichten tot er puppy-producten zijn.
4. **Mega-menu-menu's** (`menu-verrassingsboxen` etc.) óf aanmaken/vullen óf de dropdown-bindingen leegmaken (nu lege dropdowns).

---

## 6. PDP quality audit — Verjaardag Kwispelbox

| Sectie | Bestand | Oordeel | Reden |
|---|---|---|---|
| Gallery + buybox | `main-product-kwispelbox` | **KEEP** (lichte POLISH) | Recent gepolijst: sticky ATC, varianten, personalisatie, mobiele gallery. Sterk. |
| **Wat zit er in de box?** | `box-contents` | **REDESIGN → ✅ GEDAAN** | Omgebouwd tot omkaderde split-sectie (showcase + rijke cards + geïntegreerde accordions). _Live-verificatie nog openstaand._ |
| Cadeau-extra's | `product-extras` | **KEEP/POLISH** | Werkt (quick-add + GWP-sync); visueel oké, kan iets rijker. |
| Reviews (Judge.me) | `judgeme-product-reviews` | **KEEP** | Nette lege staat; vult zodra reviews binnenkomen. |
| FAQ | `product-faq` | **POLISH** | Los technisch blok; laten aansluiten op de nieuwe box-contents-kaartstijl (zachte kaart i.p.v. kale lijnen). |
| Gerelateerde boxen | `product-related` | **POLISH** | Functioneel maar vlak; kaartstijl gelijktrekken met "Meest gekozen". |
| Laatste CTA | `product-cta` | **KEEP** | Prima afsluiter. |

---

## 7. Prioriteitenvolgorde

### P0 — eerst (compliance/verificatie, blocker voor launch)
1. **Privacy/consent-verificatie** (Admin): `/policies/privacy-policy` gevuld + native cookiebanner aan (loopt al als openstaand punt).
2. **Juridische basis compleet**: Bedrijfsgegevens + Herroepingsformulier + (verwijzing) privacybeleid — wettelijk vereist vóór echt live.
3. **Dead link `/pages/verlanglijst`** fixen (of verlanglijst-feature/route herrichten).

### P1 — zeer wenselijk vóór launch
4. **Service-template (familie A)** bouwen en uitrollen op verzending/retour/betalen/klachten (nu niveau C).
5. **Legal-template (familie D)** voor voorwaarden/cookiebeleid + nieuwe legal-pagina's.
6. **Echte content** i.p.v. placeholder-bodies op de servicepagina's.
7. **Ontbrekende service-pagina's**: Bestelling volgen, Retour aanmelden, Annuleren/wijzigen, Klantenservice-hub.

### P2 — kan na launch
8. Brand/editorial-template (familie B) + nieuwe pagina's (Waarom Kwispelbox, Veilig spelen, Onderhoud).
9. Mega-menu-dropdowns vullen of opruimen; lege collecties herrichten.
10. Orphan `product.kwispelbox.json` opruimen; PDP FAQ/related POLISH; menu-duplicaat Feestdagen/Momenten.

### PASS — al goed
Homepage, PDP-buybox, boxen/special-editions/kwispelclub/partners-pagina's, cart/GWP, fonts, Safari-flex-hardening.

---

## Openstaande handmatige checks (niet uit code/API te bevestigen)
1. Admin → Policies: privacy/retour/voorwaarden-policies gevuld?
2. Admin → Customer privacy: cookiebanner aan voor NL/EU (loopt).
3. Bedrijfsgegevens (KvK/btw/adres) correct in Settings + facturen/e-mails.
4. Live-verificatie van de nieuwe "Wat zit er in de box?" op echte iPhone + desktop.

---

## Fase 1 — Uitvoeringslog (01-10-2026)

**Gebouwd:** 2 herbruikbare families — `sections/service-page.liquid` (A) + `sections/legal-page.liquid` (D). Uitgerold (content **verbatim** gemigreerd, geen nieuwe claims):
- Service: `page.shipping`, `page.returns`, `page.complaints`.
- Legal: `page.cookiebeleid`, `page.voorwaarden`.
- `info-page.liquid` nu ongebruikt → consolidatie-kandidaat (niet verwijderd).

### CONTENT REVIEW NODIG (niet zelf ingevuld)
1. **Algemene voorwaarden** — de intro zegt letterlijk "Laat deze tekst juridisch controleren voordat je live gaat"; artikelen zijn generiek → **juridische review + de dev-note uit de intro halen**.
2. **Privacybeleid (Shopify Policy)** — bestaat op `/policies/...` maar **body is in het Engels** → vertalen/controleren (NL).
3. **Cookiebeleid / retour / klachten** — teksten zijn plausibel maar bevatten **claims** (30 dagen retour, niet-goed-geld-terug, terugbetaling 14 dagen, levertijden NL 1-2/BE 2-3 dagen, gratis vanaf €50) → **bevestigen dat deze commercieel/juridisch kloppen**.

### P0-bevindingen
- **Dead link `/pages/verlanglijst`** — zit in `sections/header.liquid` (verlanglijst-actie), een **code-route buiten de service/legal-templates**. Opties: verlanglijst-pagina maken, link herrichten, of feature verwijderen. **Vereist akkoord (header/functionele wijziging) — niet in deze fase aangeraakt.**
- **Privacybeleid** — ✅ correct als Shopify Policy (niet nagebouwd). Verifieer dat 'ie in de footer gelinkt is + NL-vertaling.
- **Bedrijfsgegevens** — aanwezig in de privacy-policy: _Kwispelbox, Kwakkenbergweg 150-05, 6571 GB Berg en Dal · +31 242 340 422 · hallo@kwispelbox.com_. Nog **geen eigen pagina** → Fase 2 (Legal-familie), data is beschikbaar.
- **Herroepingsformulier** — ontbreekt nog → Fase 2.

### Handmatige Admin-acties (geen publicatie-status gewijzigd)
- Juridische review voorwaarden + NL-vertaling privacy policy.
- Bevestigen claims (retour/levertijd/garantie).
- Beslissing dead-link verlanglijst.

### Overig voor Fase 2
Help-hub (C) + Brand/editorial (B) + nieuwe pagina's (bestelling volgen, retour aanmelden, annuleren, bedrijfsgegevens, herroepingsformulier, toegankelijkheid, veilig spelen, onderhoud). `betaalmogelijkheden`/`contact` = POLISH, geen redesign.

_Einde audit + Fase 1-log._

---

## Fase 2 — Uitvoeringslog (01-10-2026)

Gebouwd: **Help-hub (C)** + **Editorial/Brand (B)** families; uitgerold op bestaande + 10 nieuwe concept-pagina's. Service/Legal (bevroren) ongemoeid. Commits F2-C1, F2-B1, F2-A2, F2-D2, F2-H, F2-AUDIT.

### Wishlist (F2-H) — opgelost
Header verlanglijst-actie (`/pages/verlanglijst` = dead link, desktop-only) **verwijderd** + orphan CSS opgeruimd. Account/search/cart/layout intact; mobiel was al verborgen.
**Resterend (gerapporteerd, niet aangeraakt):** `sections/featured-products.liquid:49` `mg__wish` hartknop (`data-wishlist-handle`) — **non-functioneel** (geen JS). Aparte card-feature → aanbeveling: in Fase 3 verwijderen of een echte wishlist bouwen. Jouw beslissing.

### CONTENT REVIEW NODIG (nieuwe pagina's — niet zelf ingevuld)
1. **Bedrijfsgegevens** — KvK- + btw-nummer ontbreken (wettelijk verplicht) → invullen. Adres/tel/e-mail overgenomen uit privacy-policy (zie Admin-data hieronder) — **verifiëren**.
2. **Herroepingsformulier** — wettelijk **modelformulier** niet opgenomen (niet geïmproviseerd) → juridische tekst aanleveren.
3. **Toegankelijkheid** — feitelijke verklaring zonder compliance-claim; officiële verklaring/WCAG-niveau desgewenst juridisch laten opstellen.
4. **Wijzigen/annuleren** — operationele specifics (tot wanneer wijzigen mogelijk, wat precies) → bevestigen.
5. **Brand-copy** (waarom-kwispelbox/veilig-spelen/onderhoud) = neutrale merktekst, geen claims — redactioneel nalezen gewenst.

### Claims-audit (inventaris — geen stille correcties)
| Claim | Belangrijkste locaties | Bron/config | Status |
|---|---|---|---|
| Gratis verzending vanaf €50 | shipping, cart, PDP, usp/trust-bar, footer, seo-meta, announcement | `settings.kb_free_shipping` (centraal) + hardcoded in copy | **REVIEW NODIG** (bedrag bevestigen; deels hardcoded) |
| Voor 16:00 besteld | shipping, PDP, faq, usp-bar, announcement, index | `settings.kb_order_cutoff` + copy | **REVIEW NODIG** |
| Morgen in huis / zelfde werkdag op de post | shipping, PDP, usp-bar, announcement, settings | `settings.kb_delivery` + copy | **REVIEW NODIG** (leverbelofte) |
| Levering NL 1-2 / BE 2-3 werkdagen | page.shipping | hardcoded copy | **REVIEW NODIG** |
| €4,95 verzendkosten < €50 | page.shipping | hardcoded copy | **REVIEW NODIG** |
| 30 dagen retour/bedenktijd | returns, retour-aanmelden, voorwaarden, faq | hardcoded copy | **REVIEW NODIG** (juridisch) |
| Terugbetaling binnen 14 dagen | returns, retour-aanmelden, voorwaarden | hardcoded copy | **REVIEW NODIG** |
| Niet-goed-geld-terug garantie | returns, complaints, faq, PDP (`guarantee_*`), klantenservice | PDP-settings + copy | **REVIEW NODIG** (garantie-belofte) |
| Klacht: reactie 1 werkdag / oplossing 14 dagen | complaints, voorwaarden | hardcoded copy | **REVIEW NODIG** |
| Track & trace per e-mail | shipping, bestelling-volgen | hardcoded copy | **REVIEW NODIG** (klopt dit operationeel?) |

### Privacy / legal bevindingen
- **Privacy policy** = Shopify Policy (`/policies/...`) — bestaat, maar **body is Engels** → NL-vertaling/controle; verifieer footer-link. (Niet nagebouwd als page — correct.)
- **Algemene voorwaarden** — dev-note in intro blijft gemarkeerd; inhoud niet zelf gefinaliseerd.
- **Bedrijfsgegevens-data (gebruikt op de nieuwe pagina, bron = privacy-policy):** Kwispelbox · Kwakkenbergweg 150-05 · 6571 GB Berg en Dal · Nederland · +31 242 340 422 · hallo@kwispelbox.com. KvK/btw = ontbreekt.

### Admin / publicatie-acties (nog door jou)
1. **Publiceren** van de 10 nieuwe concept-pagina's zodra content akkoord is (nu draft/isPublished=false).
2. **Menu's koppelen:** klantenservice + nieuwe service/legal/brand-pagina's toevoegen aan `footer-service` / `menu-over-ons` / `footer-shop` waar gewenst (ik heb menu's niet gewijzigd deze fase).
3. **Content review** afhandelen (zie lijst) + privacy NL-vertaling + bedrijfsgegevens KvK/btw.
4. **Claims bevestigen** (tabel hierboven) vóór livegang.
5. **Wishlist mg__wish**-beslissing (verwijderen of bouwen).

### Resterend voor Fase 3 (na jouw go)
Opruimen: `info-page.liquid` (ongebruikt), orphan `product.kwispelbox.json`, lege mega-menu-menu's, lege collecties (puppy/speeltjes/snacks/knuffels), menu-duplicaat Feestdagen/Momenten, mg__wish. Menu-integratie van de nieuwe pagina's.

_Einde Fase 2-log. Geen pagina's gepubliceerd, geen publicatiestatus gewijzigd, geen orphan/menu/collectie-cleanup uitgevoerd._
