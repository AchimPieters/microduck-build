# Onderzoek: welke Microduck-build kiezen?

Opgesteld op 2026-10-31. Dit bestand bundelt alles wat in deze sessie is uitgezocht, zodat het niet verloren gaat. Prijzen zijn grove schattingen en veranderen vaak. Wisselkoersen: ¥1 ≈ €0,125, $1 ≈ €0,88.

## 1. Je eisen (zoals besproken)

1. Je wilt de **software van Pollen Robotics** gebruiken.
2. Je wilt de build die er **het meest uitziet als de Pollen Microduck**, ook visueel.
3. **Alle sensoren** moeten aanwezig zijn.
4. Je wilt het **plezier van het zelf bouwen**. Op prijs kun je niet winnen van het origineel.

## 2. De meetlat: wat Pollen zelf heeft

Bronnen: [Pollen press kit](https://pollen-robotics.com/microduck/press-kit/), [SLAM-blog](https://pollen-robotics.com/microduck/blog/microduck-slam/) en de daemons in je lokale kloon `microduck_src/repo` (`tof`, `mediad`, `nfc`, `sounds`, `pet-detect`).

| Sensor of functie | Details |
|---|---|
| Camera | Voorcamera met gebruiksindicator. Resolutie nog niet definitief in de press kit. |
| ToF-diepte | VL53L5CX, 8×8 raster (64 afstanden), 45° kegel, in de kop op ongeveer 25 cm hoogte |
| IMU | Gemeld als twee IMU's (lijf en kop). Volgens de reconstructie van fanhao375 zit er op de HAT een BMI088 die door de software **niet wordt gebruikt**. De lijf-IMU is het `imu_to_dxl`-bord (LSM6DSV16X, bus-ID 200). |
| Microfoon | MEMS-microfoon op de officiële HAT |
| Speaker | Kleine luidspreker, PAM8406-versterker op de HAT |
| NFC | Genoemd in de specs en aanwezig als daemon |
| Computer | Rockchip RK3566 (Radxa Zero 3W als stand-in), 1 GB RAM, 32 GB opslag |
| Accu | Sony NP-F550 (2S, 7,4 V) |
| Servo's | 15× Dynamixel XL330 (model niet officieel genoemd in de repo) |

Pollen heeft de mechanische CAD, de volledige BOM en het `imu_to_dxl`-bord **niet gepubliceerd**. De HAT (KiCad) is wel open. Elke replica is dus een reconstructie.

## 3. Het origineel

- $399, ongeveer €350, exclusief btw en verzending. Pre-orders openden op 27 augustus 2026. De eerste levering wordt vóór kerst 2026 verwacht.
- Winkel: [store.pollen-robotics.com](https://store.pollen-robotics.com/products/microduck)
- **Nog uit te zoeken:** voorraad, levertijd en verzendkosten naar Nederland.

## 4. XGO-Duck (Luwu Dynamics): afgevallen

**Reden:** één IMU (QMI8658A), geen camera, geen ToF, en een eigen Python-runtime in plaats van Pollens software.

Wel hebben we de prijs uitgezocht (stap 1 uit Part 4):

| Product | Prijs |
|---|---:|
| [Elektronicakit](https://shop.xgorobot.com/products/xgo-duck-biped-robot-electronics-kit-arduino-uno-q?variant=68379124138235) (zonder UNO Q en zonder geprinte onderdelen) | $400 |
| [Alleen expansion board](https://shop.xgorobot.com/products/robot-driver-board) | $20 |
| [Accu 4000 mAh + lader](https://shop.xgorobot.com/products/robot-driver-board) | $50 |
| Arduino UNO Q 4GB ([RS NL](https://nl.rs-online.com/web/p/microcontroller-development-tools/0708753)) | ≈ €67 incl. btw |
| Losse Feetech HD-1910-C001 ([Openelab EU](https://openelab.com/products/feetech-hd1910-microduck-coreless-servo)) | €29,95 per stuk (uitverkocht) |

- Subtotaal kit + UNO Q ≈ **€420**. Met 21% invoer-btw ≈ **€500** plus verzending en filament.
- Niet bevestigd: of de kit accu, lader, schroeven en lagers bevat, en of de 12% lanceerkorting al in $400 zit.
- De Arduino UNO Q kreeg een [prijsstijging](https://blog.arduino.cc/2026/06/26/a-heads-up-on-the-arduino-uno-q-board-pricing-straight-from-marcello-majonchi/): 2GB van $44 naar $59, 4GB van $59 naar $79.
- De repo `xgoduck_hardware` bevat een PCB-leverancier voor het expansion board, maar dat is niet meer nodig.

## 5. Overzicht van alle gevonden builds

Bron voor de meeste: [awesome-microduck](https://github.com/joeynyc/awesome-microduck) (hardware-sectie).

| Build | Servo's / brein | Pollen-software? | Status |
|---|---|---|---|
| **[fanhao375](https://github.com/fanhao375/microduck-replica)** (1494 sterren) | 15× HD-1910, Radxa Zero 3W, Pollens eigen RPI Robot HAT | Ja, fork van `robotd` | Loopt, voorwaartse rol, image + webconsole. Licentie: persoonlijk, niet-commercieel. |
| **[kissqy](https://github.com/kissqy/Microduck-kissqy)** (398 sterren) | 15× HD-1910-C001, Radxa Zero 3W, eigen HAT + IMU | Ja, gebaseerd op officiële 0.15.1 | Loopt, bal schoppen, voorwaartse rol, image + webconsole. Persoonlijk, niet-commercieel. |
| [MHDuck/microduck](https://github.com/MHDuck/microduck) | HD-1910 + BNO085, Radxa Zero 3W | Ja, runtime-fork (Apache-2.0) | Minder afgewerkt |
| [CMU MicroDuck](https://github.com/qilinxiaoxiang/cmu-microduck) | Radxa Cubie A7Z, camera, touch-pads | Python-runtime met Pollens policies | MIT. Oververhit: 85 °C na vijf minuten, loop zakt naar ±40 Hz en de gang divergeert. |
| [AI-FanGe](https://github.com/AI-FanGe/Microduck-build-tutorial) | 14× XL330, Pi Zero 2 W | Nee, eigen Python | Loopt. Alleen BNO085-IMU. |
| [DuckBuddy](https://github.com/Animan-AI/DuckBuddy) | Feetech HL-2915, Orange Pi | Eigen, opnieuw getrainde policies | Twee microfoons, gezichtsherkenning, spraak |
| [Dukki](https://github.com/lachlanhurst/dukki) | Unitree J288, Radxa CM4 | Gedeeltelijk | "Nowhere near complete" |
| [Microduck-S288](https://github.com/zj281136/microduck-s288) | 15× S288, Pi Zero 2 W | Gedeeltelijk | Nog niet af |
| [Nhercar](https://github.com/Nhercar/adaptation-microduck-replica) | Gebaseerd op fanhao375 | Ja | Spaanstalige gids. Sensorlijst: IMX219 + VL53L5CX. |
| [BoBoRedesign](https://github.com/AiBoBoMaker/MicroDuck-BoBoRedesign) | XL330 of HD-1910, Radxa Zero 3W | Ja | Geen bewijs dat hij loopt |
| [Fanduck](https://github.com/7757/fanduck) | — | — | Afgebouwde replica met foto's en video, in het Chinees. Niet nader bekeken. |
| pablo-mano, virtualrobotix, prodou, OpenMicroDuck e.a. | — | — | Reconstructiestudies, geen afgebouwde robots |

## 6. De twee kanshebbers naast elkaar

### Sensoren

| Sensor | fanhao375 | kissqy |
|---|---|---|
| Camera | IMX219 in de BOM. Beeld nog "standaard testbron", echte camera niet bevestigd. | IMX219 160° (¥64,35) in de lijst. Webconsole toont camerabeeld. |
| ToF 8×8 | VL53L8CX of VL53L5CX in de BOM, via Qwiic/Stemma. Validatie van echte ToF niet bevestigd. | VL53L5CX-module (¥49,50) in de lijst |
| Lijf-IMU | Eigen `imu_to_dxl`-bord, bus-ID 200 | Eigen IMU-bord, voorgeflasht (FT6) te koop |
| Kop-IMU / BMI088 | Op de officiële HAT, door software niet gebruikt | Niet bevestigd |
| Microfoon | Op de officiële HAT. Lokale spraakherkenning ("zit/sta") werkt. | Niet bevestigd (HAT is vereenvoudigd) |
| Speaker | Op HAT, met versterker | Speaker staat in de lijst |
| NFC | Niet nagegaan | Niet nagegaan |

### Hardware en prijs

| Onderdeel | fanhao375 | kissqy |
|---|---|---|
| Computer | Radxa Zero 3W | Radxa Zero 3W 2GB (¥309 ≈ €39) |
| Servo's | 15× HD-1910 | 15× HD-1910-C001 à ¥119 = ¥1785 ≈ €223 |
| Borden | Pollens officiële RPI Robot HAT (open KiCad) + eigen IMU, HUB, batterijbord. Bouwen of laten maken. | Eigen HAT + IMU, kant-en-klaar te koop op [Taobao](https://item.taobao.com/item.htm?id=1095121548583) (HAT ¥119 + IMU ¥99) |
| Accu | Sony NP-F550 | NP-F550 met twee accu's en dubbele lader (¥84,59) |
| Geprint | CAD in [microduck-replica-cad](https://github.com/fanhao375/microduck-replica-cad), ook voor XL330 | FrankenDuck-set, printservice ¥199 |
| Totaal | Eerder geschat: ≈ €375–540 (servo's uit China), ≈ €640–805 (Europa) | **¥2868,80 ≈ €359** zonder printen, **¥3067,80 ≈ €383** met printservice |
| Niet inbegrepen | Verzending, invoer, gereedschap | HUB- en batterijaansluiting, verzending, gereedschap |

- Kissqy heeft het meest complete en best onderbouwde boodschappenlijstje, met links, exacte varianten en historische prijzen.
- De kissqy-prijzen gelden voor Taobao. Of Taobao naar Nederland levert of dat je een doorstuurservice nodig hebt, is **nog niet nagegaan**. Met 21% btw kom je realistisch op ruim €450 plus verzending.
- Fanhao375 ondersteunt ook **XL330**, wat het dichtst bij Pollens originele servo komt. De servo's kosten dan wel ongeveer €600 in Europa, en Pollens servo-model is een afleiding, niet officieel bevestigd.

### Looks (bekeken op 2026-10-31)

Beide modelleren door op het publieke MJCF-model en de STL-meshes. fanhao375: CAD in SolidWorks (56 delen), handleiding van 21 pagina's. Kissqy: eigen "FrankenDuck"-set.

De foto's staan naast elkaar in `fotos-look-vergelijking.jpg`. Bronnen: [Pollen-galerij](https://pollen-robotics.com/microduck/), [fanhao375 buildlog](https://github.com/fanhao375/microduck-replica/tree/master/build-log/photos), [kissqy build-robot.jpg](https://github.com/kissqy/Microduck-kissqy/blob/main/docs/assets/build-robot.jpg).

| Onderdeel | Pollen | fanhao375 | kissqy |
|---|---|---|---|
| Kop | Gebogen koepel met grijs gezichtsvlak, grote kleurige camera-ring en een smal slot ernaast | Vrijwel identiek: grijs gezichtsvlak, gele camera-ring, slot | Ronder, wit en doorschijnend, met oranje ring en slot. Zelfde idee, andere vorm. |
| Snavel | Scharnierende snavel onder de kop, afgezet in een kleur | Rode snavel die open kan, zelfde opzet | Oranje snavel, zelfde opzet |
| Romp | Dikke, afgeronde witte blok | Witte blok, vergelijkbaar | Witte blok, vergelijkbaar |
| Heupplaten | Grote, gekleurde schalen | Witte platen, vergelijkbaar | Witte platen, op andere foto's driehoekig in kleur |
| Voeten | Afgeronde trapezium-voeten in twee kleuren | Trapezium-voeten in twee kleuren (rood/geel), zelfde vorm | Lage, boot-achtige voeten met grijze zool. Gemaakt voor roller-skates, wijkt af van Pollen. |
| Nek | Twee zichtbare zwarte servo's | Twee zichtbare servo's | Twee zichtbare servo's |

**Oordeel:** fanhao375 lijkt het meest op Pollens duck, vooral in kop en voeten. Kissqy is in opzet hetzelfde, maar kop en voeten zijn herontworpen en wijken zichtbaar af.

**Voorbehoud:** dit is een visuele indruk op basis van een paar foto's, niet gemeten. De fanhao375-foto's zijn van een eerdere build (13 september), de kleuren zijn een printkeuze, en Pollens CAD is niet openbaar, dus beide zijn benaderingen.

## 6b. Loopt hij echt? (belangrijkste criterium)

Bekeken op 2026-10-31. Bronnen: [fanhao375 historie](https://github.com/fanhao375/microduck-replica/blob/master/docs/历史更新.md), [kissqy MODEL-PROVENANCE](https://github.com/kissqy/Microduck-kissqy/blob/main/microduck-hd1910/docs/MODEL-PROVENANCE.md), [kissqy FAQ](https://github.com/kissqy/Microduck-kissqy/blob/main/docs/MICRODUCK-FAQ.en.md), [kissqy README](https://github.com/kissqy/Microduck-kissqy).

### fanhao375: gedateerd testlog, inclusief wat misging

| Datum | Wat er gebeurde |
|---|---|
| 12 okt | Model sprong naar achteren. Oorzaak: IMU ±8,5° scheef gemonteerd. Na calibratie stond hij 70 s en begon hij te stappen (afstand niet gemeten). |
| 13 okt | De hele robot viel tijdens het lopen uit: voeding te zwak. Trillende gang door servo-demping (D). Met D=0 daalde hoogfrequente trilling van 51% naar 4–8%. Voeten kwamen maar half zo hoog als in simulatie. |
| 15 okt | "Na 2–3 stappen vallen": de BNO085-houding sprong 20–60° in 20 ms. Opgelost door de gyro zelf te integreren. Daarna 60 s joystick zonder val en 10+ min in loopmodus met 1 val. |
| 24 okt | Loopvideo op tafel (±6 s) en voorwaartse rol met opstaan, op een afgebouwde HD-1910-duck. |
| 27 okt | Nieuwe image: eerste gebruik op een nieuwe kaart nog niet getest. Echte camera en ToF nog niet gevalideerd. |

### kissqy

- Beweert lopen, zitten, oppakken, schoppen, voorwaartse rol en op één been staan, met zeven echte-robotvideo's.
- De tien modellen zijn zelf getraind voor de HD-1910. Hun eigen herkomstdocument zegt dat de QEMU-controles formaat-checks zijn en **niet per beweging echte hardware-validatie**. Het loopmodel is deels voortgezet vanuit "teacher"-gewichten waarvan de oorsprong niet is vastgelegd.
- De FAQ meldt voedingsreboots door servo-pieken. Oplossing: 1000 µF buffercondensator (zit in hun HUB).
- Het hardware-oordeel "lange tijd getest, geen problemen" is een eigen bewering van de auteur. De issues bevatten alleen aankoopvragen, geen testrapporten van anderen.
- fanhao375 schrijft in zijn log van 13 okt dat kissqy's eenklik-image V164 de servo-demping en het koppellimiet niet schrijft. Dat is een claim van een concurrent en niet door mij gecontroleerd.

### Wat dit zegt

- **Beide lopen aantoonbaar, maar op de eigen robots van de auteurs.** Ik vond geen onafhankelijke rapporten van nakomers die het gewoon werkend kregen. De fanhao375-clip is zes seconden op een tafel.
- **Beide kampten met voeding, IMU en servo-afstelling.** Dat zijn de typische HD-1910-struikelblokken.
- **Risico bij HD-1910: Pollens modellen zijn getraind voor XL330.** Voor Feetech zijn de loopmodellen opnieuw getraind door de auteurs. De Nhercar-gids zegt dat dit nodig is ("re-trained weights"). Je bent dus afhankelijk van hun modellen en afstelling.
- **XL330 is de laagste-risico-route voor lopen.** Pollens officiële policies passen daar direct op. AI-FanGe en CMU laten de officiële modellen op XL330 lopen (CMU: 0,25 m/s, maar met oververhitting). Prijs: ≈ €600 voor de servo's in Europa. fanhao375's BOM waarschuwt dat de XL330 officieel 3,7–6 V aankan terwijl Microduck 6,6–8,2 V gebruikt. Dat is hun waarschuwing, niet door mij gecontroleerd.

## 6c. Doel: lopen, Pollen-software, sensoren, zo goedkoop mogelijk

Gekozen op 2026-10-31: **kissqy met HD-1910, onderdelen uit China.**

**Waarom kissqy en niet fanhao375:** goedkoper (HAT + IMU kant-en-klaar voor ¥218 ≈ €27, tegenover zelf 2 borden laten maken voor ≈ €60–150), een volledig geprijsd boodschappenlijstje met links, en de sensoren zitten erin. Fanhao375's release-notes noemen een robotd-bron in een kissqy-fork, dus de software is verwant.

### Sensoren bij kissqy (nagekeken in hun HAT-boardlijst)

| Sensor | Aanwezig? |
|---|---|
| Camera | IMX219 160° (¥64,35) |
| ToF 8×8 | VL53L5CX-module (¥49,50), via Qwiic-poort J5 op de HAT |
| Microfoon | **Ja**, MEMS-microfoon MK1 (LMA2718) op de HAT |
| Speaker | **Ja**, PAM8406-versterker op de HAT + 3525-luidspreker (¥3,08) |
| Lijf-IMU | Ja, eigen IMU-bord (LSM6DSV16X), voorgeflasht |
| Kop-IMU | Niet bevestigd. Bij Pollens HAT is de tweede IMU (BMI088) toch door de software ongebruikt. |
| NFC | Niet bevestigd |

### Boodschappenlijst (bron: kissqy `docs/build/data.json`, Taobao-prijzen)

| Post | ¥ |
|---|---:|
| 15× HD-1910-C001 (¥119) | 1785,00 |
| Radxa Zero 3W 2GB | 309,00 |
| HAT + IMU kant-en-klaar | 218,00 |
| NP-F550 accu's + dubbele lader | 84,59 |
| IMX219 camera | 64,35 |
| ToF-module | 49,50 |
| Gamepad (Gamesir-achtig) | 89,91 |
| URT2 servo-programmeerbord | 45,00 |
| Servokabels (60 stuks) | 90,00 |
| Lagers | 44,62 |
| Schroeven | 17,40 |
| microSD 32 GB | 39,90 |
| Kaartlezer (overslaan als je er een hebt) | 22,90 |
| Speaker, SH1.0-kabel, krimpkous | 8,63 |
| **Totaal** | **¥2868,80 ≈ €359** |

Niet inbegrepen: HUB en batterijaansluiting (door auteur ongeprijsd), geprinte onderdelen, gereedschap.

### Realistische kosten in Nederland (grove schatting)

| Post | Bedrag |
|---|---:|
| Onderdelen | ≈ €359 |
| Doorstuurservice: kosten per kg (±2 kg naar NL: ≈ $37 ≈ €33) en serviceopslag (0–5%) | ≈ €45–50 |
| Invoer-btw 21% over onderdelen + verzending | ≈ €85 |
| EU-invoerrecht €3 per productcategorie (sinds 1 juli 2026), ±8 categorieën | ≈ €25 |
| Afhandelingskosten ±€2 per aangifteregel (mogelijk) | ≈ €10–15 |
| Printen: filament ≈ €15–30, of Chinese printservice ¥199 (≈ €25 + extra verzending) | ≈ €15–50 |
| **Totaal** | **≈ €540–600** |

Alle posten behalve de onderdelen zijn schattingen uit verkoopsites van doorstuurservices en nieuwsberichten, niet uit een offerte. Zie [Fishgoo Taobao naar Nederland](https://blog.fishgoo.com/taobao-shipping-netherlands/) en [EU-persbericht over het €3-recht](https://commission.europa.eu/news-and-media/news/ensuring-fairness-and-safety-eur3-customs-duty-low-value-parcels-2026-06-29_en).

### Eerlijke vergelijking met het origineel

Pollen: $399 ≈ €350 **exclusief** btw, dus ≈ €425 incl. 21% btw plus verzending. De goedkoopste echte build komt realistisch op ≈ €540–600. Bouwen is dus niet goedkoper, maar je krijgt het bouwplezier en alle sensoren.

### Open punten voor deze route

- Een doorstuurservice (Superbuy, Fishgoo e.d.) kiezen en een echte offerte voor ±2 kg naar NL vragen.
- ~~Printen: eigen printer of printservice?~~ Eigen printer. Nog nagaan: past de set op jouw bed en heb je TPU?
- HUB- en batterijaansluiting ophalen bij de auteur (ongeprijsd).
- Controleren dat de loopmodellen in kissqy's R1-image goed werken; de issues bevatten geen onafhankelijke testrapporten.
- Of de HAT/IMU-verkoper naar een doorstuurservice verstuurt.

### Aanpassingen na het antwoord "ik heb een eigen printer" (2026-10-31)

**Printen:** de printservice (¥199) vervalt. Je print zelf met de [FrankenDuck-set](https://github.com/kissqy/Microduck-kissqy/tree/main/hardware/3d-print-frankenduck): 36 STL-bestanden en één 3MF met 5 platen. Referentie-instellingen: Bambu Lab P2S, 0,4 mm nozzle, 0,20 mm. De opgeslagen presets zijn PLA. **Plaat 5 is TPU** (twee zolen en een band): daar heb je TPU-filament voor nodig. Filament schat ik op ≈ €15–30, plus een rol TPU ≈ €20 als je die niet hebt. Hoeveel filament en printtijd de set kost, staat niet in de documentatie.

**Printer: Creality Ender 3 V2** (printbed 220 × 220 mm). Nagemeten op de STL-bestanden uit de set (2026-10-31):
- Het grootste onderdeel is de kopkap: 92 × 123 × 46 mm. Alle 36 onderdelen passen dus ruim op je bed. Het hoogste onderdeel is de batterijhouder (84 mm).
- De 3MF is een Bambu Studio-project voor een bed van 256 mm met PLA-presets. Slice de STL-bestanden zelf in Cura of PrusaSlicer, de platen moet je opnieuw indelen.
- **Let op:** de twee TPU-zolen (`left-sole-TPU.stl`, `right-sole-TPU.stl`) staan in **meters** opgeslagen. In je slicer lijken ze piepklein. Schaal ×1000, dan zijn ze 41 × 54 × 13 mm.
- De bouwgids zegt: "soft foot soles use the corresponding TPU parts". De TPU-zolen horen dus bij de standaardbouw. Alleen de TPU-band (`滑轮.stl`, 30 × 30 × 8 mm) voor de optionele roller-skates mag je overslaan.
- De Ender 3 V2 heeft standaard een Bowden-extruder. TPU printen kan, maar dan langzaam. Dat is mijn algemene kennis, niet iets uit de set.

**Accu en gamepad in Nederland kopen (voorstel):** lithium-accu's zijn bij doorstuurservices berucht. Eén klantverslag noemt €100 extra voor een kleine accu in het pakket (anekdote, niet bevestigd). Daarom: NP-F550-accu's en gamepad in Nederland kopen.
- 2 accu's (123accu, 2000 mAh) ≈ €32,50 plus een dubbele lader ≈ €15–20 (niet bevestigd). Een enkele Viltrox NP-F550 met USB-C kost ≈ €19,95.
- Controleer eerst welke gamepads de robot ondersteunt. De lijst gebruikt een Chinese gamepad (¥89,91).

**Herberekend totaal:**

| Post | Bedrag |
|---|---:|
| Taobao-onderdelen zonder accu en gamepad (¥2694,30) | ≈ €337 |
| Doorstuurservice: verzending (±1,2 kg) en opslag | ≈ €35–45 |
| Invoer-btw 21% | ≈ €79 |
| EU-invoerrecht (±7 categorieën) | ≈ €15–30 |
| Afhandelingskosten | ≈ €0–15 |
| Accu's en lader in NL | ≈ €50 |
| Gamepad (indien nodig) | ≈ €30 |
| Filament en TPU | ≈ €15–50 |
| **Totaal** | **≈ €580–620** |

Dit is hoger dan de eerdere schatting (€540–600), omdat accu en gamepad nu lokaal worden gekocht en de TPU erbij komt. Het blijft een schatting. De verzendkosten en recht per categorie moeten nog met een echte offerte worden bevestigd.

## 7. Voorlopige conclusie

- **XGO-Duck valt af.** Eén IMU, geen camera of ToF, geen Pollen-software.
- Kies **kissqy of fanhao375**. Beide gebruiken Pollens software, hebben een image, lopen aantoonbaar en zijn alleen voor persoonlijk gebruik.
- Kissqy lijkt praktischer om te bestellen (gedetailleerde lijst, kant-en-klare borden). Fanhao375 ligt dichter bij Pollens elektronica (officiële HAT) en heeft de grotere community.
- Voor volledige Pollen-sensoren moet je zelf camera en ToF aansluiten en testen of `mediad` en `tofd` werken. Dat is het bouwplezier.

## 8. Wat nog openstaat

1. ~~Look-vergelijking~~ gedaan: fanhao375 lijkt het meest op Pollen (zie sectie 6).
2. Controleren of echte camera en ToF bij **fanhao375** werken (hun release-notes verwijzen naar een "verificatiebereik").
3. Controleren of de **kissqy**-HAT een microfoon heeft, en of er een kop-IMU is.
4. Zoeken of Taobao naar Nederland levert, of een doorstuurservice nodig is, met totale verzend- en invoerkosten.
5. Het Pollen-origineel nalopen: voorraad, levertijd, verzending naar Nederland (stap 4 uit Part 4).
6. De HAT zelf laten maken (JLCPCB/PCBWay) of de kant-en-klare HAT/IMU van kissqy kopen.
7. Part 4 bijwerken met de nieuwe cijfers en de keuze. Part 5 wordt dan de beslissing met boodschappenlijst.
8. Part 1 noemt nog de XGO-Duck als plan. Dat past straks niet meer.

## 9. Bronnen

- Pollen: [press kit](https://pollen-robotics.com/microduck/press-kit/), [SLAM-blog](https://pollen-robotics.com/microduck/blog/microduck-slam/), [store](https://store.pollen-robotics.com/products/microduck)
- Lijst met alle projecten: [awesome-microduck](https://github.com/joeynyc/awesome-microduck)
- [fanhao375/microduck-replica](https://github.com/fanhao375/microduck-replica) en [BOM.en.md](https://github.com/fanhao375/microduck-replica/blob/master/BOM.en.md)
- [kissqy/Microduck-kissqy](https://github.com/kissqy/Microduck-kissqy) en [DUCK-SHOPPING-LIST.en.md](https://github.com/kissqy/Microduck-kissqy/blob/main/DUCK-SHOPPING-LIST.en.md)
- [Hackster: The DIY Alternative to Microduck](https://www.hackster.io/news/the-diy-alternative-to-microduck-1eb833470f6a)
- XGO-Duck: lokaal `xgoduck_hardware_src/repo/docs/BOM.md` en de Luwu-winkelpagina's hierboven
