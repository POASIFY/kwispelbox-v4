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

## Deel B — Cart drawer (uitgevoerd)
- **GWP gecentraliseerd** (`snippets/gwp-sync.liquid`, `window.kbGwp.sync()`), globaal geladen; cart-pagina delegeert ernaar (dedup). Config: drempel uit `settings.kb_gift_threshold`, gift-variant uit product `gratis-mystery-verrassing`. Idempotent + lock; gift telt niet mee als qualifying; BXGY = enige €0-bron.
- **Cart-drawer** (`snippets/cart-drawer.liquid`, mobiel ≤749px, globaal in `theme.liquid`):
  - Triggers: header-cart → drawer; hoofd-add-to-cart (#pdp-form) → add + GWP-sync + drawer open; upsell quick-add → drawer blijft open; PDP cadeau-extra quick-add → géén drawer, wel GWP-sync + tellers (`product-extras.liquid`).
  - Render uit `/cart.js` (enige bron): items (thumbnail, titel, variant, personalisatie-props, prijs, −/+ , verwijderen), gift-regel als "Gratis 🎁" zonder controls.
  - Progress: gratis-verzending-bar (`settings.kb_free_shipping`) + GWP-status (`window.kbGwp.status`), realtime.
  - Upsell: max 2 cadeau-extra's, sluit in-cart + gift uit, quick-add ✓.
  - Footer: subtotaal + groene "Naar afrekenen" (/checkout) + "Bekijk winkelwagen" (/cart fallback).
  - A11y: role=dialog, aria-modal, focus-trap (window.kbTrap), ESC, scroll-lock, safe-area-bottom, prefers-reduced-motion. z-index 400 (boven bottom-nav). Desktop: drawer opent NIET (header-cart → /cart ongewijzigd).
  - Race-guards: `busy`-flag + `kbGwp.running`-lock → geen dubbele adds/gift.

### Nog te verifiëren (screenshots/echte device) — states
€80→+€10 (gift 1x), €95→remove→€70 (gift weg), snelle quick-adds (geen dubbel), refresh >€89 (gift 1x), drawer→checkout zonder /cart (gift aanwezig), handmatige gift-remove >€89 (hersteld op cart-page/sync), cart leeg (gift weg). Logica is idempotent; visuele/interactie-check op device nodig.
