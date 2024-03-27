Windows-palvelimet - ICI001AS2A-3008
Suvi Sammakkosuo 1.2.24

# Template ja alustus

Tehtävänä oli asentaa virtuaalikone, jota käytetään mallinteena tuleville koneille. Koneeseen asennettiin Windows-käyttöjärjestelmä. Tein asennuksen ohjeiden mukaan.

Annoin virtuaalikoneelle nimeksi WindowsServerTemplate. Generation = generation 1.
Muistimääräksi vaihdoin oletuksen sijasta 3072 MB ja kytkin dynaamisen muistin allokoinnin. Se toimii samoin kuin luottokortin katevaraus: käyttäjä kertoo maksimimäärän, mutta kone "veloittaa" muistia vain tarvitun määrän. 

Luodun verkkoyhteyden adapteriksi valitsin HyperVVSwitchin, joka on luotu itse viime 25.1.24.

Virtuaalikoneelle loin myös kovalevyn, jonka nimi on WindowsServerTemplate.vhdx. VHDX on myös dynaaminen formaatti (toiminta selitetty yllä). Koko on 30 GB.

Virtuaalikoneen työpöydällä oli valmiina käyttöjärjestelmän ISO-image (Optical disc image), joka toimii asennuksessa kuin CD-ROM. Se "syötetään" koneen virtuaaliseen (mielikuvitus-)cd-asemaan ja kone lukee sieltä tiedot. Sen jälkeen "CD" poistetaan virtuaalisesta cd-asemasta.

Tämän jälkeen asennus-Wizard näytti yhteenvedon tulevan koneen konfiguroinnista. Kaikki tiedot olivat oikein (eli ne, jotka oli valittu).

Tämän jälkeen kone näkyi Hyper-V-Managerissa. Sen jälkeen sille piti tehdä prosessorin määritykset. Prosessoriytimiä valitsin oletuksen sijasta neljä. Muuta ei tarvinnut muuttaa.

Seuraavaksi otettiin yhteys virtuaalikoneeseen. Aloin asentaa sille Windows-ympäristöä. 

Ensin otettiin yhteys virtuaalikoneeseen (WindowsServerTemplate) käynnistämällä se tuplaklikkaamalla tai valikosta ("Connect").

Kone käynnistettiin painamalla "Start". Sen jälkeen ensimmäiseksi valittiin yläpalkista Zoom Level = 100 %.

> Asetukset:  
Asennuskieli: English (US)  
Aikavyöhyke: Finland  
Näppäimistökieli: Finnish  
--> Install now.  

Tämän jälkeen hyväksyttiin lisenssiehdot. Varsinaisen asennuksen valinnat:

> Installation: Custom (Install Windows only (advanced)) -->    
Next -->  
...jonka jälkeen alkaa itsestään _"Installing _Windows"_ 

Uudelleenkäynnistymisen jälkeen virtuaalikoneesta otettiin pois ISO-"levy":
Media --> DVD Drive --> Eject "ISO-levyn nimi"

Käyttäjätunnus ja salasana asetettu ohjeen mukaan.

Tämän jälkeen koneella avautui automaattisesti ns. Server Manager. Sieltä näkee yhteenvedon käynnissä olevista palvelinohjelmista (Roles and Server Groups). Yhteenvedossa näkyvien ikkunoiden määrä kasvaa sitä mukaa, kun luodaan uusia tietokoneita.

Local Server -kohdassa näkyy palvelimen konfiguraatiota. Näitä voi muokata tulevaisuudessa, kun luodaan lisää koneita eri toiminnoille (mm. tiedostopalvelin). 

Asennuksen jälkeen templatekoneelle tehtiin vielä alustus (sysprep). Tulevilla koneilla tulee olla eri Security Identifier (SID), jotta koneiden yhteistyö onnistuu. Sysprep generoi SID:n siten, että tämä onnistuu.

Templatekoneella avattiin komentorivi (cmd), jonne annettiin komento
"C:\Windows\System32\Sysprep\sysprep.exe /generalize /shutdown"

Avautuvasta ikkunasta valittiin
> Enter System Out-of-Box Experience
(OOBE)  
Generalize: yes  
Shutdown  

Kun kone oli käynnistynyt uudelleen, annettiin kieliasetukset uusiksi. Tämän jälkeen konetta käytettiin enää "muottina" uusien luomiselle, eli itse templatella ei tehdä mitään toimintoja. 

Kun template oli alustettu, siitä luotiin kaksi uutta konetta. Hyper-V-Managerissa valittiin templatekoneen päällä hiiren oikealla "Export". Tiedosto vietiin uutena tehtävään kansioon HyperV. Virtuaalikoneen kiintolevytiedosto siirtyy automaattisesti polkuun C:\Users\Admin123456\Desktop\HyperV\WindowsServerTemplate\Virtual Hard Disks.

Tiedosto kopioitiin kahdeksi uudeksi koneeksi polkuun C:\Users\Admin123456\Desktop\HyperV\. Koneiden nimiksi tuli

WindowsServer_DC (tuleva ohjauspalvelin) ja
WindowsServer_FileServer (tuleva tiedostopalvelin).


