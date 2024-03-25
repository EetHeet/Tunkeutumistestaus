#h0

Kysymykset:
"Mitä teit / minkä komennon annoit
Mitä tapahtui / mitä tuli vastaukseksi
Analyysi edellyttää analyysia. Selitä omin sanoin, mistä on kyse ja mitä komentojen tulosteet / lokit / siepatun liikenteen osat tarkoittavat." 

https://terokarvinen.com/2024/eettinen-hakkerointi-2024/#h0-sieppari-ruispellossa

Annetaan komento <sudo wireshark> niin aukeaa wireshark jolla voidaa analysoida verkkoliikennettä.
![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/bf0528cf-132d-49b2-ab78-8ccace3c5cfd)

Tämä IPv6 Router Solicitation viesti tarkoittaa sitä, että tietokoneeni lähettää viestin missä paikannetaan reitin siinä verkossa missä koneeni on.

Ensimmäinen numero 7 tarkoittaa että monesko ilmoitus se on wiresharkissa

Seuraava numero 40.66... tarkoittaa siitä kun verkkoliikenteen analysointi on aloitettu sekunteina.

Seuraava merkkijono on tietokoneeni IPv6 osoite

Seuraava merkkijon on IPv6 multicast osoite joka lähetetään kaikille reitittimille verkossa

Seuraava on tarkoittaa millä protokollalla viesti on lähetetty eli tässä tapauksessa ICMPv6

Seuraava numero tarkoittaa kuinka iso paketti on eli tässä tapauksessa 62 bittiä
![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/315f67ab-3212-4409-aca8-c85b92b81b0a)

Tässä kuvassa on arp kysely

Ylemässsä kohdassa ip osoite 10.0.2.15 lähettää kyselyn kaikille laitteille verkossa jossa kone etsii laitetta jonka ip osoite on 10.0.2.2

Alempi kohta vastaa laitteen MAC osoitteella, että 10.0.2.2 laitteen MAC osoite on 52:54:00:12:35:02

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/70c20272-ea17-4e07-80cd-eff38b888cc7)




