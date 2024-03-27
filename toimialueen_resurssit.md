# Toimialueen resurssit

Vaiheessa 1 kurssikerralla tutustuttiin aktiivihakemiston sisältöön. Ensin kirjauduttiin ylläpitäjänä WindowsServer_DC -tiedostopalvelimelle ja mentiin Server Managerista Tools-valikkoon. Sieltä valittiin Active Directory Administrative Center. Administrative Centerissä avattiin "puunäkymän", josta pääsee katsomaan aktiivihakemiston säiliöitä.

![image](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/toimialueen_resurssit/treeview.JPG)


## Aktiivihakemiston rakenne ja käyttäjäryhmät

Aktiivihakemistossa voi katsoa esimerkiksi, mitä ryhmiä tietyt säiliöt sisältävät. Alla säiliön Builtin sisältöä: 

![image](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/toimialueen_resurssit/builtin_in.JPG)

Tehtävässä avattiin ensin Users-säiliö. Sinne luotiin uusia käyttäjiä (ohjeessa määritellyt Head Quarter ym.). 

![image](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/toimialueen_resurssit/newuser.JPG)

Käyttäjiä luodessa Password-kohdasta valittiin, että salasana ei vanhene ("Other password options" --> "Password never expires").

Käyttäjät nimettiin osastojen mukaan:
- Headquarters
- Production
- Marketing

Jokainen työnkuva sisälsi kaksi ihmistä (esim. Prod Uction ja Prodnon Uction). Alla ko. käyttäjä. Kuva otettu myöhemmässä vaiheessa, joten kuvassa käyttäjät ovat siirrettynä luomaani Production-ryhm

![image](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/toimialueen_resurssit/prodnon.JPG)


Sen jälkeen Users-säiliön alle luotiin kaksi uutta ryhmää (Non-management ja Management). 

![image](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/toimialueen_resurssit/nonman.JPG)

Käyttäjät siirrettiin ryhmiin käyttäjän hierarkian mukaan:
> Create Group -->   
Members -->  
Add -->  
Advances --> 
Find now --> 
valitse listalta käyttäjät aktivoimalla (valitse useita ctrl-näppäin pohjassa) -->  
OK --> OK.  

Tehtävässä lisättiin siis käyttäjät

- _Management-rooliin_ (esim. Prod Uction) tai
- _Non-management -rooliin_ (esim. Prodnon Uction)

Käyttäjät voivat kuulua useampiin ryhmiin kuin yllämainittuun. Esimerkiksi vaikka "Project1_responsible" tai "HR".

## Organisaatioyksiköt

Aktiivihakemistoon luotiin myös organisaatioyksiköt yrityksen ym. osastoille (joille käyttäjät oli jo olemassa). Sen lisäksi luotiin erilliset organisaatioyksiköt erilaisille tietokoneille (mm. läppärit/serverit). Nämä sijoitettiin hierarkkisesti pääyksiköiden alle. Kaikki tietokoneet päätyivät pääorganisaatioyksikköön "Resources" ja kaikki henkilöitä kuvaavat säiliöt pääorganisaatioyksikkön "Accounts" alle.

![image](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/toimialueen_resurssit/accounts.JPG)

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

![image](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/toimialueen_resurssit/dbquery1.JPG)

Query String: "(|(&(objectClass=User)(objectCategory=Person))(objectClass=Computer)(CN=Management)(CN=NonManagement)(objectClass=OrganizationalUnit))"

> Subtree search -->  
OK.  

## Tietokantakyselyn lopputulos: 

![image](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/toimialueen_resurssit/adsi2.JPG)
