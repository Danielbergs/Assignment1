# Labbmiljö, Git, CLI och AI

Kurs: Introduktion till yrkesrollen och grunderna i IT-infrastruktur

Namn: Daniel Bergström

Datum: 2026-xx-xx

## Introduktion 
Här kommer jag att dokumentera arbetet med uppgiften Labbmiljö, Git, CLI och AI. 


## Labbmiljö och nätverk
Jag har skapat en labbmiljö med två virtuella maskiner i VirtualBox som är min hypervisor. Labbmiljön består av en Linux-server och en Windows-server. 
![Virtuella maskinerna](bilder/VMsVirtualbox.png)

Jag har placerat båda servarna i ett gemensamt internt nätverk som jag döpt till LabNet. Jag har även gett dem statiska IP-adresser och placerat dem i samma subnät. Detta för att de ska kunna kommunicera enkelt med varandra utan att behöva gå via en router. 

### Statisk IP-adress i Ubuntu
Jag konfigurerade en statisk IP-adress på Ubuntu servern genom Netplan. Det jag gjorde var att stänga av DHCP och ändrade konfigurationen i nätverkskortet ´enp0s3´
![Ubunutu-konfiguration](bilder/UbuntuStaticIPSubnet.png) 

### Statisk IP-adress i Windows 11
Jag konfigurerade även en statisk IP-adress på Windows severn genom Windows > Network & internet > Ethernet. Jag ändrade DHCP till manuell och ställde in IP-adress och subnätmasken.
![Windows-konfiguration](bilder/Windows11StaticIPSubnet.png)

Se tabell nedan: 

|Hostname|Operativsystem|IP-adress|Subnätmask|Standard Gateway|
|:---    |:---          |:---     |:---      |:---            |
|Ubuntu  |Linux         |192.168.50.23|255.255.255.0|Ingen|
|Windows11|Windows      |192.168.50.24|255.255.255.0|Ingen|


## Kommandoradsarbete & Felsökning

### Linux Bash
Jag skapade mapp och mellanmapparna Var, Systementor, Konsultdata i Linux Bash: _sudo mkdir -p /var/systementor/konsultdata_:
![Mapparna](bilder/varsystementorkonsultdata.png)

Sedan skapade jag filen anteckningar.txt i mappen kosultdata: _sudo touch anteckningar.txt_.

Efter det skapade jag gruppen Konsulter: _sudo groupadd konsulter_.
Och därefter tilldelade jag gruppen konsulter till mappen: _sudo chown :konsulter /var/systementor/konsultdata_
Sedan samma med filen anteckningar.txt: _sudo chown :konsulter /var/systementor/konsultdata/anteckningar.txt_

Jag ställde in behörigheterna på mappen kosnultdata, jag ville att ägaren skulle ha tillgång till att läsa, skriva och använda(rwx), gruppen skulle kunna läsa och använda (rx) mappen och övriga skulle inte ha någon behörighet alls. Det gjorde jag genom: _sudo chmod 750 /var/systementor/konsultdata_.


I filen anteckningar.txt ville jag ha behörigheterna: Ägare - rw, Gruppen - r och Övriga ingen behörighet. Det gjorde jag genom: _sudo chmod 640 /var/systementor/konsultdata/anteckningar.txt_ 

För att kunna komma åt mappen och filen enligt gruppens behörigheter behövde jag först lägga till mig själv i gruppen Konsulter. Detta gjorde jag genom kommandot: _sudo usermod -aG konsulter lablord_, lablord är mitt användarnamn i Ubuntu servern. Sedan använde jag kommandot _ls -la_ för att se behörigheterna för mappen och filen: 


![Permissions](bilder/Permissionskonsultdataanteckningar.txt.png) 

På bilden visar det att behörigheterna för mappen _konsultdata_ är rwxr-x---, vilket motsvarar 750. Samt att konsulter är gruppen som är tilldelad mappen. 
Vi ser även att behörigheterna i filen _anteckningar.txt_ är rw-r-----, vilket motsvarar 640. Även där kan vi se att gruppen konsulter är tilldelad filen.  


### Nätverksanslutningen i Ubuntu
Jag verifierade att nätverksanslutningen till min Windows 11 server funkade genom att pinga till den serverns IP-adress: _ping 192.168.50.24_ : 

![Ping till Windows servern](bilder/UbuntutillWindowsPing.png)

Nätverkskortets detaljer: 

![NätverkskortdetaljerUbuntu](bilder/NätverkskortDetUbuntu.png)


### Windows Powershell
Jag skapade mappen C:\Systementor\KonsultData i PowerShell genom: _New-Item -ItemType Directory -path "C:\Systementor\KonsultData"_

![Mapparna](bilder/MapparnaWindows.png)

Sedan kollade jag behörigheten i mappen Konsultdata i PowerShell genom att använda: _Get-Acl_ (Access Control List). Jag använde även _Get-Acl | Format-List_ för att få en tydligare vy rad för rad i PowerShell: 

![Permissions](bilder/Permissionkonsultdatawindows.png)

Det vi kan se där är att Labadmin är ägare till mappen. Det visar att ingen specifik grupp är angiven som ägare för denna mapp. Administratörer får full kontroll över mappen. Användare har behörighet att läsa och använda mappen. Användare som däremot har autentiserats av windows har behörigheten att ändra i mappen. 


### Nätverksanslutnignen i Windows
Jag verifierade att nätverksanslutningen till min Ubuntu server fungerade genom att pinga till den serverns IP-adress: _ping 192.168.50.23_:

![Ping till Ubuntu servern](bilder/WindowstillUbuntuPing.png)

Nätverksinställnignarna i Windows servern i kort: 

![Nätverksinställningar](bilder/NätverksinstWindows.png) 

För att få fram denna information använde jag: _ipconfig /all_. Det visar bland annat den IP-adress och subnätmask som servern konfigurerats med. DHCP är avstängt eftersom jag manuellt konfigurerade IP-adressen. 