# Kwispelbox — Loyalty Trial Protocol & Results (Fase 3C-1)

_Opgesteld: 01-10-2026 · **STATUS: WACHT OP HANDMATIGE TRIAL-UITVOERING.**_

> **⚠️ Waarom nog geen resultaten:** de assistent kan **geen Shopify-apps installeren**, **geen testorders plaatsen** en **de interactieve loyalty-/account-UI niet bedienen**. Beschikbare tools = Admin-API (producten/collecties/pagina's/kortingen/metafields; orders read-only). Conform §2/§29 van de opdracht: **geen fictieve testresultaten**. Dit document bevat het **runbook + lege matrix + scoremodel**; de PASS/PARTIAL/FAIL-cellen vult de uitvoerder tijdens de echte trial.
>
> **Provisionele lean (documentatie-gebaseerd, GÉÉN trialresultaat):** trial **Smile eerst** (beste budget-fit per 3B.1), **Rivo-lean parallel** als benchmark. Definitieve winnaar pas ná ingevulde matrix.

---

## A. Waar de assistent stopte (jouw handmatige acties)

### A1. Smile — install & trial-config
- **App:** "Smile: Loyalty & Rewards" — `apps.shopify.com/smile-io`.
- **Te testen tier:** start **Free ($0)**; upgrade naar **Essential ($15)** om **free-product reward + Shopify Flow + dedicated loyalty page** te testen. **Standard ($79)** alléén als je "points op productpagina / bonus events" wilt testen (niet MVP-vereist). _Budget-check: ≤€100/mnd ✓._
- **Verwachte permissions (bevestig op consent-scherm):** customers (read/write), orders (read), discounts (write), online store / theme app extensions, customer accounts, content. Noteer exact wat gevraagd wordt.
- **Initiële settings (trial-veilig):**
  1. **Launcher/widget UIT op storefront** (program "hidden"/disabled) → geen publieke loyalty-belofte tijdens trial.
  2. Points-currency naam → **"Kwispels"**, programma → **"Kwispelclub"** (test NL-copy).
  3. Earning rules: **Purchase** (dummy earn-rate, bv. 1/€) + **Welcome/points for signup** (dummy).
  4. Rewards: 1× **vaste korting** + 1× **free-product** (gebruik het TEST-product, §A3).
  5. Taal → Nederlands waar beschikbaar.
  6. Combineerbaarheid: zet loyalty-korting op **"combineert niet met andere kortingen"** (test §Test 7 t.o.v. GWP).

### A2. Rivo — install & trial-config
- **App:** "Rivo: Loyalty Program, Rewards" — `apps.shopify.com/rivo-loyalty`.
- **Te testen tier:** **Scale ($49+, 7-daagse trial)**. **Belangrijk (3B.1):** tel **alleen Scale-capabilities**; **metafields/Developer Toolkit = Plus ($499)** en **Rivo Accounts = apart product ($499)** → NIET meetellen als Scale-feature. Test in §Test 8 exact **wat Scale zelf** in de New Customer Accounts toont (embedded widget?) zonder het $499-Accounts-product.
- **Verwachte permissions:** vergelijkbaar met Smile (customers, orders read, discounts, theme app extensions, customer accounts). Noteer exact.
- **Initiële settings (trial-veilig):** zelfde als Smile A1 (widget uit op storefront, "Kwispels"/"Kwispelclub", purchase+welcome dummy, 1 vaste korting + 1 free-product via TEST-product, NL-copy, niet-combineerbaar).

### A3. TEST-reward-product (apart van GWP) — spec
> Maak een apart product zodat de loyalty-free-product-test **nooit** botst met de bestaande €89-GWP (`gratis-mystery-verrassing`) / `window.kbGwp` / BXGY.
- **Titel:** `TEST — Kwispelclub reward` · **status: DRAFT (unpublished)** · **geen** verkoopkanaal · **niet** in collecties · **niet** indexeerbaar (draft = niet in search/SEO) · prijs irrelevant (reward zet €0).
- **Rollback:** na trial **verwijderen/archiveren**.
- **Assistent kan dit op jouw go via de Admin-API als draft aanmaken** (reversibel) — zeg "maak testproduct" als je wilt dat ik die ene stap doe. (Nu bewust niet gedaan: geen premature store-mutatie.)

### A4. Test-customer
- Eén **testaccount** (eigen e-mail/alias), gedocumenteerd: enrollment-state, beginbalance (0), accounttype, gekoppelde testorders. **Geen echte klantdata.**

### A5. Trial-veiligheid (beide)
- Storefront-widget/launcher **uit**; geen loyalty-announcement; geen live "Spaar Kwispels"-copy (3C-0 heeft dat al op coming-soon gezet — **laten staan**); gebruik test-customer + testorders; geen echte klanten muteren.

---

## B. Test-matrix (vul PASS / PARTIAL / FAIL / NOT-ON-PLAN / MANUAL-VERIFY + evidence)

| # | Test | Smile (Essential) | Rivo (Scale) | Evidence-ref | Notities |
|---|---|---|---|---|---|
| 1 | Enrollment / welcome (1×, logout/login, dup, mobiel) | _pending_ | _pending_ | | waar ziet klant lid-status + saldo? vendor- of Shopify-UI? |
| 2 | Purchase earning (paid-trigger, basis excl. shipping/btw/na-discount, rounding) | _pending_ | _pending_ | | welke orderstatus triggert? instelbaar excl. shipping/tax/na-discount? |
| 3 | **Full refund** → earning teruggedraaid? (direct/vertraagd, negatief saldo) | _pending_ | _pending_ | | **kill-kandidaat** |
| 4 | **Partial refund** → proportioneel? (CRUCIAAL) | _pending_ | _pending_ | | **kill-kandidaat** |
| 5 | Fixed-discount reward (redeem→cart→checkout, eenmalig, min-spend, expiry, mobiel) | _pending_ | _pending_ | | |
| 6 | Free-product reward (TEST-product, €0, voorraad, qty>1 blok, misbruik) | _pending_ | _pending_ | | tier-afhankelijk (Smile = Essential) |
| 7 | Combinability vs GWP/free-shipping (geen €89-mechanisme wijzigen) | _pending_ | _pending_ | | combineerregels afdwingbaar? |
| 8 | **New Customer Accounts** (embedded/hub/extern, branding, balance, rewards, redeem; mobiel+desktop) | _pending_ | _pending_ | | **zware weging**; Rivo: wat op Scale vs $499-Accounts? |
| 9 | Mobile UX ~390px (login→hub→redeem→terug→cart) | _pending_ | _pending_ | | concrete frictie noteren |
| 10 | NL-copy / localization (Kwispelclub/Kwispels, earning/redeem/reward/account/e-mails; NL+BE Markets) | _pending_ | _pending_ | | **kill-kandidaat** (cruciale NL blijft EN?) |
| 11 | Branding ("voelt het geloofwaardig Kwispelbox?", powered-by weg?) | _pending_ | _pending_ | | |
| 12 | Flow / automation + **dog-birthday**-route (betaalbaar plan? anders welke tier/API?) | _pending_ | _pending_ | | Smile Flow=Essential; Rivo API=Plus |
| 13 | Data export / exit (member/balance/transactions/rewards/referrals; CSV/API/support) | _pending_ | _pending_ | | lock-in zwaar |
| 14 | App removal / rollback (discounts/widgets/scripts/account-blocks/data na uninstall) | _pending_ | _pending_ | | documenteer vóór uninstall |
| 15 | Admin UX (reward maken, klant zoeken, punten corrigeren, refund inspecteren, pauzeren, analytics, support) | _pending_ | _pending_ | | dagelijks beheer zonder specialist? |
| — | Performance/theme-impact (scripts sitewide vs account-only, app-embeds, duplicate assets) | _pending_ | _pending_ | | |
| — | Privacy/GDPR (DPA, subprocessors, export/deletion, EU-transfer, customer-privacy-integratie) → OK/REVIEW/BLOCKER | _pending_ | _pending_ | | niet zelf juridisch goedkeuren |

_Documentatie-bewijs en praktijk-bewijs apart noteren; geen PASS op alleen docs als een echte trial mogelijk is._

---

## C. Scoremodel (gewogen, vul 1-5 + evidence)

| Criterium | Gewicht | Smile | Rivo-lean |
|---|---:|---:|---:|
| Purchase/refund correctness | 18% | _ | _ |
| New Customer Accounts UX | 14% | _ | _ |
| Redemption / free product | 14% | _ | _ |
| Pricing / value | 12% | _ | _ |
| Admin simplicity | 8% | _ | _ |
| Localization NL/BE | 8% | _ | _ |
| Branding | 6% | _ | _ |
| Flow / future dog-birthday | 6% | _ | _ |
| Export / exit | 6% | _ | _ |
| GDPR / privacy | 4% | _ | _ |
| Performance / theme-impact | 4% | _ | _ |
| **Totaal (/5)** | 100% | **_** | **_** |

## D. Kill-criteria (vink af tijdens trial)
- [ ] Refunds niet betrouwbaar corrigeerbaar (Test 3/4)
- [ ] Rewards praktisch onbruikbaar op mobiel (Test 9)
- [ ] New Customer Accounts-integratie ontbreekt volledig (Test 8)
- [ ] Cruciale NL-copy blijft Engels (Test 10)
- [ ] Free-product reward alleen op onrealistisch duur plan (Test 6)
- [ ] Data niet exporteerbaar / ernstige lock-in (Test 13)
- [ ] Totale MVP-prijs > ~€100/mnd zonder businesscase

## E. Vendor-decision (invullen ná matrix)
**WINNER — MVP:** _(pending trial)_ · gekozen tier: _ · verwachte maandklasse: _ · launch-features: _ · bewust uit: _ · beperkingen: _ · upgrade-triggers: _.
**SECOND CHOICE:** _ · wanneer beter: _.

## F. Trial-cleanup checklist (na afloop)
- [ ] Testorders annuleren/verwijderen (waar Shopify toestaat)
- [ ] Test-discounts uitschakelen
- [ ] TEST-reward-product verwijderen/archiveren
- [ ] Trial-widgets niet live laten (launcher uit)
- [ ] Test-customers gedocumenteerd
- [ ] Geen losse snippets/embeds live achtergelaten
- [ ] Bij uninstall: data-impact vooraf documenteren (Test 14)

---

## G. Community-MVP-backlog — impact per vendor-uitkomst (geen code)
Zodra de winnaar bekend is, geldt voor de 3C-backlog:
- **Kwispelclub-pagina / Family E:** blijft **presentatie** (coming-soon → live tekst); balance-weergave = **vendor-rendered** (geen custom Liquid-balance tenzij later Smile Standard / Rivo Plus).
- **Account / points balance / redemption:** **vendor-hub** in New Customer Accounts (Test 8 bepaalt de kwaliteit).
- **Birthday V2 / referrals V2:** afhankelijk van Flow/API-tier (Test 12) — plan ná MVP.
- **UGC / Partners / Zakelijk:** **Shopify-native** (metaobjects/forms), **onafhankelijk** van de vendorkeuze → kunnen parallel.

_Einde 3C-1. READ-ONLY tot trial: geen app geïnstalleerd, geen order/customer/discount/metafield gemuteerd, geen Family E gebouwd. Volgende stap = jij draait de trial met dit runbook (of geeft go voor de ene API-stap: TEST-reward-product als draft)._
