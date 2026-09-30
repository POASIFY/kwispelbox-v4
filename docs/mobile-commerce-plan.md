# Kwispelbox — Mobile commerce plan (PDP + cart drawer)

_Gestart 30-09-2026. Mobile-first (360/390/430, check 749). Desktop + header/drawer bevroren._

## Audit (uitgangspunt)
- **Cart = pagina** (`sections/cart.liquid`, `templates/cart.json`) met AJAX (cart/add|change|update.js), GWP-logica (GIFT_ID/THRESHOLD) en progress. **Geen cart-drawer** — header linkt hard naar `/cart`.
- **PDP-mobile al klaar** (vorige batches): sticky ATC (groen/safe-area/observer/realtime prijs), gallery `object-fit: contain`, geen shipping-badge op beeld, compacte varianten/trust/GWP, guarantee verwijderd, reviews empty-state, "Wat zit erin" no-image 2-koloms.

## Staged commits
- **A1** (hoger risico, screenshot gewenst): gallery swipe + dots; varianten mobiel compact.
- **A2** (laag risico): gerelateerde boxen swipe ✅ · cadeau-extra's swipe (kaart-redesign nodig) · FAQ/support compact.
- **B1** cart-drawer basis (nieuw component, hergebruik cart.liquid-logica).
- **B2** free-shipping/GWP progress + upsell (max 2) + AJAX qty/remove.
- **B3** a11y (dialog/focus-trap/ESC/scroll-lock) + QA breakpoints/states.

## Openstaande beslissingen (NIET zelf gekozen)
1. Cart-drawer vervangt hard-nav naar /cart op mobiel? (cart-page blijft als "Bekijk winkelwagen").
2. 30-dagen-garantie: blijft weg tenzij formele policy bevestigd.
3. Quick-add extra → drawer openen vs. alleen ✓-feedback.

## Uitgevoerd
- A2 (deel): **gerelateerde boxen** → horizontale scroll-snap-carousel op ≤749px (~1.25 kaart, edge-bleed). `sections/product-related.liquid`.

## Uitgevoerd — vervolg
- **A2 rest:** cadeau-extra's → compacte verticale-card swipe-carousel (~1.25 kaart, quick-add ✓ blijft, geen drawer); FAQ + support-card mobiel compacter. (`product-extras.liquid`, `product-faq.liquid`)
- **A1:** PDP-gallery horizontale swipe + dots op ≤749px (thumbnails→dots, zoom-knop uit op mobiel, `object-fit: contain` behouden); compacte variant-rows. Desktop fade-slide + thumbnails ongewijzigd; lightbox/variant/sticky-JS intact. (`main-product-kwispelbox.liquid`)

### Screenshot nodig voor visuele finetuning A1
Een **390px PDP-screenshot van een product met ≥2 media** (om swipe + dots + gallery-hoogte te verifiëren). De boxen hebben nu 1 media (main packshot) → dots verschijnen pas bij ≥2 media (Fase-2 gallery-beelden).

## ⚠️ Open architectuur-/GWP-punt vóór cart drawer (Deel B)
De **gratis verrassing (GWP)** wordt nu als fysiek regelitem toegevoegd/verwijderd door de **JS van de cart-PAGINA** (`cart.liquid`, op basis van drempel) en gratis gemaakt door de automatische **BXGY**-korting. De BXGY discount voegt het product niet zelf toe — hij verlaagt het alleen naar €0 als het in de cart zit.

Gevolg voor een **drawer-first flow**: als een klant kwalificeert (≥ €{{ gift }}) maar nooit de cart-pagina bezoekt, wordt het gratis product niet toegevoegd → hij mist de verrassing.

Jij gaf aan: "drawer toont alleen status, niet zelf dupliceren." Dat volg ik. Maar dan is er een keuze:
- **A.** Drawer toont alleen GWP-status; gift-add blijft alleen op de cart-pagina (risico: gemist bij pure drawer→checkout).
- **B.** Gift-add-logica globaliseren (één gedeelde bron die op elke cart-mutatie draait, ook in de drawer) — netter, maar raakt de bestaande cart-JS.

Dit is een architectuurkeuze — ik kies niet zelf. Rest van de drawer (items, qty/remove, gratis-verzending-progress, GWP-status-display, checkout-CTA) kan ik gewoon bouwen.
