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
