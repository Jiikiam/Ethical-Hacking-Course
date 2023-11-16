# h4 Totally Legit Sertificate
## x) Lue/katso ja tiivistä
OWASP Top 10:2021, A01: Broken Access Control

Syitä:
-Vähimmäisoikeusperiaatteen tai oletuskiellon puuttuminen
-Pääsynhallinnan tarkistusten ohittaminen muuttamalla URLia, sisäisen sovelluksen tilaa tai HTML-sivua tai hyödyntämällä hyökkäystyökalua API-pyyntöjen muokkaamiseen.
-Salliminen toisen henkilön tilin katseluun tai muokkaamiseen toimittamalla sen yksilöllinen tunniste
-API:n käyttö ilman tarvittavia pääsynhallintatarkistuksia POST-, PUT- ja DELETE-toiminnoille.
-Käyttöoikeuksien korottaminen.
-Metadatan manipulointi
-CORS-määritysvirheet
-Pakotettu selaus

Keinot välttää:
Vähimmäisoikeusperiaate, oletuskielto, pääsynhallintamekanismien toteuttaminen vain kerran ja niiden uudelleenkäyttö koko sovelluksessa, sekä muita käytäntöjä kuten istunnon tunnisteiden oikea käsittely ja API-pyynnöille asetettavat rajoitukset.

A10: SSRF Server-Side Request Forgery (SSRF)

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

PortSwigget Academy:
  Access control vulnerabilities and privilege escalation (IDOR on osa tätä)
  Server-side template injection
  Server-side request forgery (SSRF)
  Cross-site scripting
Karvinen 2020: Using New Webgoat 2023.4 to Try Web Hacking
