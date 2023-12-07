#H7 Oma moduuli
### Tavoitetila
Moduulin tavoitteena on luoda vagranttia käyttäen kahden koneen herra/orja arkkitehtuuri.<br>
Tarkoitus olisi masteria käyttäen luoda orjalle ympäristö verkkosivujen luomiseen ja mahdollistaa selaimella niiden tarkastelu.<br>
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






https://terokarvinen.com/2023/salt-vagrant/#ready-made-Vagrantfile-for-three-computers <br>
https://stackoverflow.com/questions/18878117/using-vagrant-to-run-virtual-machines-with-desktop-environment <br>
