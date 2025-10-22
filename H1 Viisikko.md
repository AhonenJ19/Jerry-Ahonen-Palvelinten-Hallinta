#  H1-Viisikko
## Tiivistelmät
- Saltin avulla määritellään esim palvelimen asetukset koodina (IaC)
- Uusi apt arkisto koostuu 2 tiedostosta, PGP-puplic key sekä sources.list
- PGP  varmentaa ladatut ohjelmistot aidoiksi
- Sources.list sisältää url-osoitteen, josta ohjelmisto/paketit ladataan
- Saltin avulla voidaan ajaa komentoja paikallisesti
- Hyötyjä ovat nopea asetusten tarkastus
- Sopii harjoitteluun ja testaukseen
- Pystytään kontrolloimaan lukuisia koneita
- Ainostaan master-palvelimen pitää olla julkisesti saavutettavissa
- Orja-koneet voivat sijaita missä vain: natin tai palomuurin takana, tuntemattomassa osoitteessa
- Onnistuneen raportin pohjalta toinen opiskelija pääsee samaan lopputulokseen
- Täsmällisyys/tarkkuus tärkeää, mikä koneen malli, sijainti, aika yms
- Kerrottava kaikki työvaiheet ja mitä komentoja käytettiin
- Ongelmatilanteiden selittämin/ratkominen myös tärkeää

## b.) Saltin asennus klo (18:10-18:40)
Ensimmäinen tehtävävaihe oli Saltin asennus, jota ennen haetaan uusimmat päivitykset sekä asennetaan wget tiedostojen lataamista varten

$ sudo apt-get update
$ sudo apt-get install wget

Luodaan uusi hakemisto SALTREPO

$ mkdir saltrepo/
$ cd saltrepo/

Ladataan hakemistoon kaksi tiedostoa: pgp-key ja salt.sources

$ wget https://packages.broadcom.com/artifactory/api/security/keypair/SaltProjectKey/public
$ wget https://github.com/saltstack/salt-install-guide/releases/latest/download/salt.sources

Varmistetaan, että kyseessä tosiaan on oikea avain, sen sijainti ja url-osoite

$ less public
$ less salt.sources
$ gpg --show-key --with-fingerprint public

Kun varmistuttu, että avainti on oikea, kopioidaan se luotettujen avainten hakemistoon, jonka jälkeen lisätään saltin asennusta varten oleva osoite hakemistoon

$ sudo cp public /etc/apt/keyrings/salt-archive-keyring.pgp
$ sudo cp salt.sources /etc/apt/sources.list.d/

Nyt kun olemme onnistuneesti kertoneet koneelle, että mistä salt voidaan ladata, sekä varmistuttu siitä, että ladatut paketit ovat aitoja, voimme asentaa Saltin
Ensin vielä päivitys

$ sudo apt-get update
$ sudo apt-get install salt-minion salt-master

Onnistuneen asennuksen jälkeen testaus:

$ salt --version
Tulostaa: salt 3007.8 (Chlorine)

Kokeillaan salt komentoa: luodaan tiedosto /tmp/hellojerry, file.managed avulla
$ sudo salt-call --local state.single file.managed /tmp/hellojerry

Onnistunut tulos:
<img width="496" height="370" alt="image" src="https://github.com/user-attachments/assets/188590e8-bc8e-40b0-9d6e-f42e5d687e6b" />

Varmistetaan vielä, että äsken luotu tiedosto on olemassa
$ ls /tmp/hellojerry

## c.) 5 tärkeintä funktiota 18:40-19:10
### pkg.installed/removed
Pakettien asentaminen/poistaminen (tree)

$ sudo salt-call --local -l info state.single pkg.installed tree
$ sudo salt-call --local -l info state.single pkg.removed tree

### File.managed/absent
Tiedostojen luominen,muokkaaminen ja poistaminen

Luo tiedoston:$ sudo salt-call --local -l info state.single file.managed /tmp/hellojerry

Luo ja lisää sisältöä $ sudo salt-call --local -l info state.single file.managed /tmp/moijerry contents="juu"

Poistaa: $ sudo salt-call --local -l info state.single file.absent /tmp/hellojerry

### Service.running/dead
Palveluiden hallinta (apache2)

Palvelun automatisoitu käynnistyminen: $ sudo salt-call --local -l info state.single service.running apache2 enable=True

Palvelun pysäyttäminen/automaattisen käynnitysksen esto : $ sudo salt-call --local -l info state.single service.dead apache2 enable=False

### User.present/absent
Käyttäjien hallinta

Luo käyttäjän jerry19: $ sudo salt-call --local -l info state.single user.present jerry19

Poistaa sen : $ sudo salt-call --local -l info state.single user.absent jerry19

### Cmd.run
Komentojen suoritus

Luo tiedoston /tmp/juu VAIN jos sitä ei ole olemassa

$ sudo salt-call --local -l info state.single cmd.run 'touch /tmp/juu' creates="/tmp/juu"

## c.) Idempotenssi 19:10-30
Esimerkki: 
$sudo salt-call --local -l info state.single cmd.run 'touch /tmp/juu' creates="/tmp/juu"

Komento luo tiedoston /tmp/juu JOS sitä ei ole olemassa

creates="/tmp/juu" tekee tästä komennosta idempotentin, eli jos tiedosto on jo olemassa, komentoa ei suoriteta, eikä muutoksia tehdä

Ensimmäisen kerran tulos:

<img width="459" height="457" alt="image" src="https://github.com/user-attachments/assets/6cb51243-83bd-49d8-9318-cfeece620cdd" />

Kun tiedosto jo löytyy ja komento suoritetaan uudestaan on tulos tämä:

<img width="184" height="80" alt="image" src="https://github.com/user-attachments/assets/fd6f176c-7270-4816-94e2-4a9b9e29f85e" />


## Lähteet
Karvinen, Tero: Oppitunti 20/10/2025, Palvelinten hallinta -kurssi

Karvinen, Tero: Palvelinten hallinta, saatavilla: https://terokarvinen.com/palvelinten-hallinta/

Karvinen, Tero: Run Salt Command Locally, saatavilla: https://terokarvinen.com/2021/salt-run-command-locally/

Karvinen, Tero: Install Salt on Debian 13 Trixie, saatavilla: https://terokarvinen.com/install-salt-on-debian-13-trixie/

Karvinen, Tero: Salt Quickstart – Salt Stack Master and Slave on Ubuntu Linux, saatavilla: https://terokarvinen.com/2018/03/28/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/

Karvinen, Tero: Raportin kirjoittaminen, saatavilla: https://terokarvinen.com/2012/08/13/2006/raportin-kirjoittaminen-4#viittaa_lahteisiin



