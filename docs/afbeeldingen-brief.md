# Afbeeldingen-brief — Kwispelbox homepage (KB-v4)

Alles wat aan beeld nodig is voor de homepage volgens `docs/mockup-home.png`.
Kant-en-klare generatie-prompts, formaten, exacte bestandsnamen en mobiele varianten.

## Uploadregels (belangrijk)
- **Bestandsnamen exact overnemen**: lowercase, koppeltekens, **geen spaties of accenten**.
  Shopify plakt er anders een suffix (`_1`) achteraan en dan kan ik het beeld niet automatisch koppelen.
- **Waar uploaden:**
  - Losse beelden (hero, polaroid, slider, footer) → **Content → Bestanden (Files)**.
  - De 4 productfoto's → op het **product zelf** (Producten → [product] → Media).
- **Formaat:** PNG **mét transparantie** waar aangegeven, anders JPG (hoge kwaliteit).
  Upload gerust groot (2000px+/enkele MB); Shopify verkleint automatisch. Geen tekst in beeld
  behalve de opdruk op de doos.
- Ik koppel `shopify://shop_images/<bestandsnaam>` als default, dus zodra jij uploadt met de juiste
  naam verschijnt het beeld vanzelf in de sectie.

## Merk-stijl (plak dit vóór elke prompt voor consistentie)
> Kwispelbox: speelse, kleurrijke hondenwereld. Het hoofdproduct is een **kraftbruine kartonnen doos**
> met groene (#66AA44) en roze (#FFB8CD) pootjes en de opdruk **"Kwispelbox.com"** met daaronder
> "voor blije honden". Warme crème achtergrond #FBF6EE. Accentkleuren groen #66AA44, roze #FFB8CD,
> oranje #F7A048, geel #FFE071. Zacht natuurlijk licht, vrolijk, premium maar vriendelijk.
> De doos staat centraal. Fotorealistisch.

---

## 1. Hero

### `hero-home-desktop.png`
- **Formaat:** PNG met **transparante achtergrond**, ± **1760×1600 px** (≈1.1:1)
- **Prompt:**
  > [merk-stijl] Een blije golden retriever ligt naast een open Kwispelbox (kraftbruine doos met
  > groene en roze pootjes en opdruk "Kwispelbox.com"). De doos puilt uit met hondencadeautjes:
  > een roze donut-knuffel, een gevlochten touwspeeltje, een tennisbal, snackzakjes ("Good Dog
  > Treats", "Pawsome"), een klein bruin teddybeertje en een kaartje "Voor een heel bijzondere hond!"
  > dat eruit gluurt. Onderwerp **vrijstaand op transparante achtergrond** (geen kamer/omgeving),
  > zodat het op onze crème sectie zweeft. Fel, vrolijk, zachte slagschaduw.

### `hero-home-mobile.png`
- **Formaat:** PNG met **transparante** achtergrond, **1200×1200 px** (1:1) of **1080×1350** (4:5)
- **Prompt:**
  > [merk-stijl] Blije golden retriever met een roze gestippeld feesthoedje, zittend vlak naast een
  > open Kwispelbox (kraftbruine doos met groene en roze pootjes en opdruk "Kwispelbox.com — voor blije
  > honden"). De doos puilt uit met: PAWSOME natural treats-zakje, roze GOOD DOG TREATS-zakje, bruin
  > teddybeertje, roze donut-knuffel, vilten verjaardagstaartje met kaarsje, tennisbal met pootje,
  > roze/wit/groen touwspeeltje en een kaartje "Voor een heel bijzondere hond!". **Verticale/vierkante
  > compositie**: hond en doos dicht bij elkaar en gecentreerd zodat het goed leest op een smal
  > telefoonscherm (390px). Onderwerp **volledig vrijstaand op transparante achtergrond** (geen omgeving),
  > zachte natuurlijke slagschaduw, fotorealistisch, vrolijk. Zelfde stijl en dezelfde hond/box als de
  > desktop-hero.

> Liever een volledige fotoscène (wazige woonkamer erachter)? Lever dan als **JPG** in dezelfde
> maten; ik pas de styling dan aan. Transparante PNG heeft de voorkeur.

---

## 2. "Meest gekozen" — productfoto's (op het product zelf)

Elk: **PNG met transparante achtergrond**, **1600×1600 px** (1:1). Open kraft-Kwispelbox van
voren, gevuld met thema-inhoud, **vrijstaand** zodat de gekleurde kaart-achtergrond doorschijnt.
Vierkant werkt ook op mobiel.

### `product-verjaardagsbox.png` — verjaardag (roze)
> [merk-stijl] Open Kwispelbox gevuld met verjaardagsthema: roze donut-knuffel, "Happy Birthday"
> snackzakje, een vilten verjaardagstaart-speeltje, slingers, pastel/roze tinten. Vrijstaand op transparant.

### `product-puppy-welkomstbox.png` — nieuwe puppy (groen)
> [merk-stijl] Open Kwispelbox gevuld met puppythema: meerdere kleine pluche puppy's, "Puppy Love"
> en "Good Dog" zakjes, zachte groene tinten. Vrijstaand op transparant.

### `product-kerstbox.png` — kerst (rood/groen)
> [merk-stijl] Open Kwispelbox gevuld met kerstthema: rendier-knuffel, "Merry Woofmas" items,
> zuurstok-touwspeeltje, rood en groen. Vrijstaand op transparant.

### `product-halloweenbox.png` — halloween (oranje/paars)
> [merk-stijl] Open Kwispelbox gevuld met halloweenthema: pompoen-knuffel, spookje-knuffel,
> "Trick or Treat" zakje, oranje/zwart/paars. Vrijstaand op transparant.

---

## 3. Verjaardagsblok — polaroid

### `verjaardag-hond-polaroid.jpg`
- **Formaat:** JPG, **1000×1000 px** (1:1) — wordt in een wit polaroid-kader getoond
- **Prompt:**
  > Schattige hond met een roze gestippeld feesthoedje, kijkt in de camera, warme onscherpe
  > achtergrond (bokeh), zacht licht. Iets kopruimte bovenaan. Vrolijk en lief.
- Mobiel: vierkant werkt zoals het is.

---

## 4. "Blije honden, blije baasjes" — fotoslider (UGC-stijl)

Elk: **JPG, 1000×1000 px (1:1)**. Echt aandoende klantfoto's van blije honden met hun Kwispelbox
of speeltjes; verschillende rassen en situaties; warm en authentiek. Vierkant werkt op mobiel.

- `blije-hond-1.jpg` — hond met feesthoedje
- `blije-hond-2.jpg` — puppy knuffelend met een pluche speeltje
- `blije-hond-3.jpg` — hond met roze donut-speeltje
- `blije-hond-4.jpg` — hond snuffelt in een open Kwispelbox
- `blije-hond-5.jpg` — hond met pompoen-speeltje (halloween)
- `blije-hond-6.jpg` — twee honden samen met speeltjes

> Prompt-basis per foto: "Realistische, warme klantfoto van [scène hierboven], natuurlijk licht,
> huiselijke setting, vrolijke sfeer."

---

## 5. Footer (optioneel)

### `footer-hond.png`
- **Formaat:** PNG met transparante achtergrond, **800×800 px**
- **Prompt:**
  > Kop en borst van een schattige hond die omhoog gluurt alsof hij over de rand van de
  > nieuwsbriefkaart kijkt. Vrijstaand op transparant.
- Alleen nodig als je het "glurende hond"-detail wilt; anders overslaan.

---

## Logo — hergebruiken (geen nieuw beeld nodig)
Het huidige gekleurde logo (groene cadeaudoos + pootje + "Kwispelbox / voor blije honden") werkt op
zowel de lichte header als de bruine footer. Alleen als je een **witte tekstvariant** voor de footer
wilt: `logo-kwispelbox-wit.png`, transparante PNG, **720×240 px**.

## Geen beeld nodig
- **Aankondigingsbalk, "Voor elk moment", USP-balk**: gebruiken inline SVG-iconen (ik voeg toe:
  taart, poot, huis, pleister, kerstboom, cadeau, klok, bel). Geen uploads.
- **Decoratie** (pootjes, hartjes, sterretjes, botjes, pootjespatroon): inline SVG in code. Geen uploads.

---

## Samenvatting bestandslijst
| Bestand | Formaat | Maat | Waar |
|---|---|---|---|
| hero-home-desktop.png | PNG transp. | 1760×1600 | Files |
| hero-home-mobile.png | PNG transp. | 1200×1200 | Files |
| product-verjaardagsbox.png | PNG transp. | 1600×1600 | Product |
| product-puppy-welkomstbox.png | PNG transp. | 1600×1600 | Product |
| product-kerstbox.png | PNG transp. | 1600×1600 | Product |
| product-halloweenbox.png | PNG transp. | 1600×1600 | Product |
| verjaardag-hond-polaroid.jpg | JPG | 1000×1000 | Files |
| blije-hond-1…6.jpg | JPG | 1000×1000 | Files |
| footer-hond.png (optioneel) | PNG transp. | 800×800 | Files |
| logo-kwispelbox-wit.png (optioneel) | PNG transp. | 720×240 | Files |
