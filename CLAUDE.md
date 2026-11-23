# Ketunkorven kotinäyttö (dashboardketunkorpi)

Omistajan iäkkäiden vanhempien seinänäyttö omakotitalossa Ketunkorvessa (Kalajärvi, Pohjois-Espoo, haja-asutusalue). Asukkaat eivät käytä julkista liikennettä, heillä on **pörssisähkösopimus**, ja talossa on **puusauna**. Ei lapsia: ei kania eikä pelillisiä ominaisuuksia.

- **Tiedosto:** `index.html`
- **Osoite:** https://simbe.github.io/dashboardketunkorpi/ (vaihda, jos repolla on eri nimi)
- **Laite:** iPad Air 2, vaakanäkymä 1024×768, Safari. Safarin palkkien kanssa käytettävä korkeus on noin 690 px, joten testaa molemmat.
- **Yhteensopivuus: iPadOS 15.** JavaScript ES5-tyyliin: ei `?.`, ei `??`, ei nuolifunktioita, ei `let`/`const`, ei template-merkkijonoja. CSS: ei `:has()`, ei `inset`. Testaa aina WebKitillä.
- **Käytettävyys iäkkäille:**
  - suomi;
  - isot fontit (tyyppiasteikko on askelta isompi kuin muissa näytöissä);
  - vähän tietoa kerralla;
  - **ei kosketustemppuja**, paitsi pitkä painallus kelloon, joka näyttää version;
  - virheet selvällä kielellä ("… Yritetään uudelleen.");
  - hintataso kerrotaan aina myös sanalla, ei pelkällä värillä.
- **Sijainti:** `LAT = 60.34, LON = 24.71`, noin kilometrin tarkkuus tarkoituksella. Älä lisää talon tarkkaa sijaintia.

## Mitä näytössä on (versio 1.1)

- **Ylärivi:**
  - viikonpäivä ja päivämäärä, liputuspäivän lippu;
  - viikkonumero, päivän pituus ja muutos eilisestä;
  - auringonnousu ja -lasku;
  - kello sekunteineen.
- **Varoitukset** yhdellä rivillä rinnakkain, vain kun aiheellisia:
  - **ukkonen:** salama alle 20 km, 30 min;
  - **liukkaus:** Open-Meteon tuntidatasta; jäätävä sade, vesisade jäiseen maahan, tai lämpötila nollan molemmin puolin ja kosteaa;
  - **lumi:** vähintään 3 cm seuraavan 24 tunnin aikana.
- **Sadetutka:** `SHOW_RADAR = true`. Pois asetuksella `false`, kokeiluun osoitteen perään `?tutka=0`. Ilman tutkaa sää ja sähkö ovat vierekkäin, ja sähkögrafiikka on isompi ja selityksen kanssa.
- **Nykyinen sää:** lämpötila, olotila, tuntuu kuin, tuuli, puuskat, kosteus, ilmanpaine. `fitCurrent()` tiivistää, ja `is-tight` piilottaa sadetodennäköisyydet, jos tila ei riitä.
- **Pörssisähkö** (spot-hinta.fi, 15 minuutin hinnat, joista tuntikeskiarvot):
  - **Hinta nyt:** iso luku, sana (Edullinen / Tavallinen / Kallis / Erittäin kallis) ja huomautus "pörssihinta, sis. alv".
  - **Pylväät:** tänään ja huomenna, keskihinnat. Huomisen hinnat tulevat noin klo 14, ja sitä ennen näkyy "Huomisen hinnat tulevat noin klo 14". Hintoja haetaan 15 minuutin välein klo 13 jälkeen, kunnes huomisen hinnat ovat tulleet.
  - **Pylväiden merkinnät:** mennyt aika himmennetty, kuluva tunti valkoisella kehyksellä, halvin 3 tunnin jakso valkoisella viivalla pylväiden alla.
  - **"Halvin 3 h tänään/huomenna"** kellonaikoineen ja keskihintoineen.
  - **Värirajat:** 5 / 10 / 20 snt (`PRICE_CHEAP`, `PRICE_HIGH`, `PRICE_VHIGH`).
  - **`PRICE_EXTRA_CENTS = 0`:** sopimuksen marginaali ja siirtomaksu (snt/kWh, sis. alv) eivät ole vielä tiedossa. Kun ne asetetaan, kaikki hinnat ovat kokonaishintoja ja huomautus vaihtuu.
- **Ennuste:**
  - 6 kohtaa 2 tunnin välein;
  - 5 päivää, joista näkyvät ylin ja alin lämpötila sekä sadetodennäköisyys.
- **Yöhimmennys** klo 22–06 (`NIGHT_DIM = 0.4`).

## Tehdyt päätökset (älä muuta kysymättä)

- **Saunavuoro poistettu,** koska talossa on puusauna.
- **Ei julkista liikennettä,** ei kania.
- **Ei vielä tehty, mahdollisia lisäyksiä:**
  - kelikamera ja tiesää (Digitraffic, Vihdintie / Kehä III);
  - toistuvat muistutukset (esim. roska-auto, sakokaivo), ilman henkilö- tai terveystietoja;
  - hallavaroitus touko–syyskuussa;
  - sähkökatkokartta ja myrskyvaroitukset (vaativat välipalvelun, ei CORSia);
  - kallein jakso (tyhjään tilaan "Halvin 3 h" alle).

## Työskentelysäännöt (koskevat kaikkia muutoksia)

- **Kieli:** vastaa suomeksi ja selkokielellä. Omistaja ei ole ohjelmoija, joten selitä mitä muutos tarkoittaa käytännössä, älä koodia.
- **Kerro suunnitelma ennen kuin teet mitään:** mitä teet ja mitä jätät tekemättä. Jos jokin on epäselvää, kysy ensin. Jos omistaja pyytää vain vastausta tai selvitystä, älä muuta koodia.
- **Versio:** nosta jokaisessa muutoksessa `VERSION`-muuttujaa (pieni muutos 1.0 → 1.1, iso muutos → 2.0).
- **Commit-viesti** aina tässä muodossa:
  ```
  Version X.Y
  - muutos 1
  - muutos 2
  ```
- **Kuvat:** anna kuvakaappaukset ennen ja jälkeen aina, kun ulkoasu muuttuu. Tee kuvat laitteen oikealla näyttökoolla.
- **Testaa ennen julkaisua:** Playwright, Chromium ja WebKit. Testaa valekellolla (`page.clock.install`) ja korvaa kaikki rajapinnat mock-vastauksilla (`page.route`), jotta eri tilanteet saadaan näkyviin (yö, varoitukset, virheet, perutut lähdöt). Tarkista, ettei mikään mene ruudun yli tai katkea. Open-Meteo antaa 429-virheen, jos sitä kutsutaan testeissä liikaa, joten käytä mockeja.
- **Pidä sivu yhtenä tiedostona:** HTML, CSS ja JS samassa tiedostossa, ei build-vaihetta eikä riippuvuuksia (paitsi Leaflet CDN:stä, jos karttaa käytetään).
- **Sivu on julkinen** (GitHub Pages): ei henkilötietoja, terveystietoja eikä tarkkoja kotiosoitteita/koordinaatteja.
- **Julkaisu:** muutos menee haaraan ja pull requestiin. Kun omistaja hyväksyy (merge) päähaaraan, GitHub Pages julkaisee sen. Pages välimuistittaa noin 10 minuuttia. Livenä olevan version voi tarkistaa hakemalla sivun ja katsomalla `VERSION`-arvon.
- **Säännöt kaikkien kolmen näytön kesken:** sama koodipohja on käytössä myös repoissa `weatherlassila`, `dashboardkilo` ja `dashboardketunkorpi`. Jos korjaat yleisen bugin (esim. sää, tutka, liputuspäivät), mainitse, että sama korjaus voi koskea muitakin näyttöjä.

## Tekniset huomiot

- **Koko:** kaikki mitoitetaan muuttujasta `--u` (ks. CSS:n alku), jotta näyttö täyttyy tarkasti. Fonttikoot tulevat muuttujista `--fs-xs` … `--fs-2xl`; älä lisää irrallisia px-kokoja.
- **Aikavyöhyke:** kaikki kellonajat lasketaan Europe/Helsinki-ajassa apufunktioilla (`helsinkiDateKey`, `helsinkiHour` jne.), ei laitteen paikallisajassa.
- **Rajapinnat, jotka toimivat suoraan selaimesta (CORS):** Open-Meteo, FMI WFS (ennuste, salamat), FMI WMS (sadetutka), Digitraffic (junat ja tiedotteet), HSL Digitransit, spot-hinta.fi.
- **Eivät toimi selaimesta (ei CORSia):** FMI:n CAP-varoitukset, MeteoAlarm, porssisahko.net. Nämä vaatisivat välipalvelun.
- **Sadetutka:** uusin kuva-aika luetaan GetCapabilities-vastauksesta (muuten CORB-ongelmia).
- **Nimipäiväluettelo on lisensoitu** (Almanakkatoimisto), joten sitä ei käytetä. Liputuspäivät tulevat virallisesta listasta (`FLAG_DAYS` + sääntöpohjaiset päivät).
