# h5 Täysin Laillinen Sertifikaatti

## a) Asenna OWASP ZAP, generoi CA-sertifikaatti ja asenna se selaimeesi.

Asennetaan ensiksi `Foxyproxy` mennään osoitteeseen [`https://addons.mozilla.org/en-US/firefox/addon/foxyproxy-standard/`](https://addons.mozilla.org/en-US/firefox/addon/foxyproxy-standard/) ja painetaan `Add to Firefox`

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/df421845-c54c-41a8-8c8c-5d44086f1043)

Sitten asennetaan `Zap` kommennolla

    $ sudo apt install zaproxy

Kun `zaproxy` on asennettu määritellään `Foxyproxy`. Valitaan oikeasta ylänurkasta palapelinpalasta `FoxyProxy` ja painetaan `Options`

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/1cda9ec6-37ac-43c4-9d7e-c2c7c98046b7)

Painetaan `Proxies` ja `Add` sen jälkeen valitaan proxille nimi laitetaan `Hostname` = `localhost` ja portiksi vaikka `8081` tällä ei ole niin väliä kunhan se laitetaan Zap:issa samoin

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/61489e53-7788-42d0-9d16-32ac88d0f02f)

Lopuksi painetaan `Save`

Seuraavaksi avataan `zap` ja `tools` alta valitaan `Options...`

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/6b246edb-6778-44e1-8f5d-0acf42d489cd)

`Options` etsitään `Network` ja sieltä `Local Servers/Proxies`. Laitetaan `Main Proxy` kohtaan samat tiedot kuin `FoxyProxy`

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/30f4f86d-755e-48a6-bd4c-c68b52b0a382)

Tämän jälkeen valitaan `Server Certificates` ja painetaan `Save`

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/e03dce63-8f42-4e47-900d-85f29b1fdcb8)

Sitten palataan `Firefox` avataan asetukset ja haetaan `certificates` tämän jälkeen valitaan `View Certificates`. Seuraavaksi painetaan `Import...`

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/f22d7d04-d9f5-4da0-aa0d-40d7cc279b5d)

Etsitään minne tallensimme sertifikaatin ja lisätään se.

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/5215ed62-5a9f-4a19-9b1e-b4dc2fc7cdd2)

Ja `OK`

Sitten `FoxyProxy` asetuksista valitaan aikaisemmin luotu proxy

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/23904e51-e898-48ad-b2b7-df0f04170388)

Nyt kun avaamme `YouTube` niin hakupyynnöt näkyvät zapissa

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/4152f3d8-0bc4-48a6-88d7-a658e0a47d1b)

## b) Käytä FoxyProxyn "Patterns" -toimintoa, niin että vain valitsemasi weppisivut ohjataan Proxyyn.

Ohjataan PortSwiggerin labrat ja localhost, niin että vain ne sivut näkyvät ZAP:issa

Avataan FoxyProxyn asetukset mennään `Proxies` välilehdelle ja lisätään nämä sinne

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/57a27590-7d5b-4e48-9360-dba021573ed0)

## PortSwigger Labs. Ratkaise tehtävät. Selitä ratkaisusi

### [Insecure Direct Object Reference (IDOR)](https://portswigger.net/web-security/access-control/lab-insecure-direct-object-references)

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/d3335e3d-c5fd-4338-bf85-862840083214)

Tehtävässä piti löytää salasan `carlos` nimiselle käyttäjälle. Tehtävässä oltiin kerrottu, että kaikki chat logit haetaan staattisella `URL`

Ensimmäisenä kun avasin labran menin `Live chat` osioon.

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/060e83d3-f998-4285-be23-c50e2c8e338d)

Sen jälkeen katsoin zapilla minkälaisia pyyntöjä lähetetään, kun painetaan `View transcript`

Zapissa näkyy että nappi tekee `GET` pyynnön serverille, joka näyttää tältä.

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/c6d1b9db-5679-4f13-ade8-5f7725cc1914)

Vastaus näyttää tältä

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/e8f9ec19-653f-4ab8-90b4-647a7e152ecb)

Pyynnöstä ja tehtävänannosta voi päätellä, että pyyntö lataa tiedoston minkä nimi on `2.txt`, joten jos zapilla muuttaa pyyntöä, että se lataisikin `1.txt`.

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/0dc645df-32e9-453a-ac72-0f578515fbb4)

Niin vastaus on tämä

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/a79d809a-5ad2-4b5c-95f0-663488a9cfd2)

salasana näkyy pyynnön vastauksessa

## Path travesal

### [File path traversal, simple case](https://portswigger.net/web-security/file-path-traversal/lab-simple)

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/40a2a07d-92bd-4868-9ebd-ca578494cae5)

Tehtävässä pitää löytää tiedoston `/etc/passwd` sisältö

Zapissa näkyy että kuvat haetaan tiedostonimen perusteella.

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/35695a30-7ed8-4cf3-9f8c-046aeda1d3cc)

Tästä voi päätellä, että kuvat sijaitsee jossakin kansiossa serverillä ja jos muutamme tätä kansiota haussa niin voimme ehkä saada muutakin tietoa.

Muutetaan hakua niin, että mennään kansiossa taaksepäin ja sitten kansioon `/etc/passwd`

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/045b11d6-b010-40c0-80ac-dc3d321d08d4)

Nyt `GET` pyynnön vastauksessa kun muutamme `BODY` tyypin tekstiksi näemme `/etc/passwd` sisällön.

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/0fe396e9-79ac-4055-902b-361ad8e1e3e6)

### [File path traversal, traversal sequences blocked with absolute path bypass](https://portswigger.net/web-security/file-path-traversal/lab-absolute-path-bypass)

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/90975327-bcb4-41a4-b72e-f2b228ba7bb3)

Tässä tehtävässä pitää taas löytää `/etc/passwd`. Tehtävässä on myös kerrottu, että applikaatio blokkaa "läpikulku sekvenssejä"

Kokeillaan siis tässä muuttaa kuvan pyyntöä niin, että käytämme haussa halutun tiedoston absoluuttista polkua eli `/etc/passwd`

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/12588f4b-9667-40e5-8846-5e3d57207397)

Saimme tiedoston sisällön.

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/2d1a5d88-a077-4678-ab7a-252779510657)

### [File path traversal, traversal sequences stripped non-recursively](https://portswigger.net/web-security/file-path-traversal/lab-sequences-stripped-non-recursively)

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/757986bf-c44e-4ce4-8203-b15a7cc43be2)

Tehtävässä pitää löytää `/etc/passwd` tiedosto. Tehtävässä on kerrottu, että käyttäjän syöttämä pyyntö käsitellään niin että siitä poistetaan "läpikulku sekvenssit", mutta se tehdään vain kerran.

Joten jos tekisimme pyynnön esim. `../../../etc/passwd` niin kaikki `../` poistetaan, mutta koska tämä tehdään vain kerran voimme silloin tehdä näin `....//....//....//etc/passwd` ja pyyntö käsitellään niin että siitä tulee `../../../etc/passwd`

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/b0b182e7-bfb4-4818-9587-df70b4c00b19)

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/8629ffb5-6e93-4ac0-b5a2-ddaf1cf3949e)

## Server Side Template Injection (SSTI)

### [Server-side template injection with information disclosure via user-supplied objects](https://portswigger.net/web-security/server-side-template-injection/exploiting/lab-server-side-template-injection-with-information-disclosure-via-user-supplied-objects)

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/52797c52-e6a3-4f97-a7a1-d9fd703b8231)

Kirjaudutaan ensiksi sisään annetuilla tunnuksilla

Sitten mennään jonkun tuotteen sivulle ja painetaan `Edit template`

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/47bbae12-fbcd-4f3e-95c4-e82efd2b69d6)

Jos kokeilemme laitta johonkin näistä arvoista `{{7*7}}`

Saamme tämän virhe ilmoituksen

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/90a2aa10-d856-48ed-aad5-6ef62684f6af)

Näemme että verkkokehyksenä käytetään `django`

https://www.cobalt.io/blog/a-pentesters-guide-to-server-side-template-injection-ssti

Tältä nettisivulta löytyi "Django tircks" josta löysin komennon `{%debug%}`

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/b9c274bf-a31d-4ed5-b9f7-00d4f045c689)

Toisen kivan komennon löysin samalta sivulta `{{settings.SECRET_KEY}}`

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/1ccfabe9-b365-4443-b770-6968a1952699)

Tämä palauttaa suoraan ratkaisun

## Server Side Request Forgery (SSRF)

### [Basic SSRF against the local server](https://portswigger.net/web-security/ssrf/lab-basic-ssrf-against-localhost)

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/998aff45-e5ce-484a-9318-697de1b3ba6c)

Tehtävässä pitää päästä pääkäyttäjän käyttöliittymään käsiksi ja poistaa käyttäjä `carlos`

Kun teemme `POST` pyynnön kysymällä jonkin tuotteen varasto tilannetta niin näemme että applikaatio niin se hakee tiedon toiselta nettisivulta.

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/f7fa2614-2b10-4f36-a14c-ab6c6000b011)

Jos muutamme `stockApi` tehtävässä annettuun `http://localhost/admin` osoitteeseen niin saamme vastauksena tämä, 

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/73f227c8-4512-44a3-9752-b8fb5058f698)

`html` koodista löytyy viite

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/07906f02-c532-4f7f-9c47-e2ef1880b6ce)

Jos jatkamme aikaisempaa osoitetta näin `http://localhost/admin/delete?username=carlos` niin carlos on poistettu.

## Cross Site Scripting (XSS)

### [Reflected XSS into HTML context with nothing encoded](https://portswigger.net/web-security/cross-site-scripting/reflected/lab-html-context-nothing-encoded)

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/974b6d05-845b-41c2-be11-15804acb9172)

Tehtävässä pitää tehdä `cross-site scriptin` hyökkäys ja kutsua `alert` toimintoa

Verkko sivulla on haku funktio johon käyttäjä voi syöttää arvon, ja koska sitä ei ole suojattu mitenkään voimme suorittaa koodia siinä

Laitetaan hakuun tämä

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/1c4c7822-d92b-41c2-b23d-f93e8f9e13dc)

kun suoritamme sen tapahtuu tämä

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/366e6aba-f752-4886-bd98-19d14d696f10)

### [Stored XSS into HTML context with nothing encoded](https://portswigger.net/web-security/cross-site-scripting/stored/lab-html-context-nothing-encoded)

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/bba640ac-901c-49b9-ae8b-5f62687c08c8)

Tehtävässä on tarkoituksen suorittaa `cross-site scriptin` hyökkäys, mikä tapahtuu aina kun kuka tahansa tulee sivulle, tämä onnistuu siten että tallennamme scriptin jonnekin sivulle

Ja mukavasti sattui niin että sivulla on kommentti funktio, jonne voimme tallentaa skriptin

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/27965cc8-c0ff-45b3-a9eb-b1611f284c89)

Tehdään tämmöinen kommentti

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/de5fcf5d-9ecb-4876-b4f9-161e08bc5fd5)

Ja nyt ainakun joku tulee tälle sivulle tapahtuu näin

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/e21128ff-f272-42e4-aff6-d5b893748e40)

## l) Asenna pencode ja muunna sillä jokin merkkijono

Asennetaan `pencode` komennolla

    $ sudo apt install golang-github-ffuf-pencode-dev

muunnetaan merkkijono `jaajaa` komennolla 

    $ echo jaajaa | pencode md5

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/b7a33fc7-774d-48f4-bf17-2a8d6f48cbe7)

## Ratkaise WebGoat 2023.4

### m) (A1) Broken Access Control

#### Hijack a session (1)

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/430611d1-e1be-4a0f-9dc9-1f325c54ed04)

Painetaan access nappia ja katsotaan zapilla minkälaisen pyyntö on

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/ce31d314-737a-4df5-8904-75d9a279c68e)

Tehtävässä on kerrottu, että meidän täytyy ennustaa `hijack_cookie`

Käytetään zapin `Generate Tokens` työkalua

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/db040886-87a5-48bd-abcf-c69e89303522)

Tallenetaan `Tokenit` jotta voimme tarkastella niitä helpommin

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/59295d7c-fc4f-401d-8933-f33f197fc544)

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/9de6f836-7e68-437b-a495-ff54f83663b6)

Jaetaan `Tokeni` kahteen osaan toinen osa alkaa `1419600...` ja toinen osa viivan jälkeen alkaa `17150...`

Kun tarkastelemme tokeneita huomaamme, että ensimmäisestä osasta viimeinen numero muuttuu melkein aina järjestyksessä, mutta joissakin kohtaa se skippaa numeron tai kaksi

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/007e9944-1c5f-4426-884e-c197808c362c)

Tästä voimme päätellä, että jokaisen kirjautumisen yhteydessä annetaan uusi `Tokeni`, jossa viimeinen numero muuttuu aina yhdellä. Joten välistä skipatut numerot ovat, silloin jonkun toisen tokeneita

Muutetaan seuraavaksi pyyntöä, siten että laitetaan tokeni, on `1419600938854570587-1715096114325`

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/f2a1ccf7-a263-4c5a-93cf-f25cb4d4e4b9)

Saamme vastaukseksi

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/8d3260bd-131d-4f77-b4cd-4f56f750d0ed)

#### Insecure Direct Object References (4)

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/702ed8fd-a983-4f57-affc-720696d90ed5)

Ensimmäisessä osiossa tunnistaudutaan tunnuksilla `tom` `cat`

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/b55a9863-c1bf-4745-bf71-736b53685a82)

Toisessa osiossa piti mainita kaksi attribuuttia mitkä näkyvät `GET` pyynnön vastauksessa, mutta eivät näy profiilissa

Kun avaamme vastauksen zapissa näemme tämän

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/e8abad91-f726-483b-a28e-f7548b973f6b)

`Role` ja `userId` eivät näy profiilissa

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/e1dd382d-80ab-4932-bf5c-71cef2484b9f)

Tehtävässä piti saada oman profiilin tiedot, niin että muokkaa vain `URL` osoitetta

Alkuperäinen `URL` on tämä `/WebGoat/IDOR/profile`, joten ajattelin. että jos haluamme saada profiilin tiedot voimme muokata osoitetta näin `/WebGoat/IDOR/profile/2342384`

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/f39666eb-8ded-4af1-bd76-3452e1d44a16)

Se toimi!

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/ba7e2600-8d64-44e7-a80a-b37bbf49a65b)

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/80d4bca5-3bb5-4a71-9348-830d788a4134)

Tässä osassa meidän pitää ensiksi löytää toinen käyttäjä ja koska käyttäjät haetaa `userId` perusteella ajattelin vain `fuzzata` omasta userId:stä ylöspäin

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/288da6b5-b41d-4abb-8921-86c4b8128f10)

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/5954bcf8-e883-48d0-b0df-87b6cf4b7570)

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/29f8767c-abf9-4f31-a02b-ebd660ee9db6)

Löysin käyttäjän `Buffalo Bill`

Seuraavassa osiossa piti muuttaa `Buffalo Bill` `color=red` ja `role=1`

Kokeilin lähettää `POST` pyynnön, jonka headerista muunnan `Conten-Type` ja bodyyn `lisään`

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/e17a9e90-3a91-46e3-9890-1def2c697e16)

Mutta vastauksesta tulee `405` `Method not Allowed`

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/38746ab9-7209-4ffc-a912-96f137d47590)

Joten muutin pyynnön `PUT`

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/c8fd1e61-f504-4da9-abdc-96167d266d64)

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/1de011d6-67a2-4c29-945b-6c7509f554a1)

Se toimi!

#### Missing Function Level Access Control (2) [Ei kohtaa 4 "The company fixed the problem, right?", se saattaa olla rikki]

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/6c5c0574-1b64-4110-a3d3-725aaa7ad09f)

Tehtävässä piti löytää kaksi näkymätöntä valikkoa ja ne löytyivät, kun katsoi sivun html koodia

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/fcf3e072-8757-4385-8fd6-f182864c18cf)

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/9637d103-a0f3-4f8b-b9fe-2f37c138bf88)

Edellisessä osiossa löysimme piilotetun menun urlista `/access-control/users` kokeillaan mitä tapahtuu, kun haemme sitä

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/d3260a1a-e2d2-46c4-9b04-c7544390b2ab)

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/b3425ff8-71bb-4371-a813-6eb822a915d0)

Lisätään pyyntöön `Content-type: application/json`

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/273edac8-6f34-447a-978c-7cc55f04a2bb)

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/12ac25b7-6718-4757-9fd1-2ee749988133)

### n) (A7) Identity & Auth Failure (WebGoat 2023.4)

#### Authentication Bypasses (1)

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/6f93b81e-07c2-4481-a250-59a92ca64918)

Tätä tehtävää en täysin tajunnut, joten katsoin läpikävely ohjeen.

https://www.youtube.com/watch?v=DErUuNMHgJo

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/43d0f979-6eb6-42c1-ab05-731f7ec85bf9)

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/32221e3d-9059-4577-a3f7-4e9a0c8dcd06)

#### Insecure Login (1)

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/08c115e6-0065-4752-8fa2-3a231654a402)

Painetaan `Log in` ja katsotaan zapissa mitä tulee

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/a4e646ba-1aab-42c6-9ed2-fad60fc80bee)

### o) (A10) Server-side Request Forgery (WebGoat 2023.4)

#### Server-Side Request Forgery (2)

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/bbebcb63-3877-48fe-a954-bdd9438f4572)

Napataan pyyntö zapilla

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/561b6d56-c7cc-453f-9bab-06b54d57ded9)

Muutetaan `tom` --> `jerry`

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/873de287-1ea8-4f2d-b562-01e43d40a5e5)

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/01469934-b23c-4bac-bf16-97bc8a6260e6)

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/f42fff52-4c7b-4022-b6c9-b209bc1746cc)

Muutetaan `url=images%2Fcat.png` --> `http://ifconfig.pro`

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/936f1d68-0d91-4e35-a820-4778b0a0bf27)

### p) Client side (WebGoat 2023.4)

#### Bypass front-end restrictions (2)

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/d9a9093f-7581-49b7-b714-4eb4b58880a3)

Lähetetään pyyntö, joka rikkoo kaikki säännöt

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/612c9d1a-83f9-49fb-837f-4cba814c770f)

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/74f6f2d7-58a2-4e57-ad84-64294311926d)

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/2c706a8f-1eb0-4e4a-a052-f8bd8032dfb8)

Rikotaan taas kaikki säännöt

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/582a3097-3a1d-4cc6-af3a-e9b2bb4b0609)

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/3d8dcdf6-0433-46c0-93c0-c72416d0ad1a)

## Lähteet

Tehtävät: https://terokarvinen.com/2024/eettinen-hakkerointi-2024/
