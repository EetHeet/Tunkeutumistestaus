
# h1

## Tehtävät: https://terokarvinen.com/2024/eettinen-hakkerointi-2024/

- x) Lue/katso ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva.)
€ Santos et al: The Art of Hacking (Video Collection): [..] 4.3 Surveying Essential Tools for Active Reconnaissance. Sisältää porttiskannauksen. 5 videota, yhteensä noin 20 min.
Hutchins et al 2011: Intelligence-Driven Computer Network Defense Informed by Analysis of Adversary Campaigns and Intrusion Kill Chains, chapters Abstract, 3.2 Intrusion Kill Chain.

- a) Ratkaise Over The Wire: Bandit kolme ensimmäistä tasoa (0-2).

- b) Asenna WebGoat ja kokeile, että pääset kirjautumaan sisään.

- c) Ratkaise WebGoatista tehtävät "HTTP Basics" ja "Developer tools". Katso vinkit alta.

- d) Ratkaise ja selitä PortSwigger Labs: Lab: SQL injection vulnerability in WHERE clause allowing retrieval of hidden data. (Edellyttää ilmaista rekisteröitymistä. Tehtävän voi ratkaista pelkästään selaimen osoitekenttää muokkaamalla.) Selitä, mitä tekniikoita kokeilit, ja mitä hyökkäyksesi eri osat tekevät.

- e) Asenna Linux virtuaalikoneeseen. Suosittelen joko Kali (viimeisin versio) tai Debian 12-Bookworm.

- f) Porttiskannaa 1000 tavallisinta tcp-porttia omasta koneestasi 
(localhost). Analysoi tulokset.

- g) Porttiskannaa kaikki koneesi (localhost) tcp-portit. Analysoi tulokset. (Edellisissä kohdissa mainittuja analyyseja ei tarvitse toistaa, voit vain viitata niihin ja keskittyä eroihin).

- h) Tee laaja porttiskanaus (nmap -A) omalle koneellesi (localhost), kaikki portit. Selitä, mitä -A tekee. Analysoi tulokset. (Edellisissä kohdissa mainittuja analyyseja ei tarvitse toistaa, voit vain viitata niihin ja keskittyä eroihin.).

- i) Asenna demoni tai pari (esim Apache ja SSH). Vertaile, miten localhost:n laajan porttiskannauksen tulos eroaa.

- j) Kokeile ja esittele jokin avointen lähteiden tiedusteluun sopiva weppisivu tai työkalu. Hyviä esimerkkejä löytyy Bazzel: IntelTechniques: Tools ja Bellingcat: Resources, voit myös käyttää muuta itse valitsemaasi työkalua. Työkalua pitää siis myös kokeilla, pelkkä nimen mainitseminen ei riitä. Pidä esimerkit harmittomina, älä julkaise kenenkään henkilökohtaisia salaisuuksia raportissasi.

- k) Vapaaehtoinen: Tee lisää harjoituksia alustoilta, joihin tässä on tutustuttu: PortSwigger Academy, Over the Wire, WebGoat. Hakkeroimaan oppii hakkeroimalla.

## x https://learning.oreilly.com/videos/the-art-of/9780135767849/9780135767849-SPTT_04_00

### 4.1

- Aktiivisen ja passiivisen tiedustelun ero on se, että passiivinen tiedustelu on "näkymätöntä", eikä siinä esim. lähetetä yhtään pakettia kohteeseen.

- Aktiivisessa tiedustelussa haetaan varmennusta niihin tietoihin mitä kerättiin passiivisen tiedustelun aikana.

### 4.2

- Tiedustelu on tärkeää, jotta voi säästää aikaa suodattamalla tärkeä tieto ja löytää oikeat järjestelmät mihin hyökätä.

- Aktiivisessa tiedustelussa on kolme vaihetta
    - Porttiskannaus
    - Webbi aplikaatioiden priorisointi (mihin tahdomme hyökätä)
    - Haavoittuvuus skannaus

### 4.3

- Porttiskannerit

    - Nmap on paras porttiskanneri
    - Masscan on nopein porttiskanneri
    - Udprotoscanner on nopea UDP skanneri

- Verkkopalvelun arvionti työkalu
    
    - EyeWitness

### 4.4

- "Networ Vulnerability" skannerit
    - ilmaiset
        - OpenVAS
        - Nmap
    - maksulliset
        - Nessus
        - Nexpose
        - Qualys

- "Web Vulnerability" skannerit
    - Nikto
    - WPScan
    - SQLMap
    - Burp Suite
    - Zed Attack Proxy

## a https://overthewire.org/wargames/bandit/

Otetaan ssh yhteys Over The Wire bandit0 palvelimelle komennolla "ssh bandit0@bandit.labs.overthewire.org -p2220" ja kirjaudutaan sisään salasanalla "bandit0"

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/55ef422c-dc1d-46eb-a77d-e118262de012)

Seuraavan tason salasanan saa lukemalla kotihakemistossa olevan "readme" tiedoston.

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/3c98a13b-4302-4834-a81f-e18630001bb8)

Seuraavan tason salasanan saa lukemmalla "-" tiedoson komennolla "cat < -"

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/a4804cf9-a02e-4f03-b78f-aae7bb682887)

Seuraavan tason salasanan saa lukemalla "spaces in this filename" komennolla "cat "spaces in this filename""

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/90182a3a-b2c1-4bf4-9830-dafb42aa2444)

## c

HTTP Basics ensimmäinen tehtävä
![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/26d33f84-6d53-4d28-a240-69d0fcbb99c8)

Tässä näemme käyttäjän antaman datan "POST" pyynnön ja siitä tulleen vastauksen.
![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/8a06221d-f6ab-42a4-8949-8b6be543d5be)

HTTP Basics toinen tehtävä
![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/602758e5-b930-4138-b19e-29def25cf6d4)

salaisen numeron löytää html koodissa olevasta "hidden” elementistä
![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/9afc41a4-da26-4162-863b-d3e5f98c556c)

Developer Tools ensimmäinen tehtävä
![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/aeb56369-ffef-411d-987a-50d53ecc9bc3)

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/3b4ff497-c1b8-479a-8338-381d90ef7a4b)

Developer Tools toinen tehtävä
![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/8f18c004-6dbe-46ec-ac43-8f4b890c3b9e)

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/2ef7a6cb-a796-47f5-b190-0ec11b5a09ed)

## d https://portswigger.net/web-security/sql-injection/lab-retrieve-hidden-data

Ensimmäisenä kokeilin sitä, että muuttaa URL:ssä olevaa productid kohtaa, jotta saisin näkyviin tuotteita, mitä ei pitäisi näkyä.

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/a53ac0df-86bd-411e-a345-f1c992a474fc)

Käsin tämä on hieman hidasta mutta, jos käytämme esimerkiksi ffuf ohjelmaa voimme kokeilla niin monella numerolla kuin haluamme erittäin nopeasti.

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/c809d8b9-2b06-4c4b-beb6-857b4e6e2ce7)

Mutta tämä ei paljasta kaikkia julkaisemattomia tuotteita(paljastaa 4 extra tuotetta) sillä, jotkut tuotteiden nettisivuista eivät johda mihinkään.

Seuraavaksi lähdin miettimään ongelmaa SQL kyselyn kautta. Tehtävänannossa on kerrottu että kun sivu hakee tuotteita se tekee tämmöisen SQL kyselyn "SELECT * FROM products WHERE category = 'Gifts' AND released = 1" elikkä jos käyttäjä valitsee kategoriaksi "Gifts" "category" kohtaan laitetaan arvo "Gifts". "Gifts" kategorian URL:ssä voimme nähdä miten sivusto syöttää tietokantaan kyseisen arvon.

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/a71bf643-2076-4813-b7c4-b8d4994fd6f6)

Jos muokkaamme URL:llää vaihtamalla "category kohtaan" "' OR 1 = 1 --" niin SQL kysely näyttäisi silloin tältä "SELECT * FROM products WHERE category = '' OR 1 = 1 --' AND released = 1" tämä muuttaa kyselyn siten että se kysyy kaikki arvot "products" taulusta missä kategoria on '' tai 1 = 1 ja koska 1 = 1 on aina totta niin kysely näyttää kaikki arvot "products" taulusta ja koska lisäämme perään "--" niin "' AND released = 1" jätetään huomioimatta, koska se on silloin kommentti.

## f

Skannataan 1000 yleisintä tcp-porttia komennolla "nmap --top-ports 1000 localhost"

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/b6e44fa3-1835-49de-bc10-39abd0ad3184)

Kaikki portit ovat kiinni

## g

Skannataan kaikki tcp portit komennolla "nmap -p1-65535 localhost"

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/fbd317df-db1f-4d65-9fda-a1ff6bccf602)

Kaikki portit ovat kiinni

## h

Tehdään laaja porttiskannaus komennolla "nmap -A localhost"

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/f81443d4-e7fa-447f-8329-914171fbf9f8)

Kaikki portit ovat kiinni

Komento -A laittaa käyttöjärjestelmän tunnistamisen, version tunnistamisen, skripti skannauksen ja "tracerouten" päälle.

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/25716e85-fb65-402f-b1c5-cb6477e16ffe)

## i

Kun asennamme apcahen ja openssh-clientin ja laitamme ne päälle komennoilla "systemctl start apache2" ja "systemctl star ssh"

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/0a9e47f5-dc8a-4c13-96c3-0e58c028ef4f)

Tehdään laaja porttiskannaus uudestaan

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/d3675a48-d1c2-4f43-b2e9-d30247f99adf)

Näemme että portit 22 ssh ja 80 http ovat auki

## j https://pentester.com/

Sivusto hakee vuotaneita käyttäjiä ja paljastaa niiden salasanat ja muuta informaatiota, mitä vuodoista löytyy.

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/05e86787-2a43-4f42-9c4c-89db24d19a91)

