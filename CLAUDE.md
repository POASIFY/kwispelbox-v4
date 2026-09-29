# Kwispelbox.com — Shopify theme (KB-v4)

## Doel
We bouwen de homepage (en daarna de productpagina) na volgens `docs/mockup-home.png`.
Kwispelbox verkoopt cadeauboxen voor honden voor verjaardagen, feestdagen en andere momenten.
Het zijn echte pakketten: een kraftbruine doos met groene en roze pootjes en de tekst "Kwispelbox.com".

Sfeer: speels, kleurrijk, echt een hondenwereld. De dozen staan centraal.

## Store & theme
- Store: `kwispelbox.myshopify.com`
- Theme: KB-v4, theme ID `156055404709`
- Repo: `github.com/POASIFY/kwispelbox-v4`, lokaal in `~/Downloads/kwispelbox-v4`
- Dit theme is gekoppeld aan GitHub (branch `main`). Shopify synct zelf met de repo.

## Werkwijze
1. Begin elke sessie met `git pull`.
2. Bekijk wijzigingen lokaal met `shopify theme dev --store kwispelbox.myshopify.com`.
3. Draai `shopify theme check` na elke sectie. Doel: 0 errors.
4. Commit per sectie met een duidelijke boodschap en push naar `main`. Shopify synct het theme daarna vanzelf.
5. Gebruik GEEN `shopify theme push` op dit theme: dat botst met de GitHub-sync.
   Bij een sync-conflict: `git fetch origin && git merge -X ours origin/main && git push`.
6. Nooit publiceren of iets aan het live theme veranderen.
7. Bouw één sectie per keer en stop daarna, zodat Jasper kan kijken.

## Merk
- Groen `#66AA44` (donker `#4d8833`, licht `#e8f5e2`)
- Roze `#FFB8CD` (donker `#e75480`, licht `#fff0f5`)
- Oranje `#F7A048` (donker `#d9823a`, licht `#fff0e4`)
- Geel `#FFE071`
- Bruin `#3d2209` (tekst), `#603A15`
- Crème achtergrond `#FBF6EE`
- Fonts: Lilita One voor koppen, Karla voor tekst. Laden via `<link>` in `theme.liquid`, nooit via `@import`.
- Knoppen: afgeronde pillen, groen met een pootje-icoon. Kaarten: ronde hoeken, pastel achtergrond.
- Decoratie: pootjes, hartjes, sterretjes en botjes als losse SVG's, subtiel pootjespatroon op de achtergrond.
- Contrast: witte tekst alleen op de donkere varianten (`#4d8833`, `#e75480`).
- Alle teksten in het Nederlands.

## Code-regels
- Elke sectie is een los bestand in `sections/` met een volledig `{% schema %}`.
  Alles wat Jasper wil aanpassen is een setting of block: teksten, kleuren, afbeeldingen, links, collecties.
- Geen hardcoded teksten, producten of prijzen. Producten komen uit collecties.
- CSS per sectie in het sectiebestand of een eigen asset, met een eigen prefix.
  Bestaande prefixes: `kw-` (header/globaal), `kc__` (collectie), `pb__` (productbox), `ps__` (product simpel).
- Iconen als inline SVG (snippets in `snippets/icon-*.liquid`), geen emoji.
- Afbeeldingen via `image_url` en `image_tag` met `loading: lazy` (behalve de hero), met alt-tekst.
- Mobiel eerst: elke sectie moet goed werken op 390px breed.
- Gebruik de Shopify Dev MCP om Liquid-filters, objecten en schema-opties te controleren.

## Homepage-secties (volgorde van bouwen)
1. **Aankondigingsbalk**: bruin, 3 blocks (icoon + tekst).
2. **Header**: logo, zoekbalk, account, verlanglijst, roze winkelwagenknop met teller.
   Menu met icoon per item: Boxen, Verjaardag, Feestdagen, Momenten, Kwispelclub, Zakelijk.
3. **Hero**: kop met woorden in drie kleuren (instelbaar per regel), subtekst, knop, grote afbeelding,
   roze ronde sticker (tekst instelbaar), 3 USP-kaartjes als blocks.
4. **Voor elk moment**: blocks met icoon, titel, achtergrondkleur en collectie-link. Standaard 6.
5. **Meest gekozen**: producten uit een gekozen collectie. Achtergrondkleur per kaart uit
   product-metafield `custom.kaart_kleur`, met fallback-kleuren in volgorde.
   Kaart: foto, titel, korte omschrijving, prijs, verlanglijst-hart, knop "Bekijk".
6. **Verjaardagsblok**: gele kaart, polaroid-foto, kop, formulier met naam hond + verjaardag + e-mail.
   Voorlopig via het Shopify klantformulier (`form 'customer'`) met tags; later mogelijk Klaviyo.
7. **USP-balk**: 4 blocks.
8. **Blije honden, blije baasjes**: fotoslider met afbeeldingsblocks.
9. **Footer**: bruin, logo, social iconen, linkkolommen, nieuwsbrief.

## Productpagina (later)
Opties op de productpagina gaan als regelitem-eigenschappen mee:
`properties[Naam op bandana]`, `properties[Kaarttekst]`, `properties[Bezorgdatum]`.
Formaat hond (Klein / Middel / Groot) is een variant.
