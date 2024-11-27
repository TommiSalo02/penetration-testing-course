# h5 Täysin Laillinen Sertifikaatti

### Johdanto

Tehtävässä h5 tuli harjoitella...

## Ennakkomateriaalit

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

Kokeilin näillä asetuksilla yhteyttä Metasploitableen ja localhostiin. Localhost palautti vastauksen `502`, koska siellä ei ole mitään. Metasploitable taas palautti vastauksen `200`, eli yhteyskokeilu onnistui.

![image](https://github.com/user-attachments/assets/0e4426ae-4830-4af8-bad9-895ca4803e8d)

_Metasploitable ja ZAP_

Pystyin hyödyntämään myös FoxyProxya localhost liikenteen tallettamiseksi.



## PortSwigger Labs (Kohdat C-J)

## Pencode (Kohta K)

## Yleiset Lähteet
