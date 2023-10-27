# x) Tiivistelmät
### Verkkosivut githubilla
- Rekisteröidy palveluun.
- Luo säiliö/säilytyspaikka sisällöllesi.
- Tee tehtävät .md tiedostoihin.
- Lisää kuvia.
- Julkaise tekeleesi.<br>
(Tero Karvinen 2023. <a href="https://terokarvinen.com/2023/create-a-web-page-using-github/">Create a Web Page Using GitHub</a>
### Salt komennot paikallisesti
- Salt minion asennus komennot.
- --local komennot ovat paikallisia komentoja joilla voidaan tarkistaa erilaisia asioita.
- pkg.installed/removed - voidaan tarkistaa ohjelman asennus/poisto komennolla.
- file.managed/absent - voidaan tarkistaa tiedoston olemassaoloa.
- service.running/dead - voidaan tarkistaa ohjelman toimivuutta.
- user.present/absent - voidaan tarkistaa käyttäjien olemassaolo.
- cmd.run - voidaan ajaa ohjelmia.
- Tee idempotentteja suoritteita. Ajetaan vain jos tarvitsee tehdä muutoksia.
- Ohjeita sys.state_doc komennolla.<br>
(Tero Karvinen 2023. <a href="https://terokarvinen.com/2021/salt-run-command-locally/">Run Salt Command Locally</a>
### a) Salt minion asennus
Käytän debian 12 virtuaalikonetta, virtuaalikoneen lisätiedot ja asennusohje minun raportista -> <a href="https://github.com/JussiMol/Linux-palvelimet/blob/d695d08d28af0854d2a7391a6ad9caa195325762/h1.md"> Linux-palvelimet H1 </a> <br>
H1 Raportti toteutettu <a href="https://terokarvinen.com/2021/install-debian-on-virtualbox/">Tero Karvisen ohjeita</a> mukaillen. <br>
Virtuaalikone ollut käyttämättömänä hetken joten päiviteen paketinhallinta. <br>
$ sudo apt-get update<br>
$ sudo apt-get upgrade <br>
Salt minionin asennuksessa seurataan <a href="https://terokarvinen.com/2023/configuration-management-2023-autumn"/>Tehtäväsivun ohjetta </a> H1 osiota "Saltin asennus Debian 12" . <br>
<br>
![Description](komennot.png)
<br>
<br>
Ylemmän kuvan komentoja ymmärrän sen verran että, näillä komennoilla tuodaan Salt paketinhallintaan, päivitetään se ja asennetaan ohjelma. <br>
Ajan kyseiset komennot. Keyrings tiedosto on jo olemassa ensimmäinen komento ei tee mitään. <br>
Loput toimivat odotetusti ja testataan onko salt asennettuna $ sudo salt-call --version <br>
<br>
![Description](versio.png)
<br>
Salt on asennettu onnistuneesti. <br>
### b) Viisi tärkeintä
Käytetään Tero Karvisen <a href="https://terokarvinen.com/2021/salt-run-command-locally/">ohjeita</a> ja testataan saltin paikallista toimintaa. <br>
#### pkg.installed/removed
Testataan onko micro teksti editori asennettuna virtuaalikoneella. <br>
$ sudo salt-call --local -l info state.single pkg.installed micro <br>
Odotan että tuloste sanoo ettei mikään ole muuttunut koneella ja että editori on asennettuna. <br>
<br>
![Description](micro.png)
<br>
ID = micro eli ohjelma mikä oli tapahtuman kohde.<br>
Function kertoo minkälainen komento ajettiin. Tässä tapauksessa varmistettiin onko mahdollisesti ID:n mukainen ohjelma asennettuna. <br>
True = tapahtuma on todellinen ja ohjelma asennettu.<br>
Kaikki paketit asennettuna. <br>
Aikaleima. <br>
Kesto millisekunteina.<br>
Tapahtuman suoritus on onnistunut (succeeded)<br>
Total states run 1 = ajettiin paikallisesti niin tehtiin vain kerran. <br>
Ajan vastakkaisen komennon ja muutan pkg.removed micro <br>
<br>
![Description](micro21.png)
<br>
pkg.removed - nyt poistettiin ohjelma <br>
Micro old kohdassa on micro editorin versio mikä poistettiin <br>
Succeeded (changed=1) nyt se kertoo että 1 tapahtuma onnistui - poistimme paikalliselta koneelta micro editorin.<br>
Testataan onko Micro kadonnut koneelta. <br>
Yritän avata microlla kotihakemistossa teksti tiedoston -> bash: micro: command not found <br>
Micro on poistunut. <br>
Asennan sen takaisin komennolla. $ sudo salt-call --local -l info state.single pkg.installed micro <br>
<br>
![Description](micro2.png)
<br>
Uusi versio ilmestyi. <br>
Succeeded (changed=1) Asennus tapahtui? <br>
Testataan onko micro palautunut koneelle. <br>
<br>
![Description](micro3.png)
<br>
Micro on asennettu takaisin käyttämällä salt komentoja. <br>
#### file.managed/absent
Testataan seuraavaksi tiedostojen hallintaa. <br>
$ sudo salt-call --local -l info state.single file.managed /home/jussi/suolatesti.txt <br>
<br>
![Description](suolatesti.png)
<br>
INFO kohdat kertovat meille saltin toimintoja<br>
WARNING ilmoittaa ettei tiedostolle ole määritetty '' merkkien sisällä nimettyjä ominaisuuksia. <br>
Komento on luonut meille oman kertomansa mukaan suolatesti.txt tiedoston. <br>
$ ls ja kotihakemistosta todella löytyy suolatesti.txt tiedosto. <br>
Kokeillaan poistaa se file.absent komennolla. <br>
<br>
![Description](suolatesti2.png)
<br>
$ ls ja tiedosto on poistunut kotihakemistosta. <br>
#### service.running/dead
Testataan ohjelmien komentoja Apache2:lla <br>
Katsotaan mikä on alkutilanne $ sudo systemctl status apache2 - se on päällä (active)<br>
$ sudo salt-call --local -l info state.single service.running apache2 enable=True - testataan mitä komento sanoo kun Apache2 on jo päällä. <br>
<br>
![Description](apache2.png)
<br>
Already running, muutoksia ei tehty. <br>
Sammutetaan palvelin $ sudo salt-call --local -l info state.single service.dead apache2 enable=False <br>
<br>
![Description](apache21.png)
<br>
INFO osioista huomataan heti, että salt ajaa enemmän komentoja juurihakemistossa. <br>
Tappofunktion lopputulos = True <br>
1 onnistunut tapahtuma ja muuttunut tila. <br>
Testataan weppipalvelimen tila $ sudo systemctl status apache2 - inactive(dead) <br>
Testaus selaimella - ei mitään elämää <br>
service.running komennolla voisin sen laittaa päälle, mutta jätetään pysäytetyksi aiheuttamaan ihmetystä seuraavaa kertaa varten. <br>
#### user.present/absent
Testataan onko minun käyttäjä olemassa <br>
$ sudo salt-call --local -l info state.single user.present jussi<br>
<br>
![Description](jussi.png)
<br>
Onhan se <br>
Siirryn juurihakemistoon ja katsotaan /home ja ls onko koneella muita käyttäjiä. Pentti löytyy.<br>
Annetaan pentille kenkää <br>
<br>
![Description](pentti.png)
<br>
Pentti ja pentti group removed. Poisto onnistui <br>
