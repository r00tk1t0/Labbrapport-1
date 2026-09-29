# Labbrapport:  labbmiljö, Git, CLI och AI

  

###  **Namn:** _Murad Abdullah_ (ICS26)
###  **Kurs:** _Introduktion till yrkesrollen och grunderna i IT-infrastruktur_
### **Datum:** _2026-09-29_
### **Kursmål:** _8,9,10 & 11_

  

##  Introduktion

Detta är en praktisk moment där jag visar upp konfiguration av nätverk i virtual machine, hantering av filer & behörigheter via kommandoren i Windows och linux. Versionhantering i git, där commits sker löpande. Utvärdering av AI och granskning under arbetet.

  
  

##  Labbmiljö & Nätverk (Kursmål 8)

  

vi börjar med att sätta på två virtuella maskiner i **VirtualBox** som ska placeras eventuellt på ett gemenssamt nätverk. 



 |CPU kärnor  |      ram           |            OS               |
|-------------|---------------|----------------------------------|
|    _4_       |    8GB       |`Windows server 2025`      |_Windows_|
|    _2_       |    8GB       | `Ubuntu-26.04. LTS`      |_Ubuntu_|
  
  
  


| Hostname              |               OS              |           IP address       |  Subnätmask         | Standard Gateway               
|-----------------------|------------------------------ |-----------------------|-------------------|-----------------|
| **Windowslabbserv**   |     _Windows Server 2025_     | `192.168.1.50`         |``255.255.255.0``  | - 
| **labbmijo**         |    _Ubuntu 26.04 LTS_         |  `192.168.1.51`        | ``255.255.255.0`` |-

(_Själva standard gateway är inte konfigurerad då det inte behövs eftersom båda maskinerna ligger på samma nätverk_)


- Innan installationen bör man ändra till _internal network_ eller _Host-only_ vilket görs enkelt genom VirtualBox, _inställningar_ > _Nätverk_

![VM adapters](</bilder/VM adapters.png>)


- Under installationen av `Ubuntu 26,04 LTS` så kunde man redan konfigurera statiska IP-adresser genom att välja nätverkskortet `enp0s3`, ändra från `DHCP` till manuellt och sedan fylla i följande IP-adresserna och subnät.
---

För `Windows Server 2025` i `Sconfig` väljer man 8) __"Network Settings

![Sconfig](/bilder/Sconfig.png)

Nedan så ser den förvalda IP_adressen, den ändras via Network adapter och man ska välja *statisk*.


```

6 |169.254.229.209 | Ethernet | Intel (R) PRO/1000 MT Desktop Adapter

```

### <U>Försök 1</u>

Adressen kunde inte verkställas manuellt vi Sconfig, problemet var att den inte lyckades uppdatera den nya adressen. lösningen var att genomföra konfigurationen manuellt via `PowerShell`

``` PowerShell
New-NetIPAddress -InterfaceIndex 6 -IPAddress "192.168.1.50" -PrefixLength 24
```
Därefter bled det lyckad och jag kunde bekräfta genom ```ipconfig```


# Kommandoradsarbete & Felsökning (Kursmål 9)

I detta avsnitt demonsteras hur man hanterar kommandoren i Linux (bash) och Windows (PowerShell), mapp och filhantering, konfiguration av användarbehörigheter,, verifering av nätverk och slutligen felsökning.

 ## **Ubuntu** (Bash)

Vid skapandet av mappkatalogen:

Först kollar vi vart vi befinner oss:


![Pwd](/bilder/pwd.png)


- Vi skapar mappstrukturen med `sudo mkdir -p /var/systementor/konsultdata`

- `sudo touch /var/systementor/konsultdata/anteckningar.txt` skapar en fil inuti katalogen

![mkdir](/bilder/mkdir.png)

för att bekräfta så flyttar vi till sökvägen med `cd` och listar filen med `ls`

![touch](/bilder/touch.png)

- Sen skapar vi användargruppen **konsulter** med `sudo groupadd konsulter`

- `sudo chown :konsulter /var/systementor/konsultdata` 
- `sudo chown :konsulter /var/systementor/konsultdata/anteckningar.txt`

 tilldelar behörigheterna 


- `sudo chmod 750 /var/systementor/konsultdata` 
- `sudo chmod 640 /var/systementor/konsultdata/anteckningar.txt`


![chmod](/bilder/sudo%20chmod.png)


 kommandot ``ls -la`` för att inspektera behörigheterna

- behörigheter har tilldelats

Mappen (750) Ägaren har rätt till att läsa, skriva, öppna. Gruppen enbart läsa, öppna. De övriga ingen åtkomst.
Filen (640) ägaren kan läsa & skriva, gruppen enbart läsa och övriga ingen åtkomst.

![permissions](/bilder/permissions.png)



För att verifera att anslutnigen mellan maskinerna behöver vi pinga till dens IP address, i detta fall **Windows server**

```
ping 192.168.1.50

```

![ping](/bilder/ping.png)

- [ x ] Lyckad resultat!



### Detaljer om nätverksortet

``` Bash
ip addr show

```


![ipaddr](/bilder/ip%20addr%20show.png)

## **Windows (PowerShell)**

Det gäller sammma princip här att skapa mappen ``C:\Systementor\KonsultData`` via PowerShell, samma kommando gäller öven här 

![mkdirWin](/bilder/mkdir%20Win.png)



kommmandot visar behörighetsstruktren

``` PowerShell
Get-Acl C:\Systementor\KonsultData

```

![getACL](/bilder/getAcl.png)


Här har vi en mer detaljerad lista,
``` PowerShell
Get-Acl C:\Systementor\KonsultData | Format-list

```
Mappen ägs av Admin, både SYSTEM och admin har kontroll och vanliga användare kan läsa samt skapa filer, mappen har fått samma behörighet som C:

![getACList](/bilder/getACLlist.png)

Nu ska vi verifera anslutingen **TILL** _Ubuntu Servern_ med 

```PowerShell
ping 192.168.1.51
```

![PingUbun](/bilder/pingUbun.png)


Anslutningen lyckades!


### Nätverksinställningarna:

![IPconfWin](/bilder/IPconfWin.png)

# Git & Versionshantering (Kursmål 10):

### Länk till min Git-repository:

  ### [Labbrapport: labbmiljö, Git, CLI & AI](https://github.com/r00tk1t0/Labbrapport-1.git)

>_Git commit historik:_ 
>>
>![GitLog](/bilder/gitlogNy.png/)


# AI-logg & Reflektion (Kursmål 11) 




![Geminis svar](/bilder/GeminiSvar.png)
### AI-verktyg som användes: **Gemini**

### Prompt: "hur konfigurerar jag statisk IP-adress för Windows Server"

### Kritisk granskning av AI: 

Svaret jag fick var mestadels korrekt men i början så nämner den inte ``Sconfig`` vilket är ett kommandoradserktyg där man ändrar Te.x nätverksinställningar eller ändrar namn på host, men den antog att jag använde GUI (grafisk gränssnitt).
Den föreslog även upsättning av DNS (8.8.8.8) vilket är irrelevant för denna miljö, även för standard gateway `192.168.1.1` vilket det inte behövs heller.

En liten sak jag märkte dock på kommandot `New-NetIPAddress -InterfaceAlias`, AI använde sig utav parametern ``"InterfaceAlias "Ethernet"``, medan jag använde "`New-NetIPAddress -InterfaceIndex 6`, skillnaden är att jag fick använda dens ID  istället för en nätverkskortets namn (**"Ethernet"**). Utöver svaret från Gemini så fanns det inga hallucinationer eller säkerhetsbrister.

Jag verfierade svaret genom att köra kommandot i virtuella Windows servern och bekräftade genom `/ipconfig all`. 






