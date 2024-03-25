# Toimialueen resurssit

Vaiheessa 1 kurssikerralla tutustuttiin aktiivihakemiston sisältöön. Ensin kirjauduttiin ylläpitäjänä WindowsServer_DC -tiedostopalvelimelle ja mentiin Server Managerista Tools-valikkoon. Sitä kautta päästiin "tree view" näkymän valinnalla katsomaan aktiivihakemiston säiliöitä.

!! Lisää tähän kuvakaappaus


## Aktiivihakemiston rakenne ja käyttäjäryhmät

Ensin Users-säiliöön luotiin uusia käyttäjiä (ohjeessa määritellyt Head Quarter ym.). Password-kohdasta valittiin, että salasana ei vanhene ("Password never expires").

Käyttäjät nimettiin osastojen mukaan:
- Headquarters
- Production
- Marketing

Jokainen työnkuva sisälsi kaksi ihmistä (esim. Prod Uction ja Prodnon Uction).

KUVA

Sen jälkeen Users-säiliön alle luotiin kaksi uutta ryhmää (Non-management ja Management). Käyttäjät siirrettiin ryhmiin käyttäjän hierarkian mukaan:
> Create Group -->   
Members -->  
Add -->  
Advances --> 
Find now --> 
valitse listalta käyttäjät aktivoimalla (valitse useita ctrl-näppäin pohjassa) -->  
OK --> OK.  

Tehtävässä lisättiin siis käyttäjät

- Management-rooliin (esim. Prod Uction) tai
- Non-management -rooliin (esim. Prodnon Uction)

Käyttäjät voivat kuulua useampiin ryhmiin kuin yllämainittuun. Esimerkiksi vaikka ryhmä nimeltä "Turun toimipiste". 

## Organisaatioyksiköt

Aktiivihakemistoon luotiin myös organisaatioyksiköt yrityksen ym. osastoille (joille käyttäjät oli jo olemassa). Sen lisäksi luotiin erilliset organisaatioyksiköt erilaisille tietokoneille (mm. läppärit/serverit). Nämä sijoitettiin hierarkkisesti pääyksiköiden alle. Kaikki tietokoneet päätyivät pääorganisaatioyksikköön "Resources" ja kaikki henkilöitä kuvaavat säiliöt pääorganisaatioyksikkön "Accounts" alle.

Ryhmien luonti:
> toimialueen nimi (sammakkosuo) -->  
New -->  
Organizational Unit -->  
nimeä, valitse tahattomalta poistamiselta ("Protect from accidential deletion") -->
OK.

Sekä Accounts -yksikön että Resources-yksikön alle luotiin kolme aliorganisaatioyksikköä samalla tekniikalla. Yllä luodut käyttäjät Users-säiliöstä siirrettiin käyttäjän nimiä vastaaviin yksiköihin.


# Ote aktiivihakemiston rakeenteesta

Aktiivihakemiston LDAP (kansioprotokolla Lightweight Directory Access Protocol) -tietokannasta tehtiin lopuksi kysely, jonka tuloksena saadaan näkyviin aktiivihakemiston rakenne. 
WindowsServer_DC:
> Tools -->  
ADSI Edit -->  
Connect to -->  
OK -->  
Default naming context --> 
New -->  
Query -->  
opiskelijatunnus --> 
Root of Search / Browse / Root of Query / oma toimialue (sammakkosuo)

Query String: "(|(&(objectClass=User)(objectCategory=Person))(objectClass=Computer)(CN=Management)(CN=NonManagement)(objectClass=OrganizationalUnit))"

> Subtree search -->  
OK.  

## Tietokantakyselyn lopputulos: 

![create](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/toimialueen_resurssit/adsi2.JPG)
