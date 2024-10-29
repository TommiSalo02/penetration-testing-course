# h1 Hacker's journey

Tehtävässä h1 tuli tutkia verkkoskannaamista kahden virtuaalikoneen avulla. 
Aloitin tehtävän lataamalla toiselle virtuaalikoneelle Kalin (kali-linux-2024.3-live-amd64.iso) ja toiselle Metaspoitablen 2 (metaspoitable-linux 2.0.0.). Latasin Kalin manuaalisesti pre-made vm sijaan, jotta pääsen näkemään mistä on kyse. Kali asensi itsestään Virtual Box Guest Toolit, joka oli mukava ominaisuus.

Parhaan ymmärrykseni mukaan Kali on Linux-distribuutio joka on suunniteltu nimenomaan tunkeutumis- ja tietoturvatestaamiseen. Metaspoitable on tahallisesti hyökkäysaltis virtuaalikone, jonka avulla voi harjoitella erinäisiä hyökkäyksiä. Yhdessä nämä muodostavat hyvän tunkeutumistestausalustan kurssia varten, jossa voi turvallisesti harjoitella.

### Lähteet 

https://www.kali.org/docs/virtualization/install-virtualbox-guest-vm/

## Tiivistykset



## Kali porttiskannaus

Kali-virtuaalikoneeni sai NAT-verkkokortin kautta yhteyden verkkoon. 

![image](https://github.com/user-attachments/assets/3d410b75-5f74-46a2-9424-48617ecc890b)

_Kali-virtuaalikone saí NATin kautta yhteyden verkkoon_

![image](https://github.com/user-attachments/assets/55e61188-9404-41f8-b99b-a8867a1650a4)

_Yhteyskokeilu toimii_

Irroitin tämän kortin ennen porttiskannauksen testaamista, jotta en vahingossa skannaisi muita portteja.

![image](https://github.com/user-attachments/assets/04331c98-272c-48b9-b771-494ecac7ec37)

_Kortti irroitettu_

![image](https://github.com/user-attachments/assets/ff855f17-4aa5-41c2-bfb0-295d02508501)

_Yhteyskokeilu EI toiminut_

![image](https://github.com/user-attachments/assets/500f0763-8851-4487-aee6-9b12316717e5)

_Selain EI toiminut_

Suoritin `nmap -A localhost` porttiskannauksen.

![image](https://github.com/user-attachments/assets/a6f92320-91cc-4e05-8a55-0d4cc0564fb4)

_Porttiskannauksen tulokset_

Sain porttiskannauksesta halutut tulokset. Skannaus ei löytänyt DNS-palvelimia, sillä verkkoyhteys oli katki. Se skannasi localhostin laitteen IP-osoitteessa `127.0.0.1`, mutta ei muita laitteita. Skannattu kohde ei sisältänyt avonaisia portteja.

Latasin seuraavaa kohtaa varten kaksi demonia, eli `apache2` ja `openssh`. Tämä tehtiin laittamalla verkkokortti tilapäisesti takaisin päälle ja kommennoilla `sudo apt update > sudo apt install openssh-server -y > sudo apt install apache2 -y`.

Tämän jälkeen pistin molemmat demonit päälle, verkkokortin pois päältä kokeilujen kera ja lopulta porttiskannasin uudelleen.

![image](https://github.com/user-attachments/assets/a682466f-f161-47c8-816a-98700ebc8953)

_Porttiskannaukset tulokset demonien kera_

Tulokset olivat muuten samat, mutta demonit pitivät portteja 22 (SSH) ja 80 (HTTP) auki omaa toimintaansa varten. Näin myös muuta tietoa, kuten apache2 ja openssh demonien versiot.

## Metaspoitable porttiskannaus

Seuraavaksi siirryin metaspoitablen pariin.

## Lähteet
Valkamo, Tuomas 2022. Hacking into a Target Using Metasploit. https://tuomasvalkamo.com/PenTestCourse/week-2/
