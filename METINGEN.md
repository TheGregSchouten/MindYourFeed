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
