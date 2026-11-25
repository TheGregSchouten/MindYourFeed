# Todo

## Nu
- [ ] Eigenaar: advertenties (Mediavine) controleren op telefoon, vóór en na eerste scroll; en snelheid laten meten met PageSpeed Insights of Rocket Insights (echte winst van stap 2 zichtbaar met advertenties).
- [ ] Later: alternatief voor RUCSS onderzoeken (bijv. 'CSS asynchroon laden' of RUCSS met uitzonderingen voor de inline Elementor-CSS), eerst testen buiten de live site.

## Metadata en alt-teksten (afspraken 1 en 2)
- [ ] Nulmeting: per pagina en post nagaan of meta-title, meta-omschrijving, focus-keyword, samenvatting en uitgelichte afbeelding aanwezig zijn.
- [ ] Nulmeting: afbeeldingen zonder (goede) alt-tekst in posts en mediabibliotheek in kaart brengen.
- [ ] Ontbrekende metadata aanvullen, te beginnen bij de pagina's en de best bezochte posts.

## Snelheid en mobiel (afspraken 3, 4 en 5)
- [ ] Footer 'Instagram Feed' toont 'Error validating application' op elke pagina (Instagram-koppeling verlopen/ongeldig); koppeling vernieuwen of blok verwijderen.
- [ ] `robots.txt` reageert traag (2–4 s, niet gecachet); nagaan of WP Rocket/nginx hem kan cachen.
- [ ] Nulmeting (Home en 1 post gedaan, zie `METINGEN.md`) laadsnelheid (Lighthouse mobiel) van alle 35 pagina's en een steekproef van posts.
- [ ] WP Rocket stap voor stap aanzetten (door eigenaar in WP-admin; API kan alleen lezen), na elke stap meten en controleren of advertenties en receptkaarten werken: 1) JS defer (klaar, met jQuery-uitzondering), 2) JS-uitvoering uitstellen (klaar, 13 uitzonderingen), 3) ongebruikte CSS verwijderen (geprobeerd, brak de homepage, weer uit; zie alternatief), 4) CSS-minify (klaar) / JS-minify, 5) afbeeldingsafmetingen toevoegen. Zie `METINGEN.md`.
- [ ] Snelheid van Home bevestigen met PageSpeed Insights (API-sleutel nodig of handmatig via pagespeed.web.dev).
- [ ] Home: eerste slide sneller tonen (slideshow wordt pas na JavaScript geladen; bijv. voorladen van de eerste foto).
- [ ] WP Rocket-instellingen nalopen: CSS optimaliseren / ongebruikte CSS verwijderen, JavaScript uitstellen (render-blokkering ± 2,8 s).
- [ ] Imagify: WebP-weergave aanzetten en controleren dat WebP ook echt geserveerd wordt.
- [ ] Speedtest herhalen met PageSpeed Insights (inclusief advertenties en echte gebruikersdata).
- [ ] Mobiele check op 375px en 768px van alle pagina's.
- [ ] 9 afbeeldingen in oude posts staan op een externe Amazon S3-server; nagaan of ze naar de mediabibliotheek moeten.

## Naamgeving (afspraak 9)
- [ ] Home: alle blokken heten "Container"; logische namen geven per sectie.
- [ ] Elementor-pagina's (Werk met mij, Persoonlijk Hormoon Traject, Popup Modal) nalopen op element- en classnamen en de indeling in de navigator.

## Oude blogposts (review van de 100 oudste, 2013–2014)
- [ ] Gezondheidsclaims nalopen, te beginnen met "Bewust eten: Waarom spelt zo gezond is" (onjuist: spelt is niet veilig bij tarweallergie).
- [ ] Oude socialmediaregel (Twitter, Bloglovin) vervangen in alle posts.
- [ ] `http://`-links omzetten naar `https://` (62 van de 100 oudste posts).
- [ ] Overbodige `&nbsp;` opschonen (62 van de 100 oudste posts).
- [ ] 17 posts met shortcodes controleren of die nog werken.
- [ ] Externe links naar verdwenen foodblogs controleren (kon niet vanuit de cloudomgeving).
- [ ] Recept "Curry met kip en zoete aardappel" opknappen (tekst over foto's, currypasta als lijst, porties en tijd, spelfout).
- [ ] Dezelfde review uitvoeren op alle 2160 posts.

## Afgerond
- [x] 2026-11-25: WP Rocket stap 2 (JS-uitvoering uitstellen) actief met 13 uitzonderingen; menu, foto's, slideshow, zoeken, receptkaart en nieuwsbrief gecontroleerd.
- [x] 2026-11-25: RUCSS uitgezet na kapotte homepage; alle paginatypen visueel weer gelijk aan nulmeting. Home 59.
- [x] 2026-11-25: CSS-minify aan en gecontroleerd (geen visuele schade).
- [x] 2026-11-25: Site traag na aanzetten RUCSS (16:15–16:35), daarna vanzelf hersteld (home ±1,2–1,9 s).
- [x] 2026-11-25: WP Rocket stap 1 (defer) actief, met jQuery-uitzondering; mobiel menu hersteld. Home 30 → 58.
- [x] 2026-11-25: Lange taak van 2–3 s in `lazyload.js` (thema SmartMag) onderzocht: waarschijnlijk meetartefact van Lighthouse in de cloudcontainer; echte blokkeertijd komt van scripts van derden.
- [x] 2026-11-25: Home-slideshow: 5 foto's vervangen door 1536px-WebP (3,1 MB → 0,9 MB), live en gecontroleerd op 375/768/1440px.
- [x] 2026-11-25: Nieuw applicatiewachtwoord in gebruik, oude ingetrokken.
- [x] 2026-11-25: WordPress-MCP-server en REST-toegang ingericht, login veilig opgeslagen.
- [x] 2026-11-25: Review van de 100 oudste posts.
- [x] 2026-11-25: Afspraken vastgelegd in `CLAUDE.md`.
