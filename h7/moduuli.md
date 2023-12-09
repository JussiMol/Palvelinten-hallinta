# H7 Oma moduuli
### Tavoite
Moduulin tavoitteena on luoda vagranttia käyttäen kahden koneen herra/orja arkkitehtuuri.<br>
Tarkoitus olisi masteria käyttäen luoda orjalle ympäristö verkkosivujen luomiseen, web-palvelimella niiden ylläpito ja mahdollistaa selaimella niiden tarkastelu.<br>
### Lähtötiedot
#### Isäntäkone
OS: Windows 10 Home 64-bit<br>
CPU: AMD Ryzen 5 5600x 6-core<br>
RAM: 16Gb<br>
#### Master kone. <br>
Debian 11 kone salt-master asennettuna provisioinnin yhteydessä.<br>
#### Debian 11 kone <br>
Minion shell skriptissä asennetaan salt-minion.<br>
Asennetaan xfce4 joka on <a href="https://www.xfce.org/">kevyt työpöytäympäristö</a>. <br>
Lisätään käyttäjä don ja asetetaan se sudo ryhmään. <br>
Koneelle asennetaan graafinen käyttöliittymä sekä enemmän prosessointitehoa ja RAM muistia. <br>
<br>
Q: Miksi lisätä käyttäjä koneiden luonnin aikana eikä tilatiedostolla?<br>
A: Olen tehnyt ennenkin tilatiedostolla käyttäjän luomisen, halusin kokeilla muuta lähestymistapaa. Tapa ei ole paras mahdollinen käytäntö mutta halusin kokeilla tätä tyyliä. <br> 
Käyttäjien hallinnointiin virkistin muistia <a href="https://www.freecodecamp.org/news/how-to-manage-users-in-linux">FreeCodeCampin</a> linux artikkelista. <br>
<br>
<br>
![Description](vagrant3.png)
<br> 
<br>
Vagrant tiedostoon on saatu pohja Tero Karvisen <a href="https://terokarvinen.com/2023/salt-vagrant/#ready-made-Vagrantfile-for-three-computers">Vagrant artikkelista</a>. Muokkasin tiedostoa hieman ja tein omat lisäykset. <br>
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
Luon ensimmäiseksi vanhoja muistiinpanojani sekä Tero Karvisen <a href="https://terokarvinen.com/2018/04/03/apache-user-homepages-automatically-salt-package-file-service-example/">Apache PFS esimerkkiä</a> hyödyntäen top.sls ja tila.sls tiedostot masterin /srv/salt hakemistoon. <br>
Tilan ajaminen komennolla 'sudo salt '*' state.apply'<br>
<br>
![Description](tila2.png)
<br>
Kuva otettu toisesta ajosta, jotta kaikki saadaan nätisti yhteen kuvaan. Tilan ajaminen on onnistunut ilman virheitä, niistä tulisi ilmoitukset kunkin epäonnistuneen kohdan yhteydessä. <br>
Muutokset ilmoitettaisiin myös niissä kohdissa, joissa muutoksia tehdään. <br>
Succeeded: 6 perässä lukisi (changed:(numero)) riippuen kuinka monta muutosta tapahtuu. <br>
<br>
![Description](ajo2.png)
<br>
<br>
Seuraavaksi tarkistus apassi koneelta onko Firefox selain ja apache2 pyörimässä. <br>
<br>
![Description](firefox.png)
<br>
<br>

Nyt meillä on ympäristö tarvittavilla sovelluksilla sekä käyttäjällä verkkosivujen luomiseen. <br>
Jätin tarkoituksella "kehityksen" orja koneelle enkä masterille. Mielestäni se olisi hieman sekava pakka pitää kasassa jos kehitys olisi tehty masterilla ja seurattu source tiedostoja. <br>
Automaatio sai hoitaa meille ympäristön rakentamisen ja manuaalisesti jatkuisi hommat. <br>
Mitä idempotenssiin tulee niin tässä tapauksessa tarvittavien ohjelmien olemassaolo, sekä käyttäjän mahdollisuus luoda omia kotisivuja ovat se tila mitä halutaan pitää yllä. <br>
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
https://www.freecodecamp.org/news/how-to-manage-users-in-linux/
