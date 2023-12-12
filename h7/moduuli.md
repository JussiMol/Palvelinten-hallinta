# H7 Oma moduuli, kehitysympäristö
### Tavoite
Moduulin tavoitteena on luoda vagranttia käyttäen kahden koneen herra/orja arkkitehtuuri.<br>
Herraa käyttäen luoda orjalle ympäristö käyttäjän verkkosivujen luomiseen, web-palvelimella niiden ylläpito ja mahdollistaa selaimella sivujen tarkastelu.<br>
### Lähtötiedot
#### Isäntäkone
OS: Windows 10 Home 64-bit<br>
CPU: AMD Ryzen 5 5600x 6-core<br>
RAM: 16Gb<br>
#### Master kone. <br>
Debian bullseye 11 kone salt-masterilla asennuksen yhteydessä.<br>
#### Minion kone <br>
Debian bullseye 11 
Minion shell skriptissä asennetaan salt-minion.<br>
Asennetaan xfce4 joka on <a href="https://www.xfce.org/">kevyt työpöytäympäristö</a>. <br>
Lisätään käyttäjä don sudo oikeuksilla. <br>
Koneelle asennetaan graafinen käyttöliittymä sekä enemmän prosessointitehoa ja RAM muistia. <br>
<br>
Mikäli koneita olisi enemmän niin don luotaisiin jokaiselle minionille. Harkitse teetkö näin vai user.present funktiolla. <br>
<br>
Käyttäjien hallinnointiin virkistin muistia <a href="https://www.freecodecamp.org/news/how-to-manage-users-in-linux">FreeCodeCampin</a> linux artikkelista. <br>
Vagrant tiedostoon on saatu pohja Tero Karvisen <a href="https://terokarvinen.com/2023/salt-vagrant/#ready-made-Vagrantfile-for-three-computers">Vagrant artikkelista</a>. Muokkasin tiedostoa hieman ja tein omat lisäykset. <br>
<br>
<br>
![Description](vagrant3.png)
<br> 
<br>
***Miksi lisätä käyttäjä provisioinnissa eikä tilatiedostolla?*** <br>
***user.present/absent käyttäjien hallinta on tuttua, halusin kokeilla muuta lähestymistapaa.*** <br>
<br>
Puran hieman omia lisäyksiä auki. <br>
 useradd - käyttäjänlisäys<br>
 (-m) luo käyttäjän kotihakemiston<br>
 (-c $full_name) määrittää käyttäjän koko nimen "Don Devaaja"<br>
 (-s /bin/bash) määrittää käyttäjän oletuskomentotulkin (bash)<br>
 $username määrittää käyttäjän joka lisätään järjestelmään. <br>
 echo"$username:$password" | sudo chpasswd - komento päivittää käyttäjän salasanan ylempänä annetulla "testisalis" arvolla. <br>
 sudo usermod -aG sudo "$username" - lisätään "don" ryhmään sudo. <br>
 <br>
 Käyttäjän lisäykseen saatu vinkkejä kaverilta, joka on Linuxin kanssa tekemisissä päivittäin. <br>
 Osien selitykset poimittu 'man useradd' manuaalisivuilta. <br>
 <br>
Ympäristön käyttöönotto 'vagrant up' komennolla. <br>
Asennuksen jälkeen testataan ensimmäiseksi apassi koneelle kirjautuminen don käyttäjällä sekä sudo oikeudet. <br>
<br>
![Description](don.png)
<br>
<br>
![Description](sudo.png)
<br>
<br>
Kirjautuminen onnistuu, siirryn työskentelemään isäntäkoneen komentokehotteeseen masterille ssh-yhteydellä. <br>
Master koneella testi onko salt asennettu ja avaimien hyväksyminen 'sudo salt-key -A'
<br>
<br>
![Description](avain.png)
<br>
<br>
'sudo mkdir /srv/salt' hakemiston luonti mihin tilatiedostot sijoitetaan. <br>
Luon vanhoja muistiinpanojani sekä Tero Karvisen <a href="https://terokarvinen.com/2018/04/03/apache-user-homepages-automatically-salt-package-file-service-example/">Apache PFS esimerkkiä</a> hyödyntäen top.sls ja tila.sls tiedostot. <br>
<br>
![Description](tila2.png)
<br>
<br>
Tila.sls osat. <br>
apache2 / firefox-esr / micro - pakettien asennus. <br>
apassi - don käyttäjälle public_html hakemiston luonti ja sen käyttöoikeuksien määrittäminen. <br>
a2enmod userdir - otetaan apachen vakiotiedosto userdir.conf käyttöön joka mahdollistaa kotihakemistosta html sivujen käyttämisen. <br>
apache2service - pitää palvelun päällä ja seuraa muutoksia userdir.conf tiedostossa. Tarpeen mukaan käynnistää apachen uudestaan. <br>
<br>
Tilan ajaminen komennolla 'sudo salt '*' state.apply'<br>
Kuva otettu toisesta tilatiedoston ajokerrasta, jotta kaikki saadaan nätisti yhteen kuvaan.<br> 
Tilan ajaminen on onnistunut ilman virheitä, niistä tulisi ilmoitukset kunkin epäonnistuneen kohdan yhteydessä. <br>
Muutokset ilmoitettaisiin myös niissä kohdissa, joissa muutoksia tehdään. <br>
Succeeded: 6 perässä lukisi (changed:(numero)) riippuen kuinka monta muutosta tapahtuu. <br>
<br>
![Description](ajo2.png)
<br>
<br>
Seuraavaksi tarkistus apassi koneelta onko Firefox selain ja apache2 asennettuna. <br>
<br>
![Description](firefox.png)
<br>
<br>
Loin manuaalisesti Microlla don käyttäjällä /home/don/public_html/index.html sivun ja katsotaan selaimella näkyykö se.<br>
<br>
![Description](testi.png)
<br>
<br>
Nyt on luotu ympäristö tarvittavilla sovelluksilla sekä käyttäjällä verkkosivujen luomiseen. <br>
Git ja virtualbox guest additions olisi ollut hyvä lisä näin jälkiviisaana. <br>
Mitä idempotenssiin tulee niin tässä tapauksessa tarvittavien ohjelmien olemassaolo, sekä käyttäjän mahdollisuus luoda omia kotisivuja ovat se tila mitä halutaan pitää yllä. <br>
Mielestäni tavoite on saavutettu. Automaatiolla rakennettiin ympäristö, joka mahdollistaa manuaalisen työskentelyn ja testauksen. <br>
<br>
Virtuaalikone ilman työpöytäympäristöä. Kuva otettu rakentamis/ongelmanratkaisu vaiheesta. <br>
<br>
![Description](gui.png)
<br>
<br>
xfce4 on löydetty Googlesta haulla linux desktop environments. <br>
Haun päällimmäisenä oli <a href="https://www.geeksforgeeks.org/best-linux-desktop-environments/">GeeksforGeeks</a> artikkeli Linux työpöytäympäristöistä. <br>
Päädyin käyttämään kyseistä ympäristöä koska siitä puhuttiin laitteistolle kevyenä ja käyttäjäystävällisenä. <br>
Tarkistin valmiilta Linux virtuaalikoneelta löytyykö tuo paketinhallinnasta. <br>
'sudo apt search xfce' ja sieltä löytyi metapaketti ja kaikki liitännäiset. <br>
### Kotitehtävät
https://github.com/JussiMol/Palvelinten-hallinta/blob/main/h1/Viisikko.md <br>
https://github.com/JussiMol/Palvelinten-hallinta/blob/main/h2/Karjaa.md <br>
https://github.com/JussiMol/Palvelinten-hallinta/blob/main/h3/Versio.md <br>
https://github.com/JussiMol/Palvelinten-hallinta/blob/main/h4/Demonit.md <br>
https://github.com/JussiMol/Palvelinten-hallinta/blob/main/h5/CSIKerava.md<br>
https://github.com/JussiMol/Palvelinten-hallinta/blob/main/h6/Windows.md <br>
### Lähteet
#### Tero Karvinen
Salt Vagrant - automatically provision one master and two slaves <br>
https://terokarvinen.com/2023/salt-vagrant/#ready-made-Vagrantfile-for-three-computers <br>
Apache User Homepages Automatically – Salt Package-File-Service Example <br>
https://terokarvinen.com/2018/04/03/apache-user-homepages-automatically-salt-package-file-service-example/ <br>
#### Stack overflow
Using vagrant to run virtual machines with desktop environment<br>
https://stackoverflow.com/questions/18878117/using-vagrant-to-run-virtual-machines-with-desktop-environment <br>
#### Xfce Development Team
https://www.xfce.org/
#### FreeCodeCamp
How to Manage Users in Linux<br>
https://www.freecodecamp.org/news/how-to-manage-users-in-linux/<br>
#### GeeksforGeeks
Best Linux Desktop Environments <br>
https://www.geeksforgeeks.org/best-linux-desktop-environments/<br>
