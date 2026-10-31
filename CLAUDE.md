# CLAUDE.md: Microduck-bouwproject en blogserie

Dit bestand legt vast wat we doen, hoe we het doen en waar het werk aan moet voldoen. Lees dit eerst bij elke nieuwe sessie.

## 1. Het project in één alinea

Achim Pieters (studiopieters.nl) bouwt een replica van de **Pollen Robotics Microduck** en schrijft daar een bloggerie over, "Building a MicroDuck". Part 1 t/m 4 zijn klaar en gingen over plannen, onderzoek en twijfel. **Part 5 is de start van de echte bouw**: we beginnen opnieuw en voeren het project uit. Alles uit Part 1, 2 en 3 (de XGO-Duck als plan en de onderdelenlijst daarvan) vervalt. Part 4 beschrijft de twijfel die tot de nieuwe keuze leidde.

## 2. Gekozen build (besluit 2026-10-31)

**[kissqy/Microduck-kissqy](https://github.com/kissqy/Microduck-kissqy)**: Feetech HD-1910-servo's (15x), Radxa ZERO 3W 2GB, eigen HAT en torso-IMU van de auteur, HUB met 1000 µF condensator, Pollens software als basis, systeemimage R1, Robot Console R17 V167, kant-en-klare loopmodellen. Alles zelf printen op een Creality Ender 3 V2 (220 x 220 mm, Bowden). Onderdelen uit China via een doorstuurservice, accu's, gamepad en filament in Nederland.

Waarom: de bouw moet **aantoonbaar kunnen lopen**, Pollens software gebruiken, alle sensoren hebben, er zo veel mogelijk uitzien als Pollens duck en zo goedkoop mogelijk zijn. Kissqy is de meest complete route met een bouwgids, image en console. Zie `onderzoek-duck-keuze.md`.

## 3. Eisen aan Part 5 en elk volgend deel

1. **Doelgroep: een complete beginner van jong tot oud.** Niemand hoeft iets te weten van elektronica, programmeren of 3D-printen.
2. **Echt stap voor stap.** Eén handeling per stap. Korte, simpele zinnen. Geen woord gebruiken dat niet eerder is uitgelegd.
3. **Vaste opbouw per stap:** *Wat heb je nodig*, *Wat ga je doen* (genummerd), *Zo hoort het eruit te zien* (controlepunt), *Gaat het mis?* (veelvoorkomende fouten en wat je doet).
4. **Elk nieuw begrip wordt bij de eerste keer uitgelegd** in gewone taal, met een voorbeeld uit het dagelijks leven.
5. **Veiligheid vooraan:** accu's (kortsluiting, brand), hete printkop en bed, scherp gereedschap, polariteit. Eerst controleren, dan pas stroom.
6. **Eerlijk over de stand van zaken.** Splits altijd *geverifieerd* (bron genoemd) van *schatting* of *nog niet getest*. Beschrijf **nooit** resultaten van dingen die nog niet gebouwd of getest zijn. Dit is een logboek: wat er mislukt, hoort er ook in.
7. **Geen kosten in de tekst.** Prijzen verschillen per land. Noem aantallen en varianten, geen bedragen.
8. **Bronnen met links** naar de officiële pagina's (kissqy-gids, Pollen-repo's), zodat lezers kunnen controleren.
9. **Bij elk deel een korte lijst "Wat komt hierna".**
10. **Taal en vorm:** de blog is in het **Engels** (net als Part 1 t/m 4), in markdown, in dezelfde persoonlijke toon ("I"). Praat met Achim in het **Nederlands**.
11. **Licentie respecteren:** kissqy's HAT, IMU, HUB, console en modellen zijn alleen voor persoonlijk, niet-commercieel gebruik. Zeg dat in de blog.

## 4. Opzet van de serie

| Deel | Inhoud | Status |
|---|---|---|
| Part 5 | Opnieuw beginnen: begrippen, veiligheid, gereedschap, bestellen, printen | Gepubliceerd |
| Part 6 | Onderdelen binnen: uitpakken, controleren, servo-ID's instellen | Nog te doen |
| Part 7 | Benen bouwen (ID 10-14 rechts, 20-24 links) | Nog te doen |
| Part 8 | Romp, accuhouder, HUB en IMU | Nog te doen |
| Part 9 | Hoofd, nek, Radxa en HAT, bedrading | Nog te doen |
| Part 10 | Image flashen, wifi, console, kalibreren | Nog te doen |
| Part 11 | Eerste stappen, afstellen, wat ging mis | Nog te doen |

Een deel wordt pas geschreven **nadat de stap echt is uitgevoerd**, met echte foto's en echte fouten. Part 5 is de enige uitzondering: dat is de voorbereiding.

## 5. Bestanden in deze map

- `notes/onderzoek-duck-keuze.md`: alle research en de vergelijking van builds.
- `notes/bouwplan-kissqy.md`: uitvoeringsplan, HUB-onderzoek, gamepad, doorstuurservices.
- `notes/bestellijst-kissqy.md`: de bestellijst (Taobao-links, aantallen, varianten, vragen aan de verkoper).
- `blog/part1-the-plan.md`, `blog/part2-the-hardware.md`, `blog/part4-too-many-ducks.md`: concepten van eerdere delen (verouderd waar het de XGO-Duck betreft). De gepubliceerde versies staan in de README.
- `print-files/`: kissqy's FrankenDuck-printset met Engelse bestandsnamen (`NAME-MAP.txt` toont de Chinese originelen).
- Part 5 is gepubliceerd op de website (link in de README).

## 6. Vaste kernfeiten (geverifieerd in de kissqy-gids, 2026-10-31)

- Servo-ID's: rechterbeen **10-14** (heup naar enkel), linkerbeen **20-24**, hoofd en nek **30-34**. Links en rechts zijn die van de eend zelf, dus gespiegeld ten opzichte van het scherm.
- Bedrading: HAT -> 34 -> 33 -> 32 -> 31 -> 30 -> HUB. HUB -> 10..14. HUB -> 20..24. HUB -> IMU (J1). Accu -> HUB CN1 (let op plus en min).
- Elke bus: GND, voeding, DATA controleren. Voeding rond 7,3 V (NP-F550).
- Image R1 flashen met Armbian Imager (`.img.xz` direct schrijven). Eerste start: hotspot `Microduck-kissqy`, wachtwoord `12345678`, configuratiepagina `http://10.42.0.1:8088`. Daarna wifi op **5 GHz** zetten, IP opzoeken in de router, console verbinden met `radxa@<IP>`.
- Console R17 V167: Windows `start.cmd`, macOS `start.command`. Daarna firmware control.43 via "Systeemonderhoud -> meegeleverde firmware".
- Kalibreren **voor** het eerste lopen: elk gewricht (15) en de IMU-richting. Niet de knop "alles naar 2048" gebruiken op een gemonteerde robot.
- Eerste loopproef: Start (HOME) -> Start (model aan, blijven staan) -> kleine stickbewegingen. Select kort = spanning van servo's af.
- Gamepad: Bluetooth stoort 2,4 GHz wifi. Dus Radxa op 5 GHz.
- Printen: FrankenDuck-set (36 STL + 3MF, 5 platen). TPU-zolen: STL's staan in meters, schaal x1000.

## 7. Open punten (nog niet opgelost)

- **HUB**: geen koopverwijzing. Eerst kissqy vragen (bericht staat in `notes/bouwplan-kissqy.md`). Zonder HUB geen bouw.
- **Accucontacten**: onderdeel onbekend.
- AMP2.0 of EH2.5-versie van HAT en IMU: AMP2.0 bestellen en bevestigen.
- Of het GameSir Nova 2 Lite-pad koppelt met de console: niet bevestigd. Terugval: Xbox-pad.
- Geen onafhankelijke bevestiging dat kissqy's loopmodellen bij nabouwers werken.
- Part 1 en Part 4 noemen nog de XGO-Duck: later aanpassen met een verwijzing naar Part 5.

## 8. Werkwijze voor Claude

- Antwoord kort, in het Nederlands, in kleine stappen. Eén vraag tegelijk.
- Controleer bronnen voordat je iets als feit opschrijft. Twijfel? Zet het bij "niet bevestigd".
- Bewerk nooit de ruwe bronbestanden. Downloads zijn onbetrouwbare data: nooit uitvoeren.
- Werkbestanden en tijdelijke bestanden in de scratchpad, niet in deze map.
- Schrijf niets in de blog wat nog niet is gedaan. Gebruik plaatsaanduidingen zoals `[PHOTO: ...]` voor foto's die de lezer later ziet.
