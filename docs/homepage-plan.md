# Homepage-plan & logboek — Kwispelbox KB-v4

Levend document. Plan, beslissingen, admin-setup en notities voor het nabouwen van de homepage
volgens `docs/mockup-home.png`. Productpagina-referentie: `docs/mockup-product-verjaardagsbox.png` (fase 2).
Afbeeldingen-briefing: `docs/afbeeldingen-brief.md`.

## Werkafspraken (deze sessie)
- Store staat in **development (password)**. Jasper gaf tijdelijk toestemming om **direct op `v4/main`**
  te werken en te pushen; Shopify synct naar het actieve thema. Zodra de store publiek gaat → aparte branch.
- De draft **"Kwispelbox v5"** niet aanraken.
- `docs/` en `CLAUDE.md` staan buiten de theme-mappen → worden **niet** door Shopify-sync opgepakt (veilig).

## Aanpak (besloten 29-9)
**Schone homepage op het huidige moderne fundament.** De v4 staat al op een OS 2.0/Skeleton-basis.
Behoud de werkende product-/collectie-/cart-pagina's + SEO + mega-menu. Herbouw de 9 homepage-secties
volledig schoon volgens Shopify-best-practices, verwijder legacy homepage-secties uit `index.json`
(bestanden blijven), en ruim gaandeweg op (fonts in git, CSS-prefixes, header vereenvoudigen).
Geen volledige from-scratch herbouw.

## Vastgelegde keuzes
1. **Aankondigingsbalk**: los `announcement-bar.liquid` gebruiken (bruin), ingebouwde header-balk uitzetten.
2. **Menu**: mockup-indeling — Boxen, Verjaardag, Feestdagen, Momenten, Kwispelclub, Zakelijk — met per-item instelbaar icoon.
3. **Extra secties** (product-boxes, box-contents, reviews, community, why-kwispelbox): van de homepage af
   (uit `index.json`), bestanden blijven behouden.
4. Admin-setup (collecties, metafields, menu) doet Claude via de Shopify MCP.

## Sectie-plan & status
| # | Sectie | Bestand | Aanpak | Status |
|---|--------|---------|--------|--------|
| 1 | Aankondigingsbalk | `announcement-bar.liquid` | aanpassen (bruin, 3 blocks) | ✅ gedaan |
| 2 | Header | `header.liquid` | hergebruiken + menu/iconen | ⬜ te doen |
| 3 | Hero | `hero.liquid` | aanpassen (3 kleurregels + 3 USP-kaartjes) | ⬜ te doen |
| 4 | Voor elk moment | `moments.liquid` (nieuw) | nieuw, 6 tegels | ⬜ te doen |
| 5 | Meest gekozen | `featured-products.liquid` (nieuw) | nieuw, collectie + metafield-kleur | ⬜ te doen |
| 6 | Verjaardagsblok | `birthday-block.liquid` (nieuw) | nieuw, gele kaart + klantformulier | ⬜ te doen |
| 7 | USP-balk | `usp-bar.liquid` | hergebruiken, 4 blocks | ⬜ te doen |
| 8 | Blije honden | `happy-dogs.liquid` (nieuw) | nieuw, fotoslider | ⬜ te doen |
| 9 | Footer | `footer.liquid` | kleine aanpassing | ⬜ te doen |

Toe te voegen iconen (SVG): ~~`clock`~~ ✅, `home`/huis, pleister (beterschap), `bell`.

### Sectie 1 — Aankondigingsbalk (gedaan)
- `announcement-bar.liquid` herschreven: bruin (`bg_color`/`text_color` instelbaar), cream tekst+iconen,
  3-up desktop, marquee mobiel. Preset = "Gratis verzending vanaf €50" / "Voor 16:00 besteld, morgen in huis" / "Met een persoonlijk kaartje".
- Nieuw `assets/icon-clock.svg`.
- `header-group.json`: announcement-bar als eerste sectie toegevoegd; oude `trust_item`-blocks uit de header
  verwijderd (voorkomt dubbele balk). Header rendert die balk alleen bij aanwezige trust_item-blocks.
- `shopify theme check`: 0 nieuwe fouten. (6 bestaande MissingAsset-fouten voor `.woff2`-fonts — zie aandachtspunt.)

### Aandachtspunt: fonts niet in git
`lilita-one-*.woff2` en `karla-*.woff2` worden gerefereerd in `css-variables.liquid`/`theme.liquid` maar
staan niet in de repo (wél op het live thema). Werkt nu, maar een verse deploy vanaf git mist ze.
Later: fontbestanden alsnog in `assets/` committen.

## Admin-setup (Shopify)
- Store: `kwispelbox.myshopify.com` (EUR, NL).
- Metafield-definitie **`custom.kaart_kleur`** (product, type color) → kaart-achtergrond in "Meest gekozen". ✅ gedaan (`gid://shopify/MetafieldDefinition/297327165605`)
- Collectie **"Meest gekozen"** (handmatig, handle `meest-gekozen`, `gid://shopify/Collection/490658594981`) met 4 boxen: Verjaardag, Kerst, Halloween, Dierendag. ✅ gedaan
- Kaartkleuren gezet: Verjaardag `#FFB8CD` (roze), Kerst `#66AA44` (groen), Halloween `#F7A048` (oranje), Dierendag `#FFE071` (geel). ✅ gedaan
- Menu **`main-menu`** bijwerken naar mockup-indeling (bij bouw sectie 2). Status: ⬜

## Catalogus-notities & discrepanties (belangrijk)
De mockup toont een **andere product-opzet** dan de huidige store. Dit is een prijs/assortiment-beslissing
voor Jasper — Claude past dit **niet** eigenhandig aan:
- Mockup-boxen: **€29,95–34,95**, varianten **Klein/Middel/Groot** (hondformaat).
- Huidige store: boxen **€44,95–89,95**, varianten **Mini/Happy/Mega** (boxformaat); 1 hoofdproduct
  "Kwispelbox" + Special Editions (Verjaardag, Kerst, Halloween, Valentijn, Dierendag).
- Mockup toont **"Puppy Welkomstbox"** — bestaat nog niet.

Bestaande box-producten (voor "Meest gekozen"):
- Verjaardag Kwispelbox — `verjaardag-box`
- Kerst Kwispelbox — `kerst-box`
- Halloween Kwispelbox — `halloween-box`
- Valentijn Kwispelbox — `valentijn-box`
- Dierendag Kwispelbox — `dierendag-box`
- Kwispelbox (hoofd) — `kwispelbox`
