#H5 CSI Kerava
Tehtävänanto. https://terokarvinen.com/2023/configuration-management-2023-autumn/#h5-csi-kerava
### x) Tiivistelmä
#### Apache User Homepages Automatically – Salt Package-File-Service Example
- Tee ensin käsin, automatisoi vasta sen jälkeen.
- Selvitä mitä tiedostoja muokkasit, sudoeditillä tehdyt muokkaukset huomaa helposti.
- Kun luot tiloilla tiedostoja, tee niistä idempotentteja.
- Tiedostojen käyttö on luotettavampaa kuin shell komentojen ajaminen.
- Jos jokin ei toimi saattaa joutua tarkistamaan hakemistojen ja tiedostojen oikeudet.
### a) CSI Kerava
Nyt ollaan tekemisissä työkalun kanssa, jota en ole käyttänyt kertaakaan. Aloitan selaamalla manuaalisivuja ja selaamalla nettiä. <br>
<a href="https://www.tecmint.com/35-practical-examples-of-linux-find-command/">35 esimerkkiä</a> find komennosta, näistä sai käsitystä miten tuota komentoa tulisi kasata. 'man find' etsitään selitykset ja kokeillaan tehtävänannon vinkeissä olevaa find komentoa. <br>
Teen ensiksi muutoksen /etc/salt/minion tiedostoon. Tapahtuma 24.11.2023 noin kello 12. Kokeillaan etsiä tapahtuma lokista.<br>
$ sudo find /etc/ -printf '%T+ %p\n' | sort -r | head
- sudo = pääkäyttäjänoikeuksilla (oletan että kaikkea ei paljasteta normaalille käyttäjälle)
- find = etsimiskomento
- /etc/ = tiedostopolku mistä etsitään
- -printf = määrittää minkälaisen merkkijonon komento tulostaa -> liittyy seuraavaan osaan.
- %T+ = muokkauspäivämäärä ja aika tarkemmalla esitystavalla (plus merkki)
- %p = näytä tiedostopolku
- \n = newline - rivinvaihto
- sort -r = lajittelee tulokset tulokset aikajärjestyksessä käänteisesti (uusin ylimpänä)
- head = näyttää uusimmat 10 riviä

Ylimmäisenä nähdään minun muokkaus minion tiedostoon.  <br>
<br>
![Description](find.png)
<br>
The time is given in the current timezone (man find) Aikaleimoja tarkastellessa täytyy olla tarkkana. <br>
Kokeillaan sama setti mutta ilman /etc/ osaa ja ollaan kotihakemistossa. Jos komento toimii kuten luulen niin se toimii myös tällä tavalla. <br>
Teen nopeaan testitiedoston findia varten noin kello 13.07. <br>
<br>
![Description](find2.png)
<br>
Toimiihan se. ./ on /etc/ tai /home/ tilalla kertomassa että komento ajettiin tämänhetkisessä hakemistossa. Tiedostokin on nähtävissä.<br>
### b) Gui2fs
Virtuaalikoneelle on ladattu VLC Media Player, muokkaan sitä ottamalla audion pois käytöstä. <br>
<br>
![Description](vlc.png)
<br>
Internetistä löydetyn tiedon mukaan <a href="https://stackoverflow.com/questions/1024114/location-of-ini-config-files-in-linux-unix">konffi tiedostojen</a> sijainti on joko /etc/ tai käyttäjän kotihakemistossa piilotettuna. <br>
Tein muutoksen ilman sudoa, omalla käyttäjällä ja sovelluksesta suoraan. <br>
Oletan että konffitiedosto löytyy kotihakemistosta. <br>
'ls -a' komento näyttää piilotetut tiedostot ja hakemistot. Siirrtyään .config hakemistoon ja etsitään mitä löytyy. <br>
<br>
![Description](audio.png)
<br>
Tämä näyttäisi olevan oikea tiedosto. Grepillä sai lyhennettyä tulostetta, enable audio boolean arvo näyttäisi olevan false. <br>
Voisin olettaa että oikea tiedosto paikannettu. <br>
find toiminnolla löysin seuraavalla tavalla melkein samaan paikkaan. <br>
<br>
![Description](home.png)
<br>
### c) Komennus
Käytän seuraavissa tehtävissä tmaster ja t001 ympäristöä. <br>
Luon /srv/salt/huomenta hakemiston. <br>
Hakemistoon huomenta shell tiedosto ja init.sls tiedosto.<br>
<br>
![Description](shell.png)
<br>
Kokeillaan ajaa tila. <br>
<br>
![Description](huomenta.png)
<br>
Tilan ajaminen onnistui ja testi t001 osoittaa sen toimivan. <br>
***Vaihdoin init.sls tiedoston kohdalla nimessä huomenta.sh -> huomenta siksi siinä kohtaa lukee updated***
### d) Apassi
Luon seuraavan hakemiston /srv/salt/apache2. <br>
/srv/salt luon top.sls tiedoston. <br>
<br>
![Description](top.png)
<br>
Tilatiedostossa on määriteltynä apache2 asennus ja masterin apache2 kansiosta kotisivu. <br>
/var/www/html/index.html määritellään file.managed moduulilla. <br>
name kohta määrittelee tiedostopolun ja source mitä tiedostoa käytetään name kohdan tiedostopolussa. <br>
(Salt states, user guide) <br>
<br>
![Description](cat.png)
<br>
Kokeillaan ajaa tilatiedosto. <br>
<br>
![Description](apache.png)
<br>
Näyttökaappaus alusta ja lopusta. <br>
<br>
![Description](uusi.png)
<br>
***Testasin ajaa aluksi tilan pelkällä apachen asennuksella siksi näyttää vain changed 1*** <br>
Succeeded: 2 (changed=1) tarkistetaan mitä curlaamalla mitä käy. <br>
Asennan curlin paketinhallinnasta ja tarkistan vagrantfilestä ip:n orjalle, katsotaan vastaako.<br>
<br>
![Description](curl.png)
<br>
Apachen asennus onnistui ja uuden sivun luonti siinä samassa. <br>
Osiossa hyödynsin <a href="https://docs.saltproject.io/salt/user-guide/en/latest/topics/states.html#state-modules">Saltin ohjetta </a>, <a href="https://terokarvinen.com/2018/apache-user-homepages-automatically-salt-package-file-service-example/?fromSearch=salt%20file">Teron ohjetta </a>  Package-file-service esimerkki ja luennolla opittuja asioita soveltamalla. <br>
### e) Ämpärillinen
Yritin osiota <a href="https://docs.saltproject.io/en/latest/ref/states/all/salt.states.file.html#salt.states.file.directory">Saltin ohjetta </a> ja aiemmin opittua soveltamalla. Ei mennyt maaliin. <br>
Loin uuden init.sls tiedoston seuraavalla sisällöllä. <br>
<br>
![Description](kansio.png)
<br>
Tilan ajamisessa on ongelmia hakemiston luomisessa. <br>
<br>
![Description](komento2.png)
<br>
Lisävilkaisu ohjeesta ja huomasin kohdan mkdirs. Init.sls tiedostoon pieni lisäys - mkdirs: true ja kokeillaan ajaa tila uudestaan. <br>
<br>
![Description](komento3.png)
<br>
Oli virhe tehdä tämä osio käyttämällä huomenta komentoa nyt tarkistuksessa ei voi olla sata varma onko kansioon asennettu huomenta komento toimiva. <br>
Kävin t001 koneella suoraan kansiossa kokeilemassa /usr/local/bin/bash/komennot kansiossa kokeilemassa komentoja. <br>
Huomenta toimii, päivää ei sama pätee .sh kokeilulla. Kaikki haluttu on asennettuna mutta alkuperäisessä tiedostossa on varmaankin joku kirjoitusvirhe tms.<br>
<br>
![Description](lista.png)
<br>
### Lähteet
#### Tero Karvinen
Tehtävänanto
https://terokarvinen.com/2023/configuration-management-2023-autumn/#h5-csi-kerava<br>
Apache User Homepages Automatically – Salt Package-File-Service Example<br>
https://terokarvinen.com/2018/04/03/apache-user-homepages-automatically-salt-package-file-service-example/<br>
#### Stackoverflow
Location of ini/config files in linux/unix?
https://stackoverflow.com/questions/1024114/location-of-ini-config-files-in-linux-unix
#### Salt project
State modules
https://docs.saltproject.io/salt/user-guide/en/latest/topics/states.html#state-modules
Salt.state.directory
https://docs.saltproject.io/en/latest/ref/states/all/salt.states.file.html#salt.states.file.directory
#### Tecmint
35 Practical Examples of Linux Find Command
https://www.tecmint.com/35-practical-examples-of-linux-find-command/

