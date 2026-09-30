# Kwispelbox — Modulaire PDP (plan & uitvoering)

_Opgesteld 30-09-2026. Doel: één herbruikbare, metafield-gedreven PDP voor alle 5 boxen; occasion-verschil uit metafields + conditionele rendering; koopflow ongemoeid; Shopify 2.0 / app blocks intact._

## Uitgangspunten (uit briefing)
- Eén gedeeld basis-template voor alle 5 boxen (geen template-per-occasion).
- Content data-gedreven uit `custom.*` metafields; geen dubbele bron van waarheid.
- Koopflow niet breken: radiogroep `name="variant-choice"` + autoritatieve hidden `name="id"` (`[data-variant-id]`), `properties[Naam hond]`, JS-hooks (prijs-sync, dogname-echo, sticky ATC, countdown), GWP/cart ongewijzigd.
- Snelle checkout blijft uit. Judge.me intact. Prijzen/varianten ongemoeid.
- NL/BE-klaar: levering/veiligheid uit aparte metafields, geen NL-only hardcode in gedeelde tekst.

## Huidige situatie (audit)
- 5 losse templates `product.{occasion}-box.json` met occasion-content **hardcoded** in section-settings/blocks van `main-product-kwispelbox` + `box-contents` + `product-faq` + `product-cta`.
- Buy box `main-product-kwispelbox.liquid` (1029 r.) leest **nog geen** `product.metafields`. Levertijd komt uit globale settings (blijft).
- Personalisatie-details bevatten nu véél velden (Naam, Levensfase, _Geboortedatum, Formaat, Favoriete speeltjes, Allergieen, Bericht, Foto) — meer dan operationeel verwerkbaar; FAQ bevat een **allergie-belofte** (moet weg).

## Metafields → PDP-mapping
| Metafield | Waar in PDP |
|-----------|-------------|
| `custom.badge` | Galerij-badge / eyebrow (fallback `section.settings.badge_primary`) |
| `custom.short_description` | Intro onder titel/prijs (fallback `section.settings.intro`) |
| `custom.occasion` | Accent-kleur-mapping + conditionele copy + cross-sell-filter |
| `custom.whats_inside` (list) | Sectie A "Wat zit erin?" als cards |
| `custom.dog_size_info` (rich text) | Sectie B "Voor welke hond?" info-blok |
| `custom.personalization_available` (bool) | Toont/verbergt personalisatie-block |
| `custom.card_available` (bool) | Toont kaartkeuze binnen personalisatie |
| `custom.delivery_note` | Sectie D "Levering" (accordion) |
| `custom.safety_note` (rich text) | Sectie E "Veiligheid" (accordion) |
| `custom.kaart_kleur` | Accent-vlak/hint (secundair) |

Accent-mapping uit `occasion` (geen per-template kleur meer): verjaardag→yellow, kerst→green, halloween→orange, dierendag→yellow, valentijn→pink. Fallback `section.settings.accent`.

## Wat blijft / wat verandert
**Behouden (ongewijzigd):** mediagalerij + lightbox, prijs/compare/save + JS-sync, variantkaarten (`box_option` blocks) + variant-mechaniek, hondennaam-veld, levertijd-belofte (globale settings), trust badges, garantie-zegel, sticky ATC, countdown, Judge.me, `product-how-it-works`, `why-kwispelbox`.

**Refactor (metafield-gedreven, met fallback naar bestaande settings):**
- `main-product-kwispelbox`: badge, intro/short_description, accent uit metafields; **nieuwe blokken** in info-kolom: "Voor welke hond?" (B), "Levering" (D) + "Veiligheid" (E) als toegankelijke `<details>`-accordions.
- **Personalisatie vereenvoudigd** naar module C: Naam hond (blijft prominent), Leeftijd (optioneel), korte Boodschap, en Kaartkeuze alleen als `card_available`. Conditioneel op `personalization_available`. **Verwijderd:** Allergieen (+ bijbehorende claim), Levensfase, Favoriete speeltjes, Foto, Formaat-property (niet operationeel verwerkbaar / dubbel met variant). Property-naam `properties[Naam hond]` blijft exact.
- `box-contents`: rendert `custom.whats_inside` als cards; valt terug op block-cards als metafield leeg. Sectie verbergt zich als er geen data is.
- `product-faq`: **allergie-FAQ verwijderd**; overige generiek gedeeld.

**Nieuw:**
- Sectie B "Voor welke hond?" (compact info-blok, uit `dog_size_info`).
- Accordions D/E (levering/veiligheid) in de info-kolom.
- Contextuele cross-sell: cadeau-extra's onder de fold (verjaardag → feesthoedje/taartje/kaartje/mystery; andere occasions → relevante subset). Data uit de `cadeau-extras`-collectie, gefilterd; geen "anderen kochten ook".
- Gerelateerde boxen (3–4 andere Special Editions, huidige uitgesloten).

## Template-consolidatie
- Nieuw gedeeld `templates/product.box.json` (metafield-gedreven, generieke settings, geen occasion-hardcode).
- Alle 5 boxen → `templateSuffix: box` (via Admin API).
- Oude `product.{occasion}-box.json` verwijderen (met redirect n.v.t. — templates hebben geen URL).
- `product.json` (default) + `product.kwispelbox.json` (gearchiveerd product) ongemoeid.

## Uitvoering in batches
1. **Buy box** metafield-wiring + nieuwe modules (B/D/E) + personalisatie vereenvoudigen + allergie-claim eruit.
2. **box-contents** ← `whats_inside`.
3. **product-faq** opschonen (allergie eruit).
4. **Template-consolidatie** → `product.box.json`, `templateSuffix` herzetten, oude templates weg.
5. **Cross-sell + gerelateerde boxen**.
6. Theme check · PDP verifiëren (box mét personalisatie + gedrag als `personalization_available=false`) · mobiel · commit per batch.

## Uitvoeringsstatus (30-09-2026)
- ✅ **Batch 1 — buy box** (`main-product-kwispelbox.liquid`): badge/short_description/accent uit `custom.*`; nieuwe modules "Voor welke hond?" (`dog_size_info`) + Levering/Veiligheid-accordions (`delivery_note`/`safety_note`); personalisatie vereenvoudigd naar module C (leeftijd optioneel, boodschap, kaartkeuze als `card_available`), conditioneel op `personalization_available`; **allergie-veld + -claim verwijderd**; volledige `descriptionHtml` niet meer getoond (geen dubbeling; blijft SEO-bron).
- ✅ **Batch 2 — box-contents**: rendert `custom.whats_inside` als cards (icoon/tekst-mapping per categorie), fallback naar block-cards.
- ✅ **Batch 4 — consolidatie**: nieuw gedeeld `templates/product.box.json` (metafield-gedreven, generiek, geen allergie-FAQ). Alle 5 boxen → `templateSuffix: box`. Oude `product.{occasion}-box.json` verwijderd. (`product.kwispelbox.json` blijft staan voor het gearchiveerde hoofdproduct — ongebruikt, later evt. opruimen.)
- ✅ Batch 3 (allergie-FAQ) opgelost via consolidatie (nieuwe FAQ zonder allergie-claim).
- ✅ **Aanscherping buy-box (UX-compositie, na review)**: koopflow gecomprimeerd naar badge → titel → short_description → prijs → maat → één "Maak het persoonlijk"-module (naam hond samengevoegd) → qty+CTA → 3 trustregels. "Voor welke hond?"/Levering/Veiligheid verplaatst naar onder "Wat zit erin" (box-contents) mét correcte rich-text-render (directe metafield-ref, niet `.value`). Variantkaarten Mini→Happy→Mega + ~30% compacter; ±verrassingen/±€-claims weg. Primaire CTA weer groen (accent enkel voor prijs/selectie/borders). Groot geel garantie/GWP-blok weg (GWP = compacte regel; 30-dagen-garantie verwijderd want onbevestigd). Losse producttekst onder gallery weg. Duplicatie verzend-/trustinfo teruggebracht.
- ✅ **Mini-spacing-pass (na review)**: rechterkolom ~10–15% compacter; prijs steviger via `--pac-strong` (Halloween-oranje leesbaarder); smallere/rustigere qty-stepper; zachter "optioneel"-label.
- ✅ **Batch 5** (`product-extras.liquid` + `product-related.liquid`): contextuele cross-sell — max 3 cadeau-extra's uit collectie `cadeau-extras`, occasion-gefilterd (taartje alleen bij verjaardag), alleen beschikbare producten, compacte cards met "+" quick-add via `cart/add.js` (valt terug op normale POST), additief (geen 2e checkout), header-cart-teller update. Gerelateerde boxen — max 3 Special Editions, huidige uitgesloten, compacte cards → PDP. Toegevoegd aan `product.box.json` (order: main, contents, extras, jmereviews, faq, related, cta).
- ⏳ **Openstaand**: 30-dagen-garantie alleen terugzetten als het formeel beleid is (nu bewust weg). Assets (product-PNG's) door Jasper. Visuele check cross-sell/related door Jasper.
- ⏳ Verificatie: PDP visueel checken (kan niet screenshotten — Jasper) op box mét personalisatie + mobiel. Theme check schoon (6 bekende font-errors + 1 pre-existing HardcodedRoutes-warning).

## Openstaand / aannames
- Alle 5 boxen hebben nu `personalization_available=true` → "een zonder" test = tijdelijk simuleren of vertrouwen op de conditie.
- Contextuele cross-sell filtert op de `cadeau-extras`-collectie (geen aparte metafield nodig); later evt. `related_products` metafield.
- Visuele stijl = homepage (crème, Lilita/Karla, rounded cards, pastel, zachte shadows, subtiele doodles).
