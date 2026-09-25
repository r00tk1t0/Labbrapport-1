# Labbmiljö, Git, CLI och AI

  

###  **Namn:** _Murad Abdullah_
###  **Kurs:** _IT Infrastructur secure cloud (ISCX26)_
### **Datum:** _2026-09-20_
### **Kursmål:** _8,9,10 & 11._

  

##  Indroduktion

Detta är en praktisk moment där jag visar upp konfiguration av nätverk i virutal machine, hantering av filer & behörigheter via kommandoren i Windows och linux. Versionhantering i git, där commits sker löpande. Utvärdering av AI och granskning under arbetet.

  
  

##  Labbmiljö & Nätverk (Kursmål 8)

  

vi börjar med att sätta på två virtuella maskiner i **VirtualBox** som ska placeras eventuellt på ett gemenssamt nätverk. 



 |CPU kärnor  |   ram |     OS |
|-------------|---------------|----------------------------------|
| _4_      |       8GB   |`Windows server 2025`      |_Windows_|
| _2_       |    8GB    |     `Ubuntu-26-04. LTS`      |_Ubuntu_|
  
  


| Hostname  |            |   OS              |         IP adress|  subnätmask     |     Standard Gateway 
|--------------------|-----------------------|--------------------|----------------|---- |--------|
| **Windowslabbserv**  ||  Windows Server 2025 | `192.168.1.50`  |``255.255.255.0``|-
| **labbmiljo** |      |    Ubuntu 26.04 LTS   |  `192.168.1.51`   | `255.255.255.0`| -

- Innan installationen bör man ändra till _internal network_ eller _Host-only_ vilket görs enkelt genom VirtualBox, _inställningar_ > _Nätverk_

![alt fdtext](image.png)

- Under installationen av `Ubuntu 26,04 LTS` så kunde man redan konfigurera statiska IP-adresser genom att välja nätverkskortet `enp0s3`, ändra från `DHCP` till manuellt och sedan fylla i följande IP-adresserna och subnät.
---

För `Windows Server 2025` i `Sconfig` väljer man 8) __"Network Settings

Nedan så ser den förvalda IP_adressen, den ändras via Network adapter och man ska välja *statisk*.
![alt text](image-1.png)


```

6 |168.254.229.209 | Ethernet | Intel (R) PRO/1000 MT Desktop Adapter

```

### <U>Försök 1</u>

Adressen kunde inte verkställas manuellt vi Sconfig, problemet var att den inte lyckades uppdatera den nya adressen. lösningen var att genomföra konfigurationen manuellt via `PowerShell`

```PowerShell
New-NetIPAddress -Interface 6 -IPAddress "192.168.1.50" -PrefixLenght 24
```
Därefter bled det lyckad och jag kunde bekräfta genom ```ipconfig```
