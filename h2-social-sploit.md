# Social sploit

## Johdanto

Kotitehtävässä h2 tuli jatkaa kokeiluja porttiskannaamisesta (kotitehtävä h1) ja kokeilla muutamia hyökkäyskeinoja Metasploitablen avulla (aloitin tämän jo 1.11 oppitunnilla).

Asensin Metasploitablen ja Kalin sekä HostOnly-ethernetverkon jo aikaisempaa kotitehtävää varten, joten siirryin suoraan tehtävän pariin.

## Tiivistys (kohta X)



### Lähteet

## Porttiskannaus (kohdat A-F)

Varmistin ensin tuttuun tapaan, että laitteet ovat irti Internetistä ja että ne saavat yhteyden toisiinsa `ping` komennolla. Molemmat virtuaalikoneet ovat HostOnly-verkossa, jonka lisäksi Kali-virtuaalikoneella on NAT-verkkokortti, joka on irroitettu.


![image](https://github.com/user-attachments/assets/f2a45fd1-2004-48b1-bf73-ef999d3e6b4a)

![image](https://github.com/user-attachments/assets/c3f97317-d817-4b9f-9fa9-b309df1677e5)

_Totesin testein, että koneet ovat irti Internetistä, mutta saavat HostOnly-verkossa toisiinsa yhteyden._

Seuraavaksi etsin Metasploitablen porttiskannaamalla ja tarkistin olevani oikeassa IP-osoitteessa selaimen avulla. Tätä varten avaan ensin Metasploitablen konsolin komennolla `msfconsole` ja tämän jälkeen suoritan aliverkon porttiskannauksen komennolla `db_nmap -sn 192.168.82.0/24`

![image](https://github.com/user-attachments/assets/49d9b573-0d62-43cd-8d61-04c1686fd9a2)

_Aliverkon porttiskannaus_

Aliverkko `192.168.82.0/24` skannattiin ja lopputuloksena 256 IP-osoitteesta vain kaksi oli käytössä, eli Kali osoitteessa `192.168.82.4` ja toinen kone osoitteessa `192.168.82.3`. Nopea Firefox-haku totesi jälkimmäisen osoitteen omaksi Metasploitable-virtuaalikoneeksi.

![image](https://github.com/user-attachments/assets/b5e4c63e-c30a-451a-b11d-1ea532a33242)

_Metasploitablen IP-osoite hakukoneessa._

TÄYSI SKANNAUS, TALLENUS TIETOIHIN JA VERTAILU, NYT ETEENPÄIN

### Lähteet

## Tunkeutuminen (kohdat G-N)



### Lähteet

## Yleiset lähteet

