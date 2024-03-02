# NordPool Integratsioon Home Assistantis

See README kirjeldab, kuidas seadistada NordPooli elektrihinna integratsiooni Home Assistantis

## Eeldused

- Home Assistanti instants peab olema seadistatud ja töötama.
- Kasutaja peab olema tuttav YAML konfiguratsioonifailide redigeerimisega Home Assistantis.

## Seadistamine

NordPooli integratsiooni lisamiseks Home Assistanti, tuleb järgida alltoodud samme. See võimaldab jälgida elektrihindu ja lisada neile täiendavaid kulusid, arvestades käibemaksu ja võrgutasusid.

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


