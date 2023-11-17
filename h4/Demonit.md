# Demonit
Tehtävänanto H4 demonit https://terokarvinen.com/2023/configuration-management-2023-autumn/
## x) Tiivistelmät
### Salt vagrant
init.sls tiedosto sisältää käskyt toteutettaville tiloille.<br>
top tiedosto määrittää mitkä koneet ajavat mitäkin tiloja.<br>
### Salt overview
#### Rules of YAML
Data rakentuu (avain): (arvo) pareihin, jotka määrittyvät kaksoispisteellä. <br>
Arvoilla voidaan muuttaa toimintoja. <br>
Syntaksi on tarkkaa (pienet kirjaimet). <br>
Tabulaattorin käyttö kielletty, käytä välilyöntiä. <br>
Kommentit # merkin perään. <br>
#### YAML simple structure
Skalaari on avain: arvo pari.<br>
Lista on avain jonka alle sisennettynä 2 välilyönnillä tehdään lista. <br>
Kirjasto on kokoelma kahta edellä mainittua. <br>
#### Lists and dictionaries - YAML block structures
YAML on jaettu lohkorakenteisiin.<br>
Sisennys asettaa kontekstin. SINUN ON sisennettävä ominaisuudet ja listasi. Kaksi välilyöntiä on standardi.<br>
Kokoelma, joka voi olla lista tai sanakirja-lohkosarja, osoitetaan jokainen merkinnällä yhdysviiva ja välilyönti.
(Salt project, salt user guide)
### Salt states
#### State modules
Tila moduuleilla määritetään mitä halutaan suorittaa ajettavalla koneella.<br>
Moduuleissa on varottava ristiriitoja tai määriteltävä niitä syvemmin ettei tule ristiriitoja suorituksissa. <br>
#### The state SLS data structure
.sls tiedostoon määritellään tunniste, tila, funktio, nimi ja argumentit.<br>
#### Organizing states
Tilatiedostot tulisi kirjoittaa niin että toinen kehittäjä voi ymmärtää helposti ja vaivattomasti mitä tilat tekevät. <br>
Hyvä käytäntö vähentää tilatiedoston monimutkaisuutta on käyttää vain muutamaa sisennystä lohkoa kohti. <br>
#### The top.sls file
top.sls tiedostolla voidaan määrittää mille koneille ajetaan tiloja. Suurissa kokonaisuuksissa ei ole käytännöllistä ajaa monia tiloja yksittäisinä. <br>
Tiedostolla voidaan myös määritellä erilaisia solmuja ja poimia ajettavia tiloja eri tiedostoista. <br>
Tiedoston sisältö ja rakenne kannattaa olla mahdollisimman joustava ja yksinkertainen. <br>
#### Create the SSH state, Create the Apache state
Tiedostot näyttävät siltä ettei ne päivity muutosten tekemisen yhteydessä. <br>
(Salt project, salt user guide)
### Pkg-File-Service
Artikkelissa on sshd tilan rakenne sekä määritystiedoston sisältö. <br>
(Tero Karvinen, Pkg-File-Service – Control Daemons with Salt – Change SSH Server Port) <br>
## a,b) Hello SLS! Top
Aloitan luomalla vagrantilla herran ja kaksi orjaa. Ympäristön luomisen jälkeen ssh yhteydellä herralle.<br>
Hyväksyn avaimet 'sudo salt-key -A'. Testaan kuuleeko orjat 'sudo salt '*' test.ping' <br>
Orjat vastaavat.
<br>

Seuraan <a href="https://terokarvinen.com/2023/salt-vagrant/#infra-as-code---your-wishes-as-a-text-file"> Teron ohjeita </a> init.sls ja top.sls tiedostojen luomisessa. <br>
Luon komennolla 'mkdir -p /srv/salt/hello' hakemiston. (-p = luo puuttuvat ylähakemistot samalla(parent directory)) <br>
<br>
![Description](init.png)
<br>
tilatiedostolla luodaan hello-world niminen tiedosto orjille. Testataan toiminta. <br>
<br>
![Description](hello.png)
<br>
Orjat väittää luoneensa tiedoston, käydään tarkistamassa tilanne. <br>
<br>
![Description](tmp.png)
<br>
Tiedostot on luotu, tilatiedosto toimi halutusti. <br>
Kokeillaan top.sls tiedoston toimintaa. <br>
<br>
![Description](top.png)
<br>
Tällä tiedostolla määritellään kaikille orjille ajettavaksi tila nimeltä "hello". <br>
Kokeillaan uudestaan ajaa tila 'sudo salt '*' state.apply'
<br>
![Description](top2.png)
<br>
Sama tila ajettiin uudestaan onnistuneesti, ei muutoksia. <br>
### c) Apache
Ajan herralla komennon 'sudo salt 't001' state.single pkg.installed apache2' tämä komento käskee orjaa asentamaan apache2 paketit. <br>
<br>
![Description](apache2.png)
<br>
Pitkä litania uusia muutoksia ja alhaalla lukee succeeded (changed=1), katsotaan onko asentunut ja mennyt päälle. <br>
Kysytään ip-tiedot orjilta ' sudo salt '*' cmd.run 'hostname -I' ' <br>
t001 192.168.12.100 <br>
Kokeillaan curlata t001 osoitetta.<br>
<br>
![Description](grep.png)
<br>
Apache2 on asennettuna ja päällä koneella. <br>
Poistetaan ja testataan että apache poistui. <br>
'sudo salt 't001' state.single pkg.removed apache2' (changed=1) <br>
curlilla testi -> connection refused. <br>
Poistettu onnistuneesti. <br>
Sitten kokeillaan automatisoida. (olen aiemmin jo tehnyt itse sivuja apachelle ja käynnistellyt/sulkenut sitä.) <br>
Mikäli nyt tulkitsin <a href="https://docs.saltproject.io/salt/user-guide/en/latest/topics/states.html#create-the-apache-state">Salt state ohjeita </a> oikein niin voin herra koneelle tehdä /srv/salt/apache2/index.html ja käyttää sitä tilatiedostossa määrittelemään minkälainen sivu tehdään. <br>
Teen uuden sivun ja uuden init.sls tiedoston hakemistoon /srv/salt/apache2. <br>
<br>
![Description](cat.png)
<br>
Ylempi kuva on korjattu versio init.sls tiedostosta.
Kokeilin kerran ajaa ja se ei onnistunut, yksi välilyönti oli liikaa ennen file.managed: kohtaa. <br>
Kokeillaan uudestaan ajaa tilatiedosto ja toivotaan parasta. <br>
<br>
![Description](apply.png)
<br>
Molemmilla koneilla Succeeded: 2(changed=2) tarkistetaan mitä curlaamalla mitä käy. <br>
<br>
![Description](curl2.png)
<br>
Apachen asennus onnistui ja uuden sivun luonti siinä samassa. <br>
### d) SShouto
