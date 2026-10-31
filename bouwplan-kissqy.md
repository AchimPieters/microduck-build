# Bouwplan: kissqy Microduck (HD-1910)

Besluit van 2026-10-31: we bouwen de **[kissqy/Microduck-kissqy](https://github.com/kissqy/Microduck-kissqy)**. Achtergrond en vergelijking staan in `onderzoek-duck-keuze.md`.

## Stap 1: printen (Ender 3 V2, kan nu al)

Download de set: [FrankenDuck-set (RAR, 14,5 MB)](https://github.com/kissqy/Microduck-kissqy/raw/refs/heads/main/hardware/3d-print-frankenduck/Microduck-Print-FrankenDuck-20261015.rar). Bijgeleverd: 36 STL-bestanden en `FrankenDuck-plate-layout.3mf` (Bambu Studio-project). Je print de STL-bestanden zelf met Cura of PrusaSlicer. Alle onderdelen passen op je bed van 220 × 220 mm (grootste: 92 × 123 mm).

Instellingen in het projectbestand van de auteur: 0,2 mm laagdikte, 15% vulling, 2 wanden, nozzle 0,4 mm. Het README zegt dat je dit op je eigen materiaal en printer moet afstemmen.

Kleuren in het project: oranje `#FF9016`, zwart `#000000`, wit `#FFFFFF`, paars `#7248BD`. De kleurkeuze is vrij. Wil je dichter bij Pollen zitten, kies dan wit voor de romp en een accentkleur voor snavel en voeten.

### Printlijst per plaat (uit het 3MF-project)

| Plaat | Kleur in project | Onderdelen |
|---|---|---|
| 1 | Oranje | linker- en rechterenkel (ankle arc), linker- en rechtervoet (`foot_left/right`), snavel (`jaw`), onderste kopschaal achter, één klein blind-gat-onderdeel |
| 2 | Paars | linker- en rechterromp (shell), linker- en rechterbovenbeen, bovenste kopschaal met ventilatie |
| 3 | Wit | gezichtsdeel voor IMX219-camera, twee heupdelen, twee buitenbenen, twee nekdelen (`neck_pitch`, `neck`), IMU-clip (`power_support`) |
| 4 | Zwart | `motor_support`, `trunk_base`, twee `yaw2roll`, één `yaw_roll_motion` |
| 5 | TPU | twee zolen (`sole_left/right`) |

**Optioneel (roller-skates), kun je overslaan:**
- Plaat 1: 2× `roller_blade_clean_mount` (rolsteunen).
- Plaat 4: 2× `bearing_roll`.
- Plaat 5: 4× `tire_623_TPU` (banden).

**Niet op de platen, apart kiezen en printen:** één accuhouder uit `stl-models/body-parts/`:
- `battery-holder_M2.5-screws_HUB-combo.stl` (klemt ook de HUB-print vast), of
- `battery-holder_thick-clips.stl` (officiële stijl met dikkere klikken).

De HUB-variant lijkt me logisch zodra je de HUB koopt, maar de gids laat dat aan jou.

**TPU-zolen:** de losse STL's (`left-sole-TPU.stl`, `right-sole-TPU.stl`) staan in meters. Schaal ×1000, dan zijn ze 41 × 54 × 13 mm. De Ender 3 V2 heeft een Bowden-extruder, dus print TPU langzaam.

Ik heb niet kunnen vaststellen hoeveel filament of printtijd de set kost. De documentatie noemt dat niet. Begin met één kleine plaat om te zien hoe de pasvorm is. Het README raadt ook aan om de assemblage-aantallen niet uit de bestandslijst af te leiden ("file counts are not assembly quantities").

## Stap 2: bestellen

**Via een doorstuurservice uit China (Taobao):** zie het boodschappenlijstje in sectie 6c van `onderzoek-duck-keuze.md` en de [kissqy-lijst](https://github.com/kissqy/Microduck-kissqy/blob/main/DUCK-SHOPPING-LIST.en.md). Onderdelen zonder accu en gamepad: ¥2694,30 ≈ €337.

**In Nederland kopen:**
- 2× NP-F550-accu (2000–2200 mAh) en een dubbele lader. 123accu heeft er twee voor ongeveer €32,50.
- Gamepad: controleer eerst welke de robot ondersteunt.
- TPU, 95A, rol van 250 g (≈ €15–20).

**Eerst uitzoeken voor je bestelt:**
1. De kosten voor de HUB en accuaansluiting bij de verkoper navragen (via het WeChat-QR in de repo of een issue). Nodig voor de 1000 µF buffercondensator.
2. Of de verkoper van de HAT en IMU naar een doorstuurservice verstuurt.
3. Een echte offerte van een doorstuurservice voor ±1,2 kg naar Nederland.

## Stap 3: bouwen, flashen, kalibreren

Volg de [online bouwgids](https://kissqy.github.io/Microduck-kissqy/build/start.html): budget, montage en wiring, dan de eerste stappen (image R1 flashen, Wi-Fi instellen, console R17 V167, servo's en IMU kalibreren, gamepad koppelen, eerste loopproef). Reken op afstelwerk voor voeding, IMU en servo-demping.

## HUB en accuaansluiting (uitgezocht op 2026-10-31)

Bronnen: [assembly-components.js](https://github.com/kissqy/Microduck-kissqy/blob/main/docs/assembly-components.js), [assembly-hub-layout.json](https://github.com/kissqy/Microduck-kissqy/blob/main/docs/assets/assembly-hub-layout.json), [ASSEMBLY-NOTES](https://github.com/kissqy/Microduck-kissqy/blob/main/docs/ASSEMBLY-NOTES.en.md) en de issues [#3](https://github.com/kissqy/Microduck-kissqy/issues/3), [#4](https://github.com/kissqy/Microduck-kissqy/issues/4) en [#7](https://github.com/kissqy/Microduck-kissqy/issues/7).

### Wat de HUB is

Een klein eigen printje van de auteur (`DXL_HUB_V2`, ontwerp van 2026-10-30), 23 × 34 × 1,6 mm, met vier montagegaten (Ø2,4 mm, hart-op-hart ≈ 19 × 30 mm).

| Onderdeel | Wat |
|---|---|
| C1 | Condensator 1000 µF / 16 V (op de achterkant gemonteerd), over voeding en massa |
| J1–J4 | Vier haakse 3-pins AMP2.0-aansluitingen (type A2007WR-3P). Pin 1 = servobus, 2 = voeding (VIN), 3 = massa. Alle vier hebben dezelfde drie netten. |
| CN1 | Accu-ingang: pin 1 = GND, pin 2 = VIN |

Het verdeelt dus accuvoeding en de servobus over meerdere kabels, en dempt stroompieken met de condensator. Zonder die condensator kan de Radxa herstarten als de servo's veel stroom trekken (zie `ASSEMBLY-NOTES`). Dat de HUB er al een heeft, staat in de lijst: koop er geen aparte.

### Waar je hem koopt: dat is niet vastgelegd

- De repo zegt zelf: *"No standalone purchase link for the HUB in the repository; first confirm the actual model with the author."* (letterlijk uit de bron vertaald).
- Het ontwerpbestand van de HUB (`.epro2`) staat **niet** in de repo. Alleen HAT en IMU staan er. Je kunt de HUB dus niet zelf bij een printfabriek laten maken.
- De auteur antwoordt in issue #7 (2026-10-29): *koop direct in mijn Taobao-winkel, het hoeft niet via WeChat*. In #3 en #4 zegt hij dat HAT en IMU naar een doorstuurmagazijn kunnen en dat de firmware dan geflasht is. Of de HUB in dezelfde winkel zit, is niet bevestigd.

### De accuaansluiting

- Elektrisch is het eenvoudig: twee draden (GND en VIN) van de accucontacten naar CN1 op de HUB.
- **Welk onderdeel de contacten in de accuhouder levert, staat nergens.** De lijst noemt "accucontacten" zonder model. Dit moet je bij de auteur navragen.
- NP-F-plaatadapters met draad bestaan in de handel (bijvoorbeeld SmallRig 752, met DC-kabel van 5,5 × 2,5 mm). Of zo'n plaat in de geprinte houder past, kon ik niet vaststellen.

### Mogelijke eigen oplossing (mijn inschatting, niet bevestigd)

Alle vier HUB-aansluitingen hebben dezelfde drie netten. Het is dus waarschijnlijk een passieve verdeler plus condensator. Je zou hem op een stukje printplaat kunnen bouwen met vier 3-pins AMP2.0-connectoren, een condensator van 1000 µF / 16 V en een 2-pins connector. Controleer wel de pinvolgorde, want hun notities waarschuwen dat de aansluitingen niet zomaar verwisseld mogen worden en dat je doorgang moet controleren.

### Tweede bron: fanhao375 heeft eigen HUB- en accucontact-ontwerpen

Bron: [hardware/新版/README.md](https://github.com/fanhao375/microduck-replica/blob/master/hardware/新版/README.md), status van 2026-10-20. Het zijn EasyEDA Pro-projecten (`.eprj2`), nog **zonder Gerber, BOM of bestelinfo** en **nog niet gemaakt of getest**.

| Ontwerp | Wat | Status |
|---|---|---|
| `wire_hub_1V0A` | Passieve HUB zonder actieve onderdelen: zes connectoren (2× 1,2 mm, 2× A2007WR-3P haaks, 2× A2007WV-3P recht) en een schroefklem voor de accu. Alle op de netten Battery, DXL_BUS, GND. **Geen condensator.** | Niet gemaakt. De schroefklem CN1 is 8,4 mm hoog en botst met de constructie. Alternatieven: een WAGO-klem van 4,5 mm of direct solderen. |
| `POGO_PIN_1V0A` | Accucontactbord: 39,19 × 6,79 mm met twee banaanbussen (Ø1,7 mm, hart-op-hart 33,8 mm) en twee soldeerpads. Naam zegt "pogo pin", maar er zitten geen veerpinnen op. | Niet gemaakt. De vorm is afgeleid van een referentiebord `banana_pcb`. |

**Verband met Pollen:** het officiële Pollen-robotmodel bevat een onderdeel `banana_pcb_locker` (STL staat in je lokale kloon: `microduck_rl_src/repo/src/mjlab_microduck/robot/microduck/assets/banana_pcb_locker.stl`). Pollens accucontact lijkt dus ook een klein printje met banaanbussen te zijn, vastgehouden door een geprinte "locker". Het printje zelf is niet openbaar. Kissqy's contacten zijn waarschijnlijk dezelfde aanpak, maar dat is niet bevestigd.

**Aanbeveling:** vraag eerst kissqy (bericht hieronder). Zijn HUB is de enige met condensator, van een auteur die zijn borden getest heeft. Fanhao375's ontwerpen zijn een noodoplossing: je moet ze zelf in EasyEDA openen, laten maken en een condensator toevoegen, zonder dat iemand ze getest heeft.

### Bericht aan de verkoper (Chinees, met Engelse vertaling)

> 你好！我在制作 kissqy 的 FrankenDuck Microduck（飞特 HD1910 版）。请问：
> 1. 你的淘宝店是否出售 HUB（DXL_HUB_V2，带 1000µF 电容）？价格是多少？
> 2. 电池仓里 NP-F550 的电池触点／接线配件是否出售？具体型号是什么？
> 3. HAT、IMU 和 HUB 能否一起发到转运仓？
> 谢谢！

> *Hello! I'm building your FrankenDuck Microduck (Feetech HD1910 version). Questions: 1) Does your Taobao shop sell the HUB (DXL_HUB_V2, with the 1000 µF capacitor)? Price? 2) Do you sell the NP-F550 battery contacts / wiring parts for the battery holder? Which model? 3) Can the HAT, IMU and HUB ship together to a forwarding warehouse?*

Een doorstuurservice kan zo'n bericht voor je naar de verkoper sturen.

## Gamepad (uitgezocht op 2026-10-31)

**Wat de auteur gebruikt:** "盖世小鸡启明星2" (GameSir, "Nova 2"), ¥89,91, ingesteld op Bluetooth / Xbox-modus. Het exacte model in het Westen is niet bevestigd. Mogelijk de Nova 2 Lite.

**Knoppen van kissqy's console (V167-standaard):**

| Knop | Functie |
|---|---|
| Start | Eerste keer: HOME. Daarna: loopmodel aan of uit |
| Select | Kort: spanning van de servo's halen. Lang (2 s): zitten, ontspannen en Zero uitzetten |
| Linker stick / rechter stick | Lopen en zijwaarts / draaien |
| Y / B | Kop- of lichaamshouding |
| A / X | Voorwerp oppakken (bukken) / voorwaartse rol |
| LB / RB | Linker- / rechtervoet schoppen |
| LT / RT | Lang / kort kwaken (de snavel gaat open) |
| D-pad omlaag | Zitten en weer opstaan |
| D-pad omhoog, 3 s | Wisselen tussen lopen en roller-modus (alleen met skates) |

Je hebt dus een pad met **A/B/X/Y, twee sticks, D-pad, LB/RB en LT/RT** nodig.

**Wat Pollen ondersteunt** (bron: `microduck_src/repo/docs/robot/pair-a-gamepad.md`): Xbox Wireless Controller, DualSense en Switch-achtige pads die als "Pro Controller" verschijnen. Koppelen gaat met `sudo robotctl pad pair`.

**Kissqy's fork:** de console noemt alleen de GameSir. Of een Xbox- of DualSense-pad werkt, heb ik niet bevestigd, maar de fork is op Pollens software gebouwd. Bij fanhao375 kun je knoppen opnieuw toewijzen, bij kissqy staat "eigen indeling per installatie".

**Let op:**
- Het pad gebruikt Bluetooth (2,4 GHz) en stoort de Wi-Fi van de Radxa. Zet de Radxa op **5 GHz Wi-Fi**. Je router moet dus 5 GHz hebben. Controleer de band met `nmcli -f IN-USE,SSID,FREQ,SIGNAL device wifi list`.
- GameSir-pads kunnen een Nintendo-indeling gebruiken (A/B en X/Y omgewisseld). Dan zijn de acties verkeerd om, tenzij je de knoppen aanpast. Dat is een mogelijkheid, niet bevestigd voor dit model.

**Prijzen in Nederland (indicatie, niet bevestigd):**
- [GameSir Nova 2 Lite](https://www.proshop.nl/Gaming-accessoires/GameSir-Nova2Lite-Wireless-Controller-White-Controller/3422550): Proshop.nl heeft hem, prijs onbekend. In Europa zag ik ≈ €32–34.
- Xbox Wireless Controller: de zoekresultaten tonen uiteenlopende bedragen (€95–118), wat me onbetrouwbaar lijkt. Kijk de actuele prijs na op bol.com.

**Situatie (2026-10-31):** je hebt geen Xbox- of PlayStation-pad, en je router heeft 5 GHz. Je moet dus een pad kopen.

**Nieuwe bevindingen:**
- "启明星2" is een gamepad van ongeveer ¥99,9 die in maart 2026 uitkwam, met Xbox-indeling en halltriggers ([IT Home](https://www.ithome.com/0/880/890.htm)). Dat past bij de GameSir **Nova 2 Lite**, maar de naamkoppeling is mijn gevolgtrekking en niet bevestigd.
- De [handleiding van de Nova 2 Lite](https://gamesir.com/support/manuals/gamesir-nova-lite2) zegt dat hij standaard de modus zelf detecteert. Met Menu + View 2 seconden indrukken wissel je tussen **XBOX- en Switch-modus**. Dat is dezelfde "Xbox-modus" als in kissqy's lijst. De koppelcombinaties verschillen per taalversie van de handleiding (bijvoorbeeld Home + A of Home + Y), dus volg de handleiding die bij het pad zit.
- Prijzen: Nova 2 Lite ≈ €32–34 (Europese listings, niet Nederlands bevestigd). Xbox Wireless Controller ≈ €53–70 ([Klarna](https://www.klarna.com/nl/shopping/pl/cl165/3200090626/Gamecontrollers/Microsoft-Xbox-Wireless-Controller-Pulse-Red/): vanaf €57; Microsoft Store NL: €64,99–69,99).

**Advies:** koop de **GameSir Nova 2 Lite**. Het is waarschijnlijk wat de auteur gebruikt en kost ongeveer €20–35 minder. Zet hem in **Xbox-modus** voordat je hem koppelt. Als koppelen niet lukt, is een officiële Xbox-controller de zekere terugval, want die staat expliciet in Pollens documentatie. Controleer bij aankoop dat je hem kunt retourneren.

## Doorstuurservices vergeleken (2026-10-31)

**Eerlijk voorbehoud:** ik vond geen neutrale vergelijking. Bijna alle bronnen zijn blogs van de services zelf of hun affiliates, zoals [Fishgoo](https://blog.fishgoo.com/best-taobao-agent/) (zet zichzelf op 1 en zegt "yes, there's bias here") en [aigcmkt.com](https://www.aigcmkt.com/en/best-taobao-agent.html) (geschreven vanuit het perspectief van cfans). De cijfers spreken elkaar ook tegen. Gebruik ze als richting en controleer alles op de officiële pagina van de service.

| Service | Servicekosten (volgens bronnen, tegenstrijdig) | Gratis opslag | QC-foto's | Opmerkingen |
|---|---|---|---|---|
| Superbuy | 0% op Taobao (nieuwste bronnen) of ≈ 5% (oudere) | 90 dagen | 3, uitgebreide foto's betaald | Grootste naam. Verzendkosten worden vooraf geschat en later verrekend. Betaling: Wise, kaarten, Alipay, WeChat Pay. Eén forumbericht (jan 2025): bestelling na 37 dagen ontvangen, trage communicatie. |
| CSSBuy | ≈ 3–6% | 90 dagen | 3–5 | Staat bekend om verzendprijzen. Verouderde website, geen 24/7-ondersteuning, mogelijk koersopslag. |
| Sugargoo | 0% of ≈ 5% | 100 dagen (30 voor doorsturen van eigen pakketten) | 5 | Optionele betaalde "Star Agents". |
| Basetao | ≈ 5% | 90–120 dagen | 5 | Ouder platform, 80+ routes. |
| Fishgoo | 0% (eigen claim) | 100 dagen | 5 | Nieuw merk, weinig reviews. Heeft een Nederland-pagina met officiële bronnen voor btw en recht. De eigen verzendcijfers komen alleen van henzelf. |
| Hoobuy | niet vast te stellen | niet vast te stellen | niet vast te stellen | Alleen een kandidaat. |

**Wat geen enkele bron gaf:** echte verzendprijzen naar Nederland per kilo voor deze service (ze staan achter een calculator met login), en welke regels voor elektronica of magneten gelden.

### Waar je op moet letten (belangrijker dan de servicekosten)

1. **Wie betaalt btw en invoerrecht, en wanneer?** Als de service dat bij het afrekenen int (IOSS/DDP), heb je geen verrassingen. Zo niet, dan rekent PostNL **€10 afhandeling** (tot €150, daarboven €16) plus btw en het recht van €3 per categorie bij levering. Dat staat in de [Fishgoo-pagina](https://blog.fishgoo.com/taobao-shipping-netherlands/) met verwijzing naar PostNL en de Raad van de EU. Vraag of de Nederlandse lijn DDP is.
2. **Opslag en samenvoegen.** Je koopt bij veel winkels (kissqy voor HAT/IMU/HUB, andere voor servo's, Radxa, camera, ToF). Alles komt in het magazijn en gaat in één pakket. Gratis opslag van minimaal 90 dagen is nodig, vooral als de HUB of servo's later leverbaar zijn.
3. **Elektronica en magneten.** De HD-1910 heeft een magnetische encoder (volgens de specs van de repo). Of dat als "magnetisch" geldt bij sommige luchtlijnen, weet ik niet. Vraag het vóór je betaalt. Accu's koop je in Nederland, dus lithium komt niet in het pakket.
4. **Betalen.** Controleer of je kunt betalen met je eigen kaart of Wise. iDEAL staat in geen van de bronnen.
5. **Echte offerte.** Maak een account en laat de calculator het pakket schatten (≈ 1,2 kg, Nederland). Doe dat bij 2 of 3 services en vergelijk het totaal inclusief servicekosten en verzending. Ik kan geen accounts aanmaken.

### Mijn voorstel

Begin met een proefberekening bij **Superbuy** (grootste, vermoedelijk 0% servicekosten op Taobao, 90 dagen opslag) en **CSSBuy** (verzendprijzen), en eventueel **Fishgoo** als derde. Kies op het goedkoopste totaal met een DDP-lijn en geen beperkingen voor je onderdelen. Dat is een inschatting uit de bronnen, geen test.

## Open punten

- HUB: bevestiging dat de auteur hem verkoopt, plus prijs (zie bericht hierboven).
- Accucontacten: welk onderdeel, of een eigen oplossing.
- Doorstuurservice: offerte en regels voor deze producten.
- Gamepad: kies er een (zie sectie hierboven). Router met 5 GHz nodig.
- Welke accuhouder: HUB-variant of dikkere klikken.
- Microfoon, kop-IMU en NFC: niet bevestigd bij kissqy (alleen de microfoon op de HAT is gezien).
- Loopmodellen: geen onafhankelijke testrapporten van nabouwers.
