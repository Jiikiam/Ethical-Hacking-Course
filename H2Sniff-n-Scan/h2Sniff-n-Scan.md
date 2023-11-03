# h2 Sniff-n-Scan
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
    
![Alt text](/H2Sniff-n-Scan/h1.a1.png)

Avasin selaimen localhost osoitteen 127.0.0.2:8000, jotta sain varmistuksen, että harjoitusmaalin on pystyssä.

![Alt text](/H2Sniff-n-Scan/h1.a2.png)

Sitten fuzzaamaan. Ensiksi testasin komentoa (-v=verbose output, -c=colorize output, -w=wordlist, -u:target url):

    $ ffuf -v -c -w common.txt -u http://127.0.0.2:8000/FUZZ 
    
![Alt text](/H2Sniff-n-Scan/h1.a3.png)

Komento antoi todella paljon vastauksia, joten haluamme suodattaa ei toivottut vastauksen pois. Se onnistuu jo mainitsemieni suodattimien avulla (-f/c,l,r,s,t,w,mode). Katsomalla tulostusten listaa on vastauksilla paljon yhteistä. Tässä tapauksessa voimme suodattaa vastaukset vaikka koon mukaan lisäämällä -fs 154(filter size) komennon perään.

    $ ffuf -v -c -w common.txt -u http://127.0.0.2:8000/FUZZ -fs 154
Tämän jälkeen vastaukseksi tuli 7 aliverkko osoitetta.

![Alt text](/H2Sniff-n-Scan/h1.a4.png)

Kun testasin antamia aliverkkojen osoitteita, kaikki näyttivät melkein samalta pois lukien osoitte http://127.0.0.2:8000/render/https://www.google.com. 

![Alt text](/H2Sniff-n-Scan/h1.a5.png)

![Alt text](/H2Sniff-n-Scan/h1.a6.png)

Jokaisen sivun paitsi /render... sivun sanoman "You found it!" mukaan oletan, että pääsin tehtävän läpi.

## b) Fuffme. Asenna Ffufme harjoitusmaali paikallisesti omalle koneellesi. Ratkaise tehtävät (kaikki paitsi ei "Content Discovery - Pipes")
Basic Content Discovery
Content Discovery With Recursion
Content Discovery With File Extensions
No 404 Status
Param Mining
Rate Limited
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
