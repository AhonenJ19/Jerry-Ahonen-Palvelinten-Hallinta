# H5 Toimiva versio

Tämä harjoitus on tehty perjantaina 21/11/2025

Käytetty työasema

Laitteen nimi: LAPTOP-7I2PFE0L

Käyttöjärjestelmä Windows 11

Prosessori: Intel(R) Core(TM) i5-8250U CPU @ 1.60GHz (1.80 GHz)


## Tiivistelmät

- Gitissä saat täydellisen kopion historiasta, et vain viimeisintä versiota
- Git ei tallenna pelkkiä muutoksia, vaan kokonaisia "snapshotteja" eli tilannekuvia, jossa näkyy koko tiedoston historia
- Voi työskennellä offline-tilassa
- Git add lisää kaikki muutetut tiedostot staging-alueelle
- Git commit tallentaa muutokset paikalliseen repositioon "snapshotina"
- Git pull hakee ja yhdistää muutokset etävarastosta paikalliseen varastoon
- Git push lähettää paikkalliset muutokset etävarastoon (synkronointi)

## a.) Online

Asennetaan ensin git koneelle, luodaan Githubiin uusi repo nimeltä Snow-project

```sudo apt install git-all```

Lisätään sinne myös README.md sekä GNU General Public License 3

Kloonataan repositio koneelle, tehdään muutos tiedostoon ja pusketaan sen palvelimelle

```git clone https://github.com/AhonenJ19/Snow-project.git```

```echo "Uusi projekti, Sataisipa enemmän LUNTA" >> README.md```

```git add README.md```

```git commit -m```

```git push```











## Lähteet

Karvinen, Tero: Oppitunti 10/11/2025, Palvelinten hallinta -kurssi

Karvinen, Tero: Pkg-File-Service https://terokarvinen.com/2018/04/03/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/?fromSearch=karvinen%20salt%20ssh

Karvinen, Tero: Salt Quickstart https://terokarvinen.com/2018/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/?fromSearch=salt%20quickstart%20salt%20stack%20master%20and%20slave%20on%20ubuntu%20linux

Karvinen, Tero: Salt Vagrant - automatically provision one master and two slaves https://terokarvinen.com/2023/salt-vagrant/#infra-as-code---your-wishes-as-a-text-file

Karvinen, Tero: Raportin kirjoittaminen, saatavilla: https://terokarvinen.com/2012/08/13/2006/raportin-kirjoittaminen-4#viittaa_lahteisiin

Tehtävässä käytetty Microsoft Copilotia (2025) Virhetilanteiden ratkaisemista varten.
