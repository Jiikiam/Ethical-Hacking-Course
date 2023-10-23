#h0 Alkutesti
## a) Sieppaa ja analysoi verkkoliikennettä. 
Koska tehtävän anto oli hyvin vapaa käytin verkkoliikenteen analysointiin wirehark työkalua.  Yritin tarkastella TCP handshakea (3-way handshake). TCP käyttää kolmivaiheista kättelyä (three-way handshake) luodakseen luotettavan yhteyden laitteiden välille. Yhteyden muodostaminen koostuu kolmesta vaiheesta: SYN, SYN-ACK ja ACK. Yhteyden tarkastelua varten hain selaimella helsec.fi osoitetta. Tulos oli seuraava (piilotin tulosteesta ip osoitteet varmuuden vuoksi):

![Alt text](/H0startingtest/h0startingtest.png)

Kuvassa nähdään tcp protocolla muodostavan handshaken hakemani osoitteen kanssa. Yleisesti oletuksena on, että liikennettä näkyisi vain yksi jokaista kohtaa ja tässä vaiheessa ei riitä aika sen tutkimiseen. Tämä kyllä johtuu siitä, että liikenne lähtee kahdesta eri portista, mutta en osaa sanoi, että miksi näin tapahtuu. Kuitenkin tcp handshaken jälkeen nähdään, että yhteys on onnistunut, koska get http pyyntö meni hakemalleni sivulle. Get pyynnön avaamalla nähdään mihin osoitteeseen selain otti yhteyden.

![Alt text](/H0startingtest/h0http.png)

# Lähteet

[Tero Karvinen Ethical Hacking 2023](https://www.google.com](https://terokarvinen.com/2023/eettinen-hakkerointi-2023/)https://terokarvinen.com/2023/eettinen-hakkerointi-2023/)
