# h2 Sniff-n-Scan
Tehtävät on tehty käyttäen kali linuxa 2023.3. Ffuf versio 2.1.0. Nmap versio 7.94.
## x) Tiivistä

FUZZ:
Käytetään automatisoimaan pyyntöjen lähettämistä kohteelle (verkkosivusto, rajapinnat jne.) ja samalla yritetään löytää virheitä, jotta kohde paljastaa joitakin tietoja, joita sen ei pitäisi paljastaa.
Yleisiä kohteita: GET-parametrit: nimi, arvot... Headers: host, authentication, cookies, proxy headers. Post-data: lomaketiedot, JSON-tiedostot.

Filters (-f/c,l,r,s,t,w,tila) ja matchers (-m/c,k,mode,r,s,t).

Mielenkiintoisia ffuf-esimerkkejä videossa. Pass bruteforcing. Virtualhost discovery. Parameter discovery. Template injections. Usein käytettyjä komentoja ovat esimerkiksi: -od output found context to txt file, -u target url, -w wordlist, -v verbose output, -json json output.

Sanalistoja: seclists, fuzz.dp, all.txt, jhaddixin sanalista. Tarvittaesa luo kohdennettuja sanalistoja: kohde-, sovellus-, konteksti-, ohjelmointikieli-, kielikohtaisia.

## a) Fuff
Ensiksi siirryin kansioon /Downloads, Latasin kansioon harjoitusmaalitidoston dirfutz-1 ja muuten sen oikeudet niin, että sen tiedoston voi ajaa, jonka jälkeen ajoin tiedoston dirfutz-1, että sain harjoitusmaalin auki. Lisäksi latasin samaan kansioon common.txt, joka sisältää sanalistan yleisimmin käytetyistä verkkojen aliosoitteiden nimistä.

    $ wget https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/dirfuzt-1
    $ chmod +x dirfuzt-1
    $ ./dirfizt-1
    $  wget https://raw.githubusercontent.com/danielmiessler/SecLists/master/Discovery/Web-Content/common.txt
    
![Alt text](/H2Sniff-n-Scan/h2.a1.png)

Avasin selaimen localhost osoitteen 127.0.0.2:8000, jotta sain varmistuksen, että harjoitusmaalin on pystyssä.

![Alt text](/H2Sniff-n-Scan/h2.a2.png)

Sitten fuzzaamaan. Ensiksi testasin komentoa (-v=verbose output, -c=colorize output, -w=wordlist, -u:target url):

    $ ffuf -v -c -w common.txt -u http://127.0.0.2:8000/FUZZ 
    
![Alt text](/H2Sniff-n-Scan/h2.a3.png)

Komento antoi todella paljon vastauksia, joten haluamme suodattaa ei toivottut vastauksen pois. Se onnistuu jo mainitsemieni suodattimien avulla (-f/c,l,r,s,t,w,mode). Katsomalla tulostusten listaa on vastauksilla paljon yhteistä. Tässä tapauksessa voimme suodattaa vastaukset vaikka koon mukaan lisäämällä -fs 154(filter size) komennon perään.

    $ ffuf -v -c -w common.txt -u http://127.0.0.2:8000/FUZZ -fs 154
Tämän jälkeen vastaukseksi tuli 7 aliverkko osoitetta.

![Alt text](/H2Sniff-n-Scan/h2.a4.png)

Kun testasin antamia aliverkkojen osoitteita, kaikki näyttivät melkein samalta pois lukien osoitte http://127.0.0.2:8000/render/https://www.google.com. 

![Alt text](/H2Sniff-n-Scan/h2.a5.png)

![Alt text](/H2Sniff-n-Scan/h2.a6.png)

Jokaisen sivun paitsi /render... sivun sanoman "You found it!" mukaan oletan, että pääsin tehtävän läpi.

## b) Fuffme harjoitusmaali
Ensiksi asensin docker.oi, koska se pitää olla asennettuna. Lisäksi asensin 3 sanalistaa omaan kansioon valmiiksi, joita tarvitsemme fuzzaukseen.

    $ sudo apt install docker.io
    $ mkdir $HOME/wordlists
    $ cd $HOME/wordlists
    $ wget http://ffuf.me/wordlist/common.txt
    $ wget http://ffuf.me/wordlist/parameters.txt 
    $ wget http://ffuf.me/wordlist/subdomains.txt
Sen ladataan harjoitusmaalin koneelle ja käynnistetään se. (Jos apache on päällä kuten itsellä oli ei käynnistys mene läpi, koska apache on vallannut portin 80. $ sudo systemctl disable apache2, laittaa apachen pois päältä.)

    $ sudo apt-get install docker.io git ffuf
    $ cd ffufme/
    $ sudo docker build -t ffufme .
    $ sudo docker run -d -p 80:80 ffufme
![Alt text](/H2Sniff-n-Scan/h2.b1.png)
Ja sitten fuzzaamaan.Käytän komennossa harjoitusmaalisivulla mainutun komennon lisäksi -v -c valintoja, koska tulostus selkeämmin luettava.

Basic Content Discovery. 

    $ ffuf -v -c -w ~/wordlists/common.txt -u http://localhost/cd/basic/FUZZ
![Alt text](/H2Sniff-n-Scan/h2.b2.png)

Content Discovery With Recursion. (-recursion kertoo ffuf-ohjelmalle, että jos se kohtaa kansion, sen tulisi aloittaa toinen skannaus kyseisessä kansiossa ja niin edelleen, kunnes enempää tuloksia ei löydy.)

     $ ffuf -v -c -w ~/wordlists/common.txt -recursion -u http://localhost/cd/recursion/FUZZ
![Alt text](/H2Sniff-n-Scan/h2.b3.png)

Content Discovery With File Extensions.(-e märittää tiedostopäätetyypin jota haemme. Tiedostotyypin lisätään jokaisen sanalistan sanan loppuun, jotta löydämme oikeat lokitiedostot.)

    $ ffuf -v -c -w ~/wordlists/common.txt -e .log -u http://localhost/cd/ext/logs/FUZZ 
![Alt text](/H2Sniff-n-Scan/h2.b4.png)

No 404 Status.

    $ ffuf -v -c -w ~/wordlists/common.txt -u http://localhost/cd/no404/FUZZ
![Alt text](/H2Sniff-n-Scan/h2.b5.png)   
Harjoitussivun esimerkki antaa todella paljon vastauksia, joten lisätään suodatin (joista mainittu aikaisemmin kohdissa x ja a), jonkun yleisen vastauksen piirteen mukaan. Kuten harjoitussivulla on mainittu lisätään suodatin koon mukaan -fs 669.

    $ ffuf -v -c -w ~/wordlists/common.txt -u http://localhost/cd/no404/FUZZ -fs 669
![Alt text](/H2Sniff-n-Scan/h2.b6.png)  

Param Mining. (Nyt etsimme parametrien arvoja, joita sivulta saattaisi löytyä, koska localhost/cd/param/data ilmoittaa "Required Parameter Missing". Koska etsimme parametrejä käytettään eri sanalistaa parameters.txt.)
![Alt text](/H2Sniff-n-Scan/h2.b8.png) 

    $ ffuf -v -c -w ~/wordlists/parameters.txt -u http://localhost/cd/param/data?FUZZ=1

![Alt text](/H2Sniff-n-Scan/h2.b7.png)  
![Alt text](/H2Sniff-n-Scan/h2.b9.png) 

Rate Limited. (Tämän harjoitussivun komennot on väärin kirjoitettu ffuf -w ~/wordlists/common.txt -u http://ffuf.test/cd/rate/FUZZ -mc 200,429. "ffuf.test" pitää korvata "localhost". -mc näyttää vain halutut statukset, tässä tapauksessa http-statukset 200 ja 429.)  

    $ ffuf -v -c -w ~/wordlists/common.txt -u http://localhost/cd/rate/FUZZ -mc 200,429
![Alt text](/H2Sniff-n-Scan/h2.b10.png) 
 
Komento antaa vastaukseksi vain paljon 429 HTTP statusksia. Tämä tarkoittaa, että minut on väliaikasesti estetty lähettämästä pyyntöjä kyseiseen osoitteeseen. Harjoitusmaalisivun esimerkki komentoon lisätään -t =(määrittää samanaikaisesti suoritettavien hakujen määrän) ja -p =(kytkin määrittää aikaviiveen sekunteina) eli hidastamme omaa hakuliikennettä, jotta meidän toiminta ei ole niin huomattavaa.

    $ ffuf -v -c -w ~/wordlists/common.txt -t 5 -p 0.1 -u http://localhost/cd/rate/FUZZ -mc 200,429
![Alt text](/H2Sniff-n-Scan/h2.b11.png) 
![Alt text](/H2Sniff-n-Scan/h2.b12.png) 

Subdomains - Virtual Host Enumeration. 
    
    $ ffuf -v -c -w ~/wordlists/subdomains.txt -H "Host: FUZZ.ffuf.me" -u http://localhost
![Alt text](/H2Sniff-n-Scan/h2.b13.png) 

Komennolla vastauksia tulee taas paljon, joten lisätään suodatin koon mukaan -fs 1495. 

    $ ffuf -v -c -w ~/wordlists/subdomains.txt -H "Host: FUZZ.ffuf.me" -u http://localhost -fs 1495
![Alt text](/H2Sniff-n-Scan/h2.b14.png) 

Löysimme halutun subdomainin, joten moduuli suoritettu.

## c) nmap -sT
Porttiskannaan paikallista konetta 127.0.0.2 kaikissa kohdissa missä käytän nmap skanneria.

Nmap skannaa perus asetuksella portit väliltä 1-1000. -sT =(TCP connect scan) muodostaa yhteyden löytyneisiin portteihin.

    $ nmap -sT 127.0.0.1
![Alt text](/H2Sniff-n-Scan/h2.c1.png) 
![Alt text](/H2Sniff-n-Scan/h2.c7.png) 
Minulta löytyi omalta koneelta vain yksi auki oleva TCP-portti 22/ssh, johon nmap sai muodostettua yhteyden. Koska tarkastelin liikennettä localhostilla 127.0.0.1 on wiresharkissa source ja destination osoitteet samat. Nmap lähetti noin 1000 SYN sanomaa yhteensä ja vastaanotti saman verran [RTS, ACK] sanomia, koska ilman erillistä määrittelyä nmap skannaa 1000 porttia. Normaalisti vastaus olisi [SYN, ACK] sanoma, mutta koska tein tehtävän localhostissa oli vastauksena [RST, ACK] sanoma.

## d) nmap -sS.
-sS on nmapissa yleisesti parempi kuin -sT skanni. -sT skanni vie vähemmän aikaa ja vaatii vähemmän paketteja saman tiedon saamiseksi.

    $ sudo nmap -sS 127.0.0.1
![Alt text](/H2Sniff-n-Scan/h2.c2.png) 
![Alt text](/H2Sniff-n-Scan/h2.d1.png) 
Vastaukset näyttävät samanlaisilta kuin aiemmassa kohdassa. Paketteja on taas n. 2000, koska skannattavia portteja on 1000.
## e) nmap -sN

    $ sudo nmap -sN 127.0.0.1
![Alt text](/H2Sniff-n-Scan/h2.c3.png)
![Alt text](/H2Sniff-n-Scan/h2.e1.png)

Komento antaa lisää tietoa portin tilasta. open|filtered. Nmap asettaa portit tähän tilaan, kun se ei pysty määrittämään, onko portti avoin vai suodatettu. Tämä tapahtuu sellaisissa skannaustyypeissä, joissa avoimet portit eivät anna mitään vastausta. Vastauksen puuttuminen voi myös tarkoittaa sitä, että pakettisuodatin hylkäsi kyselyn tai minkä tahansa vastauksen, jonka se aiheutti. Asetuksen -sN takia vastauspakettien kentissä ei ole asetettu mitään tiettyä lippua, siksi nmapin lähettämissä paketeissa lukee [<NONE>] eikä [SYN] verrattuna aikaisempiin nmap hakuihin.
## f) nmap -Pn
-Pn estää ping-tarkistuksen, joten Nmap yrittää skannata kohdeosoitteen välittämättä siitä, vastaako se ping-pyyntöihin vai ei.

    $ sudo nmap -Pn 127.0.0.1
![Alt text](/H2Sniff-n-Scan/h2.c4.png) 
![Alt text](/H2Sniff-n-Scan/h2.f1.png) 

Päällisin puolin wiresharkin liikenne näyttää samalta kuin tapauksissa c ja d.
## g) nmap -sV
-sV yrittää selvittää, minkä version ohjelmaa palvelin käyttää vertaamalla vastauksia tiettyihin pyyntöjen parametreihin.

    $ sudo nmap -sV 127.0.0.1
![Alt text](/H2Sniff-n-Scan/h2.c5.png) 
![Alt text](/H2Sniff-n-Scan/h2.g1.png) 

Wiresharkissa nähdään kuinka lopussa SSH lähettää tiedot nmapille ja nmap tulostaa arvioidun version.
## h) nmap -oA
-oA foo tallentaa tulokset kolmeen eri tiedostoon: foo.nmap, foo.gnmap ja foo.xml.

    $ sudo nmap -oA foo 127.0.0.1
![Alt text](/H2Sniff-n-Scan/h2.c6.png)
![Alt text](/H2Sniff-n-Scan/h2.h1.png)
foo.nmap ja foo.gnamp käy ihan manuaaliseen tarkasteluun. .nmap on perus teksti tiedosto. .gnamp tiedoston grepable-muoto on tarkoitettu helpottamaan tiedon käsittelyä skripteillä tai komentorivityökaluilla. Foo.xml on strukturoitu data tiedosto, joka soveltuu tietokantaintegraatioihin ja ohjelmalliseen käsittelyyn. 

Wiresharkin liikenne näyttää jälleen päällisinpuolin "normaalilta".
## i) nmap ajonaikaiset toiminnot(runtime interaction)
-T 0-5 pystyy hidastamaan yksinkertaisesti nmappia ja sen on pakko tehdä tässä, koska skannaan localhostia, jonka takia skannaus on todella nopea. Nmapin suoritus aikaa pystyy hallitsemaan myös laajemmin eri nmapin ominaisuuksilla esimerkiksi --min-rate tai --max-rate asetuksilla.

     $ sudo nmap -T 1 -sS 127.0.0.1

![Alt text](/H2Sniff-n-Scan/h2.i1.png)
![Alt text](/H2Sniff-n-Scan/h2.i2.png)

Kuten kuvasta näkyy tarpeeksi nmapin pyyntöjä hidastamalla liikennettä pystyy kontrolloida. Kun painoin suorituksen aikana ? näkyy saatavilla olevat kontrollit suorituksen aikana: ?, v/V, d/D, p/P, anything else Print status.
## j) Ninjojen tapaan. Piiloutuuko nmap-skannaus hyvin palvelimelta? Vinkkejä: Asenna Apache. Aja nmap-versioskannaus -sV tai -A omaan paikalliseen weppipalvelimeen. Etsi Apachen lokista tätä koskevat rivit. Wiresharkissa "http" on kätevä filtteri, se tulee siihen yläreunan "Apply a display filter..." -kenttään. Nmap-ajon aikana p laittaa packet tracing päälle. Vapaaehtoinen lisäkohta: jääkö Apachen lokiin jokin todiste nmap-versioskannauksesta?

## k) UDP-skannaus. UDP-skannaa paikkalinen kone (-sU). "Mulla olis vitsi UDP:sta, mutta en tiedä menisikö se perille":
## l) Miksi UDP-skannaus on hankalaa ja epäluotettavaa? Miksi UDP-skannauksen kanssa kannattaa käyttää --reason flagia ja snifferiä? (tässä alakohdassa vain vastaus viitteineen, ei tarvita testiä tietokoneella)

## Sources
[Tero Karvinen/eettinen-hakkerointi-2023](https://terokarvinen.com/2023/eettinen-hakkerointi-2023/)

[joohoi ffuf esitys](https://www.youtube.com/watch?v=mbmsT3AhwWU)

[nmap org port scanningtechniques](https://nmap.org/book/man-port-scanning-techniques.html)

[nmap org port scanning basics](https://nmap.org/book/man-port-scanning-basics.html)

[Tero Karvinen/fuzz-urls-hidden directories](https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/?fromSearch=ffuf#your-turn---challenge)

[Ffuf github](https://github.com/ffuf/ffuf)

[Tero Karvinen fuffme-web-fuzzing-target](https://terokarvinen.com/2023/fuffme-web-fuzzing-target-debian/)

[adamtlangley ffufme github](https://github.com/adamtlangley/ffufme)
