# Snelheidsmetingen

Lighthouse 12.6.1, mobiel profiel (gesimuleerde 4G, Moto G-scherm), vanuit de cloudomgeving.
Let op: scripts van derden (Mediavine-advertenties, Google Tag Manager, Facebook, Shareaholic,
Grow, WonderPush, Brevo) worden in de cloudomgeving geblokkeerd. De echte site is daardoor
waarschijnlijk trager dan deze cijfers. PageSpeed Insights gaf "te veel verzoeken" (geen API-sleutel).

## 2026-11-25 – nulmeting

| Pagina | Prestaties | Toegankelijkheid | Best practices | SEO | FCP | LCP | TBT | CLS | Paginagewicht |
|---|---|---|---|---|---|---|---|---|---|
| Home (`/`) | 55 | 80 | 93 | 100 | 4,8 s | 17,1 s | 180 ms | 0,001 | 4.660 KiB |
| Post "Aardappels uit de oven" (ID 33918) | 64 | 87 | 93 | 100 | 5,4 s | 5,7 s | 70 ms | 0 | 1.975 KiB |

Doelen (Google "goed"): LCP ≤ 2,5 s, CLS ≤ 0,1, TBT ≤ 200 ms.

### Bevindingen
- **Home – LCP-element is de achtergrond-slideshow (Elementor) bovenaan.** De slides zijn
  originele JPG's van 520–720 KiB per stuk (5 slides ≈ 3 MB) en worden lui geladen.
  Imagify heeft WebP-versies gemaakt (`.jpg.webp`, ongeveer de helft kleiner), maar die worden
  niet geserveerd: CSS-achtergrondafbeeldingen gaan buiten Imagify's `<picture>`-vervanging om.
- **Render-blokkerende CSS/JS: ± 2,8 s winst mogelijk.** Dit is de hoofdoorzaak van de trage
  FCP op alle pagina's (ook bij de post, waar het LCP-element gewoon tekst is).
- Ongebruikte JavaScript ± 168 KiB, ongebruikte CSS ± 156 KiB (o.a. `uael-frontend.min.css`, 109 KiB).
- Moderne afbeeldingsformaten ± 1,6 MB winst; niet-passende afbeeldingsformaten ± 223 KiB;
  afbeeldingen buiten beeld ± 249 KiB.
- Serverresponstijd 760 ms (WP Rocket-cache actief, nginx).
- Lighthouse meldt HTTP/1.1; dat komt waarschijnlijk door de proxy van de cloudomgeving, niet door de server.

## 2026-11-25 – Home na vervangen slideshowfoto's (1536px WebP)

Twee runs; de cloudcontainer was tussen de voor- en nameting opnieuw opgestart.

| Run | Prestaties | FCP | LCP | TBT | CLS | Paginagewicht | Afbeeldingen |
|---|---|---|---|---|---|---|---|
| Voor (1 run) | 55 | 4,8 s | 17,1 s | 180 ms | 0,001 | 4.660 KiB | 3.859 KiB |
| Na, run 1 | 30 | 5,1 s | 13,3 s | 2.050 ms | 0 | 2.514 KiB | 1.699 KiB |
| Na, run 2 | 30 | 4,8 s | 12,5 s | 2.110 ms | 0,003 | 2.514 KiB | 1.699 KiB |

### Duiding
- Gewicht −46%, afbeeldingen −2,1 MB, LCP ruim 4 s sneller; totale main-thread-tijd daalde van 7,1 s naar 5,6–6,4 s.
- De lagere score komt volledig door TBT. Zowel vóór als na zit er één lange taak van 1,7–2,5 s in de pagina
  (na: toegeschreven aan `lazyload.js` 10.5.2). Vóór viel die taak vóór de eerste weergave (telt niet mee voor TBT),
  nu erna (telt wel mee). Waarschijnlijk dezelfde taak die verschuift doordat de foto's eerder binnen zijn,
  geen extra werk. Niet bewezen: één voormeting, en de omgeving was herstart.
- Controle met PageSpeed Insights nodig (gaf opnieuw 429, geen API-sleutel).
- De oorzaak van die lange taak (lazyload/WP Rocket, jQuery) is een apart verbeterpunt.

## 2026-11-25 – Onderzoek naar de lange script-taak (Home)

- **Herkomst:** `lazyload.js?ver=10.5.2` is de lazysizes-bibliotheek van het thema SmartMag, niet WP Rocket.
  Het script laadt ±15 afbeeldingen in de receptblokken (`data-src`, `data-bgsrc`); uitzetten breekt die afbeeldingen.
- **A/B-test in Lighthouse (gesimuleerd):** zonder dit script TBT 1.770 → 80 ms, maar dan laden die afbeeldingen niet.
  Zonder de slideshow, JetElements of Instagram/Facebook-embed: geen verschil in TBT.
- **Lighthouse met echte vertraging (devtools):** score 26, TBT 3.150–3.730 ms, taak van 2,7–3,1 s in `lazyload.js`.
- **Gewone browser (Playwright, 4× tragere CPU, Android-UA, zelfde schermformaat), 6 runs:** langste taak 135–158 ms,
  TBT 135–295 ms. De taak van 2–3 s komt daar niet voor, ook niet met animaties uit.
- **Conclusie:** de taak van 2–3 s treedt alleen op binnen Lighthouse in deze cloudcontainer (geen GPU; de taak bestaat
  vrijwel geheel uit wachten op het tekenen, met 660 animatie-events). Waarschijnlijk een meetartefact. Bevestigen
  met PageSpeed Insights.
- **Echte gebruikersmeting (Rocket Insights, GTmetrix, recept "Kipsaté marinade", mobiel, feb. 2026):** score 49,
  TBT 3.794 ms, 7,9 MB en 936 verzoeken, volledig geladen na 19,3 s. Hier draaien de advertenties en andere scripts
  van derden wel mee: dáár zit de echte blokkeertijd.

### WP Rocket-instellingen (export, 2026-11-25)
| Instelling | Stand |
|---|---|
| Cache (ook mobiel) | aan |
| CDN | aan |
| CSS-bestanden verkleinen (minify) | uit |
| JavaScript-bestanden verkleinen (minify) | uit |
| JavaScript uitgesteld laden (defer) | uit |
| JavaScript-uitvoering uitstellen (delay) | uit |
| Ongebruikte CSS verwijderen | uit (optimize_css_delivery staat wel op 1, maar zonder methode) |
| Lazyload afbeeldingen/iframes/CSS-achtergronden | uit |
| Ontbrekende afbeeldingsafmetingen toevoegen | uit |
| Lettertypen lokaal hosten / voorladen | uit |

## 2026-11-25 – Na WP Rocket stap 1: JavaScript uitgesteld laden (defer)

Actief gecontroleerd: `defer_all_js = 1`; Home 24/33 en post 28/39 scripts met `defer`. Cache eerst opgewarmd.

| Pagina | Run | Prestaties | FCP | LCP | TBT | CLS | Speed Index |
|---|---|---|---|---|---|---|---|
| Home | voor (na slideshow) | 30–31 | 4,8–5,1 s | 12,5–13,3 s | 1.770–2.110 ms | 0 | – |
| Home | na 1 | 60 | 4,2 s | 10,5 s | 90 ms | 0 | 7,0 s |
| Home | na 2 | 60 | 4,2 s | 11,8 s | 80 ms | 0 | 6,8 s |
| Post 33918 | voor | 64 | 5,4 s | 5,7 s | 70 ms | 0 | – |
| Post 33918 | na 1 | 66 | 4,8 s | 5,7 s | 10 ms | 0 | 5,6 s |
| Post 33918 | na 2 | 65 | 4,7 s | 5,6 s | 10 ms | 0,001 | 5,8 s |

- Nog ±2,1 s (Home) en ±2,6 s (post) render-blokkerende bronnen: vooral CSS → stap 3 (ongebruikte CSS).
- SEO 92 in één run: `robots.txt` reageerde te traag (2–4 s, niet gecachet). Los van deze wijziging.

### Functionele controle (375px en 1440px)
| Onderdeel | Resultaat |
|---|---|
| Slideshow wisselt | ok |
| Lazy foto's na scrollen | 14/14 geladen |
| Loop-grids met recepten | ok (4, 4, 6, 3 items) |
| Zoekveld hero | ok (`?s=pompoen`) |
| Receptkaart (WPRM) | ok (7 ingrediënten) |
| JavaScript-fouten | geen |
| Horizontale scroll | geen |
| **Mobiel menu (hamburger)** | **kapot**: klik doet niets, geen klik-handlers gekoppeld |

### Oorzaak mobiel menu (getest met aangepaste HTML in de testbrowser)
| Variant | Menu opent |
|---|---|
| Alles uitgesteld (live) | nee |
| jQuery + jquery-migrate uitgezonderd | **ja** |
| Alleen smartmenus uitgezonderd | nee (fout: jQuery is not defined) |
| Elementor(-Pro) uitgezonderd | nee (fouten) |
| Niets uitgesteld (vóór de wijziging) | ja |

## 2026-11-25 – Na uitzondering jQuery (defer blijft aan voor de overige 22 scripts)

| Pagina | Run | Prestaties | FCP | LCP | TBT | CLS | Speed Index |
|---|---|---|---|---|---|---|---|
| Home | 1 | 58 | 4,5 s | 12,1 s | 160 ms | 0 | 7,0 s |
| Home | 2 | 58 | 4,5 s | 12,4 s | 140 ms | 0,002 | 7,1 s |
| Post 33918 | 1 | 64 | 5,0 s | 6,0 s | 90 ms | 0 | 5,8 s |
| Post 33918 | 2 | 63 | 5,1 s | 5,3 s | 50 ms | 0 | 8,8 s |

- Kosten van de uitzondering: ±2 punten en ±60 ms TBT; winst t.o.v. vóór stap 1 blijft groot (Home 30 → 58).
- Mobiel menu: opent op home en post (375 en 768px), 8 zichtbare links, navigeren naar Weekmenu werkt.
- SEO 92 in enkele runs: weer `robots.txt` (traag), los van deze wijziging.

## 2026-11-25 – Nulmeting vóór WP Rocket stap 3 (ongebruikte CSS verwijderen)

Lighthouse mobiel, 1 run per pagina (Home en post: zie meting hierboven, na jQuery-uitzondering).

| Pagina | Prestaties | FCP | LCP | TBT | CLS | Render-blokkerend | Ongebruikte CSS |
|---|---|---|---|---|---|---|---|
| Home | 58 | 4,5 s | 12,1 s | 160 ms | 0 | ±2.460 ms | ±156 KiB |
| Post 33918 | 64 | 5,0 s | 6,0 s | 90 ms | 0 | ±2.920 ms | ±188 KiB |
| Categorie Weekmenu | 65 | 4,5 s | 4,6 s | 30 ms | 0,001 | ±2.300 ms | ±179 KiB |
| Werk met mij (`/coaching/`) | 57 | 4,5 s | 7,7 s | 50 ms | 0,001 | ±2.500 ms | ±172 KiB |
| Recepten index | 58 | 4,5 s | 6,8 s | 50 ms | 0 | ±2.370 ms | ±180 KiB |

Visuele nulmeting: 35 screenshots (7 paginatypen × 375/768/1440px, volledige pagina + open mobiel menu) in de
scratchpad van de sessie (77 MB, te groot voor de repository); worden na de wijziging pixel voor pixel vergeleken.
