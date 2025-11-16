# h4 Pkg-file-service

## x) 

Perusidea: Pkg-File-Service

Saltissa daemonien hallinta tehdään yleensä kaavalla:

1. Pkg → 2. File → 3. Service

## pkg.installed – asenna ohjelma

file.managed – hallitse asetustiedostoa Salt-masterilta

service.running – käynnistä/pidä käynnissä ja reagoi muutoksiin

Tämä tekee tilasta idempotentin (ajokertojen määrä ei muuta lopputulosta).

sshd.sls – SSH-portin vaihto
openssh-server:
  pkg.installed

/etc/ssh/sshd_config:
  file.managed:
    - source: salt://sshd_config

sshd:
  service.running:
    - watch:
      - file: /etc/ssh/sshd_config

Mikä tässä tapahtuu?

Asennetaan openssh-server

Korvataan /etc/ssh/sshd_config Salt-masterin versiolla

Jos config muuttuu → sshd restartataan automaattisesti (watch)


## a)

Alotin tehtävän avaamalla ssh:n asennustiedoston: sudo nano /etc/ssh/sshd_config

Tiedostoon lisäsin seuraavat tekstit :

![ssh](https://github.com/TuukkaMero/linux-course/blob/main/ssh%20asennustiedosto.png)

Minulla on nyt portit 22 ja 2222

Testasin käsin että ssh kuuntelee molemmilla porteilla:

![kuuntelu](https://github.com/TuukkaMero/linux-course/blob/main/ssh%20kuuntelu%20testi.png)

Seuraavaksi loin salt.sls tiedoston nimeltä ssh.sls

Tiedoston sisältö:

![sisältö](https://github.com/TuukkaMero/linux-course/blob/main/ssh.sls%20sis%C3%A4lt%C3%B6.png)

Seuraavaksi loin sshd_config salt-hakemistoon:

![sisältö](https://github.com/TuukkaMero/linux-course/blob/main/ssh%20config%20sis%C3%A4lt%C3%B6.png)

Ajoin salt-call komennon joka näytti toimivan

Seuraavaksi poistin open ssh serverin käsin komennolla:

sudo apt remove openssh-server -y

Viimeikseksi ajoin tilan uudestaan ja sain vastaukseksi:

local:
----------
          ID: openssh-server
    Function: pkg.installed
      Result: True
     Comment: Package openssh-server was successfully installed
     Changes:
              ----------
              openssh-server:
                  New installation

----------
          ID: /etc/ssh/sshd_config
    Function: file.managed
      Result: True
     Comment: File /etc/ssh/sshd_config was updated
     Changes:
              ----------
              diff:
                  --- /etc/ssh/sshd_config
                  +++ /etc/ssh/sshd_config
                  @@
                  -#Port 22
                  +Port 22
                  +Port 2222
                  -#PermitRootLogin yes
                  +PermitRootLogin no

----------
          ID: ssh
    Function: service.running
      Result: True
     Comment: Service ssh was started
     Changes:
              ----------
              ssh:
                  Started

Summary for local
-------------
Succeeded: 3
Failed:    0
-------------
 Tilani korjasi puutteet joten idempotenssi on saavutettu

## Tiivistelmä

1. SSH-palvelin asennus

- Saltilla asennettiin openssh-server paketti

2. SSH-konfiguraation hallinta

- sshd_config-tiedosto kopioitiin Saltin hallinnoimasta lähteestä (file.managed) oikeaan paikkaan /etc/ssh/sshd_config.

- Asetettiin omistaja (root), ryhmä ja tiedostolle oikeudet (600).

- Määriteltiin, että palvelu käynnistetään uudelleen automaattisesti, jos tiedosto muuttuu (watch_in).

3. SSH-palvelun käynnissäolon varmistus

- ssh-palvelu asetettiin käynnistymään ja pysymään käynnissä (service.running).

- Salt huolehti, että palvelu käynnistyy tarvittaessa uudelleen konfiguraatiomuutosten jälkeen.

4. Idempotenssin testaus

- Saltin tila voidaan ajaa useaan kertaan ilman, että järjestelmä muuttuisi turhaan.

- Ensimmäinen ajokerta voi asentaa paketin ja kopioida tiedoston; myöhemmät ajot eivät tee muutoksia, jos kaikki on jo oikein.


 ## Lähteet 

 https://terokarvinen.com/2018/04/03/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/?fromSearch=karvinen%20salt%20ssh











