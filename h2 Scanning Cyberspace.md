# h2 Scanning Cyberspace

## a) Asenna Kali virtuaalikoneeseen (VirtualBox)

Ladataan kalin VirtualBox image https://www.kali.org/get-kali/#kali-virtual-machines

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/26a07b13-3c9d-40fe-9989-6ef1dc5f7060)

Kun ladattu tiedosto on purettu, avataan VirtualBox ja valitaan "add" vaihtoehto

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/79f99d56-bd11-4483-baa9-5cb2eee7ac37)

Mennään kansioon ja valitaan valmis kali Linux image

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/f7af52bd-3516-4f4f-95ec-639899c6593e)

## b) Asenna Metasploitable 2 virtuaalikoneeseen

Ladataan Metasploitable 2 https://sourceforge.net/projects/metasploitable/

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/b22db75c-a984-47da-b4f9-4cfc0ea32492)

Puretaan tiedosto ja valitaan VirtualBoxissa "new" vaihtoehto

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/52af641e-63f7-4b1f-b69b-c38d1c7c7acf)

Annetaan virtuaalikoneelle nimi ja valitaan Linux

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/4f2b3b95-fdae-4702-9617-16d9bf160f13)

Sitten painetaan "next" kunnes ollaan tällä sivulla

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/7f0d8a8c-75c4-43e9-a63e-e70b6b0bae5c)

Valitaan "Use an Existing Virtual Hard Disk File"

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/eaebe0e0-ebdf-4eff-ae53-6488b422e873)

Valitaan "add" ja mennään kansioon missä Metasploitable 2 sijaitsee

## c) Tee koneille virtuaaliverkko

Painetaan Tools kohtaan ja valitaan "Network"

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/9ab3cf6c-31d2-4bce-b7a7-849dadbdb80f)

Mennään “Host-Only Networks" välilehdelle ja valitaan "Create"

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/59e0314a-447b-4330-b6a7-279e27d7bf11)

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/a21d0842-8db6-47d9-924f-d05a48370289)

Virtuaaliverkko on luotu

Lisätään kali ja Metasploitable 2 virtuaaliverkkoon

Valitaan ensiksi Metasploitavle 2, jonka olen nimennyt "MS2" ja painetaan "Settings"

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/fa1909f1-0f9e-47e8-b426-f84f447da0db)

Setting kohdalla laitetaan "Attached to" kohtaa "Host-only Adapter" ja valitaan aikaisemmin luotu verkko

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/461f592f-02de-404f-8514-8c3813fff379)

Seuraavaksi lisätään Kali virtuaaliverkkoon

Kalissa laitetaan "Adapter 1" kiinni NAT:tiin niin pääsemme kalilla vielä tarvittaessa verkkoon kiinni

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/7843dcde-7560-4f87-a4d4-8f035e358883)

"Adapter 2" määrittelemme samalla tavalla kuin Metasploitable 2:n verkon

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/e7038c88-6a87-41fb-97a5-d28d2008f71f)

Kun yritämme pingata googlea Metasploitable 2:sessa niin se ei onnistu mutta se on kuittenkin yhdistetty luomaamme virtuaaliverkkoon

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/f8ccd17c-468f-40bc-8fd8-04c5ee849515)

Kalissa näemme, että ensimmäinen verokortti jolla pääse nettiin on "DOWN", koska otin piuhan irti  

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/6d8f5899-867b-4648-a08e-7a092a2d63b2)

mutta toinen verkkokortti on kytketty kiinni virtuaaliverkkoon

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/aaae062c-c204-4675-be94-a38956133c68)

## d) "Etsi Metasploitable porttiskannaamalla (db_nmap -sn)"

Jotta voimme skannatessa tallentaa tulokset suoraan tietokantaan meidän pitää ensiksi käynnistää postgresql komennolla.

    $ systemctl start postgresql

Ensimmäisellä käyttökerralla tietokanta pitää luoda ja sen voi tehdä komennolla

    $ sudo mfsdb init

Tämä komento käynnistää ja luo tietokannat (msf ja mfs_test), luo tietokantaan käyttäjän, luo config tiedoston ja taulukkorakenteet

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/f02de387-e043-4f70-a46c-d43cc65a2323)

Sitten voimme käynnistää msfconsolen komennolla

    $ sudo msfconsole

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/834c8f75-0b42-4c7e-9f4a-5082e7fc57b1)

Sitten voimme etsiä Metasploitble 2 koneen komennolla

    $ db_nmap -sn

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/b4b7805f-3b42-4228-ba17-b08706c9a65c)

Wiresharkkia seuraamalla voimme nähdä että nmap lähettää ARP kyselyn kaikille mahdollisille 192.168.56.0 verkossa oleville IP-osoitteille ja katsoo ketkä vastaa.

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/a96ded67-736c-4a61-8c4b-9926c7e4730d)

Koska skannauksessa näkyy vain kaksi laitetta joilla on VirtualBox verkkokortit

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/c67ee7ec-7232-4353-b64e-63a5803aa993)

Ja tiedän että kalin ip on 192.168.56.102 niin Metasploitablen ip on 192.168.56.101

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/e29f31f4-352b-447d-9171-77040b2553ea)

## e) "Porttiskannaa Metasploitable huolellisesti (db_nmap -A -p0-)."

Tehdään laaja skannaus Metasploitablen kaikille porteille komennolla

    $ nmap -A -p0-

### Tulokset:

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/138190c4-0c1b-4062-a0b2-538a9bdb3827)

Metasploitableen voi siirtää tiedostoja ftp:llä ja siihen ei tarvitse salasanaa

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/c5af26da-17f6-41d4-81b2-d4fe41ced290)

Metasbloitableen voi ottaa yhteyden ssh:lla

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/9af918e1-d8bf-46b0-b300-43880749a77b)

Metasploitableen pääsee ottamaan yhteyden telenetillä ja siellä on sähköpostipalvelin

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/06cf433f-0c51-4bdb-81bc-4b732dbc2abd)

Tästä ehkä eniten kiinnittyy huomioon porttiin 1524 josta pääsee yhdistämään vaikka netcatilla suoraan Metasploitableen rootti oikeuksilla

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/e39fb798-7e92-4bd2-b4b1-83a8976a16ec)

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/25f99189-42ec-4d4e-90f9-fa9133d133f4)

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/d6a2cda9-cc83-41be-b9c1-57979c9bde82)

## f) Tallenna portiskannauksen tulos tiedostoon käyttäen nmap:n omaa tallennusta (nmap -oA foo).

Tallennetaan tulokset komennolla

    $ db_nmap -A -p0- -oA <nimi>

Tiedostot tallennetaan käyttäjän kotikansioon

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/faf00c54-84c6-4f7b-9bfe-3a8e91e54787)

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/f6156acf-2b1a-454b-af49-078d31692c17)

## g) Tallenna shell-sessio tekstitiedostoon script-työkalulla (script -fa log001.txt)

Aloitetaan shell-session tallentaminen komennolla

    $ script -fa <nimi>

Tämä luo tiedoston käyttäjän kotikansioon joka tallentaa kaiken mitä shell-sessiossa tapahtuu

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/7fa34b79-b2a9-4bb6-bbf3-2abb6ef056b3)

tallennus loppuu kun komennolla 

    $ exit

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/fa908c5c-e6d7-4df6-beca-2b5e2413f3d1)

## h) Etsi kaikki maininnat jostain osoitteesta, palvelusta tai vastaavasta kaikista tallennetuista tuloksista ja lokeista.

En oikee hiffaa mitä tässä haetaan

## i) Anna esimerkit nmap ajonaikaisista toiminnosta.

v/V: kertoo mitä ohjelma tekee

p/P: laittaa packet tracingin päälle/pois

d/D: kertoo myös mitä ohjelma tekee

## lähteet

Tehtävät: https://terokarvinen.com/2024/eettinen-hakkerointi-2024/

https://nmap.org/book/man-runtime-interaction.html

https://docs.rapid7.com/metasploit/metasploitable-2/

https://www.kali.org/faq/

https://nmap.org/
