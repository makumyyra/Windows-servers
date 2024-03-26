# Ryhmäkäytäntöjen mallinnus

Tehtävänä oli tehdä RSoP-ryhmäkäytännös mallinnus siitä, mitä käytäntöjä tiettyyn tilanteeseen sovelletaan. Tilanteeksi oli ohjeena annettu "käyttäjä Prod Uction tai Prodnon Uction kirjautuu pöytäkoneelle, joka kuuluu organisaatioyksikköön Desktops". Mallinnuksen nimeksi tulee siis Production on Desktops.

## Mallinnuksen tekeminen

Mallinnuksen tekoon käytettiin tiedostopalvelinta (WindowsServer_DC). Server Managerin Toolsin kautta avattiin Group Policy Management. Sieltä valittiin Group Policy Modeling, ja hiiren oikealla Group Policy Modeling Wizard.

![image](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/ryhmakaytannot2/gpwizard.JPG)

Toimialueen ohjauspalvelin oli tietenkin Controller-Sammakkosuo:

![image](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/ryhmakaytannot2/dcselection.JPG)

"User and Computer Selection"-ikkunassa valittiin, mihin "käyttäjään" (organisaatioyksikkö) ja tietokoneeseen mallinnus kohdistuu. Kohde valittiin selaamalla hakupuuta.

![image](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/ryhmakaytannot2/gpwizard_ldap.JPG)

User Security Groups sekä Computer Security Groups -ikkunoiden valinnaksi laitettiin everyone. Suotimiksi valittiin All linked filters (kohdat WMI Filters for Users ja
WMI Filters for Computers). 

Mallinnuksen yhteenveto:

![image](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/ryhmakaytannot2/yhteenveto.JPG)

### Mallinnuksen tulokset

![image](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/ryhmakaytannot2/summary.JPG)

Tallensin mallinnuksen html-dokumenttina WindowsServer_DC:n työpöydälle. Tiedoston nimi bhd682 - Production on Desktops.htm.

![image](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/ryhmakaytannot2/proddeskhtml.JPG)

## Mallinnuksen hyödyt

Avattuna dokumenttina mallinnus näyttäisi heti esimerkiksi virheet asetuksissa (esim. mahdollisissa rajauksissa). Myös ryhmäkäytäntöjen järjestyksen voi hahmottaa helposti. Mallinnuksen voi toistaa rerun-toiminnolla. Asetuksia on siis helppo korjata ja tarkistaa nopeasti uudelleen. Mallien avulla voidaan siis muokata käyttöjärjestelmää halutulla tavalla (Luomanen 2009). Kun kyseessä on tiedostopalvelin, joka jakaa asetukset kaikille koneille, voidaan helposti hallita yleistä tilannetta esimerkiksi yrityksen eri osastojen kohdalla. Myös ohjelmistoihin voi tehdä käyttörajoituksia, esimerkiksi kytkeä tekstityökalusta oikoluku pois päältä (Luomanen 2009). 

Tuloksista voi suoraan nähdä, mitkä määritykset vaikuttavat Production-käyttäjiin Desktops-tietokoneilla. Jos tulokset eivät ole sitä, mitä haluttiin, ylläpitäjä voi muuttaa ryhmäkäytäntöihin kirjattuja määrityksiä tai käytäntöjen toteutusjärjestystä.



### Lähteet

Luomanen Ville-Veikko 2009. Ryhmäkäytännöt Satakunnan ammattikorkeakoulun liiketoiminnan ja kulttuurin Porin yksikössä. Luettavissa: https://www.theseus.fi/bitstream/handle/10024/3668/Luomanen_Ville.pdf?sequence=1 Luettu 26.3.24.