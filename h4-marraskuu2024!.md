# h4 Marraskuu2024!

## Johdanto

Tehtävässä h4 tuli harjoitella salasanojen murtamista tiivisteistä, jota käytiin viikon 46 tunnilla. Lisäksi tehtävässä tuli harjoitella fuzzaamista `Ffufme`-harjoitusmaalin avulla ja luoda oma hyökkäys `msfvenom`-työkalulla.

## Ennakkomateriaalit

### Lähteet
https://terokarvinen.com/2022/cracking-passwords-with-hashcat/

## Hashcat (Kohta A)

Hyödynsin tehtävässä `Hashcat`-työkalua käsittelevää artikkelia. Latasin ensin `Hashcat`-työkalun komennolla `sudo apt-get -y install hashid hashcat wget`. Tämän jälkeen loin uuden hakemiston `hashed`, johon latasin `Rockyou`-sanakirjan.

Artikkelissa annetaan esimerkki hash `6b1628b016dff46e6fa35684be6acc96`. Analysoin ensin sen tyypin komennolla `hashid -m`.

![image](https://github.com/user-attachments/assets/880634ab-fca2-4a83-8d25-ebed62fb9a3e)

_Analysoinnin tulokset_

Viikon 46 tunnilta muistin, että hash-tyyppi on yleensä ensimmäisen kolmen vaihtoehdon joukossa. Näistä ylivoimaisesti yleisin on `MD5`. Mursin hashin seuraavaksi komennolla `hashcat -m 0 '6b1628b016dff46e6fa35684be6acc96' rockyou.txt`. Komennossa näkyy aikaisemman analyysin antaman tyypin numeerinen arvo `0`, sanakirjatiedosto `rockyou.txt` ja itse hash.

![image](https://github.com/user-attachments/assets/659f0ad4-2227-4b95-99b2-3afa9d759e8b)

_Cracked_

## John the Ripper (Kohta B)



## Fuffme (Kohta C)

Latasin ensin `fuffme`-harjoitusmaalin artikkelissa annettujen ohjeiden mukaan.

![image](https://github.com/user-attachments/assets/5579aff3-f918-4c53-8689-9a7b0edbdb67)

_Harjoitemaali ladattu_


### Lähteet
