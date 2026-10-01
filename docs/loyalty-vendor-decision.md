# Kwispelbox — Loyalty Vendor Decision (Fase 3B)

_Opgesteld: 01-10-2026 · **READ-ONLY** — alleen documentatie. Geen app geïnstalleerd/geconfigureerd._
_Onderzoeksdatum vendordata: 01-10-2026 (bronnen onderaan). Vendor-marketingclaims zijn als "verify" gemarkeerd waar niet onafhankelijk bevestigd._

---

## 0. Samenvatting / aanbeveling

**RECOMMENDED (MVP): Rivo** — hybride: Rivo = engine (ledger/earning/redemption/referrals) + Rivo Accounts-extensie als interactieve "Mijn Kwispelbox"-hub in de nieuwe customer accounts + Rivo Liquid-metafields voor onze eigen storefront-presentatie; hond-/UGC-/partner-/lead-data blijft in **Shopify-metaobjects**. Scale-plan **~$49/mnd**, month-to-month. Past in budget en in de rijke-account-ambitie.

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
