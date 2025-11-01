#  H2 Infraa koodina

Tämä harjoitus on tehty launtaina 01/11/2025

Käytetty työasema

Laitteen nimi: LAPTOP-7I2PFE0L

Käyttöjärjestelmä Windows 11

Prosessori: Intel(R) Core(TM) i5-8250U CPU @ 1.60GHz (1.80 GHz)

## Tiivistelmät

- Uuden tilan luominen, varmistetaan, että tietty tekstitiedosto on olemassa
- $ sudoedit init.sls
- Tiedostoon idempotenttia saltin omalla kielellä
  - /tmp/hellojerry:
  file.managed
 - YAML renderöijän tehtävänä on muuntaa YAML tietorakenne siten, että salt pystyy sitä käyttämään (Python)
  - YAML koostuu kolmesta peruselementistä: Sklaarit, Listat ja Sanakirjat
  - Sisennys olennaista, se määrittelee kontekstin
  - Top tiedosto määrittää, mitkä asetukset tai tilat koskevat mitäkin konetta
  - Top tiedostossa on kolme pääkomponenttia: Ympäristö, Kohde ja Tilatiedostot
  - Hierarkia: Ympäristöt sisältävät Kohteita ja Kohteet sisältävät Tiloja

    ## a.) Hei infrakoodi!  12:30-12:40

    Luodaan ensin kansio, jonka jälkeen init.sls tiedosto

    $ sudo mkdir -p /srv/salt/hello/

    $ sudoedit init.sls

    Kirjoitetaan idemptotentti koodina, luomaan esimerkki tiedosto, jonka jälkeen ajetaan se

    /tmp/hellojerry:
  file.managed

      $ sudo salt-call --local state.apply hello

    Ënsimmäisen kerran tulos
    <img width="592" height="377" alt="Screenshot 2025-11-01 124721" src="https://github.com/user-attachments/assets/731e73d1-d6d2-483d-992f-0ca8ac37f1a9" />

Toisella kerralla ei enää muutoksia
<img width="920" height="333" alt="Screenshot 2025-11-01 124752" src="https://github.com/user-attachments/assets/9cba9310-189c-4312-9d49-bf7a6be361ab" />


## b.) Top-file 12:40-13:00

Luodaan tarvittavat tiedostot ja sisällöt Top-fileä varten

$ sudo mkdir -p /srv/salt/hellopkg

$ sudoedit /srv/salt/hellopkg/init.sls

micro:
  pkg.installed

$ sudo mkdir -p /srv/salt/helloservice

$ sudoedit /srv/salt/helloservice/init.sls

apache2:
  service.running:
    - enable: True

$ sudo mkdir -p /srv/salt/hellouser

$ sudoedit /srv/salt/hellouser/init.sls

jerrytst:
  user.present:
    - shell: /bin/bash


Sen jälkeen luodaan top.sls tiedosto, lisätään sisältö ja ajetaan komento

$ sudoedit /srv/salt/top.sls

base:
  '*':
    - hellojerry
    - hellopkg
    - helloservice
    - hellouser

$ sudo salt-call --local state.apply


 Ensimmäisellä kerrall 2 muutosta, sillä kaikki annetut paketit ovat jo asennettuna, sekä esimerkkitiedosto on jo luotu


<img width="215" height="80" alt="image" src="https://github.com/user-attachments/assets/970a22f4-e39f-40ab-9377-186edebed0ab" />


## c.) Viisikko tiedostossa 13:00-13:15

Edellisessä kohdassa loimme jo kaikki tilat paitsi cmd, joten luodaan se vielä erikseen ja ajetaan kaikki komennot

$ sudo mkdir -p /srv/salt/hellocmd

$ sudoedit /srv/salt/hellocmd/init.sls

say_hello:
  cmd.run:
    - name: echo "Salt toimii hienosti!" > /tmp/saltcmd.txt

Nyt kun ajetaan kaikki komennot uudestaan vain yksi muutos, eli cmd

$ sudo salt-call --local state.apply hellopkg
$ sudo salt-call --local state.apply hellojerry
$ sudo salt-call --local state.apply helloservice
$ sudo salt-call --local state.apply hellouser
$ sudo salt-call --local state.apply hellocmd


<img width="762" height="433" alt="image" src="https://github.com/user-attachments/assets/69524b40-8fcd-46bd-9656-9e0555c6ffc0" />

## d.) Sls- tiedosto (pkg,service ja file) 13:15-13:35

Luodaan sls tiedosto, joka varmistaa, että apache2 on asennettu, käynnissä sekä ilmoittaa jos/kun ehdot toteutuvat

$ sudo mkdir -p /srv/salt/helloweb

$ sudoedit /srv/salt/helloweb/init.sls

apache2-package:
  pkg.installed:
    - name: apache2

apache2-service:
  service.running:
    - name: apache2
    - enable: True
    - require:
      - pkg: apache2-package

say_ok:
  cmd.wait:
    - name: echo "Kaikki kunnossa"
    - watch:
      - service: apache2-service


$ sudo salt-call --local state.apply helloweb

Eli tässä tulos, kun apache on otettu pois käytöstä


<img width="224" height="92" alt="image" src="https://github.com/user-attachments/assets/09c77cdf-1bad-4b61-9c44-d5729179fdc1" />

Ja tässä toinen ajo


<img width="472" height="139" alt="image" src="https://github.com/user-attachments/assets/f7ebb8b5-4f95-49ec-b0b7-0d9af5b9b2f4" />

## Lähteet 

Karvinen, Tero: Oppitunti 27/10/2025, Palvelinten hallinta -kurssi

Karvinen, Tero. Hello Salt Infra-as-Code. Saatavilla: https://terokarvinen.com/2024/hello-salt-infra-as-code/

Salt Project. Rules of YAML. In Salt User Guide. Saatavilla: https://docs.saltproject.io/salt/user-guide/en/latest/topics/overview.html#rules-of-yaml

Salt Project. The Top File. Saatavilla: https://docs.saltproject.io/en/latest/ref/states/top.html

Tehtävässä käytetty tekoälyä luomaan esimerkki tiedostoja "Keksi esimerkki sls tiedostosta, jossa vähintään kahta eri tilafunktiota"

Karvinen, Tero: Raportin kirjoittaminen, saatavilla: https://terokarvinen.com/2012/08/13/2006/raportin-kirjoittaminen-4#viittaa_lahteisiin

