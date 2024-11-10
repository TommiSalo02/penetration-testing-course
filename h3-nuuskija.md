# h3 Nuuskija

## Johdanto

Kotitehtävässä h3 oli tarkoituksena syventyä kotitehtävästä h2 tuttuihin Metasploitable-hyökkäyspaketteihin, viikon 45 oppitunnilla käytyihin hyökkäysten lähdekoodiin, verkkosniffaamiseen ja HackTheBoxiin sekä uutena asiana Fuzzaamiseen.

## Ennakkomateriaali (Kohta X)

Viikon aiheeseen liittyen tuli tiivistää Popovin artikkeli Wiresharkista ja Karvisen artikkeli Fuzzista.

### Wireshark Tricks

- Wiresharkin tuloksistaa voi `Analyze` ja `Statistics` -osioista löytää tärkeitä tietoja esimerkiksi pakettien tarkasta sisällöstä, käytetyistä protokollista ja DNS-kyselyistä

- Wiresharkin haku- ja suodatustyökaluja voi käyttää esimerkiksi HTTP-headerin tai hostin nimen selvittämiseen.

- Wireshark voi jopa dekryptoida HTTPS-liikennettä käyttämällä selaimen TLS sessio-avaimia tai palvelimen yksityistä avainta.

### Fuzz URL's with ffuf

- Fuzzaamisessa testataan verkkopalvelimen vasteita syöttämällä sille erinäisiä syötteitä (useimmiten satunnaisesti).

- Fuzzaamalla voidaan löytää verkkopalvelimesta piilotettuja hakemistoja, joihin ei ole muuten pääsyä.

- Ffuf on työkalu, joka suorittaa fuzzaamisen nopeasti ja automaattisesti.

### Lähteet

Popov 2024: Hacktricks: Wireshark tricks. Luettavissa: https://book.hacktricks.xyz/generic-methodologies-and-resources/basic-forensic-methodology/pcap-inspection/wireshark-tricks#improve-your-wireshark-skills. Luettu 10.11.2024

Karvinen 2023: Find Hidden Web Directories - Fuzz URLs with ffuf. Luettavissa: https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/. Luettu 10.11.2024

## Hyökkäys ja sorsastus (Kohdat A-B)

Aloitin tehtävän tarkistamalla tuttuun tapaan, että koneet saavat yhteyden VAIN toisiinsa HostOnly-verkossa, eivätkä muualle.

![image](https://github.com/user-attachments/assets/9512273c-df9c-4c83-9e9d-06bb99d4b51f)
![image](https://github.com/user-attachments/assets/f6dc88d7-ccce-4897-864f-d8833e2fe618)

_Yhteyskokeilu_

Tämän jälkeen avasin msf-konsolin ja valmistelin exploitin. Otin tehtävää varten itselleni uuden exploitin, eli `DistCC Daemon Command Execution` -hyökkäyksen. 

![image](https://github.com/user-attachments/assets/f0ab69b9-b989-4203-8982-b95fb128f457)

_Hyökkäyksen valinta ja konfigurointi_

Tässä vaiheessa hyödyllinen on `show options`-komento, joka näyttää ne parametrit, joita voin konfiguroida. Tässä tapauksessa olennaista on konfiguroida hyökkäyskohde `set RHOST` ja hyökkääjä `set LHOST`.

Valitettavasti tässä tapauksessa exploit ei saanut yhteyttä kohteeseen. Ohjelman antamasta palautteesta pystyin arvioida, että kyse on todennäköisesti virheellisestä payloadista. Metasploit antoi automaattisesti payloadin `cmd/unix/reverse_bash`, mutta tämä ei ole välttämättä oikea.



### Lähteet

Metasploit: Metasploitable 2 Exploitability Guide. Luettavissa: https://docs.rapid7.com/metasploit/metasploitable-2-exploitability-guide/.
Metasploit: DistCC Daemon Command Execution. Luettavissa: https://www.rapid7.com/db/modules/exploit/unix/misc/distcc_exec/.
