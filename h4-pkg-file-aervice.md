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

Tiedostoon lisäsin seuraavat tekstit:

![ssh](https://github.com/TuukkaMero/linux-course/blob/main/ssh%20asennustiedosto.png)

seuraavaksi loin saltssh hakemiston johon loin salt tilan:

![ssh.sls](https://github.com/TuukkaMero/linux-course/blob/main/ssh.sls%20sis%C3%A4lt%C3%B6.png)



