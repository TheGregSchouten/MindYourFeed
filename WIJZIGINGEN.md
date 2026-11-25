# Wijzigingen aan www.mindyourfeed.nl

Nieuwste bovenaan. Per wijziging: datum, pagina/post (titel + ID), wat, waarom,
en de meetwaarden voor/na (Lighthouse mobiel: score, LCP).

| Datum | Pagina / post | Wat is er gewijzigd | Waarom | Snelheid voor → na |
|---|---|---|---|---|
| 2026-11-25 | Hele site – WP Rocket | jQuery en jquery-migrate uitgezonderd van uitgesteld laden (door eigenaar). Mobiel menu werkt weer (home + post, 375/768px, navigeren via menu ok); slideshow, lazy foto's 14/14, zoeken, receptkaart ok, geen JS-fouten | Herstel mobiel menu na stap 1 | Home 60 → 58, TBT 80–90 → 140–160 ms; post 65–66 → 63–64 |
| 2026-11-25 | Hele site – WP Rocket | **JavaScript uitgesteld laden (defer)** aangezet door eigenaar. Gecontroleerd: slideshow, lazy foto's (14/14), loop-grids, zoeken, receptkaart, geen JS-fouten, geen horizontale scroll. **Probleem:** mobiel menu (hamburger) opent niet meer; oorzaak: jQuery wordt uitgesteld. Oplossing getest: jQuery + jquery-migrate uitzonderen | Stap 1 van het WP Rocket-plan | Home 30 → 60, TBT ±2.000 → 80–90 ms, LCP ±12,9 → 10,5–11,8 s; post 64 → 65–66, FCP 5,4 → 4,7 s |
| 2026-11-25 | Home (ID 27053), slideshow bovenaan (container `5587d7a`) | De 5 slideshowfoto's vervangen door de 1536px-WebP-versies van Imagify (`…-1536x1024.jpg.webp`). Opgeslagen in `_elementor_data` via de REST API, daarna één keer zonder inhoudelijke wijziging via Elementor opgeslagen en gepubliceerd om de Elementor-cache te verversen (undo-revisie 33997). Backup: `backups/2026-11-25_home-27053_elementor_data.json`. Gecontroleerd op 375/768/1440px: foto's laden, geen horizontale scroll; screenshots in `metingen/screenshots/` | Home: LCP 17,1 s door zware slideshowfoto's (3,1 MB) | Score 55 → 30; LCP 17,1 → 12,5–13,3 s; gewicht 4.660 → 2.514 KiB; TBT 180 → ±2.100 ms (zie `METINGEN.md`) |
| 2026-11-25 | – (WordPress-gebruiker corigen) | Oud applicatiewachtwoord ingetrokken, nieuw wachtwoord in gebruik via het netwerkgeheim | Oude wachtwoord stond in een chatgesprek | – |
| 2026-11-25 | – (cloudomgeving) | `www.mindyourfeed.nl` toegestaan en WordPress-login als netwerkgeheim opgeslagen | Veilige toegang tot de site vanuit cloudsessies | – |

