# Ryhmäkäytännöt 1

Ryhmäkäytäntöjen käsittely aloitettiin määrittelemällä pääsyoikeuksia käyttäjäryhmille. Pääsyoikeuksia lisättiin aiemmin tehdylle Management-ryhmälle ja rajattiin Non-Management-ryhmälle. Määritykset tehtiin jälleen tiedostopalvelimelle (WindowsServer_DC). 

# Pääsyoikeuksien muokkaus:

## Computer Management ja kansiopolku

Ensin valittiin Server Manager -ikkunasta All Servers ja hiiren oikealla palvelinkoneen päällä "Computer Management". Sen alla:
> System Tools -->  
Shared Folders -->   
Shares -->   
Person -->  
Properties.

## Folder Permissions

Person Properties -valikossa mentiin välilehdelle Security. Sen jälkeen:

> Advanced -->  
Disable inheritance:  
"Convert inherited permissions into explicit permissions on this object."  

Näkymässä poistettiin permission entries -listalta Users-ryhmät. Person-jakoon jäivät käyttäjiksi vain ylläpito ja Management.

![image](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/ryhmakaytannot1/share_permissions.JPG)

Sen jälkeen lisättiin ryhmä Management ja sille oikeuksia.
Ryhmän lisäys:

> Add -->    
Select a principal:  
Advanced -->   
Find now:  
Valitse "Management" -->   
OK.

Oikeuksien määrittely:
> Applies to: This folder only -->
Show advanced permissions:  
Valitse "List folder/read data" sekä "Create folders/append data" -->  
OK.

![image](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/toimialueen_resurssit/verkkojako_management.JPG)

![image](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/toimialueen_resurssit/manag_rights.JPG)

## Share Permissions

Person-verkkojaolle määriteltiin vielä verkon yli meneviä oikeuksia. Komennot (alkukohtana Person Properties -ikkuna):
>Share permissions  
Add --> 
Advanced --> 
Find now, valitaan Management -->  
OK --> OK.

![image](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/toimialueen_resurssit/verkkojako_paasyoik.JPG)

Data-verkkojaolle ei laitettu rajauksia, koska kaikilla käyttäjillä haluttiin olevan pääsyoikeus koko jaon sisältöön.

![image](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/ryhmakaytannot1/datashares.JPG)

# Ryhmäkäytäntöjen lisäys

Tehtävä aloitettiin luomalla ryhmäkäytäntöjä organisaatioyksiköille. Tätä varten mentiin taas Toolseihin (Server Manager > Tools > Group Policy Management). Avautuneesta ikkunasta päästiin valitsemaan Domain > Group Policy Objects.

Täällä lisättiin uusia Group Policy Objecteja ("New") äsken luoduille säiliöille.

![image](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/toimialueen_resurssit/group_pol_man.JPG)

Objektit linkitettiin säiliöihin valitsemalla Group Policy Management -ikkunassa oikea organisaatioyksikkö. Hiiren oikealla valittiin "Link and Existing GPO". Avautuvasta ikkunasta pystyi valitsemaan haluamansa ryhmäkäytäntöobjektin. Kun kaikki GPO:t oli linkitetty, lopputulos näytti tältä:

![image](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/toimialueen_resurssit/group_pol_man.JPG)

Group Policy Management -ikkunasta voi valita organisaatioyksikön ja tarkistaa sen ryhmäkäytäntöjä. Linked Group Policy Objectissa voi vaihtaa käytäntöjen linkkijärjestystä. Kaikki organisaatioyksikköön vaikuttavat ryhmäkäytännöt ja niiden periytyvyyden voi tarkistaa välilehdeltä Group Policy Inheritance.

![image](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/toimialueen_resurssit/gpol_inheritance.JPG)

# Ryhmäkäytäntöjen sisältö

Ryhmäkäytäntöjä voi muokata kohdasta Group Policy Management > Domains > sammakkosuo.lan > Group Policy Objects > Default Domain Policy > edit.

Ensin kytkettiin pois päältä Internet Explorer -varoitukset. Yllämainitun polun Default Domain Policyn alta löytyy User Configurations > Preferences > Control Panel Settings > Internet Settings. Hiiren oikealla voi valita New > Internet Explorer 10. Avautuvasta ikkunasta New Internet Explorer 10 ja Security. Otetaan suojattu tila pois käytöstä: 

![image](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/toimialueen_resurssit/gpol_intexpl.JPG)

Sen jälkeen määriteltiin ryhmäkäytäntöä:
Default Domain Policy > Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Password Policy:

Laitetaan päälle "Password must meet complexity requirements".

Interaktiivinen sisäänkirjautuminen laitetaan päälle kohdasta 
Default Domain Policy > Computer Configuration > Policies > Windows Settings > Security Settings > Local Policies > Security Options:
"Interactive Logon: Do not require CTRL+ALT+DEL".

Myös skriptien toteutus kuuluu asettaa päälle. 
Default Domain Policy > Computer Configuration > Policies > Administrative Templates > Windows Components > Windows PowerShell:
Turn on Script Execution (allow local scripts and remote signed scripts).

![image](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/toimialueen_resurssit/scripts.JPG)


## LogonPolicyGP

Logon Policy GP:stä kytkettiin päälle kaksi asetusta. 
LogonPolicyGP > Computer Configuration > Policies > Administrative Templates > System > Logon:
"Always wait for the network at computer startup and logon"
"Do not display network selection UI"

![image](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/toimialueen_resurssit/logon_pol.JPG)


## DesktopsGP

DesktopsGP:stä kytkettiin päälle asetus, joka estää edellisen käyttäjän näytön. 
DesktopsGP > Computer Configuration > Policies > Windows Settings > Security Settings > Local Policies > Security Options:
"Interactive logon: Don't display last signed-in".

![image](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/ryhmakaytannot1/last_signin_notshown.JPG)

Lisäksi 
DesktopsGP > Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Kerberos Policy:
"Enforce user logon restrictions"
"Maximum lifetime for ticket renewal": 3 days.


## ResourcesGP

Määritettiin, että Network Discovery menee toimialueen tietokoneilla päälle automaattisesti.
ResourcesGP >  Computer Configuration > Policies > Administrative Templates > Network > Link-Layer Topology Discovery:
"Turn on Mapper I/O (LTTDIO) driver"
"Turn on Responder (RSPNDR) driver"

![image](https://raw.githubusercontent.com/makumyyra/Windows-servers/main/md_images/ryhmakaytannot1/network_discovery.JPG)

Molempiin valittiin vaihtoehto "Allow operation while in domain".
