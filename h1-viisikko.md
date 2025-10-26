# h1 - viisikko

x) 
- Salttia käytetään orjakoneiden hallitsemiseen

- Saltin asentamiseen käytettiin: $ sudo apt-get update ja $ sudo apt-get -y install salt-minion

- Masterin asennus:

  master$ sudo apt-get update

master$ sudo apt-get -y install salt-master

master$ hostname -I

10.0.0.88

  - Orjan asennus:

     slave$ sudo apt-get update

slave$ sudo apt-get -y install salt-minion

- Esimerkkikomentoja orjille:

  master$ sudo salt '*' cmd.run 'hostname -I'

master$ sudo salt '*' grains.items|less

master$ sudo salt '*' grains.items

master$ sudo salt '*' grains.item virtual

master$ sudo salt '*' pkg.install httpie

master$ sudo salt '*' sys.doc|less

a) Debian asennuksessa ei ongelmaa

b) Aloitin salt minionin asennuksen asentamalla salt project repositoryn ajamalla seuraavat komennot:

mkdir -p /etc/apt/keyrings

curl -fsSL https://packages.broadcom.com/artifactory/api/security/keypair/SaltProjectKey/public | sudo tee /etc/apt/keyrings/salt-archive-keyring.pgp

curl -fsSL https://github.com/saltstack/salt-install-guide/releases/latest/download/salt.sources | sudo tee /etc/apt/sources.list.d/salt.sources

Curl komentoa ei aluksi löytynyt joten asensin sen näin: 

apt update

apt install curl -y

Ajoin seuraavaksi ohjeen mukaan sudo apt update metadatan päivittämisesksi.
Nyt olin tianteessa jossa pääsin varsinaisen tehtävän pariin eli salt minionin asennukseen.
Tähän mennessä curl komennon puutetta huomioon ottamatta en ole törmännyt ongelmiin.

Seuraavaksi varmistin ettei debian asenna vahingossa uusia päivityksiä salttiin 
Tämä on tärkeää, sillä saltin master/minion versioiden yhteensopivuus on kriittistä
Uudempaan versioon päivittäminen voi vahingossa rikkoa salt ympäristön

Tämän toteutin ajamalla ohjeen mukaisesti komennot:

echo 'Package: salt-*

Pin: version 3006.*

Pin-Priority: 1001' | sudo tee /etc/apt/preferences.d/salt-pin-1001

Valmistelujen jälkeen pääsin viimein asentamaan salt minionin komennolla:
sudo apt-get install salt-minion

Lopuksi tarkistin että salt minion on asennettu tarkistamalla sen tilan komennolla:

systemctl status salt-minion

tulosten kertoi että palvelu oli active

Tämän tehtävän tekemiseen ei mennyt kauan sillä ohjeet olivat selkeät, enkä törmännyt isompiin ongelmiin


c) 
1. pkg käytetään pakettien hallintaan
   pkg avulla salt tarkistaa onko paketti jo asennettu ja jos ei ole, se asennetaan
   pakettien hallinta on tärkeää järjestelmän automaatiossa

   Testasin pkg näin:
   
   Loin sls tiedoston -
   sudo nano /srv/salt/pkg_htop.sls

   Lisäsin sisällön -

   install_htop:

pkg.installed:
    
    - name: htop
  
   Ajoin Salt staten -
   sudo salt-call state.apply pkg_htop

   Tatkistin asennuksen -
   
   dpkg -l | grep htop

   2. file käytetään tiedostojen hallintaan
      Salt varmistaa, että tiedosto on olemassa ja sisältö vastaa määrittelyä
      Jos tiedostoa ei ole se luodaan ja jos sen sisältö on väärä se korjataan

      Testasin fileä näin:

      sudo nano /srv/salt/file_esimerkki.sls

      Tiedoston sisältö:

   
   /tmp/moi.txt:
  
  file.managed:
    
    - contents: "Moi kaikki!"
    
    - mode: '0644'
   
    - owner: root
    
    - group: root
  
      sudo salt-call state.apply file_esimerkki

      Tarkistin tuloksen:
      
      cat /tmp/moi.txt
      
      ls -l /tmp/moi.txt

      3. service käytetään palveluiden hallintaan
         salt tarkistaa onko palvelu käynnissä
         jos ei ole palvelu käynnistetään

         Testasin service näin:

         sudo nano /srv/salt/service_palvelu.sls

         Tiedoston sisältö:

         ssh_service:
         
         service.running:
         
         - name: ssh
         
         - enable: true
        
           Salt staten ajo:
        
         sudo salt-call state.apply service_palvelu

         Palvelun tarkistus:

         systemctl status ssh


      4. user käytetään käyttäjien hallintaan
         Salt varmistaa, että käyttäjä on olemassa ja että asetukset ovat oikein
         Jos käyttäjää ei ole se luodaan

         Testasin user näin:

         sudo nano /srv/salt/user_käyttäjä1.sls

         Tiedoston sisältö:
      
      devuser_account:
      
      user.present:
   
    - name: devuser
    
    - uid: 1500
    
    - groups:
      - sudo
    
    - shell: /bin/bash
  
      Salt state ajo:
     
      sudo salt-call state.apply user_käyttäjä1

      Käyttäjän tarkistus:
      id devuser

      5. cmd käytetään komentojen suorittamiseen
         Salt suorittaa komennon vain, jos ehto ei ole tosi
         Hyvä tapa tehdä pieniä mukautettuja tehtäviä, joita ei ole valmiina moduuleina

         Testasin cmd näin:

         sudo nano /srv/salt/cmd_komento.sls

         Tiedoston sisältö:
        
         create_demo_dir:
        
         cmd.run:
        
         - name: mkdir -p /opt/demo
        
         - unless: test -d /opt/demo
        
         Salt staten ajo:
         
         sudo salt-call state.apply cmd_komento

         Tarkistin tuloksen:
         
         ls -ld /opt/demo


      d) Idempotensilla tarkoitetaan, että operaation toistaminen ei muuta lopputulosta

      Esimerkki:
      Loin 2 uutta tiedostoa
      Ensimmäinen on /srv/salt/idempotent_pkg.sls jonka sisältö on:
     
      install_tree:
      
       pkg.installed:
       
       - name: tree

     Toinen on /srv/salt/idempotent_file.sls jonka sisältö on:
     
     /tmp/mitä.txt:
     
     file.managed:
    
    - contents: "Mitä kuuluu"
    
    - mode: '0644'
    
    - owner: root
    
    - group: root
  
      Kun olin luonut tiedostot ajoin seuraavat komennot:

     sudo salt-call --local state.apply idempotent_pkg
     
     sudo salt-call --local state.apply idempotent_file

     Ensimmäisellä ajokerralla:

   install_tree → changes: {'tree': 'installed'}

   file /tmp/mitä.txt → changes: {'diff': 'Mitä kuuluu\n'}

     Toinen ajokerta:

    install_htop → changes: {}

   file /tmp/terve.txt → changes: {}

    Koska tree on jo asennettu ja tiedoston sisältö vastaa määrittelyä, Salt ei tee mitään.
    Myös file tiedoston sisältö vastasi määrittelyä joten muutoksia ei tapahtunut.
    Näin näen että idempotenssi toimii sillä toisella ajokerralla muutoksia ei tehty.

   ## References https://terokarvinen.com/2025/palvelinten-hallinta--ici001as3a-3008--2025/
      
   
   
  
