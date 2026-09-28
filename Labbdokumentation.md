# Labbmiljö, Git, CLI och AI

  

###  **Namn:** _Murad Abdullah_
###  **Kurs:** _IT Infrastructur secure cloud (ISCX26)_
### **Datum:** _2026-09-20_
### **Kursmål:** _8,9,10 & 11._

  

##  Indroduktion

Detta är en praktisk moment där jag visar upp konfiguration av nätverk i virutal machine, hantering av filer & behörigheter via kommandoren i Windows och linux. Versionhantering i git, där commits sker löpande. Utvärdering av AI och granskning under arbetet.

  
  

##  Labbmiljö & Nätverk (Kursmål 8)

  

vi börjar med att sätta på två virtuella maskiner i **VirtualBox** som ska placeras eventuellt på ett gemenssamt nätverk. 



 |CPU kärnor  |      ram           |            OS               |
|-------------|---------------|----------------------------------|
|    _4_       |      8GB   |`Windows server 2025`      |_Windows_|
|    _2_       |      8GB    |     `Ubuntu-26.04. LTS`      |_Ubuntu_|
  
  


| Hostname              |               OS              |           IP address       |  Subnätmask         | Default gateway                       
|-----------------------|------------------------------ |-----------------------|-------------------|-----------------|
| **Windowslabbserv**   |     _Windows Server 2025_     | `192.168.1.50`         |``255.255.255.0``  | -
| **labbmiljo**         |    _Ubuntu 26.04 LTS_         |  `192.168.1.51`        | ``255.255.255.0`` |-

- Innan installationen bör man ändra till _internal network_ eller _Host-only_ vilket görs enkelt genom VirtualBox, _inställningar_ > _Nätverk_

![alt text](</bilder/VM adapters.png>)


- Under installationen av `Ubuntu 26,04 LTS` så kunde man redan konfigurera statiska IP-adresser genom att välja nätverkskortet `enp0s3`, ändra från `DHCP` till manuellt och sedan fylla i följande IP-adresserna och subnät.
---

För `Windows Server 2025` i `Sconfig` väljer man 8) __"Network Settings

![alt text](/bilder/Sconfig.png)

Nedan så ser den förvalda IP_adressen, den ändras via Network adapter och man ska välja *statisk*.


```

6 |168.254.229.209 | Ethernet | Intel (R) PRO/1000 MT Desktop Adapter

```

### <U>Försök 1</u>

Adressen kunde inte verkställas manuellt vi Sconfig, problemet var att den inte lyckades uppdatera den nya adressen. lösningen var att genomföra konfigurationen manuellt via `PowerShell`

```PowerShell
New-NetIPAddress -Interface 6 -IPAddress "192.168.1.50" -PrefixLenght 24
```
Därefter bled det lyckad och jag kunde bekräfta genom ```ipconfig```


# Kommandoradsarbete & Felsökning (Kursmål 9)

I detta avsnitt demonsteras hur man hanterar kommandoren i Linux (bash) och Windows (PowerShell), mapp och filhantering, konfiguration av användarbehörigheter,, verifering av nätverk och slutligen felsökning.

## **Ubuntu (Bash)**

Vid skapandet av mappkatalogen:

Först kollar vi vart vi befinner oss:


![ss](/bilder/pwd.png)


- Vi skapar mappstrukturen med `sudo mkdir -p /var/systementor/konsultdata`

- `touch /var/systementor/konsultdata/anteckningar.txt` skapar en fil inuti katalogen

![bild](/bilder/mkdir.png)

för att bekräfta så flyttar vi till sökvägen med `cd` och listar filen med `ls`

![bild](/bilder/touch.png)

- Sen skapar vi användargruppen **konsulter** & tilldelar `sudo groupadd konsulter`

- `sudo chown :konsulter /var/systementor/konsultdata` 

![bild](/bilder/sudo%20chmod.png)


 kommandot ``ls -la`` för att inspektera behörigheterna
- behörigheter har tilldelats
![bild](/bilder/permissions.png)


För att verifera att anslutnigen mellan maskinerna behöver vi pinga till dens IP address, i detta fall **Windows server**

```
ping 192.168.1.50

```

![bild](/bilder/ping.png)

- [ x ] Lyckad resultat!



### Detaljer om nätverksortet

``` Bash
ip addr show

```


![bild](/bilder/ip%20addr%20show.png)

## **Windows(PowerShell)**

Det gäller sammma princip här att skapa mappen ``C:\Systementor\KonsultData`` via PowerShell, samma kommando gäller öven här 

![dd](/bilder/mkdir%20Win.png)



kommmandot visar behörighetsstruktren och vi kan konstatera att vi har full kontrol

``` PowerShell
Get-Acl 

```

![](/bilder/getAcl.png)


Nu ska vi verifera anslutingen **TILL** _Ubuntu Servern_ med 

```PowerShell
ping 192.168.1.51
```

![ff](/bilder/pingUbun.png)


Anslutningen lyckades!


### Nätverksinställningarna:

![ss](/bilder/IPconfWin.png)

