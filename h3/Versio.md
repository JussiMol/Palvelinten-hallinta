# H3 Versio
### a) Online
Your Repositories sivulta painetaan vihreää painiketta New ja syötetään repon tiedot. <br>
<br>
![Description](repo.png)
<br>
Create Repository ja osio valmis. <br>
### b) Dolly
Teen osion osittain ulkomuistista ja ohjeita käytän kun tulee ongelmia.<br>
Katsotaan onko git valmiiksi jo asennettuna. <br>
<br>
![Description](git.png)
<br>
Kloonaus linux terminaalista. <br>
<br>
![Description](finger.png)
<br>
Ei ole lupaa päästä käsiksi git repoon, täytyy luoda julkinen avain ja lisätä se githubin luotettuihin avaimiin. <br>
Luon julkisen avainparin. <br>
<br>
![Description](luonti.png)
<br>
Avaimen julkinen puoli täytyy lisätä githubiin. <br>
Githubin oikea yläkulma paina avatarin kuvaa -> valikosta settings -> Access osiosta SSH and GPG keys. <br>
New SSH key ja copy pastetaan linux terminaalista julkinen puoli avaimesta. <br>
<br>
![Description](ssh.png)
<br>
Kokeillaan kloonausta uudelleen. <br>
<br>
![Description](clone.png)
<br>
Ilmeisesti onnistui, testataan saadaanko tehtyä muutoksia. <br>
Käytän tässä muistin virkistämiseen https://github.com/joshnh/Git-Commands listan komentoja. <br>
Kotihakemistoon ilmestyi Versiotesti hakemisto. Siirryn sinne ja kokeilen git pull. Everything up to date. <br> 
Teen testitiedoston. Tiedoston lisääminen $ git add testi.md ja perään git commit -m "uusi tiedosto" <br>
Sen jälkeen voidaan puskea tiedosto pilveen. $ git push <br>
<br>
![Description](commit.png)
<br>
Kuvassa on commit komennon alla kommentti jota ei tarvitse noteerata, ajoin komennot uudestaan nätimpää kuvaa varten. <br>
Tarkistetaan tuliko webbiliittymään mitään. <br>
<br>
![Description](uusi.png)
<br>
Tiedosto lisätty onnistuneesti. <br>
### c) Doh!
Kissa käveli näppäimistöllä ja onnistui tekemään muutoksia testitiedostoon ja lisäämään sen $ git add testi.md komennolla. <br>
Perutaan muutokset $ git reset --hard komennolla (muistakaa ne välilyönnit) ja katsotaan korjautuiko tilanne. <br>
<br>
![Description](kissa.png)
<br>
Onnistui. <br>
### d) Tukki
$ git log --patch
<br>
![Description](logi.png)
<br>
Kuvassa logista otettu kaappaus. Siitä voidaan nähdä kuka on tehnyt muutoksia ja milloin.<br>
< jussi@localhost.localdomain > osasta voidaan nähdä mistä muutoksia on tehty (SSH-yhteys) <br>
"uusi tiedosto" on commit -m kommentti joka olisi suotavaa olla englanniksi. <br>
Kommentin alapuolisista tiedoista ei ole täyttä varmuutta, muuta kuin new file (uusi tiedosto) <br>
plus merkin jälkeen on nähtävillä lisäykset. <br>
Katsotaan käyttäjän tiedot. Mikäli ymmärsin <a href="https://stackoverflow.com/questions/46941346/how-to-know-the-git-username-and-email-saved-during-configuration"> config kysymyksen ja vastauksen </a> oikein niin $ git config --global user.name tai user.email on kysely ja $ git config --global user.name "nimi" on lisäys. Testataan <br>
<br>
![Description](conffi.png)
<br>
Näyttäisi olevan "oikeat" tiedot. <br>
