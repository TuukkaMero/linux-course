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
     Comment: Package openssh-server is already installed
     Changes:

----------
          ID: /etc/ssh/sshd_config
    Function: file.managed
      Result: True
     Comment: File /etc/ssh/sshd_config is in the correct state
     Changes:

----------
          ID: ssh
    Function: service.running
      Result: True
     Comment: Service ssh is running
     Changes:

Summary for local
-------------
Succeeded: 3 (unchanged=3)
Failed:    0
-------------
 Tilani korjasi puutteet joten idempotenssi on saavutettu


 ## Lähteet 

 https://terokarvinen.com/2018/04/03/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/?fromSearch=karvinen%20salt%20ssh











