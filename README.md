# Elektri Lõpphinna Arvutamise Script Home Assistantis

See dokument kirjeldab scripti, mis on loodud Home Assistanti keskkonnas kasutamiseks. Script arvutab elektri lõpphinna (koos käibemaksuga), võttes aluseks NordPooli börsihinna ilma käibemaksuta. Skript arvestab lisakuludega nagu võrgutasud ja marginaalid ning lisab käibemaksu, pakkudes täpset ülevaadet elektrikuludest. Eriti oluline on skripti paindlikkus päikeseenergia tootmise perioodidel, kohandades võrgutasusid sõltuvalt päikese kättesaadavusest.

## Olulised Muutujad ja Nende Kirjeldused

- **`sunrise_offset` ja `sunset_offset`**: Sekundid, mis lisatakse päikese tõusu ja looja aegadele, et määrata päikeseenergia tootmise algus ja lõpp.
- **`network_fees`**: Võrgutasude määrad eurodes MWh kohta, eristades päeva- ja öötariifi.
- **`margins`**: Müügi- ja ostumarginaalid eurodes MWh kohta. Müügimarginaal rakendub päikeseenergia müügile võrgule, ostumarginaal elektri ostmisel.
- **`vat`**: Käibemaksu määr.
- **`solar_energy_sale_months`**: Massiiv, mis määratleb kuud (aprillist oktoobrini), millal päikeseenergia müüki arvestatakse.
- **`is_weekend`, `is_daytime`, `is_solar_energy_sale_period`**: Loogilised muutujad, mis määravad hetke seisu: nädalavahetus, päeva aeg või päikeseenergia müügiperiood.
- **`is_sunup_effective`**: Kontrollib, kas päike on üleval ja kas on päikeseenergia müügiperiood, mõjutades marginaalide valikut.
- **`fee`**: Rakendatav võrgutasu, olenevalt ajast ja päevast.
- **`marginal`**: Rakendatav marginaal, olenevalt päikeseenergia müügiperioodist.
- **`fee_with_vat` ja `marginal_with_vat`**: Võrgutasude ja marginaalide väärtused pärast käibemaksu lisamist.
- **`vat_amount`**: Käibemaksu summa, mis arvutatakse börsihinnast.
- **`additional_costs`**: Lisakulud, mis sisaldavad võrgutasusid ja marginaale koos käibemaksuga.
- **`total_cost`**: Elektri lõpphind, mis sisaldab börsihinda, lisakulusid ja käibemaksu.

## Eeldused

- Home Assistanti instants peab olema seadistatud ja töötama.
- Nordpooli integratsioon peab olema seadistatud
- Kasutaja peab olema tuttav YAML konfiguratsioonifailide redigeerimisega Home Assistantis.

## Seadistamine

NordPooli integratsiooni lisamiseks Home Assistanti, tuleb järgida alltoodud samme. See võimaldab jälgida elektrihindu ja lisada neile täiendavaid kulusid, arvestades käibemaksu ja võrgutasusid. Script arvestab, et nordpooli integratsioonis pole kasutatud käibemaksu. See lisatakse koos lisatasudega lõpphinnale.

### 1. Integratsiooni lisamine

Lisa oma `configuration.yaml` faili järgnev konfiguratsioon:

```yaml
sensor:
  - platform: nordpool
    low_price_cutoff: 1
    VAT: false
    region: "EE"
    precision: 3
    price_type: MWh
    additional_costs: '
    <Siia tuleb sinu arvutatud lisakulude skript>
    '
```

Selgitused konfiguratsiooni parameetritele:

low_price_cutoff: Määrab hinna, millest madalamal peetakse elektrihinda madalaks. See on näiteks oluline automaatikate jaoks, mis käivituvad madala elektrihinna korral.
VAT: Seadistatakse false, kuna börsihind (current_price) peab olema ilma käibemaksuta.
region: Määrab piirkonna, mille elektrihindu soovid jälgida. Antud näites kasutatakse Eestit ("EE").
precision: Määrab, mitme kümnendkoha täpsusega hinnainfo kuvatakse.
price_type: Veendu, et see väärtus oleks MWh, vastavalt nõuetele.

### 2. Lisakulude arvutamine
Lisakulude arvutamiseks kasutatakse Jinja skripti, mille olete lisanud additional_costs parameetri väärtusena. Veendu, et see skript arvestaks käibemaksu, võrgutasusid ja marginaale vastavalt vajadusele, skripti sees on selleks muutujad välja toodud.

### 3. Kontrolli ja taaskäivita
Pärast configuration.yaml faili uuendamist kontrolli konfiguratsiooni kehtivust Home Assistanti UI-s ja seejärel taaskäivita oma Home Assistanti instants, et muudatused jõustuksid.

## Kasutamine
Pärast integratsiooni edukat seadistamist ja Home Assistanti taaskäivitamist, kuvatakse NordPooli elektrihinna sensorid automaatselt Home Assistanti kasutajaliideses. Saad neid kasutada automaatikate loomiseks, energiakulude jälgimiseks ja optimeerimiseks.


