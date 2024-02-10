# Ohjauspalvelimen luonti

### Ohjauspalvelimen rungon määritys
Aloitin luomalla Wizardin avulla uuden tietokoneen nimeltä WindowsServer_DC. Virtuaalikovalevynä käytettiin jälleen aiemmin tehtyä WindowsServer_DC-kovalevyä. Koneen luomisen jälkeen kasvatin prosessoriytimien määrän neljään.

Seuraavaksi käynnistin uuden koneen. Koska templatekoneelle oli tehty sysprep, käynnistyksen yhteydessä kysyttiin kieli- ja näppäimistöasetuksia. Salasana asetettu koulun ohjeen mukaisesti. Ohjauspalvelinkoneen nimi on Sammakkosuo-Controller.

### Ohjauspalvelimen käyttöjärjestelmän aktivointi

Avasin komentorivin (cmd) adminina. 
Suoritin komennot
``` 
- slmgr.vbs (parametrien listaus / Windows Script Host)
- slmgr.vbs /dlv (nykyaktivoinnin tila)
- slmgr /skms kms.core.windows.net (kms-palvelimen sijainnin asetus)
- slmgr /ato
```

Tämän jälkeen sain virheviestin siitä, että kms:ään (Key Management Serviceen) ei saa yhteyttä.
Yritin tarkistaa osoitetta komennolla 
``` nslookup kms.core.windows.net” ``` 

Tästä seurasi virheviesti "Unknown can't find kms.core.windows.net: No response from server"

![no-response](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/no-response.JPG)

En tiennyt, missä vika oli, joten aloin käydä asennusohjeita läpi uudelleen. Huomasin, että olin vahingossa unohtanut määrittää staattisen IP-osoitteen. Kävin asettamassa sen. Samalla asetin koneen verkon yksityiseksi. IP-osoite määriteltiin Server Managerin kautta (Local Server --> Ethernet --> Properties --> IPv4 / Properties).

![ip](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/ip.png)

Näiden jälkeen verkkoyhteys toimi odotetusti. 
![ok](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/verkkoyhteys_ok.jpg)

Asetin seuraavaksi lukitusnäytön pois päältä, mutta jostain syystä kone kysyy sitä edelleen. Ongelma ei kuitenkaan ole iso, joten siirryin ohjeissa eteenpäin.
![lockscreen](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/lockscreen.jpg)

Seuraavaksi yritin aktivoida ohjauspalvelimen käyttöjärjestelmän uudestaan. Tällä kertaa kms:n määritys ja aktivointi onnistuivat.
![kms-ok](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/kms_ok.jpg)
![aktivointi-ok](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/winss.jpg)


## Ohjauspalvelimen AD DS (Active Directory Domain Services) -ohjelmiston lisäys ja roolin määritys

Lisäys suoritettu ohjeiden mukaan Server Managerin Wizardin avulla. Wizardiin syötettiin ohjeessa mainitut tiedot. Tämän jälkeen palvelin alkoi asentaa AD DS -roolia. 
![adds-role](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/adds_install.jpg)

AD DS -ohjelmiston asentamisen jälkeen virtuaalikone piti vielä korottaa ohjauspalvelimen rooliin. Tämä tehtiin myös Wizardin avustuksella (Server Manager --> All Servers Task Details --> Promote this server to a domain controller). Toimialueen määrittelyssä valitsin add a new forest. Salasanan asetin ohjeen mukaan. Wizardin käytön loputtua kone käynnistyi uudelleen.

