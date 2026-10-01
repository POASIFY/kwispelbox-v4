# Kwispelbox — Loyalty Vendor Decision (Fase 3B)

_Opgesteld: 01-10-2026 · **READ-ONLY** — alleen documentatie. Geen app geïnstalleerd/geconfigureerd._
_Onderzoeksdatum vendordata: 01-10-2026 (bronnen onderaan). Vendor-marketingclaims zijn als "verify" gemarkeerd waar niet onafhankelijk bevestigd._

---

## ⚠️ 3B.1 — ENTITLEMENT REALITY CHECK (CORRECTIE, 01-10-2026)

> **Deze sectie corrigeert §0 en §6 hieronder.** Verificatie tegen officiële Rivo-bronnen toont een **materiële fout** in de oorspronkelijke 3B-conclusie: de aanname "Rivo Scale (~$49) + Rivo Accounts + Liquid-metafields als eigen presentatielaag" is **onjuist**. Scale levert dat **niet**; die features zitten op **Plus ($499)** en/of in het **aparte Rivo Accounts-product ($499)**.

### Entitlement-matrix (officiële bronnen)
| Feature | Scale ($49+) | Plus ($499) | Rivo Accounts (**apart** $499) | Bron |
|---|---|---|---|---|
| Points earning | CONFIRMED | CONFIRMED | — | pricing |
| Vaste/percentage korting-rewards | CONFIRMED | CONFIRMED | — | pricing |
| Free-product rewards | UNCLEAR / SALES | CONFIRMED | — | pricing (tier onduidelijk) |
| Referrals | CONFIRMED | CONFIRMED | — | pricing |
| Shopify Flow | Basic | Advanced | — | pricing-vergelijking |
| Checkout-extensies | UNCLEAR / SALES | CONFIRMED | — | pricing |
| **Shopify Metafields / Liquid points-balance** | **NOT INCLUDED / Plus-only (UNCLEAR, leunt Plus)** | **CONFIRMED** | — | dev-docs + pricing (tegenstrijdig; help: "hangt van plan af") |
| **REST API / JS API / Webhooks / Developer Toolkit** | **Plus (UNCLEAR op Scale)** | **CONFIRMED** | — | dev-docs: toolkit = Plus-narratief |
| Custom actions (API-driven) | NOT op Scale (UNCLEAR) | CONFIRMED | — | dev-docs |
| **Volledige "Rivo Accounts" (branded account-hub)** | **NOT INCLUDED** | **NOT INCLUDED** | **apart product, $499/mnd** | pricing (eigen sectie) |
| Embedded account-widget (basis) in New Customer Accounts | UNCLEAR / SALES | UNCLEAR / SALES | CONFIRMED (volledig) | pricing (onduidelijk voor Scale) |
| Data-export | UNCLEAR / SALES | UNCLEAR / SALES | — | — |

**Tegenstrijdigheid gevonden:** de pricing-vergelijkingstabel lijkt "Metafields Access" bij Scale **én** Plus te tonen, terwijl de **developer-docs/Developer-Toolkit-narratief** metafields/API aan **Plus** koppelen; de help-pagina zegt "hangt van je plan af — vraag je CSM". → **Status: UNCLEAR, waarschijnlijk Plus. Alleen Rivo Sales kan dit hard bevestigen.**

### Hero-use-case test
| Use case | Werkt op Scale? | Vendor- of custom-rendered | Min. tier/product | Nodig | Bron-status |
|---|---|---|---|---|---|
| 1. Klant ziet Kwispels in New Customer Accounts | **UNCLEAR** (basis-widget?) / volledig = Accounts | vendor | Scale(?) of **Rivo Accounts $499** | account-widget | SALES |
| 2. Kwispelbox toont zelf "Je hebt 420 Kwispels" op /pages/kwispelclub | **NEE op Scale (waarschijnlijk)** | custom (Liquid) | **Plus $499** (metafields) | `customer.metafields.custom.rivo.value.points_balance` | dev-docs → Plus |
| 3. Custom **hond-verjaardag**-reward triggeren | **NEE op Scale (waarschijnlijk)** | custom | **Plus $499** (API/webhook) of Flow-actie (UNCLEAR) | API/Flow | dev-docs → Plus |
| 4. Reward kiezen/inwisselen | **JA** | vendor | Scale | base loyalty | CONFIRMED |
| 5. Referral-programma | **JA** | vendor | Scale | referrals | CONFIRMED |

**Kernconclusie:** op **Scale ($49)** krijg je een **vendor-rendered** loyalty-ervaring (punten/rewards/referrals) — **geen** eigen Kwispelbox-balance in Liquid, **geen** API-gedreven hond-verjaardag, **geen** volledige branded account-hub. Die "rijke, gebrande Mijn Kwispelbox" uit 3B vereist **Plus ($499) + mogelijk Rivo Accounts ($499) = ~$500-1000/mnd** → **ruim buiten** het €40-60-budget.

### Kostenscenario's
| Scenario | Tier/producten | Startkosten/mnd | Ontbreekt |
|---|---|---|---|
| A. **Rivo Lean MVP** | Scale ($49) | ~$49 | custom Liquid-balance, API/dog-birthday, volledige Accounts-hub |
| B. **Rivo Branded MVP** | Plus ($499) (+ evt. Accounts $499) | ~$499-998 | niets wezenlijks, maar **budget-breuk** |
| C. **Full Rivo Ecosystem** | Plus + Accounts (+ Enterprise) | ~$998+ | — |

### Cost-adjusted re-score
**A. BEST TECHNICAL FIT (budget genegeerd):** **Rivo (Plus + Accounts)** — rijkst gebrande account + metafields + API; daarna LoyaltyLion; dan Smile. _Maar $499-998/mnd._
**B. BEST MVP FIT onder ~€60-100/mnd:**
| Vendor | MVP-config | /mnd | Custom balance (Liquid) | Free-product | Flow | Account-hub | Cost-adj. score |
|---|---|---|---|---|---|---|---|
| **Smile** | Essential $15 → Standard $79 | $15-79 | ⚠️ "points op productpagina" (Standard) | ✅ Essential | ✅ Essential | ✅ Loyalty Hub | **hoogste** |
| **Rivo (lean)** | Scale $49 | $49 | ❌ (Plus-only) | ⚠️ verify | Basic | vendor-widget (UNCLEAR) | midden |
| **LoyaltyLion** | paid $199 | $199 | widgets/API | ✅ | ⚠️ | ✅ | buiten budget |

Onder budget verschuift de winst naar **Smile** (free-product + Flow al op $15; Loyalty Hub in accounts; Standard $79 geeft points-embed) omdat de equivalente Rivo-capaciteiten pas op **$499** zitten.

### HERZIENE AANBEVELING
**→ "SMILE BETTER MVP, RIVO LATER"** (met Rivo Scale-lean als verdedigbaar alternatief).
- **MVP (binnen budget): Smile** — Essential $15 of Standard $79: echte engine + free-product rewards + Flow + branded Loyalty Hub in de nieuwe customer accounts, **zonder** de $499-sprong. Geen custom Liquid-balance nodig voor een geloofwaardige MVP (vendor-rendered account + Kwispelclub-marketingpagina volstaan).
- **Rivo = premium/later:** zodra de rijke, volledig gebrande "Mijn Kwispelbox" + custom Liquid-balance + API-gedreven hond-verjaardag commercieel te rechtvaardigen zijn (Plus + Accounts, ~$500-1000/mnd).
- **Rivo Scale-lean ($49)** blijft acceptabel **als** je Shopify-exclusieve native-accounts prioriteert én accepteert dat alles vendor-rendered is (geen eigen balance/geen API-dog-birthday bij launch). Beslis in trial.
- **LoyaltyLion:** niet voor MVP (budget).

### Lean MVP-account (herzien)
Shopify New Customer Accounts + **vendor loyalty-block/widget** (Smile Loyalty Hub) + **Kwispelclub-marketingpagina** (family E, uitleg + join/login-CTA) + **géén** custom balance in Liquid. Custom balance/dog-birthday/API = **LATER** (Smile Standard+ of Rivo Plus). Dit is aanzienlijk rationeler binnen budget.

### Open vragen — alleen Rivo (resp. Smile) Sales kan bevestigen
1. Rivo: zit **een** account-widget (New Customer Accounts) in **Scale**, of vereist élke in-account-weergave het **Accounts-product ($499)**?
2. Rivo: zijn **metafields/Developer Toolkit** écht Plus-only (pricing-tabel suggereert anders)?
3. Rivo: free-product-rewards + data-export op welk tier?
4. Smile: exacte metafield/Liquid-exposure + EU-datalocatie/DPA op Essential/Standard?
5. Beide: NL/BE-valuta/locale + DPA/EU-datalocatie.

### Gecorrigeerde 3B-claims
- ❌ "Rivo Scale + Rivo Accounts" → Rivo Accounts is **apart $499**, niet in Scale.
- ❌ "Scale + Liquid-metafields als eigen presentatielaag" → metafields = **Plus ($499)/UNCLEAR**, niet Scale.
- ❌ "~$49/mnd dekt de rijke-account-ambitie" → rijke/branded account = **$499-998/mnd**.
- ✅ Hybride-filosofie blijft; **vendorkeuze + kostenverwachting gecorrigeerd** (Smile MVP, Rivo premium-later).

_§0 en §6 hieronder = **oorspronkelijke 3B-versie, gedeeltelijk achterhaald door 3B.1**. Bewust niet verwijderd (geen stille rewrite)._

---

## 0. Samenvatting / aanbeveling _(ORIGINEEL 3B — zie ⚠️ 3B.1 voor correctie)_

**RECOMMENDED (MVP): Rivo** — hybride: Rivo = engine (ledger/earning/redemption/referrals) + Rivo Accounts-extensie als interactieve "Mijn Kwispelbox"-hub in de nieuwe customer accounts + Rivo Liquid-metafields voor onze eigen storefront-presentatie; hond-/UGC-/partner-/lead-data blijft in **Shopify-metaobjects**. Scale-plan **~$49/mnd**, month-to-month. Past in budget en in de rijke-account-ambitie. _(**ACHTERHAALD:** zie 3B.1 — Scale levert dit niet; Smile = betere MVP, Rivo = premium-later.)_

**SECOND CHOICE: Smile.io** — sterkste prijs/eenvoud (Free→Essential $15→Standard $79). Kies dit als budget/simpliciteit zwaarder weegt dan een maximaal gebrande account-UX, of als Rivo's account-ervaring in de trial tegenvalt.

**NOT RECOMMENDED voor MVP: LoyaltyLion** — technisch sterk/enterprise, maar betaald **vanaf $199/mnd** (order-volume-pricing) ligt ruim boven het €40-60-budget; de gratis tier is te beperkt voor een echt programma. Benchmark/"later bij schaal".

**Rivo-first hypothese = BEVESTIGD** (scorecard §2: Rivo 4,36 / Smile 4,01 / LoyaltyLion 3,71).

---

## 1. Vendoronderzoek (okt 2026)

| Dimensie | **Rivo** | **Smile.io** | **LoyaltyLion** |
|---|---|---|---|
| Prijs (instap bruikbaar programma) | Scale **~$49/mnd**, Plus $499, Enterprise custom; **month-to-month** | Free $0 (≤200 orders) · Essential **$15** · Standard **$79** · Growth $199 · Plus $999 | Free (~≤400 orders) · paid **vanaf $199** (500 orders) → $399/$729/$1650 (volume) |
| New Customer Accounts | ✅ Rivo Accounts: loyalty-dashboard, referrals, wishlist, subscriptions in account | ✅ Loyalty Hub in customer accounts | ✅ loyalty-page/points-blokken in customer accounts |
| Storefront via Liquid/metafields | ✅ **points/VIP via Liquid-metafields** + JS/SDK | ⚠️ "embed points op productpagina" (Standard $79); metafield-breedte **verify** | ⚠️ widgets/API; metafield-breedte **verify** |
| Checkout-extensies | ✅ (8, Plus-eligible) | ✅ (checkout) | ✅ |
| API / webhooks / export | ✅ REST API, JS, webhooks, developer toolkit | ✅ API + export | ✅ sterke API + export/analytics |
| Shopify Flow | ✅ | ✅ (vanaf **Essential $15**) | ⚠️ **verify** tier |
| Referrals (native + fraude) | ✅ (incl. advocate-stats) | ✅ (zelfs op Free) | ✅ |
| Reward-types incl. **free product** | ✅ (verify tier) | ✅ free-product (vanaf **Essential**) | ✅ (vouchers/giftcard/free product/free shipping/custom) |
| Birthday / **custom event** | customer-birthday native; **custom** via API/webhook/Flow | customer-birthday; **custom** via Flow/API | customer-birthday; **custom** via API |
| Markets / NL-BE / currencies | ✅ (EU-datalocatie/DPA **verify**) | ✅ 20+ talen, POS (EU/DPA **verify**) | ✅ (EU/DPA **verify**) |
| Install-base / rijpheid | groeiend, Shopify-exclusive | **grootste** install-base, veteraan | enterprise/multi-store |
| Branding-vrijheid | midden-hoog (native theme) | volledige branding zelfs op Free | hoog |

**Let op (geen van de drie):** **hond**-verjaardag is géén native feature — alle drie kennen alleen *customer* birthday. Hond-birthday vereist **onze** data (metaobject) + **custom automation** (Flow/API). Zie §4.

---

## 2. Gewogen scorecard

Gewichten (som 100), gekozen voor Kwispelbox (rijke account-ambitie + hybride + NL/BE + budget):

| Criterium | Gewicht | Waarom dit gewicht | Rivo | Smile | LoyaltyLion |
|---|---:|---|---:|---:|---:|
| New Customer Accounts / hub | 16 | kern van "Mijn Kwispelbox" | 5 | 4 | 4 |
| Storefront-presentatie (Liquid/metafields) | 10 | eigen merk-presentatielaag | 5 | 4 | 4 |
| Referrals (native + fraude) | 7 | groei (V2) | 4 | 4 | 4 |
| Hond-verjaardag (custom data/event) | 9 | signature-feature | 4 | 3 | 3 |
| Redemption (discount + free product + stacking) | 12 | MVP-core | 4 | 4 | 5 |
| API / webhooks / export | 10 | hybride + exit | 5 | 4 | 5 |
| Shopify Flow | 6 | automation-lijm | 4 | 4 | 3 |
| Markets / EU / GDPR | 8 | NL/BE + compliance | 4 | 4 | 4 |
| Kosten binnen budget | 9 | €40-60 MVP | 4 | 5 | 1 |
| Implementatie-complexiteit | 5 | time-to-live | 4 | 5 | 3 |
| Vendor lock-in | 4 | hybride-filosofie | 3 | 3 | 2 |
| Future fit (dogs/UGC/tiers/B2B) | 4 | doorgroei | 5 | 4 | 5 |
| **Gewogen totaal (/5)** | **100** | | **4,36** | **4,01** | **3,71** |

(Scores 1-5; "verify"-punten conservatief ingeschat. Trial bevestigt account-UX + metafield-breedte + EU-datalocatie.)

---

## 3. Plan-tier reality check (vs budget €40-60/mnd)

| Vereiste feature | Rivo | Smile | LoyaltyLion |
|---|---|---|---|
| Points + purchase earning | ✅ Scale | ✅ Free+ | ✅ |
| Welcome reward | ✅ | ✅ | ✅ |
| Redemption (korting) | ✅ | ✅ Free+ | ✅ |
| **Free-product reward** | ✅ (verify tier) | ✅ **Essential $15** | ✅ (paid $199) |
| Referrals | ✅ | ✅ Free | ✅ |
| New Customer Accounts hub | ✅ Scale | ✅ | ✅ |
| Account blocks/extensies | ✅ | ✅ | ✅ |
| **Liquid/metafield points-exposure** | ✅ Scale | ⚠️ Standard $79 (productpagina) / verify | ⚠️ verify |
| API + webhook | ✅ | ✅ | ✅ |
| Shopify Flow | ✅ | ✅ Essential $15 | ⚠️ verify tier |
| **Custom (hond-)birthday event** | ✅ via API/Flow | ✅ via Flow/API | ✅ via API |
| Branding verwijderen | ✅ | ✅ (zelfs Free) | ✅ |
| Analytics | ✅ | Standard+ | ✅ (sterk) |
| Export/migratie | ✅ | ✅ | ✅ |

**Budget-conclusie:**
- **Rivo Scale ~$49/mnd** dekt de MVP-requirements **binnen budget** (verify: free-product-reward + exacte metafield-exposure op Scale).
- **Smile**: fundamenten op **Essential $15** (incl. free-product + Flow + loyalty-page). **Points-embed op productpagina pas Standard $79** (net boven budget) — maar voor MVP niet vereist. Beste **value**.
- **LoyaltyLion**: bruikbaar programma = **$199/mnd → buiten budget**. Niet voor MVP.

⚠️ **Expliciet:** gebruik geen "vanaf $X" alsof dat plan onze requirements dekt — bevestig in de trial dat free-product-reward + metafield-exposure + Flow op het gekozen (budget)plan zitten.

---

## 4. Rivo-first validatie + architectuur

**Hypothese:** Rivo = engine · Rivo Accounts = interactieve loyalty-UI · Rivo metafields/API = storefront-presentatie · Shopify-native dog/community/partner-data. → **Houdbaar.**

**Beantwoord:**
- **Puntensaldo op theme tonen?** Ja — via Rivo Liquid-metafields (bijv. op Kwispelclub-pagina/header, ingelogd). ⚠️ exacte metafield-keys/tier verifiëren.
- **Via Liquid/metafields beschikbaar:** points-balance, VIP-tier (weergave). **Alleen via API/SDK:** transactie-historie, redemption-acties, referral-stats. **Alleen in Rivo UI/account-extensie:** interactieve inwissel-/referral-acties, dashboard.
- **Redemption:** Rivo genereert een Shopify-korting (of free-product) bij inwisseling; toegepast in cart/checkout.
- **Refunds/cancellations:** earning moet terugdraaien bij refund/cancel → Rivo-regels + webhooks (verify gedrag; spec §6/§7 in plan).
- **Hond-birthday → Rivo reward?** Niet native. Route: **ons `metaobject dog.birthdate` → Shopify Flow (of Klaviyo) → Rivo API / reward-trigger**. Minimaal nodig: dog-metaobject + Flow/automation + Rivo API-credential. (V2.)

**Architectuur (tekst):**
```
STOREFRONT (theme, family E)        ACCOUNT (Shopify New Customer Accounts)
  Kwispelclub-pagina ─┐                 Rivo Accounts-extensie
  header/club-indicator │  (Liquid         ├─ Kwispels-balance
  (balance via metafield)│  metafields)     ├─ beschikbare/ingewisselde rewards
                         │                   ├─ referral-link
         ┌───────────────┘                   └─ (bestellingen = Shopify native)
         ▼
   RIVO (engine / source of truth: punten)
     earning rules ◄── Shopify order webhooks (paid/refund/cancel)
     redemption ──► Shopify discount / free product
     referrals (ledger + fraude)
         ▲
         │ API / Flow
   SHOPIFY-NATIVE DATA (onze source of truth)
     metaobject dog (naam, birthdate, size?, foto?) ─► Flow/Klaviyo ─► Rivo reward (verjaardag)
     metaobject ugc_submission / partner / business_lead
```

---

## 5. Hond-verjaardag-integratie (per vendor)

Flow: `Customer → metaobject dog → birthdate → automation → loyalty-engine → reward → communicatie`.

| Route | Hoe | Rivo | Smile | LoyaltyLion |
|---|---|---|---|---|
| A. Shopify Flow | Flow leest dog-birthdate (metaobject) → vendor-actie/API | ✅ (Flow + API) | ✅ (Flow vanaf Essential) | ⚠️ (Flow-connector verify) |
| B. App automation | vendor native birthday = **customer** only (niet hond) | ❌ hond | ❌ hond | ❌ hond |
| C. Custom scheduled job | custom app cron → API | ✅ (meer werk) | ✅ | ✅ |
| D. Klaviyo + loyalty-trigger | Klaviyo date-property → reward | ✅ | ✅ | ✅ |
| E. Combinatie (aanbevolen) | Flow/Klaviyo op metaobject → vendor-API-reward | ✅ best | ✅ | ⚠️ |

**Conclusie:** hond-birthday is V2 en vendor-agnostisch oplosbaar; Rivo/Smile het eenvoudigst (Flow + API). **Cruciaal:** hond-verjaardag ≠ customer-birthday — nooit de native customer-birthday-feature misbruiken.

---

## 6. Go/No-Go

- **RECOMMENDED:** **Rivo (Scale ~$49/mnd)** + hybride-architectuur (§4). Min. tier: Scale (verify free-product + metafields). Kostenklasse: ~€45-50/mnd MVP. Required integrations: Rivo Accounts-extensie, order-webhooks, Liquid-metafields; later Flow+API voor hond-birthday. Risks: metafield/tier-verificatie, EU-datalocatie/DPA, account-UX-branding-grenzen. Lock-in: ledger bij Rivo (gemitigeerd door export + onze native data). Exit: export punten + onze metaobjects blijven → migreerbaar.
- **SECOND:** **Smile.io** (Essential $15 / Standard $79) — beter als budget/eenvoud wint of Rivo-account-UX tegenvalt in trial.
- **NOT FOR MVP:** **LoyaltyLion** ($199+ buiten budget; enterprise-later).

**Implementatie-sequence (hoog niveau):** zie `community-ecosystem-plan.md` §Fase 3B "Build backlog" (3C-0 … 3C-QA). **Eerste stap is altijd 3C-0: pre-live loyalty-copy veilig maken**, onafhankelijk van vendorkeuze.

---

## 7. Open blockers (vóór 3C-build)
1. **Vendorkeuze bevestigen** (Rivo vs Smile) — idealiter beide 14-daagse trial, check: account-UX, free-product-reward op budgetplan, Liquid-metafield-keys, EU-datalocatie/DPA.
2. **Business-waarden** (`community-ecosystem-plan.md` §16): Kwispel-waarde, earn-ratio, drempels, welkomstbonus, vervaldatum, refund-handling.
3. **Pre-live copy-besluit:** "coming soon" nu, óf engine meteen in MVP (bepaalt timing 3C-0).
4. **Mystery-gift vs GWP** conflict-oplossing bevestigen (apart loyalty-gift-product — zie plan §Fase 3B redemption).

---

## Bronnen (onderzoek 01-10-2026)
- Rivo: [pricing](https://www.rivo.io/pricing) · [editions spring 2026](https://www.rivo.io/editions/spring2026) · [Shopify App Store](https://apps.shopify.com/rivo-loyalty) · [shopify-loyalty-app](https://www.rivo.io/blog/shopify-loyalty-app)
- Smile.io: [pricing](https://smile.io/pricing) · [App Store](https://apps.shopify.com/smile-io) · [Capterra pricing](https://www.capterra.com/p/169446/Smile-io/pricing/)
- LoyaltyLion: [Capterra](https://www.capterra.com/p/140592/LoyaltyLion/) · [GetApp](https://www.getapp.com/customer-management-software/a/loyaltylion/)
_Prijzen/tiers/feature-beschikbaarheid + EU-datalocatie: in-app/trial verifiëren vóór definitieve keuze._
