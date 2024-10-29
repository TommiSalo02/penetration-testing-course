# h1 Hacker's journey

Tehtävässä h1 tuli tutkia verkkoskannaamista kahden virtuaalikoneen avulla. 
Aloitin tehtävän lataamalla toiselle virtuaalikoneelle Kalin (kali-linux-2024.3-live-amd64.iso) ja toiselle Metaspoitablen 2 (metaspoitable-linux 2.0.0.). Latasin Kalin manuaalisesti pre-made vm sijaan, jotta pääsisin näkemään mistä oli kyse. Kali asensi itsestään Virtual Box Guest Toolit, joka oli hyvä ominaisuus.

Parhaan ymmärrykseni mukaan Kali on Linux-distribuutio joka on suunniteltu nimenomaan tunkeutumis- ja tietoturvatestaamiseen. Metaspoitable on tahallisesti hyökkäysaltis virtuaalikone, jonka avulla voi harjoitella erinäisiä hyökkäyksiä. Yhdessä nämä muodostavat hyvän tunkeutumistestausalustan kurssia varten, jossa voi turvallisesti harjoitella.

### Lähteet 

https://www.kali.org/docs/virtualization/install-virtualbox-guest-vm/

## Tiivistykset



## Kali porttiskannaus

Ensiksi suoritin porttiskannauksen Kalissa. Tarkistin heti verkon tilan, jotta voisin tehdä skannauksen ilman riskejä.

![image](https://github.com/user-attachments/assets/3d410b75-5f74-46a2-9424-48617ecc890b)

_Kali-virtuaalikone saí NATin kautta yhteyden verkkoon_

![image](https://github.com/user-attachments/assets/55e61188-9404-41f8-b99b-a8867a1650a4)

_Yhteyskokeilu toimi_

Katkaisin NAT-yhteyden ennen porttiskannauksen testaamista, jotta en vahingossa skannaisi muita portteja.

![image](https://github.com/user-attachments/assets/04331c98-272c-48b9-b771-494ecac7ec37)

_Kortti irroitettu_

![image](https://github.com/user-attachments/assets/ff855f17-4aa5-41c2-bfb0-295d02508501)

_Yhteyskokeilu EI toiminut_

![image](https://github.com/user-attachments/assets/500f0763-8851-4487-aee6-9b12316717e5)

_Selain EI toiminut_

Suoritin tämän jälkeen `nmap -A localhost` porttiskannauksen.

![image](https://github.com/user-attachments/assets/a6f92320-91cc-4e05-8a55-0d4cc0564fb4)

_Porttiskannauksen tulokset_

Sain porttiskannauksesta halutut tulokset. Skannaus ei löytänyt DNS-palvelimia, sillä verkkoyhteys oli katki. Se skannasi localhostin laitteen IP-osoitteessa `127.0.0.1`, mutta ei muita laitteita. Skannattu kohde ei sisältänyt avonaisia portteja.

Latasin seuraavaa kohtaa varten kaksi demonia, eli `apache2` ja `openssh`. Tämä tehtiin laittamalla verkkokortti tilapäisesti takaisin päälle ja kommennoilla `sudo apt update > sudo apt install openssh-server -y > sudo apt install apache2 -y`.

Tämän jälkeen pistin molemmat demonit päälle, verkkokortin pois päältä kokeilujen kera ja lopulta porttiskannasin uudelleen.

![image](https://github.com/user-attachments/assets/a682466f-f161-47c8-816a-98700ebc8953)

_Porttiskannaukset tulokset demonien kera_

Tulokset olivat muuten samat, mutta demonit pitivät portteja 22 (SSH) ja 80 (HTTP) auki omaa toimintaansa varten. Näin myös muuta tietoa, kuten demonien versiotiedot.

## Metaspoitable porttiskannaus

Seuraavaksi siirryin metaspoitablen pariin. Aluksi loin uuden Host-Only Ethernet Adapterin johon kytkin DHCP-palvelimen päälle.

![image](https://github.com/user-attachments/assets/b6afa843-bad9-43d8-bb4b-b40b7aa492fc)

_Uusi Ethernet Adapteri_

Tämän jälkeen asensin Kaliin ja Metaspoitableen verkkokortin joka hyödynsi tätä adapteria. Kalissa säilyy lisäksi NAT-verkkokortti verkkoyhteystä varten.

![image](https://github.com/user-attachments/assets/519fcbb6-0a3c-467f-99b4-67142ea88c4a)

_Kalin verkkokortit_

Seuraavaksi käynnistin molemmat laitteet ja tarkistin, että verkkoyhteys on molemmissa pois päältä. Tämän jälkeen selvitin Metaspoitable-virtuaalikoneen IP-osoitteen komennolla `ifconfig`. Tällä paljastui sen olevan `192.168.82.3`. Nopea ping todisti, että tämä IP-osoite on samassa Host-Only verkossa (ping ei saavuttaisi sitä muuten, sillä verkkoyhteys ei ole päällä).

![image](https://github.com/user-attachments/assets/65fdeb77-782e-4829-b566-4290c6a67a26)

_Metaspoitablen IP-osoite ping_

Seuraavaksi porttiskannasin Metaspoitablen IP-osoitteen `nmap -sn` komennolla ja tarkistin sen olevan oikea osoite verkkoselaimen avulla.

![image](https://github.com/user-attachments/assets/53bc6599-0d58-4e46-a172-af92e6146369)

_Metaspoitable porttiskannaus, verkkoselain_

Lopuksi skannasin vielä Metaspoitablen IP-osoitetta tarkemmin komennolla `nmap -A -p-`. Muutaman minuutin jälkeen sain pitkän syötteen erinäisiä tietoja Metaspoitable-koneesta. Skannattuja portteja oli kokonaisuudessaan 65,535, joista 30 oli auki. ChatGPT osasi nostaa näistä avonaisista porteista kaksi, jotka ovat hyvin haavoittuvaisia hyökkäyksille. Kyseessä olivat portti 21 (FTP) ja portti 1524 (Bindshell)

![image](https://github.com/user-attachments/assets/a5f8b6d4-cbcb-4a7c-af70-21f2513f0fad)

_Portti 21_

![image](https://github.com/user-attachments/assets/4fd13cd1-aed7-4df9-9278-fa8d0d3c44bc)

_Portti 1524_

Portti 21

## Lähteet
Valkamo, Tuomas 2022. Hacking into a Target Using Metasploit. https://tuomasvalkamo.com/PenTestCourse/week-2/
