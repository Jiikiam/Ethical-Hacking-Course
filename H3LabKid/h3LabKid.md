#h3 Lab Kid
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

Tämän jälkeen testasin pingiä kalin ja metasploitablen välillä. Näyttää toimivan. Lisäksi testasin 

![Alt text](/H3LabKid/h3.c9.png)
![Alt text](/H3LabKid/h3.c10.png)

## d

## Lähteet
https://rikumannonen935063021.wordpress.com/

https://sourceforge.net/projects/metasploitable/

https://www.youtube.com/watch?v=aYxfhMrjVhk&ab_channel=RKiLAB

https://www.geeksforgeeks.org/how-to-install-metasploitable-2-in-virtualbox/


