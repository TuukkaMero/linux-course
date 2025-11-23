# h5 

## 1.3 getting started - What is git?

Snapshottien idea

- Jokainen commit = kuva kaikista projektin tiedostoista juuri sillä hetkellä.

- Jos tiedosto ei muutu → Git ei tallenna uudelleen, vain linkki aiempaan versioon.

- Toisin sanoen: Git = pieni, nopea tiedostojärjestelmä + työkaluja sen päälle.



Gitin eheys: SHA-1

- Jokainen objekti (tiedosto, kommitti, puu) saa SHA-1–tarkistussumman.

- Esim.: 24b9da6552252987aa493b52f8696cd6d3b00373

- Git viittaa kaikkeen hashin perusteella, ei tiedostonimellä.

- Mahdotonta muuttaa dataa niin, ettei Git havaitse.


Gitin kolme tilaa

- Modified – tiedosto muuttunut, ei vielä staged.

- Staged – versio lisätty indexiin (menee seuraavaan commitissa).

- Committed – tallessa Gitin tietokannassa.


Git-projektin kolme aluetta

- Working Tree

Fyysiset tiedostot koneellasi.

- Staging Area (Index)

Valinta siitä, mitä muutoksia menee seuraavaan commitissa.

- Git Directory (.git)

Kaikki historian ja metadatan sisältävä Git-tietokanta.


## Varaston terokarvinen/suolax/ historia

![historia](https://github.com/TuukkaMero/linux-course/blob/main/karvinen%20commits.png)


## a) 

Aloitin luomalla GitHUbiin uuden varaston nimeltä Snowrepo ja lisäsin sinne tiedostot README.md ja GNU General Public License 3

![snowrepo](https://github.com/TuukkaMero/linux-course/blob/main/snowrepo.png)

## b)

Tässä tehtävässä tein seuraavat toimenpiteet:

![gitaddcommit](https://github.com/TuukkaMero/linux-course/blob/main/git%20addcommit.png)

git push komennon jälkeen snow.txt tiedosto näkyi weppiliittymässä:

![weppi](https://github.com/TuukkaMero/linux-course/blob/main/repossan%C3%A4kyy.png)

## c)

Tein tyhmän muutoksen gittiin seuraavalla tavalla: 

![dumbchange](https://github.com/TuukkaMero/linux-course/blob/main/git%20huonomuutos.png)

poistin tyhmän muutoksen seuraavalla tavalla:

![gitreset](https://github.com/TuukkaMero/linux-course/blob/main/gitreset.png)

## d) 

Nimeni ja sähköpostiosoitteeni näkyy haluamallani tavalla kuten aikaisemmissa harjoituksissa näkyy

Jos kuitenkin haluaisin muokata sähköpostia ja nimeä tekisin sen komennoilla:

git config --global user.name "Oikea Nimi"

git config --global user.email "sahkoposti@esimerkki.com"

Logissani näkyy tällä hetkellä luomani snow.txt tiedosto ja initial commit joka syntyi repon luonnin yhteydessä

![snowlog](https://github.com/TuukkaMero/linux-course/blob/main/snowlog.png)

## e)

Aloitin luomalla git varastooni hakemiston salt ja sinne loin apache.sls tiedoston joka asentaa apachen 

Muutokset weppiliittymässä:

![weppisalt](https://github.com/TuukkaMero/linux-course/blob/main/weppisalt.png)

Lisäsin uudet tiedostot gitin seurantaan:

![gitaddsalt](https://github.com/TuukkaMero/linux-course/blob/main/gitreposaltadd.png)

![gitlogsalt](https://github.com/TuukkaMero/linux-course/blob/main/gitlogsalt.png)

Viimeiseksi ajoin salt tilani git varastosta:

![gitsaltcall](https://github.com/TuukkaMero/linux-course/blob/main/gitsaltcall.png)

kaikki toimi odotetusti ja apache asentui


## Lähteet 

https://git-scm.com/book/en/v2/Getting-Started-What-is-Git%3F

https://terokarvinen.com/palvelinten-hallinta/#h5-toimiva-versio

