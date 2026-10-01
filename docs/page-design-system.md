# Kwispelbox — Page Design System (4 herbruikbare families)

_Opgesteld: 01-10-2026 · Hoort bij `docs/store-quality-audit.md`._
_Doel: niet 20 pagina's los ontwerpen, maar **4 herbruikbare page-families** zodat elke basispagina met minimale inzet een hoogwaardige Kwispelbox-ervaring wordt._
_Dit is een **plan** — nog geen implementatie. Bouwen pas na akkoord, gefaseerd._

---

## 0. Uitgangspunt

De theme heeft al bruikbare bouwstenen (`info-page`, `about-page`, `contact-page`, `payment-page`, `page-intro`, `product-faq`, `product-cta`, `product-how-it-works`, `box-compare`, `product-boxes`). We **bouwen hierop voort** en voegen een consistente *shell* + een paar herbruikbare blokken toe, i.p.v. alles opnieuw.

Elke familie = **1 sectiebestand (of set) + schema met blocks**, zodat Jasper in de editor teksten/links/iconen beheert zonder code.

---

## 1. Designtaal (tokens — gelijk aan homepage/PDP)

| Token | Waarde | Gebruik |
|---|---|---|
| Crème achtergrond | `#FBF6EE` (`--color-cream`) | pagina-grond |
| Chocoladebruin | `#3d2209` (`--color-ink`) | koppen/tekst |
| Kwispelbox-groen | `#66AA44` / donker `#4d8833` | primair accent, CTA |
| Roze | `#FFB8CD` / donker `#e75480` | secundair accent, eyebrow |
| Oranje/geel | `#F7A048` / `#FFE071` | highlights |
| Koppen | **Lilita One** | hero/sectiekoppen |
| Body | **Karla** | lopende tekst |
| Vormen | `--radius-lg`/`--radius-pill`, zachte borders `rgba(61,34,9,.06)`, schaduw `0 10-24px rgba(61,34,9,.05-.07)` | kaarten/panelen |
| Details | pootjes/hartjes/sterretjes/gift als inline SVG (`kb-icon`), subtiel | decoratie |

**Regel:** premium & rustig > kinderachtig. Decoratie subtiel; whitespace royaal; max ~1 decoratief accent per hoek. Witte tekst alleen op `#4d8833`/`#e75480`.

**Gedeelde shell (alle families):**
- Volle-breedte sectie op crème grond.
- **Branded hero/intro-card** bovenaan: eyebrow (pill) + Lilita-kop + korte intro, op een zacht paneel (gradient crème→licht-tint) met subtiele border/schaduw + 1 hoekdecoratie.
- Content `max-width` ~820–960px, royale `--section-pad`.
- Afsluiting: support/CTA-blok + interne links.
- Mobiel: alles compact gestapeld, ≥16px inputs, 44px tap-targets, `min-width:0` op flex/grid children (Safari-safe).

---

## A. SERVICE TEMPLATE
**Voor:** verzending · betalen · retourneren · klachten · annuleren · bestelling volgen · (account-uitleg).
**Vervangt/verrijkt:** de huidige generieke `info-page` (niveau C).

**Structuur (blocks):**
1. **Branded hero/intro-card** — eyebrow + kop + 1–2 zinnen.
2. **Quick-info cards (2–4)** — icoon + titel + 1 regel (bijv. "Voor 16:00 = morgen in huis", "Gratis vanaf €50", "14 dagen bedenktijd"). Zachte kaarten, kleuraccent per kaart.
3. **Hoofdcontent in nette secties** — kop + alinea's + evt. stappen (genummerd) of bullets met pootje-icoon.
4. **Accordions** waar geschikt (detailvragen) — zelfde stijl als de nieuwe box-contents-accordion (zachte kaart, chevron).
5. **Support/contact-CTA** — "Toch nog vragen?" → knop Contact + e-mail.
6. **Relevante interne links** (chips) — zie §Interne linking.

**Mapping:** `verzenden-en-bezorgen`, `retourneren`, `klachten`, `betaalmogelijkheden` (payment-blocks behouden), + nieuw `bestelling-volgen`, `retour-aanmelden`, `annuleren-of-wijzigen`.

---

## B. BRAND / EDITORIAL TEMPLATE
**Voor:** Over ons · Waarom Kwispelbox · Veilig spelen · Onderhoud speeltjes & knuffels.
**Vervangt/verrijkt:** `about-page` (generaliseren).

**Structuur (blocks):**
1. **Editorial hero** — groot beeld + kop + intro (image+text split, zacht paneel).
2. **Image + text split-rijen** (alternerend links/rechts) — story/uitlegblokken met beeld.
3. **Story-/value-cards** (3) — icoon + titel + korte tekst (hergebruik `value`-block-idee).
4. **Zachte achtergrond-secties** met 1 decoratief accent.
5. **CTA naar boxen** — "Verras je hond" → `/collections/boxen`.

**Mapping:** `over-ons`, + nieuw `waarom-kwispelbox`, `veilig-spelen`, `onderhoud-speeltjes-knuffels` (NB: **niet** "hondenknuffels" in titel).

---

## C. FAQ / HELP HUB
**Voor:** Klantenservice (hub, **nieuw**) · Veelgestelde vragen.

**Structuur (blocks):**
1. **Hero met zoek-/onderwerpintro** ("Waarmee kunnen we helpen?").
2. **Categorie-cards** (grid) — Bestellen & betalen / Verzending / Retour / Account / Over de box — elk linkt naar de juiste Service-pagina of FAQ-anker.
3. **Accordions** per veelgestelde vraag (hergebruik `product-faq`/`faq`).
4. **Support-CTA** — contact + e-mail + openingstijden.

**Mapping:** nieuw `klantenservice` (hub) + bestaande `veelgestelde-vragen` (opwaarderen tot deze stijl).

---

## D. LEGAL TEMPLATE
**Voor:** privacy (indien page) · algemene voorwaarden · cookiebeleid · toegankelijkheid · bedrijfsgegevens · herroepingsformulier.
**Vervangt:** legal via generieke `info-page` (niveau C).

**Structuur:**
1. **Compacte branded hero** (klein, rustig) — titel + "laatst bijgewerkt"-datum.
2. **Inhoudsopgave (TOC)** met **anchor-links** naar secties.
3. **Leesbare content-secties** met duidelijke `id`'s (anchors), goede typografie (ruime line-height, max ~70ch).
4. **Zachte branded shell** rond de tekst — géén decoratie die leesbaarheid schaadt.
5. Voetnoot met bedrijfsgegevens/contact.

**Eisen:** juridisch goed leesbaar, TOC + section-anchors, nette typografie, niet "plain Shopify". Inhoud **niet** inhoudelijk wijzigen zonder akkoord; alleen presentatie.
**Mapping:** `algemene-voorwaarden`, `cookiebeleid`, + nieuw `toegankelijkheid`, `bedrijfsgegevens`, `herroepingsformulier`. Privacy blijft Shopify Policy (link, niet nabouwen).

---

## Interne linking (IA)

Elke servicepagina linkt logisch door (chips onderaan), **geen SEO-linkspam**:

| Vanaf | Naar |
|---|---|
| Verzending & bezorging | Bestelling volgen → Contact |
| Retourneren | Retour aanmelden → Klachten & garantie |
| Bestellen & betalen | Mijn account → FAQ |
| Klachten & garantie | Retourneren → Contact |
| Bestelling volgen | Mijn account → Contact |
| FAQ / Klantenservice-hub | alle service-pagina's (categorie-cards) |
| Brand-pagina's | → `/collections/boxen` (CTA) |

Footer-menu's (`footer-service`, `menu-over-ons`, `footer-shop`) blijven de hoofdnavigatie; de Help-hub wordt het centrale service-knooppunt.

---

## Implementatie-aanpak (na akkoord, gefaseerd)

1. **Fase 1:** bouw **Service-template (A)** + **Legal-template (D)** als nieuwe sectiebestanden met schema/blocks. Rol uit op bestaande C-pagina's (verzending/retour/betalen/klachten + voorwaarden/cookiebeleid) — presentatie, bestaande content behouden/licht aanvullen.
2. **Fase 2:** **Help-hub (C)** + ontbrekende service-pagina's (bestelling volgen, retour aanmelden, annuleren).
3. **Fase 3:** **Brand-template (B)** + nieuwe content-pagina's.
4. **Fase 4:** opruimen (orphan template, mega-menu, lege collecties) + interne links + menu's bijwerken.

Per fase: theme check, 1 oog-check (390/1280px), kleine commits, review vóór volgende fase.

---

## Niet-doen (randvoorwaarden)
- Geen commerciële claims verzinnen; bestaande content hergebruiken/herschikken.
- Juridische inhoud niet inhoudelijk wijzigen zonder expliciete noodzaak/akkoord.
- Geen cart/PDP/functionele architectuur raken.
- Niets publiceren/verwijderen zonder akkoord.
- Entiteits-/bedrijfsnaam niet automatisch aanpassen (LEGAL REVIEW).

_Einde plan. Wacht op akkoord + prioriteitskeuze (voorstel: start Fase 1 = Service + Legal template)._
