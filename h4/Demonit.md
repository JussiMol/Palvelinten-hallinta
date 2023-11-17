# Demonit
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
## a) Hello SLS!
Aloitan käynnistämällä herran ja 2 orjaa vagrantilla. <br>

