
# h6 Vuohi

## Johdanto

Tehtävässä h6 tuli harjoitella tunkeutumistestausta `WebGoat`-harjoitemaalin avulla. Viime viikon tehtävän tapaan `WebGoat` on harjoitemaali, missä voi kokeilla eri hyökkäysmalleja verkkosovelluksiin turvallisesti. 

## Ennakkomateriaalit

Karvisen artikkeli käsitteli `WebGoat`-harjoitemaalin asentamista omaan käyttöön. Seurasin näitä ohjeita `WebGoat`:tia ladatessa.

## WebGoat (Kohta A)

Latasin ensin `java` ja `ufw` -palvelut, jotta voisin pyörittää WebGoattia.

![image](https://github.com/user-attachments/assets/1fda015b-a91f-40bf-a823-9ac16414bcad)

_Java ja UFW asennettu_

Tämän jälkeen kopioin `webgoat-2023.4` paikalliseen käyttöön.

![image](https://github.com/user-attachments/assets/7944c048-c8ce-4758-8eab-caea4ca1bf34)

_webgoat käytössä_

Lopuksi käytin samaa kikkaa kun viimeviikolla Foxyproxyn ja Zap avulla, jolla pystyn tarkkailla ja muokata verkkoliikennettä turvallisesti. Tätä varten tuli myös vaihtaa WebGoatin portti.

![image](https://github.com/user-attachments/assets/10ed7a59-1657-43e2-a52f-21a7f555d0c5)

_Valmiina vaikuttamaan_

## Broken Access Control (Kohta B)

Ensimmäisessä kohdassa tuli kaapata toisen käyttäjän keksi ilman autorisointia. Kokeilin ensin kirjautua sisään ja seurasin minkälaisen pyynnön tämä loi. Tässä tapauksessa `hijack_cookie` oli:

`7817068875515383156-1733328556734`

Kokeilin seuraavaksi kirjautua useampaan kertaan, jotta saisin eri keksejä. Sain kuitenkin aina saman aikaisemman keksin. Tehtävän neuvot osiossa ehdotettiin tismalleen saman asian tekemistä, jotta uusia keksejä syntyisi. Jopa `lesson reset` ei ratkaissut tätä ongelmaa. Oletetusti rikkinäisen tehtävän takia siirryin seuraavaan osioon.

Kohdassa `Insecure Direct Object References` tuli hyödyntää IDOR heikkouksia, elikkä syöttää omaa koodia tiedostohakuihin.

Ensin kirjauduin annetuin tunnuksin ja seuraavaksi tuli nimetä kaksi atribuutti, jotka eivät näy käyttäjille. Tässä tapauksessa `userId` ja `role`

![image](https://github.com/user-attachments/assets/6329ba37-0a3b-48da-849c-d46ae6495910)
 
_Atribuutit_

Seuraavassa osassa tuli arvata reitti omalle käyttäjälleen. Aikaisemmasta esimerkistä tiedän, että reitti on `/WebGoat/IDOR/profile`, elikkä todennäköisesti joudun vaan lyömään `userId`:n sen perään.

![image](https://github.com/user-attachments/assets/b3053089-d5fe-4891-8f9b-219393bbb2b5)

_Jep_

Seuraavassa osassa tuli löytää toisen käyttäjän tili ja muokata sitä. Tein pyynnön kuten yleensä ja muokkasin sen ensin vastaamaan omaa profiiliani

![image](https://github.com/user-attachments/assets/72d21996-c3d0-40ed-a2a3-b8f7e5687d68)

![image](https://github.com/user-attachments/assets/52438be5-7781-4eb9-9c9c-7b7b4966681d)

Pienellä manuaalisella fuzzaamisella löytyi toinen profiili. Tätä tuli vielä muokata hieman. En kuitenkaan ymmärtänyt paljoakaan tästä kielestä, joten muokkaaminen osoittautui hankalaksi. Tämä tehtäisiin kuitenkin `POST`/`PUT`/`PATCH` yms. komennolla.

![image](https://github.com/user-attachments/assets/3de575e1-e7d2-4036-9b7d-ca8913b34bb7)

_Vihreää_

Lopuksi tuli vielä ratkaista kaksi tehtävää liittyen `security by obscurity` -aiheeseen. Sillä tarkoitetaan toimintatapaa, jossa "suojataan" tärkeitä komponentteja jättämällä ne ilman linkkejä. Nämä voi kuitenkin löytää tutkimalla esim. `JSON`-vastausta

![image](https://github.com/user-attachments/assets/b6621767-c7d9-406b-bdfe-e3eca5080e48)

Toisessa tehtävässä tuli ratkaista "Jerryn" hash käytämällä näitä salaisia menuja.


### Lähteet

https://www.owasp.org/index.php/Top_10-2017_A5-Broken_Access_Control

## Identity & Auth Failure (Kohta C)

Kohdassa "Authentication Bypass" huonosti ohjelmoidut turvakysymykset johtivat tietoturvahäiriöön. Salasanaa palauttaessa törmäsin kahteen turvakysymykseen. Luontaisesti kokeilin ensin lähettää testivastauksen.

![image](https://github.com/user-attachments/assets/6f757fbe-576b-4b56-b80f-a8f8a0b673ea)

_Testi, testi_

Testivastaus näkyi liikenteessä ja sain takaisin vastauksen, jonka mukaan tämä ei ollut oikea vastaus. En kuitenkaan tarvinnut oikeaa vastausta, jos parametrit olivat heikko kohta. Tehtävän esimerkissä ne poistettiin kokonaan, mutta koitin itse ainoastaan muuttaa niitä olemattomaksi parametriksi.

![image](https://github.com/user-attachments/assets/875cf0f8-ae7a-4a44-91a4-9ab4ca92d425)

_Bingo_

![image](https://github.com/user-attachments/assets/c1e004ff-957d-46d7-afc5-40240766433a)

_Vihreää_

Palvelin ei tunnistanut vastaustapaa, joten se hyppäsi kokonaan validaation yli, eikä esim. antanut virheilmoitusta. Se oli siis huonosti ohjelmoitu.

Kohdassa Insecure Login näytettiin, miksi enkryptio on aina tärkeä osa tietoliikennettä. Esimerkissä `Log In` nappia painaessa palvelimelle lähti käyttäjänimi ja salasana plaintekstinä.

![image](https://github.com/user-attachments/assets/97bd3341-b594-4317-8a4b-d585f1071520)

_Pirates of the WebGoat_

![image](https://github.com/user-attachments/assets/aa56232c-ac5c-48af-8cc0-5405b319db6a)

_Vihreää_

## Server-side Request Forgery (Kohta D)

Tässä osiossa tuli luoda vääriä pyyntöjä palvelimelle, joka on yleinen tietoturvaheikkous. Ensimmäisessä tehtävässä palvelimelta pyydettän kuva. Tämän voi muuuttaa helposti `tom.png`:stä `jerry.png`:ksi.

![image](https://github.com/user-attachments/assets/d231d763-e78b-4214-8707-1a16007fa5f3)

_Tom?_

![image](https://github.com/user-attachments/assets/b2c27d27-41cf-4a84-bcfb-443b73d9e81b)

_Jerry!_

Toisessa osiossa voi vielä vähän harmillisemmin hakea tiedot aivain eri osoitteesta kuin mitä alunperin oli tarkoitettu.

![image](https://github.com/user-attachments/assets/f5689fec-fa70-4f09-af1b-89b4ebfc8c06)

_Hups_

![image](https://github.com/user-attachments/assets/ed68ebec-93fe-4e5c-a72e-88672a3feb85)

_Vihreää_

### Lähteet

https://www.hackerone.com/blog-How-To-Server-Side-Request-Forgery-SSRF

## Client side (Kohta E)

Tässä osiossa hyökkäys tuli tehdä käyttäjän puolelta `HTML`:ää muokkaamalla. Käyttäjällä on useimmiten melko vapaat kädet muokata `HTML`:ää omiin tarkoituksiinsa.

Sivusto koitti rajoittaa luovuuttani, joten muokkasin pyyntöä vähän, sallien kaikkien rajoitusten ohituksen.

![image](https://github.com/user-attachments/assets/4236a877-de41-4232-9585-246286d039b6)

_Taantumuksellista_

![image](https://github.com/user-attachments/assets/2f9b7e7f-a1a5-4eb4-b4d7-e190bb00d29e)

_Paljon parempi_

Toinen osio oli hieman paremmin suojattu, mutta käyttäjä voi melko helposti ohittaa nämä omalla pyynnöllä.

![image](https://github.com/user-attachments/assets/cc384822-f0c9-45a4-a464-b401af151919)

_Hupsis_

![image](https://github.com/user-attachments/assets/857da5a0-2e6e-422f-8b3c-1fcf547ce82a)

_Vihreää_

## (Kohta F)

## Lähteet

