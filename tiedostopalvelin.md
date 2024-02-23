# Tiedostopalvelimen luominen

Loin uuden virtuaalikoneen aiemmalla viikolla tehdyn templatekoneen sekä aiemmin tehdyn virtuaalikiintolevyn avulla. Templatekoneessa oli tehty sysprep, joten sitä ei tarvinnut tehdä erikseen tälle uudelle koneelle. Koneen nimi on WindowsServer_FileServer, ja kone toimii tiedostopalvelimena. 

![create](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/tiedostopalvelin/ws_fs_luonti.JPG)


## Tiedostopalvelin

### Käynnistys ja alkumääritykset
Koneen käynnistyttyä valitsin sopivat kieliasetukset (templaten avulla luodussa koneessa näitä ei oletuksena oltu valittu). Salasana asetettu ohjeen mukaan.

### Staattisen IP-osoitteen määritys
Katsoin ohjauspalvelimesta komennolla  ```ipconfig /all```, mikä on Default Gateway. Se oli tyhjä, joten palasin aiemman viikon ohjeeseen. Siellä ohje oli merkitty N/A, eli sitä ei kuulunut tehdä. Olisin kuitenkin tehnyt sen, mutta ohje alkoi "avaa työpöydällä oleva DHCP Manager". Sitä ei ollut työpöydällä, enkä löytänyt sitä mistään edes netin ohjeilla. 

![ipconfig_notfound](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/tiedostopalvelin/ws_fs_ipconfig.JPG)

Sen vuoksi **oletusyhdyskäytäväksi on nyt asetettu koulun esimerkki 10.208.0.10** Oletusyhdyskäytävä on nyt sama ohjauspalvelimella ja tiedostopalvelimella.

Verkon asetin yksityiseksi ryhmäkäytäntöeditorissa. (Komentorivillä ```gpedit.msc```, josta Windows Settings/Security Settings/Network List Manager Policies/Network -> Properties.)

Sitten testasin verkkoyhteyttä.
![yhteys](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/tiedostopalvelin/verkkoyht.jpg)


