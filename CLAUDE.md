# Mind Your Feed – afspraken en werkwijze

Deze repository is het logboek en de afsprakenlijst voor het onderhoud van
www.mindyourfeed.nl (WordPress). Lees dit bestand bij elke sessie en houd je aan
de afspraken hieronder. Communiceer met de eigenaar in het Nederlands.

## Afspraken

Deze afspraken gelden voor elke pagina en elke blogpost die we aanmaken of wijzigen.

1. **Metadata compleet.** Elke pagina en post heeft een Yoast-meta-title
   (`_yoast_wpseo_title`, max. ~60 tekens), een meta-omschrijving
   (`_yoast_wpseo_metadesc`, ~120–155 tekens), een focus-keyword
   (`_yoast_wpseo_focuskw`), een handmatige samenvatting (excerpt) en een
   uitgelichte afbeelding.
2. **Correcte alt-tekst.** Elke afbeelding heeft een alt-tekst die beschrijft
   wat er te zien is, in het Nederlands, zonder "afbeelding van" en zonder
   keyword-stuffing. Puur decoratieve afbeeldingen krijgen `alt=""`. Pas de
   alt-tekst aan in de HTML van de post én in de mediabibliotheek.
3. **Laadsnelheid meten.** Meet bij elke pagina die we wijzigen de laadsnelheid
   vóór en ná de wijziging (Lighthouse, mobiel profiel). Noteer de score en de
   LCP in `WIJZIGINGEN.md`. Een wijziging mag de pagina niet merkbaar trager
   maken. Vanuit de cloudomgeving zijn de metingen een indicatie; gebruik
   PageSpeed Insights als controle.
4. **Altijd mobielvriendelijk.** Ontwerp en controleer eerst voor mobiel.
   Geen horizontale scroll, tekst minimaal 16px, tikdoelen minimaal 44×44px,
   afbeeldingen schalen mee.
5. **Mobiele breakpoints.** Controleer elke wijziging op de breakpoints van
   Elementor: mobiel (≤ 767px), tablet (768–1024px) en desktop (≥ 1025px).
   Zet bij Elementor-elementen de responsive-instellingen per breakpoint
   (lettergrootte, marges, kolommen, verbergen/tonen) en test op 375px en 768px.
6. **Alle wijzigingen bijhouden.** Elke wijziging aan de site komt in
   `WIJZIGINGEN.md`: datum, pagina/post (titel + ID), wat er veranderde, waarom,
   en de meetwaarden. WordPress-revisies zijn de terugvaloptie.
7. **Todo-lijst bijhouden.** Openstaande punten staan in `TODO.md`. Werk de lijst
   bij aan het einde van elke sessie: afgerond → afvinken, nieuw → toevoegen.
8. **Afspraken bijhouden.** Nieuwe of gewijzigde afspraken komen in dit bestand,
   met datum onder "Geschiedenis van afspraken".
9. **Menselijke namen en logische indeling.** Elementor-elementen, secties,
   containers, CSS-classes, sjablonen, menu's en mediabestanden krijgen
   begrijpelijke Nederlandse namen die zeggen wat ze zijn (bijv. "Hero – Werk met
   mij", "Sectie – Recepten uitgelicht", class `knop-primair`), geen standaardnamen
   als "Container" of "Section 3". De volgorde in de navigator volgt de volgorde
   op de pagina, en elementen worden logisch gegroepeerd per sectie.

## Werkwijze

- **Eerst vragen, dan publiceren.** Wijzigingen aan live pagina's of posts laat
  ik eerst zien, of ik maak eerst een concept. Bij wijzigingen in bulk toon ik
  vooraf een voorbeeld en het aantal posts dat het raakt.
- **Kleine stappen.** Liever per pagina of in kleine batches, zodat terugdraaien
  eenvoudig blijft.
- **Zelf committen bij voortgang.** Commit en push `WIJZIGINGEN.md`, `TODO.md` en
  `CLAUDE.md` zelf zodra er voortgang is (een afgeronde taak, een wijziging aan de
  site, een nieuwe afspraak), zonder dat eerst te vragen. Werk op de branch die de
  sessie aangeeft.
- **Gezondheidsclaims.** Inhoud over voeding en gezondheid moet kloppen; twijfelachtige
  claims markeren en niet zelf "gezonder" maken zonder bron.

## De site in het kort

- WordPress met de Classic Editor; het meeste materiaal (±27 van 35 pagina's en
  vrijwel alle ±2160 blogposts) is klassiek. Een paar pagina's zijn gebouwd in
  Elementor (Pro), een paar met Gutenberg-blokken.
- Thema: SmartMag. Belangrijke plugins: Yoast SEO Premium, WP Recipe Maker
  (Premium), WP Rocket, Imagify, Jetpack, Mediavine, Broken Link Checker, Brevo.
- Toegang vanuit cloudsessies: de WordPress REST API (`/wp-json/wp/v2/...`,
  `context=edit`) en de MCP-server `mind-your-feed-elementor` voor Elementor-pagina's.
  De login wordt door de netwerkproxy toegevoegd; zet nooit een wachtwoord in
  bestanden in deze repository.

## Geschiedenis van afspraken

- 2026-11-25: afspraken 1–9 en de werkwijze vastgelegd.
- 2026-11-25: zelf committen en pushen bij voortgang.
