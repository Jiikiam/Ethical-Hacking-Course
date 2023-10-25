# h1 Hacker Warmup

Note: Assignment instructions have been given in Finnish, however, the I will answer in English. After each question is direct translations from ChatGPT.

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


## a) Ratkaise Over The Wire: Bandit kolme ensimmäistä tasoa (0-2) (Solve the first three levels (0-2) of Over The Wire: Bandit) [overthewire.org](https://overthewire.org/wargames/bandit/).

I installed ssh. Enabled it and started the ssh service.

	$ sudo apt-get install ssh
	$ sudo systemctl enable ssh
	$ sudo service ssh start 
Then I did the 0-2 levels. 
Level 0 command: ssh bandit0@bandit.labs.overthewire.org -p2220 and password: bandit0. 
Level 1 commands: ls, cat readme. The readme file contained the following string: NH2SXQwcBdpmTEzi3bvBHMM9H66vVXjL
Level 2 commands: ssh bandit1@bandit.labs.overthewire.org -p2220 and password bandit1. ls -la, cat < -.

![Alt text](/h1HackerWarmup/h1.1.png)

![Alt text](/h1 Hacker Warmup/h1.2.png)

## b) Ratkaise Challenge.fi:stä yksi tehtävä (Complete one task from Challenge.fi.) [Genz challenge 2021](https://2021.challenge.fi/challenges).

I solved all 4 "Where was this picture taken?" 

![Alt text](/h1 Hacker Warmup/h1.b1.png)

## c) Ratkaise PortSwigger Labs (Solve PortSwigger Labs): Lab: SQL injection vulnerability in WHERE clause allowing retrieval of hidden data.

I tried to display the gategory from nothing to 1=1 so it would display all the rows in the category section in the database. 

It worked with /filter?category=' or 1=1-- after the web page address.

## d) Asenna Linux virtuaalikoneeseen (Install Linux on a virtual machine).

I have already installed kali linux 2023.3 with the help of this video [How To Install Kali Linux in VirtualBox (2023)](https://www.youtube.com/watch?v=l0JgWilK6ok&ab_channel=KskRoyal)

## e) Porttiskannaa 1000 tavallisinta tcp-porttia omasta koneestasi (localhost). Analysoi tulokset (Port scan the 1000 most common TCP ports from your own machine (localhost). Analyze the results).

I used nmap to scan the tcp ports. As found in the picture only one port is open tcp port 22 (ssh service). The upper command scan 1000 most popular tcp ports. With nmap --top-ports you can specify the amount of top ports that you want to scan.

	$ nmap localhost
 	$ nmap --top-ports 1000 localhost
  
![Alt text](/h1 Hacker Warmup/h1.e1.png)

## f) Porttiskannaa kaikki koneesi (localhost) tcp-portit. Analysoi tulokset (Port scan all the TCP ports on your machine (localhost). Analyze the results.).

I scanned tcp ports between 1-65535. Again only the ssh port 22 was open.

	$ nmap -p 1-65535 localhost
![Alt text](/h1 Hacker Warmup/h1.f1.png)

## g) Tee laaja porttiskanaus (nmap -A) omalle koneellesi (localhost), kaikki portit. Selitä, mitä -A tekee. (Perform a comprehensive port scan (nmap -A) on your own machine (localhost), all ports. Explain what -A does.).

 	$ nmap -A -p 1-65535 localhost
The result showed more information because of the -A. The -A: "Enable OS detection, version detection, script scanning, and traceroute" as seen in the picture. Apparently it also scanned my ssh-hostkeys which was a surprise for me.

![Alt text](/h1 Hacker Warmup/h1.g1.png)

## h) Asenna ja käynnistä jokin palvelin koneellesi. Vertaile, miten porttiskannauksen tulos eroaa (Install and start a server on your computer. Compare how the port scan results differ.).

I installed apache2 and started it. Now nmap command gives the following: 

	$ nmap localhost
![Alt text](/h1 Hacker Warmup/h1.h1.png)

## i) Kokeile ja esittele jokin avointen lähteiden tiedusteluun sopiva weppisivu tai työkalu (Try and introduce a web page or tool suitable for open-source intelligence gathering). 

Two weeks ago I used [urlvoid](https://www.urlvoid.com/) because I checked the web page's URL that scammers sent to me. The URL was Mobiili.pivo-fi.com, and if you know something about URLs, you right away notice that the URL isn't legit. Urlvoid tells how long the URL has been registered, the IP address, and some other information. I checked the URL again, and now every piece of information has changed besides the URL. The first picture is from two weeks ago, and the second one is from today.

![Alt text](/h1 Hacker Warmup/h1.i1.png)

![Alt text](/h1 Hacker Warmup/h1.i2.png)

## j) Vapaaehtoinen: Tee lisää harjoituksia alustoilta, joihin tässä on tutustuttu (Optional: Do more exercises on the platforms that have been introduced here:): PortSwigger Academy, Over the Wire, Challenge.fi.
