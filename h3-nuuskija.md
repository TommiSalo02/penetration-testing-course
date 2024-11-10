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

-Fuzzaamisessa testataan verkkopalvelimen vasteita syöttämällä sille erinäisiä syötteitä (useimmiten satunnaisesti).

-Fuzzaamalla voidaan löytää verkkopalvelimesta piilotettuja hakemistoja, joihin ei ole muuten pääsyä.

-Ffuf on työkalu, joka suorittaa fuzzaamisen nopeasti ja automaattisesti.

### Lähde

Popov 2024: Hacktricks: Wireshark tricks. Luettavissa: https://book.hacktricks.xyz/generic-methodologies-and-resources/basic-forensic-methodology/pcap-inspection/wireshark-tricks#improve-your-wireshark-skills. Luettu 10.11.2024

Karvinen 2023: Find Hidden Web Directories - Fuzz URLs with ffuf. Luettavissa: https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/. Luettu 10.11.2024

## Hyökkäys ja sorsastus (Kohdat A-B)

