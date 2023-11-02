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
Asennusohjelma tekee tehtävänsä ja asennus on valmis. <br>
<br>
![Description](vagrant_asennus2.png)
<br>
Pyytää vielä käynnistämään koneen uudestaan, että kaikki määritykset tulevat voimaan. <br>
Avaan komentokehotteen ja ruvetaan kokeilemaan. <br>
Testataan onko ohjelma olemassa <br>
vagrant --version Vagrant 2.4.0 <br>
Ohjelma on asennettu.<br>
### b) Yksi maankiertäjä
Kokeillaan <a href="https://terokarvinen.com/2017/04/11/vagrant-revisited-install-boot-new-virtual-machine-in-31-seconds/">Vagrant Revisited</a> ohjeita ja katsotaan mitä käy. <br>
<br>
![Description](vagrantinit.png)
<br>
Okei ohjelma loi automaattisen Vagrantfilen, etsitään se näin alkuun. <br>
Löytyi käyttäjän omasta hakemistosta, katsotaan sisältö. <br>
<br>
![Description](vagrantfile.png)
<br>
Loput tiedostosta on kommenttirivejä, jotka selittävät eri osien toimintaa ja ohjeita. <br>
config.vm.box = "debian-12" Kokeillaan tapahtuuko vagrant up komennolla mitään. <br>
<br>
![Description](failure.png)
<br>
Eipä onnistunut. Etsitään ratkaisu. <a href="https://developer.hashicorp.com/vagrant/tutorials/getting-started">Vagrant Quick start</a> <br>
Ohjeita seuraamalla pitäisi tehdä hakemisto johon sijoitetaan projekti ja vagrant init komento tekisi sinne valmiin Vagrantfilen. <br>
///Vagrantfile luodaan siihen hakemistoon, jossa olet kun ajat vagrant init komennon///<br>
hashicorp/bionic64 on joku hashicorpin oma "box", joka löytyy pilvestä. <br>
Kokeillaan omaa soveltamista. <a href="https://terokarvinen.com/2023/salt-vagrant/">Teron ohjeita</a> ja omaa Vagrantfileä vertaamalla huomasin mikä itsellä taisi mennä vikaan. <br>
<br>
![Description](vagrantfile2.png)
<br>
Muutan "debian-12" -> "debian/bookworm" ja ajan uudestaan vagrant up. <br>
Sama virheilmoitus kuin äsken nimet vain vaihtuivat. Teron ohjeissa lukee "bullseye64" ja omassa ei. Lisätään se ja uusi vagrant up ajo.  <br>
Noniin, nyt alkoi tapahtua. Komentokehotteen teksti liian pitkä kuvan ottamiseen. Tarkistetaan virtualboxista onko kone päällä. <br>
<br>
![Description](virtualbox.png)
<br>
Ilmeisesti Vagrant hakee siltikin pilvestä tuon minun "boxin" <a href="https://app.vagrantup.com/boxes/search?utf8=%E2%9C%93&sort=downloads&provider=&q=debian+bookworm">Discover Vagrant Boxes</a> sivulta löytyi alle kuukausi sitten tehty debian/bookworm64 boxi. <br>
No testataan saadaanko koneeseen ssh yhteys. <br>
<br>
![Description](ssh.png)
<br>
B tehtävä onnistui. exit komennolla ulos virtuaalikoneesta ja vagrant destroy komennolla sen poisto. <br>
### c) Oma orjansa
Muokataan Vagrantfilen sisältöä. <a href="https://terokarvinen.com/2023/salt-vagrant/">Teron ohjeista</a> jätän toisen t002 koneen pois, vaihdan bullseye64 -> bookworm64. Loppuihin en koske. <br>
Katsotaan mitä tapahtuu - vagrant up <br>
Komentokehoteesta poimittuja huomioita. <br>
Molemmat koneet herra ja orja päivittävät suoraan asennuksessa paketinhallinnan. <br>
t001: ssh address 127.0.0.1:2222 ja tmaster: ssh address 127.0.0.1:2200 <br>
Matching MAC address for NAT networking. <br>
Kokeillaan ottaa ssh yhteys herraan. vagrant ssh tmaster <br>
Ja katsotaan onko avaimia jonossa hyväksyttäväksi. (Jos on hyväksytään) ja suoritetaan pingi testi. <br>
<br>
![Description](testi.png)
<br>
Teen pienen lisätestin, koska ei ole täyttä varmuutta mitä virtuaalikoneet pitävät sisällään. <br>
Kokeilen $ sudo salt 't001' grains.items ja lista tulostuu. <br>
<br>
![Description](orja.png)
<br>
Jostain tuntemattomasta syystä käytetään debian 11 bullseye.  <br>
Tätä asiaa ei tarvinnut kauaa miettiä, copy-pasten jälkeen tallensin Vagrantfile version josta oli poistettu t002, mutta ei muutettu config.vm.box osiota. <br>
### Verkon yli
Katsotaan saadaanko tehtyä. Pistän orjakoneen pystyyn linodella, lisätiedot siitä jos tehtävä onnistuu. <br>


