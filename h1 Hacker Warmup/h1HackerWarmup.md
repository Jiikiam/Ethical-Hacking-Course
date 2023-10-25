# h1 Hacker Warmup

Note: Assignment instructions have been given in Finnish, however, the I will answer in English. Below each question is direct translations from ChatGPT.

The System I am using is the latest Kali Linux [Kali.org website](https://www.kali.org/releases/).

## x) Lue/katso ja tiivistä (Read/watch and summarize). (Santos et al: The Art of Hacking (Video Collection): [..] 4.3 Surveying Essential Tools for Active Reconnaissance. Hutchins et al 2011: Intelligence-Driven Computer Network Defense Informed by Analysis of Adversary Campaigns and Intrusion Kill Chains, chapters Abstract, 3.2 Intrusion Kill Chain.)

Active reconnaissance differs from passive recon because now your actions can be seen and packets are sent to the target new work. (The illegal part starts)

Active recon can be noticed unlike passive recon.
- Port scanning some new info and maybe include some passive recon info
- Vulnerability scans
- Often can be noticable, but mentioned that many times no one doesn't look the logs

Video states that active recon is very important part of the hacking process

Methology
Port Scanning
  - confirm passive recon phase open ports and find more open ports (!!nmap!!)/ Masscan fastest port scanner. Not as versitile as nmap. Udpprotoscanner for udp ports.
Web service review
  - Which web apps to attack firts - prioritizing
  - Eyewitness tool for large networks
Vulnerability Scanning
  - will be noticed if detection is present
  - External portion vulnerabilities!	
  Network vulnerability scanners mentioned: OpenVAS, NESSUS, Nexposure, Qualys, Nmap(limited) with scripts
  Web vulnerability scanners mentioned: Nikto, WPScan, SQLMap, Burp Suite, Zed Attack Proxy

According to the research the intusion kill chain what and APTs use consist of seven steps.
1. Reconnaissance
2. Weaponization
3. Delivery
4. Exploitation
5. Installation
6. Command and Control (C2)
7. Actions on Objectives


## a) Ratkaise Over The Wire: Bandit kolme ensimmäistä tasoa (0-2) (Solve the first three levels (0-2) of Over The Wire: Bandit).

I installed ssh. Enabled it and started the ssh service.

	$ sudo apt-get install ssh
	$ sudo systemctl enable ssh
  $ sudo service ssh start 

## b) Ratkaise Challenge.fi:stä yksi tehtävä (Complete one task from Challenge.fi.), esim. Challenge.fi 2021 Where was this picture taken, Encoding basics. Tai joku Challenge.fi 2022.

## c) Ratkaise PortSwigger Labs: Lab: SQL injection vulnerability in WHERE clause allowing retrieval of hidden data. (Edellyttää ilmaista rekisteröitymistä. Tehtävän voi ratkaista pelkästään selaimen osoitekenttää muokkaamalla.)
## d) Asenna Linux virtuaalikoneeseen. Suosittelen joko Kali (viimeisin versio) tai Debian 12-Bookworm.
## e) Porttiskannaa 1000 tavallisinta tcp-porttia omasta koneestasi (localhost). Analysoi tulokset.
## f) Porttiskannaa kaikki koneesi (localhost) tcp-portit. Analysoi tulokset. (Edellisissä kohdissa mainittuja analyyseja ei tarvitse toistaa, voit vain viitata niihin ja keskittyä eroihin).
## g) Tee laaja porttiskanaus (nmap -A) omalle koneellesi (localhost), kaikki portit. Selitä, mitä -A tekee. Analysoi tulokset. (Edellisissä kohdissa mainittuja analyyseja ei tarvitse toistaa, voit vain viitata niihin ja keskittyä eroihin.).
## h) Asenna ja käynnistä jokin palvelin (apache, ssh...) koneellesi. Vertaile, miten porttiskannauksen tulos eroaa.
## i) Kokeile ja esittele jokin avointen lähteiden tiedusteluun sopiva weppisivu tai työkalu. Hyviä esimerkkejä löytyy Bazzel: IntelTechniques: Tools ja Bellingcat: Resources, voit myös käyttää muuta itse valitsemaasi työkalua. Työkalua pitää siis myös kokeilla, pelkkä nimen mainitseminen ei riitä. Pidä esimerkit harmittomina, älä julkaise kenenkään henkilökohtaisia salaisuuksia raportissasi.
## j) Vapaaehtoinen: Tee lisää harjoituksia alustoilta, joihin tässä on tutustuttu: PortSwigger Academy, Over the Wire, Challenge.fi. Hakkeroimaan oppii hakkeroimalla.
