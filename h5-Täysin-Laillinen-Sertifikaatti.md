# h5 Täysin Laillinen Sertifikaatti

### Johdanto

Tehtävässä h5 tuli harjoitella...

## Ennakkomateriaalit

Viikon ennakkomateriaalit sisälsivät kaksi artikkelia OWASP:ilta ja viisi artikkelia PortSwigger:iltä. Artikkelit käsittelivät erilaisia hyökkäysmetodeja, jotka tulivat nopeasti ilmi PortSwigger Labs -harjoituksissa.

OWASP:in artikkelissa "Broken Access Control" käsiteltiin rikkinäisiä käyttöoikeuksia, joka on yksi tietoturvaongelmista OWASP:in TOP 10 -listauksissa. Rikkinäiset käyttöoikeudet tarkoittavat tilanteita, jossa tunnistetut tai tunnistamattomat käyttäjät pääsevät käsiksi sisältöihin, joihin heillä ei pitäisi olla oikeuksia.

OWASP:in artikkelissa "Server Side Request Forgery" käsiteltiin palvelinpyynnön väärennyksiä, joka on samaan tapaan OWASP:in TOP 10:ssä. Palvelinpynnön väärennyksissä palvelin manipuloidaan tekemään pyyntöjä epäautorisoidusta osoitteesta.

PortSwigger:in artikkelissa "Insecure direct object references (IDOR)" käsiteltiin sisäisten tietojen paljastumista "Broken Access Control" -hyökkäyksen kautta. Yleistä on esimerkiksi virhe sivustolla, jossa käyttäjä voi nähdä toisen käyttäjän profiilin muuttamalla osoitteessa `https://insecure-website.com/customer_account?customer_number=132355` muuttujna `customer_number=` eri numeroksi.

PortSwigger:in artikkelissa "Path traversal" käsiteltiin samaan tapaan "Broken Access Control" -hyökkäystä, jossa sivuston piilotettuihin tietoihin voi päästä esimerkiksi lisäämällä osoitteeseeen `/passwd`. Tässä tapauksessa hyökätään tiedostotasolla, kun taas IDOR tähtää verkkosovellustason tietoihin.

PortSwigger:in artikkelissa "Server-side template injection" käsiteltiin hyökkäystä, jossa käyttäjän antama syöte suoritetaan suoraan palvelimella, sen sijaan että sitä käsiteltäisiin datana.

PortSwigger:in artikkelissa "Server-side request forgery (SSRF)" käsiteltiin samaa hyökkäystä kuin OWASP:in artikkelissa.

PortSwigger:in artikkelissa "Cross-site scripting" käsiteltiin hyökkäystä, jossa käyttäjän antama haitallinen syötä suoritetaan muiden käyttäjien selaimessa.

### Lähteet

https://owasp.org/Top10/A01_2021-Broken_Access_Control/

https://owasp.org/Top10/A10_2021-Server-Side_Request_Forgery/

https://portswigger.net/web-security/access-control/idor

https://portswigger.net/web-security/file-path-traversal

https://portswigger.net/web-security/server-side-template-injection

https://portswigger.net/web-security/ssrf

https://portswigger.net/web-security/cross-site-scripting

## Sertifikaatti kettuilua (Kohdat A & B)

Asensin aluksi OWASP ZAP -työkalun komennolla `sudo apt-get install zaproxy`. OWASP ZAP on työkalu, jolla voi valvoa, kaapata ja muokata HTTP/HTTPS liikennettä laitteen ja verkkosovelluksen välillä.

![image](https://github.com/user-attachments/assets/20ef640b-7f2d-42a3-b5cc-4072bd7ef8b4)

_OWASP ZAP asennettu_

Asensin lisäksi "FoxyProxy Standard"-selainlisäosan, jolla voi täydentää Firefoxin puuttuvia proxy-toimintoja.

![image](https://github.com/user-attachments/assets/c3920a4f-6237-43d9-97fa-548305ca3de9)

_FoxyProxy Standard asennettu_

Seuraavaksi nappasin OWASP ZAP -sertifikaatin talteen uuteen kansioon `certificates`. Sertifikaatti löytyi OWASP ZAP:in `network`-asetuksista.

Kävin lisäämässä tämän sertifikaatin Firefoxin sertifikaattimanageriin. Sertifikaatin avulla Firefox sallii ZAP:in tarkastelemaan ja muuttaamaan liikennettä.

![image](https://github.com/user-attachments/assets/aefd0368-9ddc-4f56-9725-a379717fcf05)

Lopuksi tuli vielä sallia kuvien sieppaaminen.

![image](https://github.com/user-attachments/assets/b258d395-7be2-4f69-bd42-076e00eca66c)

ZAP kuuntelee oletusasetuksilla yhteyksien varalta paikallisesti, joka toimii tässä tapauksessa.

![image](https://github.com/user-attachments/assets/1fe036e1-0de8-457c-8ad9-177da1ca9de2)

Konfiguroin seuraavaksi FoxyProxyn toimimaan ZAP:in kanssa. Voin käyttää `patterns`-osiota hyväksymään liikenteen proxyn kautta vain silloin, kun sen lähde täsmää annettua vaatimusta. Täten vältetään liikenne ZAP:iin ulkoisista lähteistä. 

![image](https://github.com/user-attachments/assets/a006c06d-d175-45b4-b17a-c64b299e070e)

Käynnistin tämän jälkeen selaimen uudelleen ja kokeilin käyttää ZAP:iä localhostin ja Metasploitablen kanssa, sillä nämä ovat turvallisia maaleja. FoxyProxyn `Proxy by Patterns` -asetuksen tulisi suorittaa kaikki muu liikenne ilman proxyä.

![image](https://github.com/user-attachments/assets/09ff8a7e-debc-46b3-b12b-7de2c11505fc)

Lopputuloksissa näkyy, että `localhost` antaa vastauksen `502 - Bad Gateway`, koska siellä ei ole mitään. Metasploitable on antanut muutaman timeoutin, mutta oikean yhteyden kytkiessä se antaa positiivisen `200 - OK` vastauksen. Wikipedia on myös auki, mutta mikään liikenne ei tule sieltä proxyn kautta, sillä sitä ei ole sallitu patterneissa.

## PortSwigger Labs (Kohdat C-J)

Tässä osiossa tein vaaditut harjoitukset PortSwigger Labs:istä. Tämä on palvelu, jossa voi turvallisesti harjoitella käytännössä erilaisia verkkosovellushyökkäyksiä.

Muokkasin ensin ProxyFoxy:ä siten, että se käyttää Proxya ainoastaan harjoitemaaliympäristössä. Pistin ensin kaikki työkalut pois päältä ja navigoin labratilaan. Labratilojen URL osoite sisältää `web-security-academy.net` ja tätä ei käytetä muualla PortSwiggerin sivuilla. Täten FoxyProxyn asetus `https://*.web-security-academy.net/*` varmistaa, että en vahingossa käytä proxy-yhteyttä muualla kuin labrassa.



## Pencode (Kohta K)

## Yleiset Lähteet
