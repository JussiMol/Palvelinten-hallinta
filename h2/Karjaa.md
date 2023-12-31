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
AMD Ryzen 5 5600X 6-Core prosessori 3,7Ghz <br>
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
Komento: vagrant --version - > Vagrant 2.4.0 <br>
Ohjelma on asennettu.<br>
### b) Yksi maankiertäjä
Kokeillaan <a href="https://terokarvinen.com/2017/04/11/vagrant-revisited-install-boot-new-virtual-machine-in-31-seconds/">Vagrant Revisited</a> ohjeita ja katsotaan mitä käy. <br>
<br>
Komento vagrant init debian-12 <br>
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
Vagrant vaatii "boxin" toimiakseen. <br>
 <br>
Kokeillaan omaa soveltamista. <a href="https://terokarvinen.com/2023/salt-vagrant/">Teron ohjeita</a> ja omaa Vagrantfileä vertaamalla huomasin mikä itsellä taisi mennä vikaan. <br>
<br>
![Description](vagrantfile2.png)
<br>
Muutan "debian-12" -> "debian/bookworm64" ja ajan uudestaan vagrant up. (Tämä oli vain oivallus kokeilla debian/bookworm64) <br>
Noniin, nyt alkoi tapahtua. Komentokehotteen teksti liian pitkä kuvan ottamiseen. Tarkistetaan virtualboxista onko kone päällä. <br>
<br>
![Description](virtualbox.png)
<br>
Ilmeisesti Vagrant hakee pilvestä tuon minun "boxin" <a href="https://app.vagrantup.com/boxes/search?utf8=%E2%9C%93&sort=downloads&provider=&q=debian+bookworm">Discover Vagrant Boxes</a> sivulta löytyi alle kuukausi sitten tehty debian/bookworm64 boxi. <br>
Testataan saadaanko koneeseen ssh yhteys. <br>
<br>
![Description](ssh.png)
<br>
B tehtävä onnistui. exit komennolla ulos virtuaalikoneesta ja vagrant destroy komennolla sen poisto. <br>
### c) Herra ja orja samalla koneella
Käytetään Vagrantfilen sisältöä. <a href="https://terokarvinen.com/2023/salt-vagrant/">Teron ohjeista</a>, vaihdan bullseye64 -> bookworm64. Loppuihin en koske. <br>
Katsotaan mitä tapahtuu - vagrant up <br>
Koneet lähtevät komentokehotteen perusteella rakentumaan. Liian pitkä tuloste, että siitä saisi järkeviä kuvia. <br>
Kokeillaan ottaa ssh yhteys herraan - vagrant ssh tmaster <br>
Ja katsotaan onko avaimia jonossa hyväksyttäväksi. $ sudo salt-key -A (Jos on hyväksytään) ja suoritetaan pingi testi. <br>
$ sudo salt-key -A = Salt stackin komentorivityökalu joka hakee hyväksyttäviä avaimia. (<a href="https://docs.saltproject.io/en/latest/ref/cli/salt-key.html">salt-key manual</a>) <br>
<br>
![Description](testi.png)
<br>
Teen pienen lisätestin, koska ei ole täyttä varmuutta mitä virtuaalikoneet pitävät sisällään. <br>
Kokeilen $ sudo salt 't001' grains.items ja lista tulostuu. <br>
<br>
![Description](orja.png)
<br>
Jostain tuntemattomasta syystä käytetään debian 11 bullseye.  <br>
Tätä asiaa ei tarvinnut kauaa miettiä, Vagrantfileä en päivittänyt boxin vaihdon jälkeen niin vagrant ajoi debian/bullseye64 version tiedostosta. <br>
### Verkossa
Jatketaan  <a href="https://terokarvinen.com/2023/salt-vagrant/">Teron ohjeilla</a> luoduilla virtuaalikoneilla tmaster, t001 ja t002 sekä suoritetaan niillä eri komentoja. <br>
ssh yhteydellä tmasterille. <br>
Kerätään kyselyllä tietoja koneilta. $ sudo salt '*' grains.item mem_total osfinger host
<br>
![Description](tiedot.png)
<br>
Muistin perusteella aika heppoinen kone. 461Mb. <br>
Ajetaan sitten molemmille koneille idempotentteja komentoja. <br>
Asennetaan aluksi microt molemmille orjille. $ sudo salt '*' single.state pkg.installed micro. <br>
Orjat tekee työtä käskettyä. <br>
Ajetaan komento uudestaan ja todetaan idempotentti. <br>
<br>
![Description](tiedot.png)
<br>
Tilat eivät muuttuneet toisella kerralla. <br>
Asennetaan vain t001 koneelle apache2 ja testataan näkyykö verkkosivut.<br>
$ sudo salt 't001' state.single pkg.installed apache2 <br>
Jälleen tuloste liian pitkä näyttökaappausta varten. <br>
Ohjelma asennettu? Succeeded: 1 (changed=1) <br>
Kokeillaan ajaa komento uudestaan. <br>
<br>
![Description](apache2.png)
<br>
Ei muutoksia. <br>
Testataan onko apache2 päällä. Otetaan esille t001 ipv4 osoitteet. <br>
$ sudo salt 't001' grains.item ipv4 <br>
$ curl -s 192.168.12.100 <br>
Apache on päällä. <br>
<br>
![Description](curl.png)
<br>
Ajan tappokäskyn ja kokeillaan tuleeko muutos. <br>
<br>
![Description](dead.png)
<br>
Tappokäsky succeeded ja tila (changed=1) <br>
Ei vastaa curl kutsuihin. Apache on pois päältä. <br>
Kokeillaan tehdä orjakoneille uudet hakemistot herran käskyllä. <br>
<br>
![Description](testikansio.png)
<br>
Jotain tapahtui, käydään t001 katsomassa onko hakemisto luotu. <br>
<br>
![Description](t001.png)
<br>
Ilmestyihän se. <br>
Testataan infraa koodina. Kokeilen <a href="https://terokarvinen.com/2023/salt-vagrant/">Teron ohjeista</a> suoraan /srv/salt/hello/init.sls esimerkkiä.<br>
<br>
![Description](infra.png)
<br>
No sehän vaan toimii ja on vielä idempotentti komento. <br>
Käydään t002 koneella katsomassa onko /tmp/infra-as-code olemassa. <br>
<br>
![Description](tmp.png)
<br>
Löytyy. Komento teki työtä käskettyä. <br>
Koneilla ei ole tällä hetkellä muuta virkaa eli ajetaan alas komennolla vagrant destroy <br>
## Lähteet
### Tero Karvinen 
Tehtävänanto - Infra as Code 2023 <br>
https://terokarvinen.com/2023/configuration-management-2023-autumn/ <br>
Vagrant Revisited – Install & Boot New Virtual Machine in 31 seconds <br>
https://terokarvinen.com/2017/04/11/vagrant-revisited-install-boot-new-virtual-machine-in-31-seconds/ <br>
Salt Vagrant - automatically provision one master and two slaves <br>
https://terokarvinen.com/2023/salt-vagrant/
### StackExchange
What is the definition of "cattle not pets"? <br>
https://devops.stackexchange.com/questions/653/what-is-the-definition-of-cattle-not-pets#654 <br>
### Hashicorp
Vagrant tutorials
https://developer.hashicorp.com/vagrant/tutorials/getting-started
Discover Vagrant Boxes
https://app.vagrantup.com/boxes/search?utf8=%E2%9C%93&sort=downloads&provider=&q=debian+bookworm
