# h2 Sniff-n-Scan
Tehtävät on tehty käyttäen kali linuxa 2023.3. Ffuf versio 2.1.0. Nmap versio 7.94.
## x) Lue/katso ja tiivistä. Hoikkala "joohoi" 2020: Still Fuzzing Faster (U fool). In HelSec Virtual meetup #1. (about 1 hour) Lyon 2009: Nmap Network Scanning: Chapter 15. Nmap Reference Guide: Port Scanning Basics. Port Scanning Techniques.

FUZZ:
Käytetään automatisoimaan pyyntöjen lähettämistä kohteelle (verkkosivusto, rajapinnat jne.) ja samalla yritetään löytää virheitä, jotta kohde paljastaa joitakin tietoja, joita sen ei pitäisi paljastaa.
Yleisiä kohteita: GET-parametrit: nimi, arvot... Headers: host, authentication, cookies, proxy headers. Post-data: lomaketiedot, JSON-tiedostot.

Filters (-f/c,l,r,s,t,w,tila) ja matchers (-m/c,k,mode,r,s,t).

Mielenkiintoisia ffuf-esimerkkejä videossa. Pass bruteforcing. Virtualhost discovery. Parameter discovery. Template injections. Usein käytettyjä komentoja ovat esimerkiksi: -od output found context to txt file, -u target url, -w wordlist, -v verbose output, -json json output.

Sanalistoja: seclists, fuzz.dp, all.txt, jhaddixin sanalista. Tarvittaesa luo kohdennettuja sanalistoja: kohde-, sovellus-, konteksti-, ohjelmointikieli-, kielikohtaisia.

## a) Fuff. Ratkaise Teron ffuf-haastebinääri.
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

## b) Fuffme. Asenna Ffufme harjoitusmaali paikallisesti omalle koneellesi. Ratkaise tehtävät.
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
 
Komento antaa vastaukseksi vain paljon 429 HTTP statusksia. Tämä tarkoittaa, että minut on väliaikasesti estetty lähettämästä pyyntöjä kyseiseen osoitteeseen. 

    $ ffuf -v -c -w ~/wordlists/common.txt -t 5 -p 0.1 -u http://localhost/cd/rate/FUZZ -mc 200,429
![Alt text](/H2Sniff-n-Scan/h2.b11.png) 

Subdomains - Virtual Host Enumeration
Porttiskannaa paikallinen kone (127.0.0.2 tms), sieppaa liikenne snifferillä, analysoi.
## c) nmap TCP connect scan -sT
## d) nmap TCP SYN "used to be stealth" scan, -sS (tätä käytetään skannatessa useimmin)
## e) nmap ping sweep -sn
## f) nmap don't ping -Pn
## g) nmap version detection -sV (esimerkki yhdestä palvelusta yhdessä portissa riittää)
## h) nmap output files -oA foo. Miltä tiedostot näyttävät? Mihin kukin tiedostotyyppi sopii?
## i) nmap ajonaikaiset toiminnot (man nmap: runtime interaction): verbosity v/V, help ?, packet tracing p/P, status s (ja moni muu nappi)
## j) Ninjojen tapaan. Piiloutuuko nmap-skannaus hyvin palvelimelta? Vinkkejä: Asenna Apache. Aja nmap-versioskannaus -sV tai -A omaan paikalliseen weppipalvelimeen. Etsi Apachen lokista tätä koskevat rivit. Wiresharkissa "http" on kätevä filtteri, se tulee siihen yläreunan "Apply a display filter..." -kenttään. Nmap-ajon aikana p laittaa packet tracing päälle. Vapaaehtoinen lisäkohta: jääkö Apachen lokiin jokin todiste nmap-versioskannauksesta?
## k) UDP-skannaus. UDP-skannaa paikkalinen kone (-sU). "Mulla olis vitsi UDP:sta, mutta en tiedä menisikö se perille":
## l) Miksi UDP-skannaus on hankalaa ja epäluotettavaa? Miksi UDP-skannauksen kanssa kannattaa käyttää --reason flagia ja snifferiä? (tässä alakohdassa vain vastaus viitteineen, ei tarvita testiä tietokoneella)

## Sources
[Tero Karvinen/eettinen-hakkerointi-2023](https://terokarvinen.com/2023/eettinen-hakkerointi-2023/)

[Tero Karvinen/fuzz-urls-hidden directories](https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/?fromSearch=ffuf#your-turn---challenge)

[Ffuf github](https://github.com/ffuf/ffuf)

[Tero Karvinen fuffme-web-fuzzing-target](https://terokarvinen.com/2023/fuffme-web-fuzzing-target-debian/)

[adamtlangley ffufme github](https://github.com/adamtlangley/ffufme)
