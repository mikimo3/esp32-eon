# SlimmeLezer telepítés – ESPHome

### Készült esp32.E.ON - 

![circ](https://raw.githubusercontent.com/mikimo3/esp32-eon/master/assets/img.png)
Hasonló módon a 3.3V-ot felhasználva, és egy tetszőleges bemenetet az esp32-n készíthető egy kapcsolás.

Sanxing SX631 S34U18-el tesztelve.

Példa adatok, és ahoz tartozó kulcsok és a felületen megjelenő nevek:

| obis(value)  |  display name | yaml key  |
|---|---|---|
|0-0:1.0.0(241229164240W)| Idő| timestamp|
|0-0:42.0.0(AUX1234567890123)| COSEM logikai készüléknév|identification|
|0-0:96.1.0(9876543210)| Mérő gyáriszám| equipment_id|
|0-0:96.14.0(0002)| Aktuális tarifa| electricity_tariff|
|0-0:96.50.68(ON)| Megszakító státusz| breaker_status|
|0-0:17.0.0(90.000*kW)| Limiter határérték| electricity_threshold|
|1-0:1.8.0(000318.842*kWh)| Hatásos import energia (+A)| energy_delivered|
|1-0:1.8.1(000139.969*kWh)| Hatásos import energia (+A) – tarifa 1| energy_delivered_tariff1|
|1-0:1.8.2(000178.873*kWh)| Hatásos import energia (+A) – tarifa 2| energy_delivered_tariff2|
|1-0:1.8.3(000000.000*kWh)| Hatásos import energia (+A) – tarifa 3| energy_delivered_tariff3|
|1-0:1.8.4(000000.000*kWh)| Hatásos import energia (+A) – tarifa 4| energy_delivered_tariff4|
|1-0:2.8.0(000034.226*kWh)| Hatásos export energia (-A)| energy_returned|
|1-0:2.8.1(000022.328*kWh)| Hatásos export energia (-A) – tarifa 1| energy_returned_tariff1|
|1-0:2.8.2(000011.898*kWh)| Hatásos export energia (-A) – tarifa 2| energy_returned_tariff2|
|1-0:2.8.3(000000.000*kWh)| Hatásos export energia (-A) – tarifa 3| energy_returned_tariff3|
|1-0:2.8.4(000000.000*kWh)| Hatásos export energia (-A) – tarifa 4| energy_returned_tariff4|
|1-0:3.8.0(001018.199*kvarh)| Import meddő energia (+R)| energy_positive_reactive|
|1-0:4.8.0(000210.285*kvarh)| Export meddő energia (-R)| energy_negative_reactive|
|1-0:5.8.0(000908.737*kvarh)| Meddő energia (QI)| reactive_energy_qi|
|1-0:6.8.0(000109.462*kvarh)| Meddő energia (QII)| reactive_energy_qii|
|1-0:7.8.0(000002.989*kvarh)| Meddő energia (QIII)| reactive_energy_qiii|
|1-0:8.8.0(000207.296*kvarh)| Meddő energia (QIV)| reactive_energy_qiv|
|1-0:15.8.0(000353.069*kWh)| Hatásos energia kombinált (|+A|+|-A|)| energy_absolute|
|1-0:32.7.0(232.9*V)| Pillanatnyi fázis feszültség L1| voltage_l1|
|1-0:52.7.0(234.1*V)| Pillanatnyi fázis feszültség L3| voltage_l2|
|1-0:72.7.0(234.0*V)| Pillanatnyi fázis feszültség L4| voltage_l3|
|1-0:31.7.0(001*A)| Pillanatnyi áram L1| current_l1|
|1-0:51.7.0(001*A)| Pillanatnyi áram L2| current_l2|
|1-0:71.7.0(001*A)| Pillanatnyi áram L3| current_l3|
|1-0:13.7.0(0.085)| Pillanatnyi teljesítmény tényező| instantaneous_power_factor|
|1-0:33.7.0(0.048)| Pillanatnyi teljesítmény tényező L1| instantaneous_power_factor_l1|
|1-0:53.7.0(0.160)| Pillanatnyi teljesítmény tényező L2| instantaneous_power_factor_l2|
|1-0:73.7.0(0.073)| Pillanatnyi teljesítmény tényező L3| instantaneous_power_factor_l3|
|1-0:14.7.0(49.98*Hz)| Frekvencia| frequency|
|1-0:1.7.0(00.068*kW)| Pillanatnyi import teljesítmény (+A)| power_delivered|
|1-0:2.7.0(00.000*kW)| Pillanatnyi export teljesítmény (-A)| power_returned|
|1-0:5.7.0(00.812*kvar)| Pillanatnyi meddő teljesítmény (QI)| reactive_power_qi|
|1-0:6.7.0(00.000*kvar)| Pillanatnyi meddő teljesítmény (QII)| reactive_power_qii|
|1-0:7.7.0(00.000*kvar)| Pillanatnyi meddő teljesítmény (QIII)| reactive_power_qiii|
|1-0:8.7.0(00.000*kvar)| Pillanatnyi meddő teljesítmény (QIV)| reactive_power_qiv|
|1-0:31.4.0(200*A)| Áram korlátozás határérték 1| electricity_threshold_l1|
|1-0:51.4.0(200*A)| Áram korlátozás határérték 2| electricity_threshold_l2|
|1-0:71.4.0(200*A)| Áram korlátozás határérték 3| electricity_threshold_l3| 


  Magyarországi mérők sajátosságai miatt a következő paraméterek megadása kötelező a konfigurációs fájlban, különben nem lesz
  képes feldolgozni és továbbítani a mérők által küldött telegram üzenetet: (alapból tartalmazza mindegyik példa yaml fájl)
   
```yaml
dsmr:
  max_telegram_length: 3000
```
A komponenst a következő bejegyzéssel lehet használni:
```yaml
external_components:
  - source:
      type: local
      path: ./components/
    components: [dsmr]
```