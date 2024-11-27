# h5 Täysin Laillinen Sertifikaatti

### Johdanto

Tehtävässä h5 tuli harjoitella webbipalveluihin murtautumista ja ohessa tuli vastaan työkaluja kuten `OWASP ZAP`, `Foxyproxy` ja `Pencode`. Harjoitukset tehtiin `PortSwigger`-harjoittelutilassa. En ollut tällä viikolla oppitunnilla, joten aihe oli aloittaessa vieras.

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

OWASP Foundation 2021: A01: Broken Access Control. Luettavissa: https://owasp.org/Top10/A01_2021-Broken_Access_Control/. Luettu 28.11.2024.

OWASP Foundation 2021: A10: Server-Side Request Forgery (SSRF). Luettavissa: https://owasp.org/Top10/A10_2021-Server-Side_Request_Forgery/. Luettu 28.11.2024.

PortSwigger Ltd: Access Control - Insecure Direct Object References (IDOR). Luettavissa: https://portswigger.net/web-security/access-control/idor. Luettu 28.11.2024.

PortSwigger Ltd: File Path Traversal. Luettavissa: https://portswigger.net/web-security/file-path-traversal. Luettu 28.11.2024.

PortSwigger Ltd: Server-Side Template Injection. Luettavissa: https://portswigger.net/web-security/server-side-template-injection. Luettu 28.11.2024.

PortSwigger Ltd: Server-Side Request Forgery (SSRF). Luettavissa: https://portswigger.net/web-security/ssrf. Luettu 28.11.2024.

PortSwigger Ltd: Cross-Site Scripting (XSS). Luettavissa: https://portswigger.net/web-security/cross-site-scripting. Luettu 28.11.2024.

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

ZAP kuunteli oletusasetuksilla yhteyksien varalta paikallisesti, joka toimi tässä tapauksessa.

![image](https://github.com/user-attachments/assets/1fe036e1-0de8-457c-8ad9-177da1ca9de2)

Konfiguroin seuraavaksi FoxyProxyn toimimaan ZAP:in kanssa. Käytin `patterns`-osiota hyväksymään liikenteen proxyn kautta vain silloin, kun sen lähde täsmää annettua vaatimusta. Täten vältin liikenteen ZAP:iin ulkoisista lähteistä. 

![image](https://github.com/user-attachments/assets/a006c06d-d175-45b4-b17a-c64b299e070e)

Käynnistin tämän jälkeen selaimen uudelleen ja kokeilin käyttää ZAP:iä localhostin ja Metasploitablen kanssa, sillä nämä ovat turvallisia maaleja. FoxyProxyn `Proxy by Patterns` -asetuksen tulisi suorittaa kaikki muu liikenne ilman proxyä.

![image](https://github.com/user-attachments/assets/09ff8a7e-debc-46b3-b12b-7de2c11505fc)

Lopputuloksissa näkyi, että `localhost` antoi vastauksen `502 - Bad Gateway`, koska siellä ei ollut mitään. Metasploitable oli antanut muutaman timeoutin, mutta oikean yhteyden kytkiessä se antoi positiivisen `200 - OK` vastauksen. Wikipedia oli myös auki, mutta mikään liikenne ei tullut sieltä proxyn kautta, sillä sitä ei oltu sallitu patterneissa.

## PortSwigger Labsc& IDOR (Kohta C)

Tässä osiossa tein vaaditut harjoitukset PortSwigger Labs:istä. Tämä on palvelu, jossa voi turvallisesti harjoitella käytännössä erilaisia verkkosovellushyökkäyksiä.

Muokkasin ensin ProxyFoxy:ia siten, että se käyttää Proxya ainoastaan harjoitemaaliympäristössä. 

Labratilojen URL-osoite sisälsi `web-security-academy.net` ja tätä ei käytetä muualla PortSwiggerin sivuilla. Täten FoxyProxyn asetus `http://*.web-security-academy.net/*` & `https://*.web-security-academy.net/*` varmistaa, että en vahingossa käyttänyt proxy-yhteyttä muualla kuin labrassa.

![image](https://github.com/user-attachments/assets/61f018b9-2606-4b25-958a-0c675e9e44d2)

_Labraa kaappaa, etusivua ei_

Käytin seuraavissa tehtävissä vinkkejä tarvittaessa, mutta pyrin nojautumaan aikaisempaan ennakkomateriaaliin jo tietämykseen, tai uuden tiedon etsimiseen.

Kohdassa `Insecure direct object references` tuli löytää käyttäjän `carlos` salasana ja kirjautua sisään heidän tunnuksillaan. Labran etusivu on tunkeutujan kannalta melko tylsä verkkokauppasivu. Ainoat linkit jotka menevät minnekkään ovat `My account`, joka menee kirjautumissivulle ja `Live chat`, joka menee chat-palveluun.

Chat-palvelussa viestin lähettäminen ei tehnyt mitään, mutta `View transcript` latasi tyhjän tekstitiedoston koneelle. Tekstitiedosto oli kuitenkin kiinnostavasti nimeltään `2.txt`, eli `1.txt` tai `0.txt` oli jo käytetty jossain. Tämä vihjasi IDOR-heikkouteen, jossa voisin nähdä muiden käyttäjien tietoja muuttamalla `.txt`-tiedoston muuttujaa. 

ZAP:ia tarkastaessa näkyi, että olin lähettänyt pyynnön `GET https://0af500eb030b6a47823f700d000e0054.web-security-academy.net/download-transcript/2.txt HTTP/1.1`. Jos chatti-logeja voi pyytää näin yksinkertaisesti, niin mitä jos lähetän saman pyynnön, mutta tällä kertaa `1.txt` osalta?

![image](https://github.com/user-attachments/assets/cf2e35e4-17c4-4baa-9cd4-74e342ac0316)

_Bingo_

Kokeilin saamaani salasanaa vielä kirjautumisnäytössä. Pääsin kirjautumaan tilille ja vaihtamaan tilin sähköpostin. Kiitos tilistä carlos!

![image](https://github.com/user-attachments/assets/a381773f-a431-4adf-ba9a-39b4a9c13844)

_IDOR ratkaistu_

## File Path Traversal (Kohdat D-F)

Labra `File path traversal, simple case` avautuu samanlaisessa kauppasivussa kuin kohdassa C. Tällä kertaa vain kuvia voi klikata. Tehtävänä oli löytää `/etc/passwd` ja näpistää sen sisällöt. Kuvaa klikatessa syntyi liikennettä. Kiinnostavasti sivusto haki kuvaa tiedostonimellä (tässä tapauksessa `18.jpg`). 

![image](https://github.com/user-attachments/assets/8f8d3c9e-5a5b-42dc-8131-50489df2c0fe)

_Kuva pyyntö_

Tämä oli altis hyökkäykselle, sillä sivusto luottaa käyttäjän antavan validin parametrin tiedostonimelle. Tosiasiassa voin luoda oman pyynnöön omalla parametrilla `../../../../etc/passwd`, joka pyrki navigoimaan haluttuun kansioon. Parametri `../` kiipeää takaisin ylös tiedostopuuta, jotta voisimme päätyä `/etc/`-kansioon.

![image](https://github.com/user-attachments/assets/951c589d-27b2-4369-b788-d6b51bb6b4bf)

_Kuva pyyntö, oma payload_

![image](https://github.com/user-attachments/assets/88b12634-56ee-4af4-8a70-ed7f642d29db)

_Tiedot hallussa_

![image](https://github.com/user-attachments/assets/991ceec2-0503-4551-a94b-032546e9f6fc)

_Simple FPT ratkaistu_

Labra `File path traversal, traversal sequences blocked with absolute path bypass` oli pitkälti samanlainen kuin aikaisempi, mutta tässä tapauksessa voimn siirtyä suoraan `/etc/passwd`-kansioon, sillä sitä ei oltu suojattu mitenkään.

![image](https://github.com/user-attachments/assets/911daf16-494e-4c7f-8282-55ab037d16b2)

![image](https://github.com/user-attachments/assets/f910e7ac-23cd-4451-9660-af8beb1bcc44)

![image](https://github.com/user-attachments/assets/a6511f7b-45db-46bb-9eff-43c949e62adf)

![image](https://github.com/user-attachments/assets/f0a0ee3e-71b2-4102-b431-8a03a7ac8286)

Labra `File path traversal, traversal sequences stripped non-recursively` seurasi samaa mallia, mutta sisälsi oman erikoisuutensa. Tässä tapauksessa palvelin osasi suodattaa niin suoran hyökkäyksen `/etc/passwd` kuin aikaisemman kiertoreitin `../../../etc/passwd`. Harmillisesti syötteeseen voi kuitenkin lisätä enemmän osia; `....//....//....//etc/passwd`. Tällöin järjestelmä kyllä suodattaa `../`-osuuden, mutta jäljelle jää vielä toinen samanlainen.

![image](https://github.com/user-attachments/assets/c6f33574-8f62-4cba-87a7-43ff0907133f)

![image](https://github.com/user-attachments/assets/763eb9cd-6f3e-4204-ad91-37b54c4d4136)

## SSTI (Kohta G)

Labrassa `Server-side template injection with information disclosure via user-supplied objects` tuli hyödyntää `server-side template injection`-hyökkäystä ja varastaa sen avulla salainen avain.

Kirjauduin ensin sisään annetuilla tunnuksilla. Löysin myös tuotteiden "edit template" ja "preview napit, jotka antavat vähän lisää tietoa. Jouduin heti tämän jälkeen siirtyä ohjeen pariin, sillä aihepiiri oli itselle varsin tuntematon. 

Videon mukaisesti komennolla `{{a'toUpperCase()}}` saa virheilmoituksen, joka kertoo palvelimen mallin olevan `Django`.

![image](https://github.com/user-attachments/assets/7c986ef5-b68b-4240-be01-6aa5d70e9766)

Djangossa komento `{% debug %}` antoi minun nähdä kaikki saatavilla olevat resurssit.

![image](https://github.com/user-attachments/assets/976b112f-5ab3-4ee9-b69c-18abe75abf90)

![image](https://github.com/user-attachments/assets/1a95bf5c-26e1-4528-9e08-3eed82071f7a)



## SSRF (Kohta H)

Labrassa `Basic SSRF against the local server` tuli hyökätä `https://localhost/admin` sivulle hyödyntämällä `SSRF`-hyökkäystä.

Tuotetta klikatessa löytyy nappi `check stock`, joka mainitaan tehtävänannossa. `SSRF`-hyökkäyksessä tämänlainen pyyntö ja palaute voi olla varsin hyödyllinen.

![image](https://github.com/user-attachments/assets/e68323cc-a65c-49f1-b7d9-e6d35c54c6e4)

_stockApi-pyyntö_

Samaan tapaan kuin aikaisemminkin, voimme lähettää oman pyynnön uusilla parametreillä. Tässä tapauksessa halusin nähdä pääseekö `http://localhost`-puolelle tätä kautta.

![image](https://github.com/user-attachments/assets/7a069824-edb1-4fd0-b80f-143697626dce)

Localhost pyyntö toi minut paikalliselle puolelle, jossa löysin HTML:stä admin paneelin. Komennolla `http://localhost/admin/delete?username=carlos`, saadaan pikaisesti pyyhittyä pois carloksen tili. Nämä toimivat, sillä palvelua ei oltu varmistettu paikallisesti ja sille puolelle pääsi yksinkertaisella väärällä pyynnöllä.

![image](https://github.com/user-attachments/assets/ed3ad226-e7bf-445e-b134-b6f2a554bdcc)

## XSS (Kohdat I & J)

Labrassa `Reflected XSS into HTML context with nothing encoded` tuli hyödyntää `XSS`-hyökkäystä kutsumalla `alert`-funktiota.

Suuntasin luontaisesti ensimmäiseen syöttökenttään minkä näin ja pastesin komennon `<script>alert(1)</script>` joka kutsuu `alert`-funktiota.

![image](https://github.com/user-attachments/assets/ea1c0751-c166-45aa-a9fd-2a0e9102b624)

![image](https://github.com/user-attachments/assets/e68a28fe-d4d7-4720-a080-8f54f996237c)

Tämä toimi, sillä käyttäjän syötteet pyörivät suoraan javascriptinä eikä niitä suodateta.

Seuraavassa labrassa `Stored XSS into HTML context with nothing encoded` ei ollut samanlaista hakukenttää mihin syöttää komentoa. Klikkaamalla artikkelia sai kuitenkin kommenttikentän näkyviin. Syötteen muodolla ei ole oikeastaan väliä jos haavoittuvuus on yhä sama.

![image](https://github.com/user-attachments/assets/d98619b6-d902-4704-953f-8098704f911d)

![image](https://github.com/user-attachments/assets/28878ff7-a57c-408d-a725-a7c185397c71)

_Hups_

## Pencode (Kohta K)

Kohdassa K tuli ladata vielä `Pencode`-työkalu ja muuntaa sillä jokin merkkijono. Latasin Pencoden github-repositiosta, purin tiedoston `tar`-komennolla ja siirsin sen `usr/local/bin`-hakemistoon. Lopulta kokeilin käyttää sitä.

![image](https://github.com/user-attachments/assets/71f8e114-50b3-4233-88c6-35926b551994)

## Yleiset Lähteet

Pohjana Tero Karvinen 2024: Tunkeutumistestaus -kurssi. Luettavissa: https://terokarvinen.com/tunkeutumistestaus/.

Tätä dokumenttia saa kopioida ja muokata GNU General Public License (versio 2 tai uudempi) mukaisesti. http://www.gnu.org/licenses/gpl.html
