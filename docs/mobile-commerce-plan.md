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
