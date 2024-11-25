# h4 Marraskuu2024!

## Johdanto

Tehtävässä h4 tuli harjoitella salasanojen murtamista tiivisteistä, jota käytiin viikon 46 tunnilla. Lisäksi tehtävässä tuli harjoitella fuzzaamista `Ffufme`-harjoitusmaalin avulla ja luoda oma hyökkäys `msfvenom`-työkalulla.

## Ennakkomateriaalit

### Lähteet
https://terokarvinen.com/2022/cracking-passwords-with-hashcat/

## Hashcat (Kohta A)

Hyödynsin tehtävässä `Hashcat`-työkalua käsittelevää artikkelia. Latasin ensin `Hashcat`-työkalun. Tämän jälkeen loin uuden hakemiston `hashed`, johon latasin `Rockyou`-sanakirjan.

Artikkelissa annetaan esimerkki hash `6b1628b016dff46e6fa35684be6acc96`. Analysoin ensin sen tyypin komennolla `hashid -m`.

![image](https://github.com/user-attachments/assets/880634ab-fca2-4a83-8d25-ebed62fb9a3e)

_Analysoinnin tulokset_

Viikon 46 tunnilta muistin, että hash-tyyppi on yleensä ensimmäisen kolmen vaihtoehdon joukossa. Näistä ylivoimaisesti yleisin on `MD5`. Mursin hashin seuraavaksi komennolla `hashcat -m 0 '6b1628b016dff46e6fa35684be6acc96' rockyou.txt`. Komennossa näkyy aikaisemman analyysin antaman tyypin numeerinen arvo `0`, sanakirjatiedosto `rockyou.txt` ja itse hash.

![image](https://github.com/user-attachments/assets/659f0ad4-2227-4b95-99b2-3afa9d759e8b)

_Cracked_

## John the Ripper (Kohta B)

Seurasin jälleen artikkelin neuvoja `John the Ripper`-työkalun lataamisessa. Latasin ensin työkalun käyttämiseen vaadittavat tiedostot ja sitten itse työkalun. Tämän jälkeen tuli vielä konfiguroida ja kompilata ladatut tiedostot. Onnistuneen latauksen jälkeen testasin käynnistää työkalun komennolla `$HOME/john/run/john `.

![image](https://github.com/user-attachments/assets/83efb739-c295-41e4-b7b5-ffadcb60a3de)

_John the Ripper_

Seuraavaksi latasin artikkelin koe ZIP-tiedoston uuteen hakemistoon `crackfile`. Tämän tiedoston purkaminen vaatii salasanan.

![image](https://github.com/user-attachments/assets/118d3d4b-859b-4e1a-9cac-c07263b4a1fe)

_ZIP-sala_

Seuraavaksi hain salasanasta hash-tiedoston `tero.zip,hash` komennolla `$HOME/john/run/zip2john tero.zip >tero.zip.hash`. John the Ripper osaa käyttää bruteforce-hyökkäystä omalla metodillaan, tai sille voi antaa oman sanakirjan kuten aikaisemmassa hashcat esimerkissä. Tässä tapauksessa annoin John the Ripperin tehdä hommansa komennolla `$HOME/john/run/john tero.zip.hash `.

![image](https://github.com/user-attachments/assets/39de5d8a-36ae-4e67-a772-6d176034b92e)

_Perhonen_

Testasin vielä löytämääni salasanaa `butterfly` tiedoston purkamisessa.

![image](https://github.com/user-attachments/assets/c683b319-f046-4fa7-9917-317f2b4051dd)

_Cracked!_

## Fuffme (Kohta C)

Latasin ensin `fuffme`-harjoitusmaalin artikkelissa annettujen ohjeiden mukaan. Tässä kohtaa tuli ladata vaadittavat tiedostot, luoda Docker container ja käynnistää maali komennolla `sudo docker run -d -p 80:80 ffufme`.

![image](https://github.com/user-attachments/assets/5579aff3-f918-4c53-8689-9a7b0edbdb67)

_Harjoitemaali ladattu_

## Tiedosto (Kohta E)

Loin ensiksi tiedoston `topsecret.txt`, jonka suojasin `GPG`-työkalulla komennolla `gpg --symmetric --cipher-algo AES256 topsecret.txt`. Annoin tiedostolle salasanaksi `password123`.

Tämän jälkeen pystyin ottaa tästä tiedostosta tiivisteen käyttämällä Johnia komennolla `gpg2john topsecret.txt.gpg > gpghash.txt`. Tämän jälkeen pääsen murtamaan salasanaa hyödyntäen Johnia ja `rockyou.txt`-sanalistaa komennolla `john --wordlist=rockyou.txt gpghash.txt`. Sain tällä metodilla salasanan ratkaistua 2 minuutissa ja 22 sekunnissa.

![image](https://github.com/user-attachments/assets/f941e5a3-3048-49f3-8a45-6b7bfd4c3716)

_Password cracked!_

![image](https://github.com/user-attachments/assets/8385b776-d1e3-419b-9dab-e36da649505b)

_topsecret.txt_

## Tiiviste (Kohta F)

Käytin tätä tehtävää varten `OpenSSL`-työkalun enkryptointia. Tämä oli nopea ja helppo tapa luoda enkryptoituja tiedostoja eri formaateilla.

Loin ensin hash-tiedoston salasanasta `alpha1`. Tämä salasana olo vähän monimutkaisempi kuin `summer` lisätyn numeron takia, mutta uskoin sen murtuvan kuitenkin melko nopeasti.

Selvitin myös salasanan olevan salattu `SHA-512`-avaimella käyttämällä `hashid`-työkalua.

![image](https://github.com/user-attachments/assets/a7c1054e-b8b3-4c9d-8465-1bc15fadf2de)

_Enkryptaus ja murto_

Tämä salasana kesti hieman pidempään, eli 13 sekunttia.

![image](https://github.com/user-attachments/assets/4b2e617e-2fa4-41da-8fae-061a72a55b9d)

_alpha1 cracked_

Lisäämällä vielä ison alkukirjaimen (Alpha1) prosessi venyi jo noin 12 minuutin tienoille.

![image](https://github.com/user-attachments/assets/37882f3d-f475-4411-b56d-8cf9996ce68b)

_Alpha1 cracked_

### Lähteet

https://www.geeksforgeeks.org/blockchain-encrypt-decrypt-files-with-password-using-openssl/
