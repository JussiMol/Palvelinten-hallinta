# H2 Karjaa
## x) Tiivistelmät
### Karjaa vai lemmikkejä
- Lemmikit ovat kriittisiä ja tärkeitä palvelimia, joita käsitellään manuaalisesti.
- Karja koostuu monista palvelimista, joita voidaan poistaa ja ottaa uusia käyttöön joustavasti.<br>
(StackExchange,  <a href="https://devops.stackexchange.com/questions/653/what-is-the-definition-of-cattle-not-pets#654">What is the definition of "cattle not pets"?</a>)<br>
### Vagrant
- Päivitä paketinhallinta
- Asenna Vagrant ja virtualbox
- Määritä asennettava virtuaalikone (käyttöjärjestelmä) ja yhdistä ssh-yhteydellä.
- Tuhoa 'vagrant destroy' käytön päätteeksi<br>
(Tero Karvinen, <a href="https://terokarvinen.com/2017/04/11/vagrant-revisited-install-boot-new-virtual-machine-in-31-seconds/">Vagrant Revisited</a>)
### Salt Vagrant
- Palvelinympäristö vaatii Vagrantin, virtualboxin ja tekstieditorin
- Virtualisointi määritellään Vagrantfile tiedostolla, sisältö muokattavissa.
- Ympäristö toimii herra/orja arkkitehtuurilla, orjat vastaavat herralle.
- Orjia voi käskeä komennoilla.
- Orjista voi kerätä tietoa.
- Päämääränä idempotentti lopputila.
- Orjille voidaan syöttää koodia ja ne toimivat sen perusteella.<br>
(Tero Karvinen, <a href="https://terokarvinen.com/2023/salt-vagrant/">Salt Vagrant - automatically provision one master and two slaves</a>)
### a) Vagrantin asennus
Isäntäkoneella Windows 10 Home 64-bit<br>
AMD Ryzen 5 5600X 6-Core prosessori <br>
16 GB RAM<br>
Vagrant lataus sivulta https://developer.hashicorp.com/vagrant/downloads Windows AMD64 Versio 2.4.0 <br>
Latauksen jälkeen ihan varmuudeksi skannaan Windows Defenderillä ladatun asennusohjelman.<br>
Ei uhkia, 183913 tiedostoa tarkistettiin. <br>
Asennusohjelma pyytää hyväksymään käyttöehdot ja lisenssin.<br>
Asennus tapahtuu järjestelmänhallitsijan oikeuksilla.<br>
<br>
![Description](vagrant_asennus1.png)
<br>
<br>
Asennusohjelma tekee tehtävänsä ja asennus on valmis. <br>
<br>
![Description](vagrant_asennus2.png)
<br>
<br> 
Pyytää vielä käynnistämään koneen uudestaan, että kaikki määritykset tulevat voimaan. <br>
