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
Lisätään käyttäjä don. <br>
Koneelle asennetaan graafinen käyttöliittymä sekä enemmän prosessointitehoa ja RAM muistia. <br>
<br>
Q: Miksi lisätä käyttäjä koneiden luonnin aikana eikä tilatiedostolla?<br>
A: Olen tehnyt ennenkin tilatiedostolla käyttäjän luomisen, halusin kokeilla muuta lähestymistapaa. Tapa ei ole paras mahdollinen käytäntö mutta halusin kokeilla tätä tyyliä. <br> 
<br>
<br>
![Description](vagrant2.png)
<br> 
<br>
Vagrant tiedostoon on saatu pohja Tero Karvisen <a href="https://terokarvinen.com/2023/salt-vagrant/#ready-made-Vagrantfile-for-three-computers">Vagrant artikkelista</a>. Muokkasin tiedostoa hieman ja tein omat lisäykset. <br>
Ympäristön käyttöönotto 'vagrant up' komennolla. <br>
Asennuksen jälkeen testataan ensimmäiseksi apassi koneelle kirjautuminen don käyttäjällä. <br>
<br>
![Description](don.png)
<br>
<br>
Kirjautuminen onnistuu, siirryn työskentelemään isäntäkoneen komentokehotteeseen masterille ssh-yhteydellä. <br>
Luon ensimmäiseksi vanhoja muistiinpanojani sekä Tero Karvisen <a href="https://terokarvinen.com/2018/04/03/apache-user-homepages-automatically-salt-package-file-service-example/">Apache PFS esimerkkiä</a> hyödyntäen top.sls ja tila.sls tiedostot masterin /srv/salt hakemistoon. <br>
Tilan ajaminen komennolla 'sudo salt '*' state.apply'<br>
<br>
<br>
![Description](tila.png)
<br>
Kuva otettu toisesta ajosta, jotta kaikki saadaan nätisti yhteen kuvaan. <br>
<br>
![Description](ajo.png)
<br>



https://terokarvinen.com/2023/salt-vagrant/#ready-made-Vagrantfile-for-three-computers <br>
https://terokarvinen.com/2018/04/03/apache-user-homepages-automatically-salt-package-file-service-example/
https://stackoverflow.com/questions/18878117/using-vagrant-to-run-virtual-machines-with-desktop-environment <br>
