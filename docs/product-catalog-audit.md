# Kwispelbox — Productcatalogus audit & opschoonplan

_Opgesteld: 30-09-2026 · Bron: Shopify Admin API (`kwispelbox.myshopify.com`)._
_Status: **FASE 1–2 = audit + voorstel. Nog NIETS gearchiveerd of qua structuur gewijzigd.**_

> Werkwijze: eerst inventariseren → mapping tonen → dan pas veilige uitvoering. Legacy pas archiveren
> ná dependency-check. Nooit hard-deleten. Prijzen/varianten nooit wijzigen zonder rapport + akkoord.

---

## Samenvatting

- **13 producten** in totaal (kleine, overzichtelijke catalogus).
- Kernstructuur is grotendeels al goed: 5 Special-Edition-boxen met correcte titels + `Special Editions` type.
- **Grootste bevindingen:**
  1. **Geen productafbeeldingen** op de 5 boxen én 4 cadeau-extra's (mediaCount 0). Dit verklaart de placeholders in "Meest gekozen". → assets nodig.
  2. **3 legacy/onduidelijke producten**: `zippypaws-burrow-pinata`, `inpakken`, en het generieke `kwispelbox` (hoofdbox).
  3. **Handles wijken af** van de gewenste canonieke vorm (`verjaardag-box` vs gewenst `verjaardag-kwispelbox`). Wijzigen raakt het live thema + vraagt redirects → beslissing nodig.
  4. **Dubbele GWP-korting**: naast de correcte BXGY staat er óók een losse Basic-korting "vanaf €89" actief.
  5. **Tags vrijwel leeg** op alle producten (boxen alleen `["box"]`, cadeau-extra's geen tags).
  6. **`meest-gekozen`** staat al in exact de gewenste volgorde ✅.
  7. **`puppy-favorieten`** is leeg maar wordt vanaf de homepage gelinkt ("Nieuwe puppy").

---

## FASE 1 — Volledige audit (13 producten)

Legenda classificatie: **A** kernproduct · **B** actuele extra · **C** systeem/GWP · **D** legacy → archiveren · **E** dubbel → consolideren · **F** onduidelijk → review.

| # | Titel | Handle | Type | Prijs | Var. | Voorraad | Media | Tags | Collecties | Klas |
|---|-------|--------|------|-------|------|----------|-------|------|-----------|------|
| 1 | Verjaardag Kwispelbox | `verjaardag-box` | Special Editions | €44,95–89,95 | 3 | 300 | **0** | box | special-editions, meest-gekozen, alle-acties | **A** |
| 2 | Kerst Kwispelbox | `kerst-box` | Special Editions | €44,95–89,95 | 3 | 300 | **0** | box | special-editions, meest-gekozen, alle-acties | **A** |
| 3 | Halloween Kwispelbox | `halloween-box` | Special Editions | €44,95–89,95 | 3 | 300 | **0** | box | special-editions, meest-gekozen, alle-acties | **A** |
| 4 | Dierendag Kwispelbox | `dierendag-box` | Special Editions | €44,95–89,95 | 3 | 300 | **0** | box | special-editions, meest-gekozen, alle-acties | **A** |
| 5 | Valentijn Kwispelbox | `valentijn-box` | Special Editions | €44,95–89,95 | 3 | 300 | **0** | box | special-editions, alle-acties | **A** |
| 6 | Persoonlijk wenskaartje | `persoonlijk-wenskaartje` | Cadeau-extra | €2,50 | 1 | 100 | 0 | — | cadeau-extras, maak-er-een-cadeautje, alle-acties | **B** |
| 7 | Verjaardagstaartje | `verjaardagstaartje` | Cadeau-extra | €6,95 | 1 | 100 | 0 | — | cadeau-extras, maak-er-een-cadeautje, alle-acties | **B** |
| 8 | Mystery verrassing | `mystery-verrassing` | Cadeau-extra | €4,95 | 1 | 100 | 0 | — | cadeau-extras, maak-er-een-cadeautje, alle-acties | **B** |
| 9 | Feesthoedje | `feesthoedje` | Cadeau-extra | €4,95 | 1 | 100 | 0 | — | cadeau-extras, maak-er-een-cadeautje, alle-acties | **B** |
| 10 | Gratis mystery verrassing 🎁 | `gratis-mystery-verrassing` | Gift | €9,95 | 1 | 0 | 0 | gwp-gift, hidden | alle-acties | **C** |
| 11 | Kwispelbox – De verrassingsbox… | `kwispelbox` | Box | €44,95–89,95 | 3 | 0 | 1 | box | boxen, alle-acties | **F** |
| 12 | Inpakken | `inpakken` | _(leeg)_ | €2,95 | 1 | 0 | 1 | — | alle-acties | **D** |
| 13 | ZippyPaws Burrow Pinata | `zippypaws-burrow-pinata` | Speeltjes | €15,95 | 1 | 0 | 1 | — | frontpage, speeltjes, alle-acties | **D** |

### Detail per product (aanvullende velden)

**Kernboxen (1–5)** — identiek opgebouwd:
- Vendor `Kwispelbox`, status ACTIVE, gepubliceerd op Webshop + POS + Copilot (niet "Shop").
- Optie **`Formaat`** met waarden **Mini / Happy / Mega** → prijzen Mini €44,95 · Happy €64,95 · Mega €89,95. Geen compare-at.
- Varianten: SKU's `VERJ-/KERST-/HALL-/VAL-/DIER-{MINI,HAPPY,MEGA}`, **geen barcodes**, weight 0, tracked, requiresShipping true, taxable true.
- `templateSuffix` = eigen template per box (`verjaardag-box`, etc.).
- Metafields: `custom.curatie_richtlijn` (intern) + SEO `global.title_tag`/`description_tag`.
  - `custom.kaart_kleur` + `custom.kaart_tekst` aanwezig op Verjaardag/Kerst/Halloween/Dierendag, **ontbreekt op Valentijn**.
- **Category (Shopify taxonomy): niet gezet** (null) op alle boxen.
- **Featured image: geen** (mediaCount 0).

**Cadeau-extra's (6–9):** vendor Kwispelbox, `templateSuffix: simple`, geen category, geen media, geen tags. SKU's `EXTRA-KAART/TAART/MYST/FEEST`. requiresShipping = true (t.o.v. `inpakken` = false).

**GWP (10) `gratis-mystery-verrassing`:** type Gift, tags `gwp-gift` + `hidden`, prijs €9,95 (compare-at €9,95), taxable **false**, tracked false, voorraad 0. Gepubliceerd op **Webshop + Copilot** (niet POS) → cart-add werkt. Heeft `judgeme.*` metafields (review-app residu).

**Legacy/review:**
- (11) `kwispelbox` — generieke "hoofdbox". Optie `Formaat` Mini/Happy/Mega, prijzen idem, maar **compare-at €59,95 op álle varianten** (dus Mini toont €44,95 "van €59,95" = ok, maar Mega €89,95 "van €59,95" = onlogisch). Category "Hondenspeeltjes". Heeft 1 afbeelding. Enige product in collectie **`boxen`** (waar menu "Boxen" + homepage naar linken).
- (12) `inpakken` — inpakservice €2,95, requiresShipping false, category "Niet gecategoriseerd".
- (13) `zippypaws-burrow-pinata` — third-party speeltje, SKU ZP905 + barcode, volledige Shopify-taxonomy metafields → duidelijk een import/testrestant. Enige product in `frontpage` + `speeltjes`.

**Orderhistorie:** niet per product opgevraagd (kostbare query). Niet nodig voor dit plan: we **archiveren** i.p.v. verwijderen, dus orderreferenties + historische data blijven hoe dan ook behouden.

---

## Afhankelijkheden (dependency-check)

| Bron | Verwijst naar | Impact bij wijziging |
|------|---------------|----------------------|
| Homepage `index.json` — moments | `/products/verjaardag-box`, `/products/kerst-box`; collecties `/collections/boxen`, `/collections/puppy-favorieten`, `/collections/special-editions` | Handle-wijziging box → link updaten + redirect |
| Homepage `index.json` — "Meest gekozen" | collectie **`meest-gekozen`** (Verjaardag, Kerst, Halloween, Dierendag) | Volgorde al correct ✅ |
| Header-menu `main-menu` | Boxen→`/collections/boxen`, Verjaardag→`/products/verjaardag-box`, Feestdagen/Momenten→`/collections/special-editions` | Handle-wijziging → menu updaten |
| Header dropdown-menu's | `menu-special-editions` items staan op `#` (nog niet gekoppeld) | — |
| Automatische korting | BXGY "Gratis verrassing vanaf €89" → `gratis-mystery-verrassing` | GWP-product niet archiveren/hernoemen |
| Templates | Elke box heeft eigen `product.<handle>.json` template-suffix | Bij handle-wijziging suffix niet automatisch mee — check |
| Collectie `boxen` (smart TYPE=Box) | bevat alleen `kwispelbox` | Als `kwispelbox` archiveert → collectie leeg |
| Collectie `frontpage`/`speeltjes` | bevatten alleen `zippypaws` | Bij archiveren → leeg (frontpage ongebruikt door thema) |

---

## Collecties-audit (11 collecties)

| Handle | Titel | Type | # | Opmerking |
|--------|-------|------|---|-----------|
| `meest-gekozen` | Meest gekozen | manual | 4 | ✅ volgorde correct (Verj, Kerst, Hall, Dier) — homepage-bron |
| `special-editions` | Special Editions | smart TYPE=Special Editions | 5 | ✅ de 5 kernboxen |
| `cadeau-extras` | Cadeau-extra's | smart TYPE=Cadeau-extra | 4 | ✅ |
| `maak-er-een-echt-cadeautje-van` | Maak er een écht cadeautje van | manual | 4 | duplicaat-inhoud van cadeau-extras (andere rol/UX?) → review |
| `boxen` | Boxen | smart TYPE=Box | 1 | alleen legacy `kwispelbox`; menu "Boxen" linkt hierheen |
| `frontpage` | Home page | manual | 1 | alleen `zippypaws`; ongebruikt door thema (legacy default) |
| `speeltjes` | Speeltjes | smart TYPE=Speeltjes | 1 | alleen `zippypaws` |
| `knuffels` | Knuffels | smart TYPE=Knuffels | 0 | leeg |
| `snacks` | Snacks | smart TYPE=Snacks | 0 | leeg |
| `puppy-favorieten` | Puppy favorieten | smart TAG=puppy | 0 | **leeg maar gelinkt vanaf homepage** ("Nieuwe puppy") |
| `alle-producten-voor-acties` | Alle producten (voor acties) | smart VENDOR=Kwispelbox | 13 | technische verzamel-collectie |

---

## GWP-audit (Fase 8)

- ✅ **BXGY** "Gratis verrassing vanaf €89" — ACTIVE. Koopt: subtotaal ≥ €89 → krijgt: 1× `gratis-mystery-verrassing` @ 100% korting. Correct server-side.
- ⚠️ **Dubbel:** daarnaast staat **DiscountAutomaticBasic** "Gratis mystery verrassing vanaf €89" óók ACTIVE. Waarschijnlijk een legacy eerste poging. Twee overlappende €89-kortingen kunnen conflicteren. → **Review: Basic deactiveren** (na akkoord; niet automatisch).
- GWP-product publicatie (Webshop, niet POS) + tags `gwp-gift`/`hidden` = correct. Prijs/één-cadeau-logica ok.

---

## FASE 2 — Canonieke productstructuur (voorstel)

### Titels — al correct ✅
`Verjaardag Kwispelbox`, `Kerst Kwispelbox`, `Halloween Kwispelbox`, `Dierendag Kwispelbox`, `Valentijn Kwispelbox`. Geen wijziging nodig.

### Handles — ⚠️ BESLISSING NODIG
| Huidig | Gewenst (jouw brief) |
|--------|----------------------|
| `verjaardag-box` | `verjaardag-kwispelbox` |
| `kerst-box` | `kerst-kwispelbox` |
| `halloween-box` | `halloween-kwispelbox` |
| `dierendag-box` | `dierendag-kwispelbox` |
| `valentijn-box` | `valentijn-kwispelbox` |

Wijzigen betekent: (a) handle updaten, (b) **redirect oud→nieuw** aanmaken, (c) links in `index.json` + `main-menu` bijwerken, (d) template-suffix-namen checken. **Aanbeveling:** de store is pre-launch (geen SEO-equity/externe links), dus dit is het goedkoopste moment om naar de canonieke handles te gaan. Vraagt wél jouw akkoord omdat het het live thema raakt.

### Vendor
`Kwispelbox` overal ✅.

### Producttype (taxonomy) — grotendeels goed
- Boxen → `Special Editions` ✅
- Cadeau-extra's → `Cadeau-extra` ✅
- GWP → `Gift` ✅
- Legacy `kwispelbox` → `Box` · `inpakken` → leeg · `zippypaws` → `Speeltjes` (worden gearchiveerd)

### Tags — nieuw consistent schema (voorstel)
- **Alle boxen:** `kwispelbox`, `cadeaubox`, `hondencadeau`, `special-edition` + occasion-tag (`verjaardag`/`kerst`/`halloween`/`dierendag`/`valentijn`).
- **Cadeau-extra's:** `kwispelbox`, `cadeau-extra`.
- **GWP:** `gwp-gift`, `hidden` (behouden).
- Opschonen: losse `box`-tag vervangen; geen tijdelijke/import-tags aanwezig.

---

## FASE 4 — Metafields (voorstel, geen duplicaten)

**Bestaand (custom):** `kaart_kleur` (color), `kaart_tekst` (single_line), `curatie_richtlijn` (multi_line, intern).

**Nieuw aan te maken (namespace `custom`, ownerType PRODUCT):**
| Key | Type | Doel |
|-----|------|------|
| `short_description` | multi_line_text_field | PDP-intro (2–3 zinnen) |
| `occasion` | list.single_line_text_field | verjaardag/kerst/halloween/dierendag/valentijn |
| `gift_box_type` | single_line_text_field | `special_edition` |
| `badge` | single_line_text_field | "Verjaardag" / "Limited" / "Favoriet" |
| `whats_inside` | list.single_line_text_field | inhoud van de box (bullets) |
| `dog_size_info` | rich_text_field | maatinfo indien relevant |
| `personalization_available` | boolean | — |
| `card_available` | boolean | — |
| `delivery_note` | single_line_text_field | "Voor 16:00 besteld, morgen in huis" |
| `safety_note` | rich_text_field | veiligheid/ingrediënten |
| `homepage_featured` | boolean | — |
| `sort_priority` | number_integer | volgorde-hint |

**Niet aanmaken (near-duplicaat):**
- `theme_accent` (color) ≈ bestaande `kaart_kleur`. → **hergebruik `kaart_kleur`**.
- `short_description` overlapt deels met `kaart_tekst`; rollen gescheiden houden: `kaart_tekst` = korte homepage-kaartteaser, `short_description` = PDP-intro.

---

## FASE 5 — Varianten (RAPPORT, niets wijzigen)

Alle 5 boxen + legacy `kwispelbox`: optie **`Formaat`** met **Mini / Happy / Mega** (= box-formaat), prijzen €44,95 / €64,95 / €89,95.

⚠️ **Commerciële discrepantie t.o.v. de mockup/PDP-plan** (beslissing Jasper, ik wijzig niets):
- Mockup/plan gaat uit van **`Formaat hond` = Klein / Middel / Groot** à **€29,95–34,95**.
- Store heeft **`Formaat` = Mini / Happy / Mega** à **€44,95–89,95**.
- Dit is een prijs- én optienaam-beslissing. Ik rapporteer alleen; **geen prijs/variant-wijziging zonder expliciet akkoord.**

Overige variant-hygiëne (veilig, later): weight 0 → invullen; barcodes ontbreken (optioneel); variant-afbeeldingen koppelen zodra media bestaat.

---

## FASE 6 — Media (RAPPORT)

**Ontbrekend (blocker voor visueel eindoordeel):**
- Boxen: geen enkele afbeelding. Gewenst: transparante PNG 1600×1600, vrijstaande box, op kleurvlak uit `kaart_kleur`.
  - `product-verjaardagsbox.png`, `product-kerstbox.png`, `product-halloweenbox.png`, `product-dierendagbox.png` (+ evt. valentijn).
- Cadeau-extra's: geen afbeeldingen (op `inpakken` na).
- **Niet automatisch koppelen** zolang bestanden niet in Shopify staan — enkel rapporteren.

---

## FASE 9 — Legacy opschoning (voorstel, NA akkoord)

### ARCHIVEREN (na dependency-check — niet hard-deleten)
| Product | Reden | Afhankelijkheden gecheckt |
|---------|-------|---------------------------|
| `zippypaws-burrow-pinata` | Third-party import/testrestant, voorraad 0, niet in huidige UX | Alleen in `frontpage`+`speeltjes` (beide legacy/ongebruikt door thema); niet in menu-links (staan op `#`); niet in GWP-productscope | 
| `inpakken` | Inpakservice, niet in huidige UX/thema | Alleen in `alle-acties`; geen thema/menu/korting-dependency |

### REVIEW (jouw beslissing)
| Product | Waarom onzeker |
|---------|----------------|
| `kwispelbox` (hoofdbox) | Enige product in collectie `boxen`, waar menu "Boxen" + homepage naar linken. Ook onlogische compare-at (€59,95 op alle formaten). Behouden als evergreen box, óf archiveren en `/collections/boxen` herrichten naar `special-editions`/nieuwe "Alle boxen"? |
| Basic-korting "Gratis mystery verrassing vanaf €89" | Waarschijnlijk dubbel met de BXGY; deactiveren? |
| `maak-er-een-echt-cadeautje-van` (collectie) | Inhoud = duplicaat van `cadeau-extras`; behouden voor een aparte UX-plek of opheffen? |
| `puppy-favorieten` (collectie) | Leeg maar gelinkt vanaf homepage ("Nieuwe puppy"). Geen puppy-product. Link herrichten of collectie vullen? |

### BEHOUDEN + VERRIJKEN
- **Kern (A):** Verjaardag, Kerst, Halloween, Dierendag, Valentijn Kwispelbox.
- **Extra (B):** Persoonlijk wenskaartje, Verjaardagstaartje, Mystery verrassing, Feesthoedje.
- **Systeem (C):** Gratis mystery verrassing 🎁 (niet merchandisen).

---

## Openstaande beslissingen voor Jasper (vóór uitvoering)

1. **Handles** naar `-kwispelbox` omzetten (met redirects + theme-linkupdates)? _[aanbevolen: ja, pre-launch]_
2. **Legacy archiveren:** `zippypaws-burrow-pinata` + `inpakken` — akkoord?
3. **`kwispelbox` hoofdbox:** behouden of archiveren (+ `/collections/boxen` herrichten)?
4. **Dubbele GWP Basic-korting** deactiveren?
5. **Prijzen/varianten** (Mini/Happy/Mega €44,95–89,95 vs mockup Klein/Middel/Groot €29,95–34,95): welke is leidend? _(ik wijzig niets tot je dit bevestigt)_
6. **`puppy-favorieten`**: homepage-link herrichten of collectie vullen?
7. **`maak-er-een-echt-cadeautje-van`** collectie: behouden of opheffen?

## Veilige verrijking die ik zónder structuurbeslissing kan uitvoeren
- Metafield-**definities** aanmaken (idempotent).
- **Tags** consistent zetten op boxen + cadeau-extra's.
- **Korte + lange beschrijvingen** (semantische HTML), **SEO title/description**, per kernproduct.
- **Metafield-waarden** vullen (occasion, badge, whats_inside, delivery_note, booleans, short_description) + ontbrekende `kaart_kleur`/`kaart_tekst` op Valentijn.
- **Shopify category** zetten op boxen/extra's.
- **Alt-tekst** zodra media bestaat (nu nog geen media).

---

_Volgende stap: na jouw akkoord op bovenstaande beslissingen voer ik de veilige verrijking in batches uit en houd ik dit document bij met wat is uitgevoerd (Fase 11 eindrapport)._
