# Tietosuojaseloste — Lintsiäppi

**Viimeksi päivitetty:** 6.5.2026

## 1. Yleistä

Tämä tietosuojaseloste koskee **Lintsiäppi**-sovellusta (paketin tunniste `fi.jukkalepisto.lintsiappi`), joka on saatavilla iOS:lle, Androidille ja webille. Sovellus auttaa Linnanmäen kävijää suunnittelemaan päivänsä: näkemään minkä laitteiden korkeus- ja ikävaatimukset perheen jäsenet täyttävät sekä saamaan jonotusaikaennusteita.

Sovelluksen ylläpitäjä ja rekisterinpitäjä:

- **Jukka Lepistö**
- Sähköposti: [mail@jukkalepisto.com](mailto:mail@jukkalepisto.com)

Sovellusta ei ole tehty Linnanmäen toimeksiannosta eikä se ole virallinen Linnanmäki-sovellus.

## 2. Periaatteet lyhyesti

- **Mahdollisimman vähän tietoja:** Kerätään vain se, mitä toiminnallisuus edellyttää.
- **Ei käyttäjätilejä:** Sovellukseen ei rekisteröidytä eikä kirjauduta. Salasanoja ei ole.
- **Perheen tiedot pysyvät laitteella:** Perheenjäsenten nimet, iät, pituudet ja avatarit tallennetaan ainoastaan oman laitteesi muistiin eivätkä ne lähde sieltä mihinkään.
- **Analytiikka vain suostumuksella:** Anonyymiä käyttöanalytiikkaa kerätään vain jos hyväksyt sen sovelluksen ensimmäisellä avauksella. Voit myös kieltäytyä kokonaan.

## 3. Mitä tietoja keräämme

### 3.1 Laitteelle tallennettavat tiedot (eivät lähde laitteelta)

Seuraavat tiedot tallennetaan vain laitteesi paikalliseen muistiin (selaimen `localStorage` tai sovelluksen vastaava). Niitä ei lähetetä mihinkään palvelimelle.

| Tieto | Mitä sisältää | Miksi |
|---|---|---|
| Perheenjäsenet | Lisäämäsi henkilöt: nimi, rooli (vanhempi/lapsi), ikä, pituus, valitsemasi emoji-avatar | Jotta sovellus voi näyttää, mille laitteille kukin pääsee |
| Suostumusvalinta | Hyväksyitkö analytiikan vai et | Jotta sovellus muistaa valintasi etkä joudu vastaamaan uudelleen |
| Anonyymi laitetunniste | Satunnainen UUID, joka luodaan ensimmäisellä käynnistyksellä | Käytetään vain analytiikkatapahtumissa erottamaan eri laitteita ilman, että tunnistat henkilöä |
| Sää-välimuisti | Linnanmäen sääennuste enintään 15 minuuttia | Jotta sääpalvelua ei kysellä turhaan |
| Teema-asetus | Vaalea/tumma teema | Säilyttää valitsemasi ulkoasun |

Tämä tieto häviää, kun poistat sovelluksen, tyhjennät sovelluksen tiedot tai selaimen `localStorage`n.

### 3.2 Analytiikkatapahtumat (vain suostumuksella)

Jos hyväksyt analytiikan, sovellus lähettää käyttötapahtumia Supabase-palvelimelle. Tapahtumiin **ei** liity nimeäsi, sähköpostiasi, puhelinnumeroasi eikä Apple- tai Google-tiliäsi. Jokaiseen tapahtumaan liitetään edellä mainittu **anonyymi laitetunniste (`device_id`)**, joka on satunnainen UUID — se ei ole laitteesi oikea tunniste eikä sitä voida yhdistää sinuun tai tiliisi.

Kerättävät tapahtumat:

| Tapahtuma | Milloin | Mitä lisätietoa |
|---|---|---|
| `app_opened` | Sovellus avataan | — |
| `personal_data_consent` | Vastaat suostumuskysymykseen | Hyväksyitkö (tosi/epätosi) |
| `member_added` | Lisäät perheenjäsenen | Rooli (vanhempi/lapsi), ikä, pituus |
| `member_removed` | Poistat perheenjäsenen | Rooli |
| `ride_modal_opened` | Avaat laitteen tiedot | Laitteen tunniste ja kategoria |

**Tärkeää:** Vaikka sovellus lähettäisi metatiedoissa nimen tai emojin, palvelin **poistaa** ne automaattisesti ennen tallennusta. Tietokantaan jää siis vain rooli, ikä, pituus ja muut ei-tunnistavat kentät.

Tallennetut kentät tietokannassa (`app_events`-taulu): `device_id`, tapahtuman tyyppi, siivottu metatieto sekä palvelimen aikaleima.

Jos kieltäydyt analytiikasta, sovellus ei lähetä mitään tapahtumia.

### 3.3 Sijaintitieto

Sovellus pyytää sijaintiasi vain, jos painat kartalla "paikanna minut" -painiketta. Sijainti haetaan kerran ja näytetään kartalla. Sitä **ei** tallenneta sovellukseen eikä lähetetä analytiikkaan. Karttapalvelu (Mapbox) saa sijainnin näyttääkseen sen kartalla.

## 4. Käsittelyn tarkoitus ja oikeusperuste

| Käsittely | Tarkoitus | Oikeusperuste (GDPR) |
|---|---|---|
| Paikalliset tiedot laitteella | Sovelluksen ydintoiminnallisuus (laitevalinta, suosikit) | Sopimus / palvelun tarjoaminen, art. 6(1)(b) |
| Analytiikka | Sovelluksen kehittäminen ja vianetsintä | Suostumus, art. 6(1)(a) — peruutettavissa |
| Sijainti pyydettäessä | Oman sijainnin näyttäminen kartalla | Suostumus (laitteen käyttöjärjestelmän lupakysely) |

## 5. Kolmannet osapuolet

| Palvelu | Mihin käytetään | Mitä tietoja siirtyy |
|---|---|---|
| **Supabase** (tietokanta + edge-funktio) | Sovelluksen julkinen sisältö (laitteet, palvelut, jonomalli) ja analytiikkatapahtumien tallennus | Anonyymi `device_id` ja siivottu tapahtumametatieto, kun analytiikka on hyväksytty. Lue: <https://supabase.com/privacy> |
| **Mapbox** | Interaktiivisen kartan piirtäminen | Tavanomainen karttapalvelun käyttö ja — vain "paikanna minut" -toiminnossa — laitteen sen hetkinen sijainti. Lue: <https://www.mapbox.com/legal/privacy> |
| **Open-Meteo** | Sääennuste | Vain Linnanmäen kovakoodatut koordinaatit. Käyttäjästä ei lähetetä mitään. Lue: <https://open-meteo.com/en/terms> |

Sovellus **ei** käytä Firebasea, Google Analyticsia, Sentryä, Crashlyticsia, mainosverkkoja, sometunnistautumista eikä maksunkäsittelijöitä.

## 6. Tietojen säilytysaika

- **Paikalliset tiedot laitteella:** Säilyvät niin kauan kuin sovellus on asennettuna tai kunnes itse poistat ne. Voit milloin tahansa poistaa perheenjäseniä sovelluksen sisältä tai tyhjentää kaikki tiedot poistamalla sovelluksen / sen tiedot.
- **Analytiikkatapahtumat (Supabase):** Säilytetään toistaiseksi sovelluksen kehitystä varten. Voit pyytää oman laitetunnisteesi tapahtumien poistoa sähköpostitse (kohta 7).
- **Sää-välimuisti:** Vanhenee automaattisesti 15 minuutissa.

## 7. Sinun oikeutesi

EU:n yleisen tietosuoja-asetuksen (GDPR) mukaan sinulla on oikeus:

- **Peruuttaa suostumus analytiikkaan.** Tällä hetkellä peruutus tapahtuu poistamalla sovelluksen tiedot tai sovellus. Suostumuksen muuttamismahdollisuus sovelluksen sisällä lisätään myöhemmin.
- **Tarkastaa tietosi.** Lähetä sähköpostia osoitteeseen [mail@jukkalepisto.com](mailto:mail@jukkalepisto.com) ja liitä mukaan laitetunnisteesi (löydät sen ottamalla yhteyttä — se on tallennettu vain laitteellesi).
- **Pyytää tietojesi poistoa.** Sama yhteystieto. Käytännössä poisto tarkoittaa, että `app_events`-taulusta poistetaan kaikki rivit, joiden `device_id` täsmää.
- **Vastustaa käsittelyä tai rajoittaa sitä.**
- **Tehdä valitus valvontaviranomaiselle:** Tietosuojavaltuutetun toimisto, [tietosuoja.fi](https://tietosuoja.fi).

Koska tunnistamme sinut vain anonyymin laitetunnisteen perusteella, oikeuksien käyttö edellyttää, että toimitat tunnisteen meille itse.

## 8. Tietoturva

- Yhteydet palvelimille tapahtuvat HTTPS-salatusti.
- Analytiikkapalvelin poistaa nimen ja emojin metatiedoista ennen tallennusta.
- Salasanoja tai maksutietoja ei käsitellä, koska niitä ei kerätä.
- Sovelluksessa ei ole käyttäjätilejä, joten tilien kaappaaminen ei ole mahdollista.

## 9. Lapset

Sovellus on suunniteltu perheille. Lapsia koskevat tiedot (nimi, ikä, pituus, avatar) tallennetaan ainoastaan vanhemman omalle laitteelle, eivätkä ne lähde laitteelta. Anonyymeissä analytiikkatapahtumissa kulkee ainoastaan ikä, pituus ja rooli ilman tunnistetietoja.

Suosittelemme, että aikuinen huolehtii sovelluksen asetuksista ja suostumuksen antamisesta lapsen puolesta.

## 10. Muutokset tähän selosteeseen

Selostetta voidaan päivittää, jos sovelluksen toiminnallisuus tai käytetyt palvelut muuttuvat. Viimeisin päivityspäivä näkyy tämän asiakirjan alussa.

## 11. Yhteystiedot

Tietosuojaa koskevissa kysymyksissä voit ottaa yhteyttä:

- Sähköposti: [jukka.lepisto@krossflow.fi](mailto:jukka.lepisto@krossflow.fi)
