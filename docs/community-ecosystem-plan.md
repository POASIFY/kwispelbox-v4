# Kwispelbox — Community & Ecosystem Plan (Fase 3A)

_Opgesteld: 01-10-2026 · **READ-ONLY** audit + architectuur + designplan. Er is niets gebouwd, geïnstalleerd, gepubliceerd of gemuteerd._
_Scope: hoe Kwispelbox doorgroeit van webshop naar merkplatform: Kwispelclub/loyalty, Kwispels, rijk account, hondprofielen, community/UGC, referrals/ambassadeurs, Partners, Zakelijk, lifecycle._

---

## 1. Executive summary

De storefront ziet er al uit als een merk met een loyaltyprogramma — maar **onder de motorkap bestaat er niets van**. "Kwispels", tiers, "50 Kwispels cadeau", ledenvoordelen: allemaal **statische marketingcopy** zonder ledger, zonder earning/redemption, zonder accountkoppeling. De customer-accounts zijn Shopify's **nieuwe (gehoste)** variant; er zijn **geen** customer-metafields, company-metafields of metaobjects. Er is dus een **schone lei** én een **compliance-risico** (we suggereren functionaliteit die niet bestaat).

**Kernaanbeveling:** bouw de loyalty-/account-laag **niet zelf** in Liquid. Kies een **hybride model**: een **loyalty-app als engine** (ledger, earning rules, rewards, referrals, account-hub in de nieuwe customer accounts) + **Shopify metaobjects/metafields als bron van waarheid voor hond-identiteit/verjaardag** + **een theme-presentatielaag** (Kwispelclub-pagina, nieuwe design family E) die het verhaal vertelt. Zakelijk en Partners worden **form-first funnels** (geen B2B-engine / geen aparte programma-infra bij launch). UGC en ambassadeurs zijn **Phase 2/3**.

**Grootste gate:** het loyalty-engine-besluit (app vs. custom) moet vallen **vóór** er ook maar één punten-UI wordt gebouwd, want de nieuwe customer accounts kun je niet vanuit het theme vullen — dat kan alleen de gekozen app (of een custom app met account-UI-extensions).

---

## 2. Current-state audit

| Onderdeel | Status nu | Backend? |
|---|---|---|
| Kwispels / punten | Statische copy in `header-group.json` (nav-dropdown: "Spaar Kwispels!", "50 Kwispels cadeau", tiers 200/400/600/1000), `kwispelclub-page` (benefit/step-blocks), `cart.liquid`, PDP `main-product-kwispelbox` | **Geen** |
| Kwispelclub | `page.kwispelclub` (`kwispelclub-page`: hero, 4 benefits, 5 steps, spotlight "Hond van de maand", 4 lege `member`-blocks, nieuwsbrief) | Marketing-only |
| Customer accounts | Shopify **new customer accounts** (gehost; geen `templates/customers/` in theme; `/account` op shopify.com) | Shopify-hosted |
| Customer data | **0** customer-metafield-definities, **0** company-metafields, **0** metaobjecten | Leeg |
| Hond-/verjaardagdata | Homepage `birthday-block`: `form 'customer'` → `contact[first_name]` (hondnaam) + `contact[note]` (datum) + tag `kwispelkalender` | Ongestructureerd (zit in klant-notitie/tag) |
| `loy_77036486821.js` | Wees-asset, zet enkel `ba_msg_active` in localStorage, nergens gerefereerd | Dood |
| Partners | `page.partners` (`partners-page`: perk/ptype/step) — marketing | Geen aanmeld-backend |
| Zakelijk | `main-menu "Zakelijk"` → `/pages/partners` (= **zelfde als Partners**) | Geen eigen funnel |
| Community/UGC | Homepage "Blije honden, blije baasjes" (statische foto-blocks) | Geen inzend/consent-flow |
| Loyalty-app | Geen integratie zichtbaar in theme | **Admin-check nodig** |

**Conclusie:** alles community/loyalty-gerelateerd is **presentatie zonder systeem**. Niets hoeft "ontward" te worden; alles moet nog worden **ontworpen en gekoppeld**.

---

## 3. Kwispelclub — propositie (herontworpen)

De club mag niet "een pagina die zegt dat je spaart" zijn, maar een **lidmaatschap dat commerce + community bindt**: *"Word gratis lid van de Kwispelclub — spaar Kwispels, vier de verjaardag van je hond, en krijg als eerste toegang tot nieuwe boxen."*

**Pijlers:** (1) sparen & belonen (Kwispels), (2) verjaardag van je hond, (3) ledenvoordelen (early access special editions), (4) community (Hond van de maand, verhalen), (5) referrals.

**Member lifecycle:** ontdekken → gratis lid (account) → welkomstbonus → sparen bij aankoop/acties → inwisselen → verjaardag-reward → ambassadeur/referral.

- **LAUNCH MVP:** gratis lid = account; echte Kwispels (engine); welkomstbonus; punten bij aankoop; 2-3 redemptions; verjaardag-capture gekoppeld; geloofwaardige clubpagina die alléén belooft wat live is.
- **PHASE 2:** hondprofielen, verjaardag-reward-automation, referrals, review/UGC-earning, "Hond van de maand" echt.
- **PHASE 3:** tiers/VIP, ambassadeursprogramma, challenges/campagnes, segmentatie.

**Anti-overengineering:** geen tiers, geen gamification-dashboard, geen ambassadeurs bij launch.

---

## 4. Naamgeving — "Kwispels" vs "Kwispelpunten"

**Aanbeveling: "Kwispels"** als valutanaam + **"Kwispelclub"** als programma. "Kwispels" is speelser, merkbaarder en al in gebruik. "Kwispelpunten" is beschrijvender maar redundant naast "Kwispels" → **niet** beide gebruiken.

**Uitlegbehoefte opvangen** met hybride taal bij eerste gebruik / in de UI-tooltip: *"Spaar Kwispels — de punten van de Kwispelclub."* Daarna consequent "Kwispels". In cart/checkout/account: "Kwispels" + waarde-equivalent tonen ("250 Kwispels = €5"). Caveat: of de gekozen app de currency vrij laat hernoemen is een **selectiecriterium** (zie §15).

---

## 5. Loyalty-architectuur — opties

**A. Loyalty-app** (Smile / Rivo / LoyaltyLion): engine + ledger + rewards + referrals + **account-hub in de nieuwe customer accounts** out-of-the-box. Snel, onderhoudsarm; minder design-vrijheid, maandkosten, enige lock-in.

**B. Custom Shopify-stack:** customer-metafields (balance) + Shopify **Functions** (korting/redemption) + **Flow** (earning-triggers) + **Customer Account UI extensions** (een custom app voor de account-UI) + webhooks. Maximale controle/branding, geen app-fee — maar **substantiële build + onderhoud**, en de account-UI vereist hoe dan ook een **custom app** (new customer accounts zijn niet vanuit Liquid te vullen).

**C. Hybride (aanbevolen):** app als **ledger/earning/redemption/referral-engine + account-hub**, met een **eigen Kwispelbox-presentatielaag** in het theme (clubpagina, "zo werkt het", rewards-showcase via family E) en **Shopify metaobjects/metafields als bron van waarheid voor hond-identiteit/verjaardag**, die via Flow/e-mail (Klaviyo later) verjaardag-rewards triggeren.

**Beoordeling nieuwe customer accounts:** Liquid-themepagina's kunnen **geen** account-dashboard renderen. Verrijking kan alleen via **Customer Account UI extensions** (app) of de **Loyalty Hub** die moderne loyalty-apps daar injecteren. Dit maakt optie A/C praktisch noodzakelijk tenzij we een custom app bouwen.

---

## 6. Aanbevolen technische architectuur

**HYBRIDE:**
1. **Loyalty-engine = app.** Shortlist Smile.io of Rivo (zie §15). App levert: points-ledger, earning rules, redemption→Shopify-korting, referrals, en de **Loyalty Hub binnen de nieuwe customer accounts** (zodat "Mijn account" punten/rewards toont zonder custom build).
2. **Hond-identiteit = Shopify-native.** `metaobject` "dog" (naam, geboortedatum, optioneel formaat/foto) + `customer`-metafield-referentie (1→n honden). Dit is **onze** bron van waarheid (geen lock-in bij de app).
3. **Verjaardag-automation.** Shopify **Flow** (of Klaviyo later) leest dog-geboortedatum → triggert verjaardag-reward via de app-API / kortingscode.
4. **Presentatielaag = theme (family E).** Clubpagina, "zo werkt het", rewards-showcase, referral-uitleg — marketing, linkt naar de app-hub/account.
5. **Zakelijk & Partners = Shopify forms** (lead/offerte), geen engine.

**WHY:** snelste geloofwaardige launch, account-UI opgelost door de app, hond-data in eigen hand (portabel), theme behoudt merkgevoel. **RISKS:** app-fee, enige lock-in op de ledger, app-branding-limieten. **EXIT:** omdat hond-data in Shopify-metaobjecten staat en punten-saldo via de app-API exporteerbaar is, is migratie naar een andere app of custom-stack mogelijk (zie §23).

---

## 7. Account-architectuur

| Laag | Nu mogelijk | Hoe |
|---|---|---|
| Login, bestellingen, NAW | ✅ standaard | Shopify new customer accounts (hosted) |
| Kwispels-saldo, earning-historie, rewards, referralcode | ⚠️ niet in Liquid | **Loyalty-app Loyalty Hub** in customer accounts, óf custom app met **Customer Account UI extensions** |
| Hond(en): naam/verjaardag/voorkeuren | ⚠️ data wel native, UI niet in Liquid | metaobject + customer-metafield als bron; **beheer-UI** via account-UI-extension (app) of (MVP) via een theme-formulier dat metafields schrijft met een klein custom app-endpoint |
| Member/community-status | Phase 2/3 | App/segmenten |

**Belangrijk:** "Mijn account = alleen bestellingen" blijft prima voor launch. De rijke "Mijn Kwispelbox" ontstaat zodra de loyalty-app live is (die levert de hub). Een volledig custom account-dashboard = **custom app**, niet verstandig vóór tractie.

---

## 8. Hondprofiel / birthday-datamodel

**Bewust minimalistisch (privacy-by-design):**

`metaobject: dog`
- `name` (single_line)
- `birthdate` (date) — alléén dag/maand nodig voor verjaardag; jaar optioneel
- `size` (enum Mini/Happy/Mega-relevant) — optioneel, alleen als het personalisatie/aanbeveling voedt
- `photo` (file) — optioneel, Phase 2
- `owner` → `customer` referentie

`customer`-metafield `custom.dogs` = list.metaobject_reference (meerdere honden).

**Keuze metaobject (niet losse metafields):** herbruikbaar, meerdere honden, relateerbaar, en los van de loyalty-app (portabel). **Birthday automation:** Flow/Klaviyo op `birthdate`. **Let op:** dit is **hond**-verjaardag, geen klant-verjaardag — loyalty-apps hebben native vaak alleen *customer* birthday; hond-birthday = **custom data + custom automation** (selectiecriterium §15). **Migratie bestaande capture:** de homepage `birthday-block` schrijft nu naar klant-notitie/tag; later ombouwen naar metaobject (Phase 2, niet nu).

**Geen** onnodige data (ras alleen als het echt iets voedt; geen medische data). Consent bij foto/UGC (§18).

---

## 9. Rewards / earning framework

**Earning-acties — beoordeeld (⟢ = MVP-kandidaat):**
| Actie | Waarde klant | Fraude/risico | Oordeel |
|---|---|---|---|
| ⟢ Aankoop (per €) | Hoog | Laag | MVP |
| ⟢ Account aanmaken (welkomstbonus) | Hoog | Laag (1×/klant) | MVP |
| ⟢ Eerste bestelling | Midden | Laag | MVP |
| Verjaardag hond toevoegen | Midden | Laag-midden (nep-honden) → cap | Phase 2 |
| Verjaardag hond (jaarlijkse reward) | Hoog (emotioneel) | Midden → 1×/jaar/hond cap | Phase 2 |
| Review achterlaten | Midden | Midden (Judge.me-koppeling, verify-only) | Phase 2 |
| UGC/foto insturen | Midden | Midden (moderatie) | Phase 2/3 |
| Nieuwsbrief opt-in | Laag | Laag (double opt-in, 1×) | Phase 2 (juridisch correct) |
| Referral | Hoog | Midden-hoog (self-referral) → engine-fraudecheck | Phase 2 |

**Redemption-types:** vaste korting (€/%); gratis cadeau-extra (bestaande `cadeau-extras`-producten!); gratis kaartje; mystery gift (sluit aan op bestaande GWP); early access special edition; verjaardag-reward. **Geen puntenwaarden** vastgelegd (zie §16).

---

## 10. Referral-architectuur

**Concept:** lid deelt unieke link → nieuwe klant krijgt welkomskorting → referrer krijgt Kwispels/korting **na geldige (niet-geretourneerde) eerste order**. **Engine:** de gekozen loyalty-app (Smile/Rivo/LoyaltyLion hebben referrals native, inclusief fraudepreventie + reward-afhandeling) — **niet** zelf bouwen. **Scheiding:**
- **Customer referral** (vriend-werft-vriend) = app-feature, Phase 2.
- **Creator/ambassador program** (influencers/retail met codes, hogere beloning, afspraken) = **aparte** business rules/affiliate-tooling, Phase 3 — **niet samenvoegen** met customer-referral.

---

## 11. UGC / community-architectuur

**Aanbeveling: gecureerd, niet een live social-feed.** Een ingebedde Instagram-feed = performance- + privacy- + afhankelijkheidsrisico. Beter:
- **Nu:** handmatig gecureerde "Blije honden"-sectie (bestaat al).
- **Phase 2:** inzend-flow (formulier: foto-upload + **expliciete consent-checkbox** voor gebruik) → opslag als `metaobject: ugc_submission` (status: pending/approved) → gecureerde gallery rendert approved items. Moderatie handmatig.
- **Phase 3:** "Hond van de maand", member stories, challenges, koppeling met Kwispels (earning voor approved UGC).

**Rechten/consent:** UGC alleen gebruiken met vastgelegde toestemming (checkbox + bewaarde timestamp); minderjarigen/identificeerbare personen vermijden; recht op verwijdering.

---

## 12. Partners — strategie

**Eén centrale partnerhub** (`page.partners`, family E later), categoriseerbaar maar niet gefragmenteerd in 4 programma's bij launch:
- **"Onze partners"** (logo-/kaart-grid, social proof) + **"Samenwerken met Kwispelbox"** (waarom + verwachtingen + selectiecriteria) + **aanvraagflow** (formulier).
- **Partner-types** als dataveld (merk/leverancier · creator/ambassadeur · retail/verkooppartner · maatschappelijk) → één `metaobject: partner` met `type`-enum; zo kun je later splitsen **zonder** nu aparte infra.
- **Aanvraag** = `metaobject: partner_application` of simpel e-mail/Shopify-form (MVP). Cases/testimonials = Phase 2.

**Partners ≠ Zakelijk** (zie §13) — menu-item "Zakelijk" moet losgekoppeld worden van `/pages/partners` (IA, §15/§12 → nav).

---

## 13. Zakelijk bestellen — strategie (los van Partners)

**Eigen warme commerciële funnel** (geen corporate uitstraling), **form-first** bij launch:

Funnel: (1) use-case (personeel/klant/relatie/event/onboarding) → (2) aantal boxen → (3) gelegenheid → (4) gewenste box/budgetrange → (5) personalisatie → (6) gewenste leverdatum → (7) bedrijfsgegevens → (8) contact → (9) opmerkingen → (10) **aanvraag/offerte versturen**.

- **LAUNCH:** nette aanvraag-/offerteflow via **formulier** (→ e-mail/lead). Geen prijzen/staffels.
- **LATER (alleen bij volume):** echte **quote-engine**, **Shopify B2B** (company accounts, catalogus, betalingsvoorwaarden), staffelprijzen, bulk-checkout. **Wanneer zinvol:** bij herhaalde grote orders (indicatief >25-50 boxen) of terugkerende zakelijke klanten die zelf willen bestellen/factureren. **Geen staffelprijzen verzinnen.**

---

## 14. Community design family (E) — specificatie (niet bouwen)

**`sections/*` family E — Community/Ecosystem**, zelfde tokens (crème/chocolade/groen/roze/geel-oranje, Lilita display + Karla, rounded, layered, premium-speels — **geen kinderachtig dashboard**).

Componenten (editor-blocks):
- **membership hero** (propositie + CTA "word gratis lid")
- **points/progress summary** (presentatie; echte data via app-hub)
- **benefit cards** / **reward cards** (icoon + titel + kosten-in-Kwispels)
- **"zo werkt het"** stappen (sparen→inwisselen)
- **member journey / badge-status** (Phase 2/3)
- **partner cards / logo-grid**
- **B2B use-case cards** + **quote/request CTA**
- **referral module** (deel-je-link, presentatie)
- **community gallery** (gecureerd) + **UGC-submission CTA**
- **FAQ** (hergebruik help-hub-accordion-stijl)
- **proof/metrics** (Phase 2, alleen echte cijfers)

Bouwprincipe gelijk aan A/B/C/D: één sectie, vaste zones, typed blocks, geen JS tenzij nodig.

---

## 15. Loyalty-app vs build — vergelijking

_Actueel onderzoek okt 2026 (zie bronnen onderaan). Exacte prijzen/limieten = **verifiëren in-app vóór keuze**._

| Criterium | **Smile.io** | **Rivo** | **LoyaltyLion** | **Custom Shopify-stack** |
|---|---|---|---|---|
| New customer accounts | ✅ Loyalty Hub in accounts | ✅ compatibel + 8 checkout-extensies | ✅ | ⚠️ zelf bouwen (UI-extensions) |
| UI-extensibility/branding | Midden | Midden-hoog (native theme) | Hoog | Volledig |
| Referrals | ✅ | ✅ | ✅ | zelf bouwen |
| Rewards/redemption → Shopify-korting | ✅ | ✅ | ✅ | Functions |
| Customer birthday | ✅ | ✅ | ✅ | metafield+Flow |
| **Hond**-birthday (custom event/data) | ⚠️ custom data/API | ⚠️ custom data/API | ⚠️ custom data/API | ✅ (eigen model) |
| API/webhooks/export | ✅ | ✅ | ✅ (sterk) | n.v.t. |
| Flow-integratie | ✅ | ✅ | ✅ | ✅ |
| Markets / NL-BE | ✅ (verify valuta/locale) | ✅ | ✅ | ✅ |
| GDPR | app-DPA (verify) | app-DPA (verify) | app-DPA (verify) | eigen beheer |
| Kostenklasse | gratis start → $$ schaalt | vanaf ~$49/mnd | $$$ (enterprise/multi-store) | geen fee, hoge build |
| Implementatiecomplexiteit | Laag | Laag | Midden | Hoog |
| Vendor lock-in | Midden | Midden | Midden-hoog | Geen |
| Best voor | grootste ecosystem, snelle start | Shopify-exclusive, referrals+checkout, lage instap | multi-store/enterprise analytics | maximale controle, later |

**RECOMMENDED ARCHITECTURE:** **Hybride met Smile.io óf Rivo** als engine (NL-SMB-schaal): **Smile** als je het grootste/bewezen ecosystem + gratis start wilt; **Rivo** als je Shopify-exclusieve native integratie + sterke referrals/checkout-extensies + voorspelbare instapprijs wilt. **LoyaltyLion** pas bij enterprise/multi-store ambitie. Hond-data **altijd** in eigen Shopify-metaobjecten. **WHY/RISKS/EXIT:** zie §6/§23.

---

## 16. Open business-decisions (waarden door jullie, niet door mij)

| Beslissing | Waarom nodig | Wanneer | Impact |
|---|---|---|---|
| Waarde van 1 Kwispel (bijv. x Kwispels = €1) | Fundament economics | Vóór engine-config | Hoog |
| Earn-ratio (Kwispels per €) | Marge vs. aantrekkelijkheid | Vóór launch | Hoog |
| Reward-drempels | Redemption-aanbod | Vóór launch | Hoog |
| Welkomstbonus (ja/hoeveel) | Aanmeld-incentive | Vóór launch | Midden |
| Vervaldatum punten (ja/nee) | Liability + activatie | Vóór launch | Midden |
| Punten bij refunds (intrekken?) | Fraude/marge | Vóór launch | Midden |
| Verjaardag-reward (type/waarde) | Signature-feature | Phase 2 | Midden |
| Referral-reward (beide zijden) | Groei vs. kosten | Phase 2 | Hoog |
| Retroactieve punten bestaande klanten | Goodwill | Bij launch | Midden |
| Meerdere honden/klant (max?) | Datamodel/fraude | Phase 2 | Laag |
| Tiering ja/nee | Complexiteit | Phase 3 | Midden |
| Zakelijk minimum-aantal | Funnel-kwalificatie | Vóór Zakelijk-launch | Midden |
| Partner-acceptatiecriteria | Kwaliteit/merk | Vóór Partner-launch | Midden |
| UGC-incentive (Kwispels voor foto?) | Misbruik vs. groei | Phase 2/3 | Laag |

**Geen waarden ingevuld.**

---

## 17. Marketing-only vs backend — matrix

| Feature | Marketing-only kan | Backend nodig | Techniek | MVP? |
|---|---|---|---|---|
| Kwispelclub-landingspagina | ✅ | — | theme (family E) | ✅ |
| Points balance (tonen) | — | ✅ | loyalty-app hub | ✅ |
| Points earning | — | ✅ | app rules/Flow | ✅ |
| Redemption | — | ✅ | app → Shopify-korting | ✅ |
| Welkomstbonus | — | ✅ | app | ✅ |
| Birthday reward (hond) | deels (capture) | ✅ | metaobject + Flow + app | V2 |
| Dog profile (1) | capture ✅ | ✅ voor opslag/UI | metaobject + account-ext | V2 |
| Multiple dogs | — | ✅ | metaobject-list | V2 |
| Referral | uitleg ✅ | ✅ | app | V2 |
| Ambassador | uitleg ✅ | ✅ | aparte tooling | V3 |
| UGC gallery (gecureerd) | ✅ | optioneel | theme + metaobject | V1/V2 |
| UGC submission | — | ✅ | form + metaobject + consent | V2 |
| Partner application | formulier ✅ | licht | Shopify form/metaobject | V1 |
| Zakelijke aanvraag | ✅ | licht | Shopify form | V1 |
| Business quote-engine | — | ✅ | B2B/custom | V3 |
| Reward history | — | ✅ | app hub | V1/V2 |
| Account dashboard (rijk) | — | ✅ | app hub / custom app | V2 |

---

## 18. Datamodel (conceptueel)

| Entity | Data | Shopify-native opslag | Externe app/DB? | Source of truth | Privacy |
|---|---|---|---|---|---|
| Customer | NAW, login | Shopify customer | — | Shopify | standaard |
| Dog | naam, geboortedatum, (size/foto) | **metaobject** + customer-metafield-ref | nee | **Kwispelbox (Shopify)** | minimaal; foto=consent |
| LoyaltyAccount | saldo, status | — | **loyalty-app** | app | app-DPA |
| PointsTransaction | earn/redeem ledger | — | **loyalty-app** | app | app-DPA |
| Reward | catalogus/kosten | app (evt. mirror in metaobject voor presentatie) | app | app | laag |
| Redemption | ingewisseld → korting | app → Shopify discount | app | app | laag |
| Referral | code, status, payout | — | **loyalty-app** | app | midden |
| UGCSubmission | foto, consent, status | **metaobject** | nee | Kwispelbox | **consent vereist** |
| Partner | naam, type, logo, status | **metaobject** | nee | Kwispelbox | laag |
| PartnerApplication | aanvraag-velden | metaobject of e-mail/form | nee | Kwispelbox | zakelijk |
| BusinessLead | funnel-velden | Shopify form/e-mail (evt. metaobject) | nee | Kwispelbox | zakelijk |

**Principe:** identiteit/content die van óns is (hond, UGC, partner, lead) → **Shopify metaobjects** (portabel); loyalty-ledger → **app** (gespecialiseerd, exporteerbaar).

---

## 19. Privacy / GDPR

- **Dataminimalisatie:** alleen hond-naam + verjaardag (dag/maand); geen jaar/ras tenzij functioneel; geen medische data.
- **Consent:** foto/UGC alleen met expliciete, gelogde toestemming + intrekbaar; nieuwsbrief-earning via **double opt-in**.
- **Verwerkers:** loyalty-app = subverwerker → **DPA** nodig + opnemen in privacy-policy; data-locatie (EU) verifiëren.
- **Privacy-policy** nu Engels → NL + loyalty/UGC-verwerking toevoegen (hangt samen met Fase 2 legal-cleanup).
- **Kinderen:** UGC met identificeerbare personen vermijden.
- **Recht op verwijdering:** hond/UGC/punten moeten verwijderbaar zijn (Shopify GDPR-webhooks + app-ondersteuning).

---

## 20. Gefaseerde roadmap

**COMMUNITY MVP (V1)** — echte Kwispelclub-propositie; **gekozen loyalty-engine** live; basis earning (aankoop + welkomstbonus) + 2-3 redemptions; account toont punten (app-hub); Kwispelclub-pagina + family E (presentatie); Zakelijk **lead-form**; Partner-pagina + **aanvraagformulier**; **statische loyalty-copy veilig maken** (§21).
**COMMUNITY V2** — hondprofielen (metaobject) + verjaardag-reward-automation; referrals (app); UGC-submission + gecureerde gallery; rijkere rewards; review-earning (Judge.me).
**COMMUNITY V3** — ambassadeursprogramma; tiers/VIP; segmentatie; Zakelijk B2B/quote-engine; challenges/campagnes.

---

## 21. Huidige misleidende/static copy — audit (vóór live)

| Copy | Locatie | Impliceert | Echt actief? | Actie |
|---|---|---|---|---|
| "Spaar Kwispels!" / "50 Kwispels cadeau" | `header-group.json` (nav-dropdown, meerdere) | Werkend spaarsysteem + welkomstbonus | **Nee** | **REWRITE ALS "BINNENKORT"** of **HIDE BEFORE LIVE** tot engine live |
| Tiers "200/400/600/1000 Kwispels" | `header-group.json` | Reward-catalogus | **Nee** | HIDE/COMING SOON tot waarden bepaald |
| "Spaar Kwispels" benefit + "Verzamel Kwispels (aankopen, reviews, acties)" step | `kwispelclub-page` | Earning-rules | **Nee** | REWRITE ALS "BINNENKORT" tot MVP live |
| "Wissel in voor beloningen" | `kwispelclub-page` | Redemption | **Nee** | COMING SOON |
| Kwispels-referentie in cart | `cart.liquid` | Sparen bij checkout | **Nee** | Verify + HIDE/COMING SOON |
| Kwispels op PDP | `main-product-kwispelbox` | Punten per aankoop | **Nee** | Verify + HIDE/COMING SOON |
| "Hond van de maand" spotlight | `kwispelclub-page` | Lopende community-actie | **Nee** | KEEP FOR LATER / COMING SOON |
| FAQ "Kwispelclub: spaar Kwispels…" | `page.faq` | Werkend programma | **Nee** | REWRITE ALS "BINNENKORT" |

**Regel:** op live **geen** niet-bestaande loyalty-functionaliteit suggereren. Aanbevolen launch-tactiek: Kwispelclub positioneren als **"gratis lid worden — sparen start binnenkort"** tot de engine live is, óf de engine in de MVP meteen live zetten en de copy waarmaken. (Uitvoering = latere batch, niet nu.)

---

## 22. Dependencies / risks

- **Gate:** engine-keuze blokkeert alle punten-UI. Beslis §15 + §16 eerst.
- **New customer accounts** beperken theme-UI → afhankelijk van app-hub of custom app.
- **Hond-birthday ≠ customer-birthday** → custom data/automation (niet native in apps).
- **App-kosten/lock-in** + DPA/EU-datalocatie.
- **Copy-risico nú live** (§21) — grootste korte-termijnrisico.
- **Zakelijk/Partners menu-koppeling** verwart nu (zelfde pagina).
- **Fraude** (referrals/nep-honden/UGC) → caps + app-fraudecheck + moderatie.

---

## 23. Exit / migratie-strategie

- **Hond/UGC/partner/lead** staan in **Shopify metaobjects** → blijven bij ons, los van elke app.
- **Punten-ledger** bij de app → kies een app met **export/API** (Smile/Rivo/LoyaltyLion bieden dit) zodat saldi te migreren zijn naar een andere app of custom-stack.
- **Rewards = Shopify-kortingen** → engine-agnostisch.
- **Presentatielaag (family E)** is theme-eigen → onafhankelijk van de engine.
- Zo blijft de **lock-in beperkt tot de ledger**, en is een latere overstap (app↔app of app→custom) uitvoerbaar.

---

## Informatie-architectuur (IA) — voorstel (§12 nav-impact)

**Header:** Boxen · Verjaardag · Feestdagen · Momenten · **Kwispelclub** (evt. submenu: Zo werkt het / Voordelen / Mijn Kwispels) · **Zakelijk** (eigen funnel, **loskoppelen van Partners**). **Partners** → verplaatsen naar **footer** (+ evt. "Over ons"-submenu), niet in hoofd-nav bij launch.
**Footer-groepen:** Shop · Klantenservice · **Kwispelclub** · **Zakelijk & Partners** · Over Kwispelbox · Juridisch.
**Account:** ontdekking van punten/hond/rewards/referral loopt via de **app-hub** in new customer accounts (+ CTA's vanaf Kwispelclub-pagina).
**Mobile drawer:** compact houden — Kwispelclub als hoofd-item, Partners/Zakelijk onder een "Meer/Over"-groep.
**Bottom-nav:** Home / Boxen / Shop / Club — **beoordeling:** "Club" is verdedigbaar als Kwispelclub een kernpijler wordt; **niet nu wijzigen** (bevestigen zodra club-MVP live is).

---

## Bronnen (actueel onderzoek, okt 2026)
- Smile.io — loyalty apps / VIP / Plus overzichten (smile.io/learn)
- Rivo — vergelijkingen & customer-accounts/checkout-extensies (rivo.io/blog)
- LoyaltyLion-positionering (via vergelijkingen) + diverse 2026-marktoverzichten
_Exacte features/prijzen/limieten en EU-datalocatie: **in-app/Admin verifiëren vóór keuze**._

---

_Einde Fase 3A-plan._

---

# FASE 3B — Loyalty Vendor Decision + Community MVP Specification

_01-10-2026 · READ-ONLY. Vendorkeuze-onderbouwing staat apart in **`docs/loyalty-vendor-decision.md`**. Hieronder de uitvoerbare MVP-spec. Geen harde waarden, niets gebouwd._

> **⚠️ CORRECTIE (3B.1, 01-10-2026):** de oorspronkelijke 3B-aanbeveling (**Rivo Scale ~$49 + Rivo Accounts + Liquid-metafields**) is **materieel gecorrigeerd** na verificatie tegen officiële Rivo-bronnen. Rivo-**metafields/Developer Toolkit** zitten op **Plus ($499)** (UNCLEAR, leunt Plus) en **"Rivo Accounts" is een apart product ($499)** — níét in Scale. De rijke/branded "Mijn Kwispelbox" kost dus **~$500-1000/mnd**, buiten budget. **Herziene aanbeveling: Smile voor de MVP (Essential $15 / Standard $79, vendor-rendered account-hub), Rivo als premium-later.** Zie `loyalty-vendor-decision.md` §3B.1. Waar hieronder "balance via metafield" of "Rivo Accounts op Scale" staat → geldt de 3B.1-correctie: in de **lean MVP géén custom Liquid-balance**, alleen een **vendor-rendered** account-widget + Kwispelclub-marketingpagina.

## 3B.1 MVP earning-spec (functioneel, zonder waarden)

**Purchase** — earned op **order PAID** (niet fulfilled), zodat annulering vóór betaling niets toekent. **Refund/cancel:** earned Kwispels **terugboeken** pro rata (webhook-driven). **Grondslag:** over **productsubtotaal excl. verzending/btw** en **ná** reeds toegepaste kortingen (geen punten over verzendkosten/btw/korting-deel). **Gift cards:** geen earning op gift-card-aankoop. **Guest checkout:** geen earning (vereist account/enrollment); evt. retroactief toekennen bij latere account-aanmaak met zelfde e-mail (vendor-afhankelijk, V2).

**Welcome** — bij **programma-enrollment** (gratis lid), **1×/klant**, fraudepreventie via account-uniek + e-mailverificatie. Niet per login herhaalbaar.

**Dog birthday (V2, technisch voorbereiden)** — vereist `metaobject dog.birthdate`; **lead time** (reward enkele dagen vóór de datum beschikbaar); **1×/jaar/hond** cap; dubbele honden → dedupe/cap; **birthdate-wijziging vlak vóór datum** → cooldown om misbruik te voorkomen. Trigger via Flow/Klaviyo → vendor-API (zie vendor-doc §5).

_Geen numerieke waarden (zie §16 open decisions + §3B.8 economics-kader)._

## 3B.2 MVP redemption-spec (beste 2-3 types)

Aanbevolen MVP-set: **(1) vaste korting**, **(2) gratis cadeau-extra**, **(3) gratis mystery-gift** — allen via de vendor als Shopify-korting/free-product.

| Reward | Mechanisme | Cart/checkout-UX | Voorraad | Refund | Combineerbaar | Misbruik |
|---|---|---|---|---|---|---|
| Vaste korting | vendor → Shopify discount-code/automatic | code/auto in cart | n.v.t. | punten terug bij refund | **niet** stapelen op GWP/andere promo zonder test | 1 actieve redemption/cart |
| Gratis cadeau-extra | free-product reward uit bestaande `cadeau-extras` | product toegevoegd à €0 | **echte voorraad** nodig (tracked) | retour → punten terug | los van GWP houden | cap per order |
| Gratis mystery-gift | **apart** loyalty-gift-product (zie ⚠️) | product à €0 | aparte variant | idem | **nooit** met GWP-product delen | 1×/redemption |

**⚠️ Conflict met bestaande GWP (gratis verrassing vanaf €89):** de huidige BXGY-korting voegt automatisch `gratis-mystery-verrassing` toe bij ≥€89. Een loyalty-mystery-reward mag **niet hetzelfde product/variant** gebruiken, anders botsen twee mechanismen (dubbel toevoegen / prijsconflict / `window.kbGwp`-sync raakt in de war). **Oplossing:** aparte **"Kwispelclub-verrassing"** als eigen product/variant, uitsluitend door de loyalty-engine toegekend; GWP-product blijft exclusief voor de €89-BXGY. Zo blijven de twee reward-mechanismen gescheiden.

**Combinability-regel (MVP):** één loyalty-redemption per order; loyalty-korting **niet** stapelen op de €89-GWP of gratis-verzending-drempel zonder expliciete test (anders margelek + verwarrende cart).

## 3B.3 Account-UX — "Mijn Kwispelbox" (toekomst, geen code)

```
LOGIN → MIJN KWISPELBOX
  [Welkom / member-status]      ← vendor hub (Shopify native shell)
  [Kwispels-balance]            ← LOYALTY APP (metafield ook op storefront)
  [Volgende reward / progress]  ← LOYALTY APP
  [Beschikbare rewards]         ← LOYALTY APP (inwisselen = app-UI)
  [Rewards-historie]            ← LOYALTY APP (API)
  [Mijn bestellingen]           ← SHOPIFY NATIVE
  [Mijn hond(en) + verjaardag]  ← CUSTOM (metaobject; beheer-UI = account-extension/LATER)
  [Referral-link]               ← LOYALTY APP (V2)
  [Profiel/instellingen/consent]← SHOPIFY NATIVE
```
Per module source: **native** (orders, profiel), **loyalty app** (balance, rewards, referral), **custom** (honden), **LATER** (rijke hond-beheer-UI). **Mobiel:** balance + volgende reward bovenaan (samenvatting-first), daarna rewards, honden, bestellingen; **desktop:** 2-koloms (samenvatting/rewards links, honden/bestellingen rechts). Géén fictief dashboard dat technisch niet kan.

## 3B.4 Storefront loyalty-touchpoints

| Touchpoint | Weergave | Fase |
|---|---|---|
| Header | **géén** constante balance (rommelig); evt. subtiele "Kwispelclub"/account-indicator | MVP (indicator) |
| Kwispelclub-pagina | programma-intro + (ingelogd) balance via metafield + earning-uitleg + rewards + join/login-CTA | **MVP** |
| PDP | "verdien X Kwispels" | **LATER** (alleen als engine realtime betrouwbaar) |
| Cart | punten-indicatie / redemption | LATER (voorkom GWP/gratis-verzending-conflict) |
| Cart-drawer | niet overladen | NIET aanbevolen |
| Post-purchase (thank-you/e-mail) | "je verdiende … Kwispels" / status | MVP (indien vendor-tier) |

## 3B.5 Family E — component-spec

| Component | Doel | Databron | Ingelogd/uit | Mobiel | Backend | Fase |
|---|---|---|---|---|---|---|
| Club-hero | propositie + join-CTA | theme | beide | stack | — | MVP |
| Points-balance | saldo tonen | loyalty-metafield | ingelogd (uit: join-CTA) | compact top | app | MVP |
| Progress-module | volgende reward | app | ingelogd | bar | app | V2 |
| Reward-cards | inwisselbare rewards | app/theme | beide | grid→stack | app | MVP |
| Earning-cards | zo verdien je | theme | beide | grid | — | MVP |
| Benefit-cards | ledenvoordelen | theme | beide | grid | — | MVP |
| Birthday-block | hond-verjaardag capture | metaobject/form | ingelogd | stack | custom | V2 |
| Dog-profile-teaser | honden tonen | metaobject | ingelogd | stack | custom | V2 |
| Referral-module | deel-je-link | app | ingelogd | stack | app | V2 |
| Partner-cards | partners/logo's | metaobject | beide | grid→scroll | native | V1/V2 |
| Business-usecase-cards | zakelijk | theme | beide | grid | — | MVP |
| Community-gallery | UGC (gecureerd) | metaobject | beide | carousel | native | V1/V2 |
| CTA/support | contact/join | theme | beide | — | — | MVP |
| FAQ | help (hergebruik help-hub-accordion) | theme | beide | accordion | — | MVP |

## 3B.6 Partners IA (definitief)

**Pagina "Partners"** (family E/editorial), secties: hero → waarom samenwerken → **partnercategorieën** (merken/leveranciers · creators/ambassadeurs · retail/verkoop · maatschappelijk-optioneel) → huidige partners (logo-grid, LATER) → voordelen → hoe de samenwerking werkt → **aanvraag-CTA** → FAQ. **Geen** aparte sites per type in MVP; type = dataveld op `metaobject partner`. **Aanvraagformulier-velden (minimaal):** bedrijf/naam, type (select), website/social, contactpersoon, e-mail, korte toelichting, consent. (Geen onnodige data.)

## 3B.7 Zakelijk IA (los van Partners)

**Pagina "Zakelijk bestellen"** (service/editorial), user-flow: landing → use-cases → mogelijkheden/voorbeelden → **aanvraag/offerte** → success/follow-up. **Lead-/offerteformulier-velden:** bedrijf · contactpersoon · e-mail · telefoon (optioneel) · use-case (select) · aantal-indicatie · budgetrange (optioneel) · gelegenheid · personalisatie-interesse · gewenste leverdatum · land (NL/BE) · opmerkingen · **consent/privacy**. **Geen** quote-engine/staffels in MVP (B2B/Shopify-B2B pas bij volume — zie 3A §13).

## 3B.8 Kwispels economics-kader (variabelen, geen cijfers)

Besliskader om later met échte data te vullen:
```
earn_rate           = kwispels_per_euro            (keuze)
redemption_value    = euro_per_reward / kwispels_cost_reward
effective_reward_%  = redeemed_reward_value / eligible_revenue     ← kerngetal marge-impact
breakage            = (earned_points - redeemed_points) / earned_points
reward_cost_impact  = effective_reward_% + free_gift_COGS% + extra_shipping%
welcome_liability   = new_members × welcome_points × redemption_value_per_point
birthday_liability  = active_dogs × birthday_reward_value × redemption_rate
margin_after_loyalty= gross_margin% − reward_cost_impact
```
Richtlijn: stuur op **effective_reward_%** binnen een door jullie gekozen marge-plafond; houd rekening met **breakage** (niet alle punten worden ingewisseld) en **free-gift COGS + verzendimpact**. **Geen verzonnen Kwispelbox-getallen** — invullen bij 3C met AOV/herhaalaankoop/marge-data.

## 3B.9 Community / UGC MVP
**MVP = gecureerd** (bestaande "Blije honden"-sectie blijft). **V2 = submission-flow** (`metaobject ugc_submission`: foto + **expliciete consent** + status pending/approved; handmatige moderatie; recht op verwijdering; retentie-afspraak; attributie @handle optioneel). **Geen** live Instagram-feed als default (performance/privacy/afhankelijkheid).

## 3B.10 Referrals
**V2** (tenzij vendor het vrijwel gratis/veilig maakt — Rivo/Smile hebben native referrals). Spec: referrer-link → referred-customer → **reward pas na geldige (niet-geretourneerde) eerste order** → refund draait reward terug → **self-referral/duplicate-account-blokkades** via engine. **Customer-referral ≠ creator/ambassador** (laatste = aparte business rules, V3). Geen rewardwaarden.

## 3B.11 Pre-live static-copy actielijst (exact)

**Regel:** zolang de engine niet live/getest is (earning + refunds + redemption), **geen** actieve spaarbelofte op live.

| Locatie | Huidige copy | Impliceert | Status | Aanbevolen pre-live |
|---|---|---|---|---|
| `sections/header-group.json` (nav-dropdown, meerdere) | "Spaar Kwispels!", "50 Kwispels cadeau", campaign-highlight | werkend sparen + welkomstbonus | **HIDE / COMING SOON** | verberg club-dropdown-rewards of herschrijf "Kwispelclub — binnenkort sparen" |
| `sections/header-group.json` | tiers "200/400/600/1000 Kwispels" | reward-catalogus | **HIDE** | verbergen tot waarden + engine live |
| `templates/page.kwispelclub.json` | benefit "Spaar Kwispels" + step "Verzamel Kwispels (aankopen, reviews, acties)" + "Wissel in voor beloningen" | earning + redemption | **COMING SOON** | herschrijf naar "Word gratis lid — sparen start binnenkort" |
| `templates/page.kwispelclub.json` | spotlight "Hond van de maand" | lopende actie | **COMING SOON / KEEP FOR LATER** | als niet actief: "binnenkort" |
| `sections/cart.liquid` | Kwispels-referentie | sparen bij checkout | **VERIFY + HIDE** | verbergen tot engine |
| `sections/main-product-kwispelbox.liquid` | Kwispels op PDP | punten per aankoop | **VERIFY + HIDE** | verbergen tot engine |
| `templates/page.faq.json` | "Kwispelclub: spaar Kwispels…" | werkend programma | **REWRITE COMING SOON** | "De Kwispelclub lanceert binnenkort…" |

**Classificatie-legenda:** HIDE (verbergen) · COMING SOON (herschrijven, geen actieve belofte) · SAFE (mag blijven) · ACTIVATE WITH ENGINE (pas tonen als engine live).

> **✅ 3C-0 UITGEVOERD (01-10-2026):** alle bovenstaande actieve loyalty-copy is naar **coming-soon** gezet (geen engine). Theme-breed geverifieerd: geen actieve earning/reward-claims meer. Detail-tabel in `store-quality-audit.md` (Fase 3C-0). PDP/cart/GWP ongemoeid. Reversibel via git (commit-log).

## 3B.12 Build backlog (toekomst, na vendor-go)

| Batch | Inhoud | Dependency | Risk | Test | Rollback |
|---|---|---|---|---|---|
| **3C-0** | pre-live loyalty-copy veilig (HIDE/COMING SOON) | geen | laag | visuele check live/concept | git revert (theme) |
| 3C-1 | vendor install + config (earning/redemption/referral rules) | vendorkeuze + §16-waarden | midden | sandbox-orders | app uninstall |
| 3C-2 | Kwispelclub MVP-redesign + family E (presentatie) | 3C-1 | midden | breakpoints + metafield-render | git revert |
| 3C-3 | customer-account-integratie (Rivo Accounts-extensie) | 3C-1 | midden | login→hub flow | extensie uit |
| 3C-4 | earning/redemption end-to-end (incl. refund/cancel + GWP-scheiding) | 3C-1..3 | **hoog** | paid/refund/redeem/GWP-samenloop | rules uit |
| 3C-5 | Zakelijk-pagina + lead-form | geen (parallel) | laag | form-submit | unpublish |
| 3C-6 | Partners-pagina + application-form | geen (parallel) | laag | form-submit | unpublish |
| 3C-7 | dog-data foundation (metaobject + capture-sync) | geen | midden | create/read dog | metaobject verwijderen |
| 3C-QA | end-to-end QA (earning/redemption/account/mobiel/Markets) | alle | hoog | volledige regressie | per-batch revert |

## 3B.13 Open blockers (herhaald, beslis vóór 3C)
1. Vendorkeuze bevestigen (Rivo vs Smile; trial-criteria in vendor-doc §7).
2. Business-waarden (§16).
3. Pre-live copy-aanpak (coming-soon nu vs engine-in-MVP).
4. Mystery-gift-vs-GWP scheiding bevestigen (apart loyalty-gift-product).

_Einde Fase 3B. READ-ONLY: niets gebouwd/geïnstalleerd/gepubliceerd/gemuteerd. Volgende = jullie go/no-go op vendor + §16-waarden, dan start 3C-0 (copy) + gekozen build-batches._

---

## Fase 3D — Community/Zakelijk/Partners gebouwd ZONDER loyalty-backend (01-10-2026)

**Scope-grens:** Smile/Rivo volledig geparkeerd. Geen app geïnstalleerd, geen puntenengine, geen
account-dashboard, geen custom loyalty-metafields, geen referral-engine, geen reward-logica. Dit is puur de
**presentatie-/funnel-laag** (Family E, zie `page-design-system.md` → Fase 3D).

**Gebouwd:**
| Pagina | Wat | Backend |
|---|---|---|
| **Partners** (live record) | Eerlijke samenwerkingspagina: waarom → partnertypes (categorieën) → 3 stappen (contact→kennismaking→passende samenwerking) → partner-form → FAQ → CTA → links | native contact-form |
| **Zakelijk** (draft) | B2B cadeau-funnel: use-cases → wat-kan-er → 4 stappen → offerte-aanvraagform → FAQ → CTA → links | native contact-form |
| **Community** (draft) | Gecureerd: hero → (lege) polaroid-gallery → momenten → eerlijke "doe mee" + consent-zin → links | geen (statisch) |

**Eerlijkheidscorrectie (belangrijk):** de oude `partners-page` beschreef een **niet-bestaand programma**
(unieke partnercode + QR, commissie/tegoed/gratis boxen "bij resultaat", "resultaten per maand", partnerpakket,
co-branded materiaal). Dat is verwijderd — Partners is nu een **honest contact-first** pagina. Partnertypes
blijven als **categorie** bestaan zonder dat er aparte programma's/voorwaarden zijn (FAQ zegt dit expliciet).

**Future loyalty-hooks (bewust nog leeg):**
- Family E `linkrow`/`cta` naar Kwispelclub staan klaar; zodra de loyalty-engine er is, kan de Kwispelclub-pagina
  van coming-soon → live, en kan een `card`/`cta` met echte voordelen worden toegevoegd (saldo blijft
  vendor-rendered in de nieuwe customer accounts, niet in Liquid).
- Community: ambassadeurs/UGC-inzending/referrals = **Phase 2/3** (vereisen engine + consent-flow); nu alleen
  "volg + tag + deel via contact".
- Zakelijk: geen quote-engine/B2B-infra; blijft form-first tot er een businesscase is.

**FUTURE ENHANCEMENT — data:** partner- en communitycontent kunnen later **metaobjects** verdienen
(partner-directory, gecureerde UGC met consent-veld). Nu bewust **niet** aangemaakt (editor-blocks volstaan,
niet overengineeren).

**Kwispelclub:** niet gemigreerd. Al coming-soon (3C-0) + brand-consistent (zelfde tokens) + werkende
nieuwsbrief (`form 'customer'`). Migratie zou de nieuwsbrief/Judge.me-integratie riskeren zonder
consistentiewinst. _Nit:_ de `kwispelclub-page` **schema-defaults** bevatten nog live-loyalty-copy ("Word lid",
"Inloggen", "Welkom Luna!") die **niet rendert** (JSON overruled), maar bij een vers ingevoegde sectie zou
terugkomen → kleine toekomstige polish.

_Einde Fase 3D. Geen loyalty-app/engine. Zakelijk + Community als DRAFT. Volgende stappen vereisen nieuwe go._
