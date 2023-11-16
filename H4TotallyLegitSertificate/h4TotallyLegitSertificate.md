# h4 Totally Legit Sertificate
## x) Lue/katso ja tiivistä
- OWASP Top 10:2021, A01: Broken Access Control

Syitä:
Vähimmäisoikeusperiaatteen tai oletuskiellon puuttuminen.
Pääsynhallinnan tarkistusten ohittaminen muuttamalla URLia, sisäisen sovelluksen tilaa tai HTML-sivua tai hyödyntämällä. Hyökkäystyökalua API-pyyntöjen muokkaamiseen.
Salliminen toisen henkilön tilin katseluun tai muokkaamiseen toimittamalla sen yksilöllinen tunniste.
API:n käyttö ilman tarvittavia pääsynhallintatarkistuksia POST-, PUT- ja DELETE-toiminnoille.
Käyttöoikeuksien korottaminen.
Metadatan manipulointi.
CORS-määritysvirheet.
Pakotettu selaus.

Keinot välttää:
Vähimmäisoikeusperiaate, oletuskielto, pääsynhallintamekanismien toteuttaminen vain kerran ja niiden uudelleenkäyttö koko sovelluksessa, sekä muita käytäntöjä kuten istunnon tunnisteiden oikea käsittely ja API-pyynnöille asetettavat rajoitukset.

- A10: SSRF Server-Side Request Forgery (SSRF)

Syitä:
SSRF-virheet tapahtuvat, kun verkkosovellus hakee etäresurssia ilman käyttäjän toimittaman URLin asianmukaista validointia. Palomuuri, VPN tai ACL eivät auta. Koska nykyaikaiset verkkosovellukset tarjoavat käyttäjille käteviä ominaisuuksia, URLin hakeminen on yleinen tilanne.

Keinot välttää:
Verkkokerroksessa:
Etäresurssien segmentointi eri verkoissa.
Oletuskielto.
Sovelluskerroksessa:
Puhdista ja validoi kaikki käyttäjän toimittamat syötteet.
Toteuta URL-skeema, portti ja kohde positiivisella sallittujen kohteiden listalla.
Älä lähetä raakoja vastauksia asiakkaille.
Poista HTTP-uudelleenohjaukset.

- Access control vulnerabilities and privilege escalation

Vertical access controls, Horizontal access controls, Context-dependent access controls. Vertical privilege escalation, Horizontal privilege escalation. IDORs. Referer-based access control, Location-based access control. 

- Server-side template injection

Voi mahdollistaa hyökkääjän syöttää ja suorittaa omaa koodia palvelinpuolella.


- Server-side request forgery (SSRF)

Voi mahdollistaa hyökkääjän suorittaa pyytöjä palvelimille, jotka vaarantavat turvallisuuden.

- Cross-site scripting

Cross-site scripting toimii manipuloimalla haavoittuvaa verkkosivustoa niin, että se palauttaa käyttäjille haitallista JavaScript-koodia. Kun haitallinen koodi suoritetaan kohteen selaimessa, hyökkääjä voi vaarantaa kohteen vuorovaikutuksen sovelluksen kanssa.

- Karvinen 2020: Using New Webgoat 2023.4 to Try Web Hacking

New webgoat asennus.

## a) Totally Legit Sertificate 
Ensiksi asensin javan kalille ja sen jälkeen latasin zapin "Cross Platform Package" https://www.zaproxy.org/download/ sivulta.

    $ sudo apt-get install default-jre

Siirryin downloads kansioon, jossa purin ja käynnistin zapin.

    $ cd Downloads
    $ unzip ZAP_2.14.0_Crossplatform.zip 
    $ cd ZAP_2.14.0 
    $ ./zap.sh
![Alt text](/H4TotallyLegitSertificate/h4.a1.png)

Genereroin sertificaatin zapissa: Tools -> Options -> Dynamic SSL Certificates, ja muutin: Network -> Local Servers/Proxies portin 8081, koska kohtasin aluksi ongelmia. 

![Alt text](/H4TotallyLegitSertificate/h4.a2.png)
![Alt text](/H3LabKid/h4.a6.png)

Kävin lisäämässä sertifikaatin firefoxiin: Firefox -> Preferences -> Privacy & Security -> View Certificates -> Authorities -> Import.

![Alt text](/H4TotallyLegitSertificate/h4.a3.png)

Kävin muuttamassa fireroxin "network settings" asetuksia. 

![Alt text](/H4TotallyLegitSertificate/h4.a5.png)

Nyt kun avasin kalin foorumin näkyi liikenne zapissa.

![Alt text](/H4TotallyLegitSertificate/h4.a4.png)

## b) Kettumaista
Ensiksi suljin ZAPin ja muutin fireroxin "network settings" asetuksia valitsemalla "no proxy". 

Firefox burger-valikko -> more tool -> Extensions for developers -> hea: foxyproxy -> install FoxyProxy Standard.

Avasin FoxyProxy Standardin laajennuksista. Painoin add ja lisäsin nimen, ip:n ja portin.

![Alt text](/H4TotallyLegitSertificate/h4.b1.png)

Avasin kali docs sivun ja nyt liikenne näkyy ZAPissa.

![Alt text](/H4TotallyLegitSertificate/h4.b2.png)

## 



## Lähteet
1 https://terokarvinen.com/2023/eettinen-hakkerointi-2023/

2 https://portswigger.net/web-security/cross-site-scripting

3 https://portswigger.net/web-security/ssrf

4 https://portswigger.net/web-security/server-side-template-injection

5 https://portswigger.net/web-security/access-control

6 https://terokarvinen.com/2023/webgoat-2023-4-ethical-web-hacking/

7 https://www.youtube.com/watch?v=JY1hsK-8gjI&t=12s&ab_channel=ArkenstoneLearning




