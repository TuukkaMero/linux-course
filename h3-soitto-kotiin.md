# h3 - Soitto kotiin

x) Muistiinpanot

## Two Machine Virtual Network With Debian 11 Bullseye and Vagrant:

- Tarkoitus: Käynnistää nopeasti kahden Debian 11 -virtuaalikoneen verkko Vagrantilla ja VirtualBoxilla.

Vagrantin edut:

- Automaattinen VirtualBox-koneiden luonti

- Automaattinen SSH-yhteys

- Ei tarvetta graafiselle käyttöliittymälle

Debian asennus:

sudo apt-get update

sudo apt-get install vagrant virtualbox

## Salt Quickstart – Salt Stack Master and Slave on Ubuntu Linux :

- Tarkoitus: Hallita useita tietokoneita keskitetysti Saltin avulla.

Arkkitehtuuri:

- Master: keskitetty hallintapalvelin (julkinen IP).

- Slave / Minion: hallittava kone, voi olla missä tahansa (myös NATin tai palomuurin takana).

  Master asennus:

sudo apt-get update

sudo apt-get -y install salt-master

hostname -I

 Slave asennus:

sudo apt-get update

sudo apt-get -y install salt-minion

sudoedit /etc/salt/minion

## Infra as Code - Your wishes as a text file

Tarkoitus:

- Määritellään haluttu järjestelmän tila tekstitiedostona (Infrastructure as Code).

- Salt toteuttaa sen automaattisesti.

Tiedoston luominen ja muokkaus:

sudo mkdir -p /srv/salt/hello

sudoedit /srv/salt/hello/init.sls

## top.sls - What Slave Runs What States

Tarkoitus:

- top.sls kertoo, mitkä Salt State -tiedostot ajetaan millä orjilla.

- Toimii “karttana” masterin ja orjien välillä.

## a)

  Aloitin asentamalla vagrantin komennoilla:  Master: keskitetty hallintapalvelin (julkinen IP).

sudo apt update

sudo apt install vagrant

Asennuksessa ei ollut ongelmia

Asennuksen tarkistus:

![VirtualBox asennus](https://raw.githubusercontent.com/TuukkaMero/linux-course/main/Vbox%20versio.png)

![Vagrant asunnus](https://github.com/TuukkaMero/linux-course/blob/main/Vagrant%20asennus.png)

## b) 

Tein uuden linux viertuaalikoneen seuraavalla tavalla:

![1kone](https://github.com/TuukkaMero/linux-course/blob/main/1%20Koneen%20asennus.png)

Koneen sisältö:

![sisältö](https://github.com/TuukkaMero/linux-course/blob/main/1%20koneen%20sis%C3%A4lt%C3%B6.png)

## c)

Aloitin luomalla virtuaalikoneet:

![2konetta](https://github.com/TuukkaMero/linux-course/blob/main/2%20koneen%20luonti.png)

Tiedoston sisältö:

![sisältö](https://github.com/TuukkaMero/linux-course/blob/main/2%20koneen%20sis%C3%A4lt%C3%B6.png)

Kun annoin komennon vagrant up sain seuraavan virheilmoituksen:

![virhe](https://github.com/TuukkaMero/linux-course/blob/main/vagrant%20up%20virhe.png)

Ilmeisesti virtuaalikoneeni ei tukenut virtualisointia joten asensin vagrantin isäntäkoneeni windowsille

PowerShellillä vagrant up komento toimi välittömästi

Testasin pingata koneet toisillaan:

![1ping](https://github.com/TuukkaMero/linux-course/blob/main/t001%20ping.png)

![2ping](https://github.com/TuukkaMero/linux-course/blob/main/t002%20ping.png)

## d)

Aloitin toisella koneellani näin:

![luonti](https://github.com/TuukkaMero/linux-course/blob/main/h3%20minion%20kone.png)

Tiedostoon kirjoitin vain master 192.168.56.101 joka on master koneeni ip osoite

Seuraavaksi palasin master koneeseen hyväksymään minionin avaimen:

![avain](https://github.com/TuukkaMero/linux-course/blob/main/h3%20minionin%20avaimen%20hyv%C3%A4ksynt%C3%A4.png)

Viimeiseksi kokeilin pingata slave konetta masterilla joka toimi näin:

![ping](https://github.com/TuukkaMero/linux-course/blob/main/h3%20orjan%20pingaus.png)

## e)

Tässä tehtävässä käytin esimerkki tiloinani pkg, service ja file

Masterille luomani tiedosto:

![master](https://github.com/TuukkaMero/linux-course/blob/main/h3%20master%20sis%C3%A4lt%C3%B6.png)

Seuraavaksi yritin ajaa tilan verkon yli

Kokeilin ajamista n. tunnin ajan mutta sain jatkuvasti virheen joka näytti tältä:

![virhe](https://github.com/TuukkaMero/linux-course/blob/main/h3%20testi%20virhe.png)

Juuri aikaisemmin minionin pingaus masterilta oli kuitenkin onnistunut joten en nähnyt mitään syytä miksi se ei tomisi nyt

Lopetin olettaen että ratkaisuni olisi pitänyt toimia mutta ongelma oli tällä kertaa itse vagrantissa

## Lähteet

https://terokarvinen.com/install-salt-on-debian-13-trixie/

https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/

https://terokarvinen.com/2018/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/?fromSearch=salt%20quickstart%20salt%20stack%20master%20and%20slave%20on%20ubuntu%20linux

https://terokarvinen.com/2023/salt-vagrant/#infra-as-code---your-wishes-as-a-text-file











