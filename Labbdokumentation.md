# Labbmiljö, Git, CLI och AI

Kurs: Introduktion till yrkesrollen och grunderna i IT-infrastruktur

Namn: Daniel Bergström

Datum: 2026-xx-xx

## Introduktion 
Här kommer jag att dokumentera arbetet med uppgiften Labbmiljö, Git, CLI och AI. 
![Virtuella maskinerna](Bilder/VMsVirtualbox.png)


## Labbmiljö och nätverk
Jag har skapat en labbmiljö med två virtuella maskiner i VirtualBox som är min hypervisor. Labbmiljön består av en Linux-server och en Windows-server. 

Jag har placerat båda servarna i ett gemensamt internt nätverk för att de ska kunna kommunicera enkelt med varandra utan att behöva gå via en router samt blockeras av brandväggen. Maskinerna ligger även i samma subnät för att servrarnas operativsystem ska fatta att de finns på samma lokala nätverk. Se tabell nedan: 

|Hostname|Operativsystem|IP-adress|Subnätmask|Standard Gateway|
|:---    |:---          |:---     |:---      |:---            |
