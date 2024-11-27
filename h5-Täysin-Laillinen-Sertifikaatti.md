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

Kävin lisäämässä tämän sertifikaatin Firefoxin sertifikaattimanageriin.

![image](https://github.com/user-attachments/assets/aefd0368-9ddc-4f56-9725-a379717fcf05)

Lopuksi tuli vielä sallia kuvien sieppaaminen.

![image](https://github.com/user-attachments/assets/b258d395-7be2-4f69-bd42-076e00eca66c)

Sertifikaatin avulla Firefox sallii ZAP:in tarkastelemaan ja muuttaamaan liikennettä ilman varoituksia.

Asetin OWASP ZAP:in selaimen proxyksi muokkaamalla sen ja Firefoxin proxyasetukset localhostiksi.

![image](https://github.com/user-attachments/assets/6ccc6d25-f64d-48f0-a31d-ffd2acdd597c)

_OWASP ZAP proxy localhost_

![image](https://github.com/user-attachments/assets/7f3bc4ce-42d3-4237-bbeb-9f96e9131c9f)

_Firefox proxy localhost_

Seuraavaa osuutta varten kytkin netin pois ja tein huolelliset yhteyskokeilut, jotta liikennettä ei karkaisi pois sisäverkosta. Lisäksi seuraavat asetukset FoxyProxyssa varmistavat, että proxya ei käytetä näiden osoitteiden ulkopuolella (localhost ja Metasploitable)

![image](https://github.com/user-attachments/assets/a0bd277c-96cd-4913-97a7-fb82bb07b2b6)

_ProxyFoxy asetukset_

Kokeilin näillä asetuksilla yhteyttä Metasploitableen ja localhostiin. Localhost palautti vastauksen `502`, koska siellä ei ole mitään. Metasploitable taas palautti vastauksen `200`, eli yhteyskokeilu onnistui.

![image](https://github.com/user-attachments/assets/0e4426ae-4830-4af8-bad9-895ca4803e8d)

_Metasploitable ja localhost_

Pystyin hyödyntämään myös FoxyProxya localhost liikenteen tallettamiseksi.

## PortSwigger Labs (Kohdat C-J)

## Pencode (Kohta K)

## Yleiset Lähteet
