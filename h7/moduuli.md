#H7 Oma moduuli
### Tavoitetila
Moduulin tavoitteena on luoda vagranttia käyttäen kahden koneen herra/orja arkkitehtuuri.<br>
Masterilla käsketään tarvittavien ohjelmien/pakettien asennus, käyttäjän luonti ja konfiguraatiot.<br>
### Alkutoimenpiteet
Master kone. <br>
Debian 11 kone salt-master asennettuna provisioinnin yhteydessä.<br>
Apassi orja. <br>
Debian 11 kone salt-minion asennettuna provisioinnin yhteydessä. <br>
Koneelle asennetaan graafinen käyttöliittymä sekä enemmän prosessointitehoa ja RAM muistia. <br>
<br>
![Description](vagrant.png)
<br>
<br>
Vagrant tiedostoon on saatu pohja Tero Karvisen<a href="https://terokarvinen.com/2023/salt-vagrant/#ready-made-Vagrantfile-for-three-computers">Vagrant artikkelista</a>. Muokkasin tiedostoa hieman ja tein omat lisäykset. <br>
