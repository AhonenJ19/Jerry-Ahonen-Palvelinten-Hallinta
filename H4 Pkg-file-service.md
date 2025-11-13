# H4 Pkg-file-service

Tämä harjoitus on tehty torstaina 13/11/2025

Käytetty työasema

Laitteen nimi: LAPTOP-7I2PFE0L

Käyttöjärjestelmä Windows 11

Prosessori: Intel(R) Core(TM) i5-8250U CPU @ 1.60GHz (1.80 GHz)


## Tiivistelmät

- Konfiguraationhallintajärjestelmän avulla varmistutaan siitä, että palvelimen asetukset pysyvät yhtenäisinä sekä helposti hallittavina
- Saltin avulla tämä voidaan tehdä lukuisille palvelimille yhdellä kertaa
- Service-watch varmistaa, että palvelu käynnistyy uudelleen aina kun konfiguraatio muuttuu, eli vanhoja asetuksia ei jää päälle
- Jos paketin asetuksia muutetaan tai se poistuu kokonaan, Saltilla ajettu tila palauttaa sen automaattisesti takaisin toimivaksi

# a.) SSHouto

Siirrytään masterkoneelle t001 ja luodaan tila sshd.sls

```vagrant shh t001```

```sudo nano /srv/salt/sshd.sls```


  Liitetään tiedostoon annettu sisältö: 


```
 openssh-server:
 pkg.installed
/etc/ssh/sshd_config:
 file.managed:
   - source: salt://sshd_config
sshd:
 service.running:
   - watch:
     - file: /etc/ssh/sshd_config
```

     
Luodaan sshd_config ja liitetään annettu sisältö + avataan portit 22 ja 1234:

```sudo nano /srv/salt/sshd_config```

```
Port 22
Port 1234

Protocol 2
HostKey /etc/ssh/ssh_host_rsa_key
HostKey /etc/ssh/ssh_host_dsa_key
HostKey /etc/ssh/ssh_host_ecdsa_key
HostKey /etc/ssh/ssh_host_ed25519_key
UsePrivilegeSeparation yes
KeyRegenerationInterval 3600
ServerKeyBits 1024
SyslogFacility AUTH
LogLevel INFO
LoginGraceTime 120
PermitRootLogin prohibit-password
StrictModes yes
RSAAuthentication yes
PubkeyAuthentication yes
IgnoreRhosts yes
RhostsRSAAuthentication no
HostbasedAuthentication no
PermitEmptyPasswords no
ChallengeResponseAuthentication no
X11Forwarding yes
X11DisplayOffset 10
PrintMotd no
PrintLastLog yes
TCPKeepAlive yes
AcceptEnv LANG LC_*
Subsystem sftp /usr/lib/openssh/sftp-server
UsePAM yes
 ```

Siirrytään minionille t002 ja lisätään portti 1234 tiedostoon

``` sudo nano /etc/ssh/sshd_config```

<img width="221" height="48" alt="image" src="https://github.com/user-attachments/assets/ba71ebbb-b531-4003-a861-a34c0db808df" />


Käynnistetään palvelu uudelleen ja tarkistetaan porttien tilanne

```sudo systemctl restart sshd```

<img width="338" height="108" alt="image" src="https://github.com/user-attachments/assets/5c1efec9-ec8d-499a-9e18-3640a71d44da" />

Siirrytään takaisin masterille ja ajetaan tila

<img width="436" height="410" alt="image" src="https://github.com/user-attachments/assets/bb82e081-2116-4e50-985d-5e1cc4e2e72a" />

1/3 onnistui, ilmoittaa, että Salt ei masterilla löydä sshd_config tiedostoa

Polku pitäisi olla oikein, joten muutin oikeudet tiedostolle seuraavasti:

```sudo chown root:root /srv/salt/sshd_config```

```sudo chmod 644 /srv/salt/sshd_config```

Tämän jälkeen saatiin onnistunut tulos:

<img width="332" height="223" alt="Screenshot 2025-11-13 184252" src="https://github.com/user-attachments/assets/c36ab6e3-1120-44ef-b84a-2c5a47dfe72b" />


Kokeillaan, että tila korjaa virheet, poistetaan ssh palvelin kokonaan ja kokeillaan ajaa tila

```sudo apt-get remove -y openssh-server```

<img width="445" height="419" alt="image" src="https://github.com/user-attachments/assets/4c9c8d3c-c6f4-4c55-9a93-669151265c9d" />


Outo virheilmoitus, tämän jälkeen minion kone t002 meni sekaisin ja se piti käynnistää uudelleen

Koneen kello oli väärässä ja se muutettiin takaisin oikeaksi

```sudo apt-get install -y ntpdate```

```sudo ntpdate pool.ntp.org```

Tämä onnistui ja saatiin taas yhteys koneelle

Ajettiin uudestaan masterilla tila

```sudo salt '*' state.apply sshd```

<img width="351" height="420" alt="image" src="https://github.com/user-attachments/assets/056ef026-abab-46b6-a1b6-1bda5cd009bb" />

## Lähteet

Karvinen, Tero: Oppitunti 10/11/2025, Palvelinten hallinta -kurssi

Karvinen, Tero: Pkg-File-Service https://terokarvinen.com/2018/04/03/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/?fromSearch=karvinen%20salt%20ssh

Karvinen, Tero: Salt Quickstart https://terokarvinen.com/2018/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/?fromSearch=salt%20quickstart%20salt%20stack%20master%20and%20slave%20on%20ubuntu%20linux

Karvinen, Tero: Salt Vagrant - automatically provision one master and two slaves https://terokarvinen.com/2023/salt-vagrant/#infra-as-code---your-wishes-as-a-text-file

Karvinen, Tero: Raportin kirjoittaminen, saatavilla: https://terokarvinen.com/2012/08/13/2006/raportin-kirjoittaminen-4#viittaa_lahteisiin

Tehtävässä käytetty Microsoft Copilotia (2025) Virhetilanteiden ratkaisemista varten.
