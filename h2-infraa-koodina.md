# h2 - Soitto kotiin

x) Muistiinpanot

## Hello Salt Infra-as-Code

- Salt voi hallita tuhansia tietokoneita, mutta aloitetaan "Hello, World!" -esimerkillä.

- Asenna Salt-minion: $ sudo apt-get -y install salt-minion.

- Käytetään micro editoria: $ sudo apt-get -y install micro ja $ export EDITOR=micro.

- Luo kansio moduulille: /srv/salt/hello/.

- Moduulin pääsisäänkäynti on init.sls.

- Kirjoita idempotentti koodi tiedoston luomiseksi:

  /tmp/hellotero:
  file.managed

  ja moduuli: $ sudo salt-call --local state.apply hello.

Salt luo /tmp/hellotero ja raportoi muutoksen.

Varmista: $ ls /tmp/hellotero.

Idempotenssi: toistuvat ajot eivät tee muutoksia, kun tila on jo oikea.

Tärkeitä Salt-funktioita: pkg, file, service, user, cmd.

## Rules of YAML

- Saltin tiedostojen oletusrenderöijä on YAML renderer.

- YAML muuntaa tekstimuotoisen datan Pythonin tietorakenteeksi Saltissa.

-  YAML:lle:

- Data on key: value -pareja.

- Mappings merkitään kolonilla ja yhdellä välilyönnillä (: ).

- Arvo voi olla erilaisissa rakenteissa (esim. lista, sanakirja).

- Kaikki avaimet ovat case-sensitive.

- Ei tabulaattoreita, käytä vain välilyöntejä.

- Kommentit alkavat #-merkillä.

  ## YAML simple structure

- YAML koostuu kolmesta peruselementistä: Scalars, Lists, Dictionaries

- Scalars: yksinkertainen key: value, arvo voi olla numero, merkkijono tai boolean

- Lists: avain, jonka alla lista-arvot, rivit alkavat kahdella välilyönnillä ja viivalla -

- Dictionaries: kokoelma key: value -pareja ja listoja, voi sisältää sisäkkäisiä rakenteita

- Kaikki avaimet ovat case-sensitive

  ## Lists and dictionaries - YAML block structures

- YAML käyttää lohkorakenteita (block structures).


- Sisennys määrittää kontekstin; välilyöntejä on pakko käyttää, kaksi välilyöntiä on standardi


- Listat ja sanakirjat (collections) merkitään rivillä viivalla ja välilyönnillä (- ) jokaisen kohteen alussa

## The top file

- Infrastruktuurit koostuvat usein ryhmistä koneita, joilla on samankaltaisia rooleja.

- Ryhmät toimivat yhdessä muodostaen sovelluspinon.

- Järjestelmän ylläpitäjän tulee pystyä määrittelemään roolit koneille (esim. Apache-webpalvelin asennettuna ja käynnissä).

- Saltissa tämä määritellään top file -tiedostossa (top.sls).

- Top file sijaitsee aina state tree -hakemiston ylimmällä tasolla

  Top file -komponentit

1. Environment: Hakemisto, joka sisältää state-tiedostoja järjestelmien konfigurointiin.

2. Target: Koneiden ryhmä, joille tietyt state-tiedostot ajetaan.

3. State files: Lista state-tiedostoista, jotka määrittelevät konfiguraatiot ja valvonnan kohdekoneille.

## a) 

Tässä tehtävässä kirjoitin sls-tiedoston, joka tekee esimerkkitiedoston /tmp/ -kansioon


Aloitin kuomalla kansion ja tiedoston init.sls

![Kansio ja tiedosto](https://github.com/TuukkaMero/linux-course/blob/main/a%20moduulinluonti.png)

Tiedoston sisältö:

![Sisältö](https://github.com/TuukkaMero/linux-course/blob/main/a%20tiedost%C3%B6sis%C3%A4lt%C3%B6.png)

Seuraavaksi suoritin staten paikallisesti:

![suoritus](https://github.com/TuukkaMero/linux-course/blob/main/statesuoritus.png)

![ls](https://github.com/TuukkaMero/linux-course/blob/main/ls%20hellotuukka.png)

## b) 

Loin tiedoston /srv/salt/top.sls

Tiedoston sisältö:

![sisältö](https://github.com/TuukkaMero/linux-course/blob/main/top%20sis%C3%A4lt%C3%B6.png)

Kokeilin että se toimii: 

![testi](https://github.com/TuukkaMero/linux-course/blob/main/topfile%20testi.png)

## c) 

Tässä tehtävässä loin erilliset esimerkit kustakin viidestä tärkeimmästä tilafunktiosta

Loin kaikkille esimerkeille omat tilat:

![Tilat](https://github.com/TuukkaMero/linux-course/blob/main/tila%20esimerkit.png)

pkg:

![Pkg](https://github.com/TuukkaMero/linux-course/blob/main/pkg%20sis%C3%A4lt%C3%B6.png)

file:

![file](https://github.com/TuukkaMero/linux-course/blob/main/file%20sis%C3%A4lt%C3%B6.png)

service:

![service](https://github.com/TuukkaMero/linux-course/blob/main/file%20sis%C3%A4lt%C3%B6.png)

user:

![user](https://github.com/TuukkaMero/linux-course/blob/main/user%20sis%C3%A4lt%C3%B6.png)

cmd:

![user](https://github.com/TuukkaMero/linux-course/blob/main/cmd%20sis%C3%A4lt%C3%B6.png)

Kokeilin lopuksi tomivuutta top.sls avulla komennolla sudo salt-call --local state.apply

## d)

![hellopkgfile](https://github.com/TuukkaMero/linux-course/blob/main/hellopkgfile%20moduuli.png)

Sls- tiedoston sisältö:

![sisältö](https://github.com/TuukkaMero/linux-course/blob/main/hellopkgfile%20sis%C3%A4lt%C3%B6.png)

Ensimmäinen testi: 

![1 testi](https://github.com/TuukkaMero/linux-course/blob/main/hellopkgfile%201%20testi.png)

Lopputuloksen tarkistus:

![Lopputulos](https://github.com/TuukkaMero/linux-course/blob/main/lopputulos.png)

Toinen testi: 

![toinentesti](https://github.com/TuukkaMero/linux-course/blob/main/toinen%20hellopkgfile%20testi.png)

Toisella testikerralla hellofilepkg ei tehnyt muutoksia joten sls-tiedosto on idempotentti.


## Lähteet

https://terokarvinen.com/2024/hello-salt-infra-as-code/

https://docs.saltproject.io/salt/user-guide/en/latest/topics/overview.html#rules-of-yaml

https://docs.saltproject.io/en/latest/ref/states/top.html







