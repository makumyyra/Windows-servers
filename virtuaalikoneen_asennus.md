Windows-palvelimet - ICI001AS2A-3008
Suvi Sammakkosuo 1.2.24

# Virtuaalikoneiden malline ja palvelinkoneiden alustus

##1. Hyper-V: uuden Windows Server -virtuaalikoneen luominen

Tehtävänä oli asentaa virtuaalikone, jota käytetään mallinteena tuleville koneille. Koneeseen asennettiin Windows-käyttöjärjestelmä. Tein asennuksen ohjeiden mukaan.

Annoin virtuaalikoneelle nimeksi WindowsServerTemplate. Generation = generation 1.
Muistimääräksi vaihdoin oletuksen sijasta 3072 MB ja kytkin dynaamisen muistin allokoinnin. Se toimii samoin kuin luottokortin katevaraus: käyttäjä kertoo maksimimäärän, mutta kone "veloittaa" muistia vain tarvitun määrän. 

Luodun verkkoyhteyden adapteriksi valitsin HyperVVSwitchin, joka on luotu itse viime 25.1.24.

Virtuaalikoneelle loin myös kovalevyn, jonka nimi on WindowsServerTemplate.vhdx. VHDX on myös dynaaminen formaatti (toiminta selitetty yllä). Koko on 30 GB.

Virtuaalikoneen työpöydällä oli valmiina käyttöjärjestelmän ISO-image (Optical disc image), joka toimii asennuksessa kuin CD-ROM. Se "syötetään" koneen virtuaaliseen (mielikuvitus-)cd-asemaan ja kone lukee sieltä tiedot. Sen jälkeen "CD" poistetaan virtuaalisesta cd-asemasta.

Tämän jälkeen asennus-Wizard näytti yhteenvedon tulevan koneen konfiguroinnista. Kaikki tiedot olivat oikein (eli ne, jotka oli valittu).

Tämän jälkeen kone näkyi Hyper-V-Managerissa. Sen jälkeen sille piti tehdä prosessorin määritykset. Prosessoriytimiä valitsin oletuksen sijasta neljä. Muuta ei tarvinnut muuttaa.

## 2. & 3. Windowsin asennus ja tutustuminen hallintanäkymään

Seuraavaksi avattiin yhteys virtuaalikoneeseen. Aloin asentaa sille Windows-ympäristöä. 

Aikavyöhykkeeksi ja näppäimistökieleksi valitsin ohjeiden mukaan suomen. 

Windowsin asennus kesti noin 10 minuuttia.

Tämän jälkeen yritin käynnistää koneen. Käynnistys epäonnistui (vikailmoitus). Azure-kone kysyi, haluanko käynnistää Windows-koneen uudelleen. Vastasin kyllä. Tällä kertaa kone käynnistyi. 

Salasana asetettu ohjeen mukaisesti.

Käytin yläpalkissa olevaa Action + ctrl+alt+del, jolla pääsin antamaan salasanan.

Kone avautui Server Manager / Dashboard -ikkunaan. Tutustuin Dashboardiin.

## 4. Palvelinkoneen alustus

Tekemääni WindowsServerTemplate-konetta tulee voida käyttää muiden kurssilla tarvittavien virtuaalikoneiden asennukseen. Alustin siis palvelinkoneen sysprep-työkalulla. Koneessa ei vielä ollut konfigurointeja, joten käytännössä sysprep poisti koneen sisäisen tunnistetiedon. Järjestelmästä tuli siis neutraali alusta. 

Sysprep käynnistettiin komentoriviltä, jonne mennään seuraavasti:
Server Manager -->
All servers --> w
Command prompt / Run as administrator --> 
Komento: C:\Windows\System32\Sysprep\sysprep.exe /generalize /shutdown

Sysprep Preparation Toolista valitsin "generalize" ja shutdown operations: Shutdown. 

## 5. Mallikoneen käyttäminen muiden palvelinkoneiden pohjana

Sysprepin ja koneen uudelleen käynnistymisen jälkeen sammutin WindowsServerTemplaten. Hyper-V-Managerissa valitsin koneen kohdalta Export. Tallensin tiedoston uuteen kansioon. Tiedoston loppu on .vhdx (=Virtual Hard Disk). 
Kopioin tiedoston kahdeksi uudeksi tiedostoksi ohjeen mukaan.


## Dokumentaatio domainista

### Verkkokortti
10.208.0.1

### Template Admin tunnukset asetettu ohjeen mukaan.
