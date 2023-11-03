# h2 Sniff-n-Scan
## x) Lue/katso ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva.)
Hoikkala "joohoi" 2020: Still Fuzzing Faster (U fool). In HelSec Virtual meetup #1. (about 1 hour)
Lyon 2009: Nmap Network Scanning: Chapter 15. Nmap Reference Guide:
Port Scanning Basics (opettele, mitä tarkoittavat: open, closed, filtered; muuten vain silmäily)
Port Scanning Techniques (opettele, mitä ovat: -sS -sT -sU; muuten vain silmäily)

FUZZ:
Is used to automate sending requests to target (website, APIs, etc) while trying to find bugs. So that the target gives some information it shouldn't give. 
Common targets: get parameters: name, values... Headers: host, authentication, cookies, proxy headers. Post date: form data, JSON files.

filters and matchers
Interesting ffuf examples in the video. Pass bruteforcing. virtualhost discovery. parameter discovery, -od output found context to txt file. Template injections. 

seclists, fuzz.dp, all.txt jhaddix wordlist. Generate own playlists: target-, app-, context-, prog.language-, language-specific.

## a) Fuff. Ratkaise Teron ffuf-haastebinääri. Artikkelista Find Hidden Web Directories - Fuzz URLs with ffuf voi olla apua.
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
