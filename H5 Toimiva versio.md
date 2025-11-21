# H5 Toimiva versio

Tämä harjoitus on tehty perjantaina 21/11/2025

Käytetty työasema

Laitteen nimi: LAPTOP-7I2PFE0L

Käyttöjärjestelmä Windows 11

Prosessori: Intel(R) Core(TM) i5-8250U CPU @ 1.60GHz (1.80 GHz)


## Tiivistelmät

- Gitissä saat täydellisen kopion historiasta, et vain viimeisintä versiota
- Git ei tallenna pelkkiä muutoksia, vaan kokonaisia "snapshotteja" eli tilannekuvia, jossa näkyy koko tiedoston historia
- Voit työskennellä offline-tilassa
- Git add lisää kaikki muutetut tiedostot staging-alueelle
- Git commit tallentaa muutokset paikalliseen repositioon "snapshotina"
- Git pull hakee ja yhdistää muutokset etävarastosta paikalliseen varastoon
- Git push lähettää paikalliset muutokset etävarastoon (synkronointi)

## a,b) Online,Dolly

Asennetaan ensin git koneelle, luodaan Githubiin uusi repo nimeltä Snow-project

```sudo apt install git-all```

Lisätään sinne myös README.md sekä GNU General Public License 3

Kloonataan repositio koneelle, tehdään muutos tiedostoon ja pusketaan sen palvelimelle

```git clone https://github.com/AhonenJ19/Snow-project.git```

```echo "Uusi projekti, Sataisipa enemmän LUNTA" >> README.md```

```git add README.md```

```git commit -m```

```git push```

<img width="662" height="255" alt="image" src="https://github.com/user-attachments/assets/8e9351f4-be66-47a8-9305-077a079f3fc6" />


## c.) Doh!

Tehdään muutos tiedostoon, mutta ei committia

<img width="721" height="217" alt="image" src="https://github.com/user-attachments/assets/d53be49c-2ab7-4660-bcec-1e4d75b43ced" />


<img width="717" height="57" alt="image" src="https://github.com/user-attachments/assets/d11f8cc2-6334-4b72-bbd3-d6b56ad2e92d" />


## d.) Tukki

Tarkastellaan lokia

```git log --color | less -R```

<img width="585" height="222" alt="image" src="https://github.com/user-attachments/assets/24476514-f03e-45d0-9d61-5c4044beb257" />

Korjataan käyttäjänimi

```git config --global user.name "Ahonen Jerry"```

<img width="393" height="35" alt="image" src="https://github.com/user-attachments/assets/2c8e9c94-587f-45df-8ee8-168d7622417c" />


## e.) Suolattu rakki


<img width="903" height="527" alt="image" src="https://github.com/user-attachments/assets/5c1eaf05-d1dd-4500-827d-f229f4fe3d44" />

Lisätään Git hubiin salt hakemisto

<img width="833" height="137" alt="image" src="https://github.com/user-attachments/assets/32db26fa-e57a-400e-975a-5be1db036c7e" />

<img width="907" height="243" alt="image" src="https://github.com/user-attachments/assets/f8406c57-dc40-478e-abf2-26ff609fe244" />


## Lähteet

Karvinen, Tero: Oppitunti 17/11/2025, Palvelinten hallinta -kurssi



Karvinen, Tero: Salt Quickstart https://terokarvinen.com/2018/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/?fromSearch=salt%20quickstart%20salt%20stack%20master%20and%20slave%20on%20ubuntu%20linux

Chacon, S., & Straub, B. (2014). Pro Git (2nd ed.). Apress. Saatavilla: https://git-scm.com/book/en/v2

Git-scm.com. (2025). Git Documentation. Saatavilla: https://git-scm.com/docs

Karvinen, Tero. (2025). Suolax GitHub-repositorio. Saatavilla: https://github.com/terokarvinen/suolax

Karvinen, Tero: Raportin kirjoittaminen, saatavilla: https://terokarvinen.com/2012/08/13/2006/raportin-kirjoittaminen-4#viittaa_lahteisiin


