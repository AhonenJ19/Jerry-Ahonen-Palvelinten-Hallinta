#  H-3 Soitto kotiin

Tämä harjoitus on tehty sunnuntaina 09/11/2025

Käytetty työasema

Laitteen nimi: LAPTOP-7I2PFE0L

Käyttöjärjestelmä Windows 11

Prosessori: Intel(R) Core(TM) i5-8250U CPU @ 1.60GHz (1.80 GHz)

## Tiivistelmät

- Vagrantin hyödyt: luo virtuaalikoneet automaattisesti, ssh:lla kirjautiminen automaattista, ei tarvita graafista käyttöliittymää
- Uuden hakemiston luominen sekä Vagranfile $ mkdir twohost/; cd twohost/
$ nano Vagrantfile
- Helposti kirjautuminen sisään: $ vagrant ssh t001
- Tuhoaminen myös todella helppoa ja nopee: $ vagrant destroy
- Orjat (minionit) ovat hallittavia koneita ja niitä voi hallita niiden sijainnista riippumatta (palomuurin takana, tuntematon osoite)
- Vain master palvelimella oltava julkinen osoite ja IP-osoite tiedettevä
- Infraa koodina tekstitiedoston luominen esim hello/init.sls, ja tilan ajaminen $ sudo salt '*' state.apply hello
- Top.sls tilan ajaminen (ei tarvitse enää nimetä moduuleja, vaan: $ sudo salt '*' state.apply


## a.) Hello Vagrant

<img width="590" height="74" alt="Screenshot 2025-11-09 133412" src="https://github.com/user-attachments/assets/876b4d26-e06e-43ad-8976-200736cc89e7" />

## b,c) Kahden koneen verkko

Luodaan uusi hakemisto sekä Vagrantfile, johon annettu sisältö


$ mkdir twohost/; cd twohost/
$ nano Vagrantfile

<img width="569" height="542" alt="image" src="https://github.com/user-attachments/assets/c98b435d-9da3-47bd-a711-a6db4036b204" />

Nyt kun kaksi konetta t001 ja t002 luotu testataan, että yhteys niiden välillä toimii pingin avulla

<img width="855" height="199" alt="Screenshot 2025-11-09 141645" src="https://github.com/user-attachments/assets/d1141bc8-de22-4c64-b86a-58ce96aae6b2" />

<img width="443" height="59" alt="image" src="https://github.com/user-attachments/assets/7d4f0e1c-ae5e-4aa8-b5df-571b1c621577" />

## d.) Herra-orja

Asennetaan koneelle t001 salt-master ja t002 koneelle salt-minion

t001$ sudo apt-get update

t001$ sudo apt-get -y install salt-master

t002$ sudo apt-get update

t002$ sudo apt-get -y install salt-minion

Kerrotaan masterin osoite minionille ja käynnistetään minion uudestaan

Tiedostoon lisättiin 

master: 192.168.88.101

<img width="366" height="35" alt="image" src="https://github.com/user-attachments/assets/24b1c042-5859-4ded-9292-769d5da73dbb" />

Siirrytään takaisin masterille ja hyväksytään avain 

<img width="339" height="74" alt="image" src="https://github.com/user-attachments/assets/c6ccd6f3-ea6b-489f-8350-45609dacb122" />


Varmistutaan vielä, että minion reagoi

<img width="380" height="59" alt="image" src="https://github.com/user-attachments/assets/045ed613-994c-4e47-a87a-1438640adc10" />


## e.) Kaksi tilaa verkon yli

<img width="359" height="320" alt="image" src="https://github.com/user-attachments/assets/2c9635cf-0ec4-4125-8121-26b903d05e60" />

Sitten vielä toinen

t002$sudo nano /srv/salt/apache.sls

Lisätään:

apache2:
  pkg.installed: []


t002$ sudo salt '*' state.apply apache


<img width="299" height="104" alt="image" src="https://github.com/user-attachments/assets/f4942561-fcc7-4e1f-9805-e1fdb418c991" />


## Lähteet

Karvinen, Tero: Oppitunti 03/11/2025, Palvelinten hallinta -kurssi

Karvinen, Tero: Two Machine Virtual Network With Debian 11 Bullseye and Vagrant https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/

Karvinen, Tero: Salt Quickstart https://terokarvinen.com/2018/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/?fromSearch=salt%20quickstart%20salt%20stack%20master%20and%20slave%20on%20ubuntu%20linux

Karvinen, Tero: Salt Vagrant - automatically provision one master and two slaves https://terokarvinen.com/2023/salt-vagrant/#infra-as-code---your-wishes-as-a-text-file

Karvinen, Tero: Raportin kirjoittaminen, saatavilla: https://terokarvinen.com/2012/08/13/2006/raportin-kirjoittaminen-4#viittaa_lahteisiin
