# Tiedostopalvelin

Loin uuden virtuaalikoneen aiemmalla viikolla tehdyn templatekoneen sekä aiemmin tehdyn virtuaalikiintolevyn avulla. Templatekoneessa oli tehty sysprep, joten sitä ei tarvinnut tehdä erikseen tälle uudelle koneelle. Koneen nimi on WindowsServer_FileServer, ja kone toimii tiedostopalvelimena. 

![create](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/tiedostopalvelin/ws_fs_luonti.JPG)

### Käynnistys ja alkumääritykset
Koneen käynnistyttyä valitsin sopivat kieliasetukset (templaten avulla luodussa koneessa näitä ei oletuksena oltu valittu). Salasana asetettu ohjeen mukaan.

### Staattisen IP-osoitteen määritys
Katsoin ohjauspalvelimesta komennolla  ```ipconfig /all```, mikä on Default Gateway. Se oli tyhjä, joten palasin aiemman viikon ohjeeseen. Siellä ohje oli merkitty N/A, eli sitä ei kuulunut tehdä. Olisin kuitenkin tehnyt sen, mutta ohje alkoi "avaa työpöydällä oleva DHCP Manager". Sitä ei ollut työpöydällä, enkä löytänyt sitä mistään edes netin ohjeilla. 

![ipconfig_notfound](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/tiedostopalvelin/ws_fs_ipconfig.JPG)

Sen vuoksi **oletusyhdyskäytäväksi on nyt asetettu koulun esimerkki 10.208.0.10** Oletusyhdyskäytävä on nyt sama ohjauspalvelimella ja tiedostopalvelimella.

Verkon asetin yksityiseksi ryhmäkäytäntöeditorissa. (Komentorivillä ```gpedit.msc```, josta Windows Settings/Security Settings/Network List Manager Policies/Network -> Properties.)

Sitten testasin verkkoyhteyttä.

![yhteys](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/tiedostopalvelin/verkkoyht.jpg)

Tiedostopalvelimen nimesin ohjeen mukaan. Tämän jälkeen kone käynnistyi uudelleen.

### Käyttöjärjestelmän aktivointi

Slmbr.vbs-työkalulle (Software Licensing Management Tool) kerroin ```slmgr /skms kms.core.windows.net ``` -komennolla, missä aktivointipalvelin on. Aktiovintipyyntö suoritettiin komennolla ```slmgr /ato```

![kms-sijainti](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/tiedostopalvelin/kms_aktivointi.jpg)

![kms-aktivointi](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/tiedostopalvelin/kms_aktivointi2.jpg)

### Toimialueeseen liittäminen

Seuraavaksi uusi kone (tiedostopalvelin) piti liittää ohjauspalvelimen määrittämään toimialueeseen. Tämä tehtiin Server Managerissa (Local Server / Workgroup). Täältä vaihdettiin ohjauspalvelimen verkossa näkyvä nimi. Toimialueen nimeksi syötetty ohjeen mukaan. Liittyminen vahvistettiin admin-tunnuksilla.

![toimialue](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/tiedostopalvelin/toimialue.jpg)


# Tiedostopalvelun asentaminen ohjauspalvelimen kautta

Ohjauspalvelimelta voi hallita muita tietokoneita ja asentaa siihen palveluja (kuten ohjelmistoja). Tätä kutsutaan etähallinnaksi.

### Lisäys palvelinlistaan

Tiedostopalvelin lisättiin palvelinlistaan ohjauspalvelimen kautta (Server Manager -> Manage -> Add Servers.) Päällä oleva tiedostopalvelin löytyi valmiiksi listasta Active Directory.

Kun tiedostopalvelin oli lisätty ohjauspalvelimen tuntemien palvelinten listaan, sille lisättiin tiedostopalvelun rooli.

### Tiedostopalveluroolin lisäys

Tällä hetkellä uuden virtuaalikoneen nimi on FileServer, mutta se ei vielä voi toimia tässä roolissa. Rooleja voi asettaa ohjauspalvelimen Server Managerista. Jälleen Manage-kohdasta löytyy Add roles and features. Sitä klikkaamalla aukeaa asennus-wizard.

Tiedostopalvelin löytyi suoraan kohdepalvelimien listasta.

![serverpool](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/tiedostopalvelin/addserver1.jpg)

Valintojen syöttämisen jälkeen sain yhteenvedon valinnoista:

![selections](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/tiedostopalvelin/addserver.jpg)

Asennuksen jälkeen tiedostopalvelin uudelleenkäynnistettiin etäkomennolla ohjauspalvelimen kautta.

# Verkkojakojen luonti

Seuraavaksi tehtiin verkkojako tiedostopalvelimen kautta. Tämäkin löytyy Server Managerista (File and Storage Services -> Shares). Sieltä aukesi jälleen uusi wizard. Verkkojaon poluksi valittiin uusi kansio (Files). Jaolle annettiin nimeksi Data. Jakoasetuksista valittiin jaon laittaminen välimuistiin (allow caching of share), jotta verkkojako säilyttää annetut tiedostot ja kansiot myös tiedostopalvelimen ollessa tavoittamattomissa. Kun olin valinnut kaikki oikeat asetukset, sain yhteenvedon valinnoista. 

![share_selections](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/tiedostopalvelin/share.jpg)

Seuraavaksi piti ottaa ohjauspalvelimelta yhteys tiedostopalvelimeen. Sain kuitenkin virheviestin, joka johtui tiedostopalvelimen palomuurin asetuksista. Niitä muokattiin tiedostopalvelimen kautta. Inbound Rules -kohdasta piti laittaa päälle kolme sääntöä.

![inbound-rules](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/tiedostopalvelin/sharekorjaus.jpg)

Tämän jälkeen tiedostopalvelin salli sisäänpäin tulevan liikenteen tarvittaviin portteihin. Ohjauspalvelimelta pääsi nyt avaamaan tiedostopalvelimen hallintanäkymän (Computer Management -> ). Uusi jako valittiin Shares-osiosta valitsemalla hiiren oikealla New Share. Tämän jälkeen jako asennettiin Shared Folder -wizardilla. Jakohakemiston kansioksi tehtiin uusi kansio nimeltä Management. Jaon nimi asetettu ohjeen mukaan.

Valintojen jälkeen wizard tarjosi yhteenvedon.

![sharefolder](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/tiedostopalvelin/sharefolder.jpg)

Verkkojaon jälkeen jako näkyi tiedostopalvelimella Server Managerin kautta.

![share_ready](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/tiedostopalvelin/sharesdone.jpg)


