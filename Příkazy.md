
## Příkazy

### OSPF 
https://itexamanswers.net/2-7-2-lab-configure-single-area-ospfv2-answers.html
```kotlin
// Směrování pomocí OSPF
//tyto příkazy zadávat pro každou síť (nezapomenout na loobacky)
R1(config)# router ospf <ID> //klidně všechny Routery stejný ID
R1(config-router)# router-id <ID Routeru> //(R1 -> 1.1.1.1, R2 -> 2.2.2.2 atd...)
R1(config-router)# network <síť, ve, které má router směrovat> <wilcard mask> area <číslo obalsti> //oblast musí být pro routery, které se mají pingnout, stejná

//Další příkazy
show ip ospf neighbor
show ip route ospf
show ip ospf interface <rozhraní>
```

### ACL
https://itexamanswers.net/5-5-2-lab-configure-and-verify-extended-ipv4-acls-answers.html
```kotlin
R1# access-list ? //postupně pozadávat to, co je potřeba - pomocí ? vyjede nápověda

//<1-99> IP standard access list = filtrování na základě zdrojové adresy
//<100-199> IP extended access list = filtrování na základě zdrojové adresy, cílové adresy a portu

R1(config)# interface <interface, na který má být aplikován ACL>
R1(config-subif)# ip access-group <označení ACL> in/out 
// in = zahazuje příchozí komunikace
// out = zahazuje se odchozí komunikace
```

### NAT
https://itexamanswers.net/6-8-2-lab-configure-nat-for-ipv4-answers.html
- Dynamic NAT = mám množinu definovanou poolem, ze které mohu vybírat
- Static NAT = mám jednu adresu, na které jsou mapovány všechny zařízení za NATem

#### Dynamic NAT
```kotlin
R1(config)# access-list 1 permit 192.168.1.0 0.0.0.255
// Definuje interní adresy, které budou podléhat překladu 
//Pokud není definován žádný přístupový seznam, který by měl být použit pro překlad adres, NAT nebude aplikován na žádný provoz. To znamená, že všechny interní IP adresy by mohly přistupovat na veřejnou síť bez jakéhokoli překladu.

R1(config)# ip nat pool PUBLIC_ACCESS 209.165.200.226 209.165.200.228 netmask 255.255.255.248
// Vytváří adresní pool s názvem PUBLIC_ACCESS - na tyto adresy se budou překládat adresy z příkazu výše
// Rozsah IP adres pro překlad je od 209.165.200.226 do 209.165.200.228 s maskou sítě 255.255.255.248.

R1(config)# ip nat inside source list 1 pool PUBLIC_ACCESS
// Říká, že adresy z access-listu 1 (192.168.1.0. - 192.168.1.255) budou podléhat překladu na adresy definované v poolu PUBLIC_ADDRESS (209.165.200.226 - 209.165.200.228)

//Definuje, kde je nat a kde veřejná síť
R1(config)# interface g0/0/1
R1(config-if)# ip nat inside
R1(config)# interface g0/0/0
R1(config-if)# ip nat outside


//Další příkazy
R1# show ip nat translations
R1# clear ip nat translations
R1# clear ip nat statistics
```

#### Static NAT
```kotlin 
R1(config)# access-list 1 permit 192.168.1.0 0.0.0.255
R1(config)# ip nat inside source static 192.168.1.2 209.165.200.229
// - Tento příkaz nastavuje statický překlad adresy pro konkrétní interní IP adresu.
// - Adresa `192.168.1.2` je interní adresa, která bude překládána.
// - Adresa `209.165.200.229` je veřejná adresa, na kterou bude interní adresa překládána (adresa musí být z rozsahu sítě, kam bude směrovat komunikace)

R1(config)# interface g0/0/1
R1(config-if)# ip nat inside
R1(config)# interface g0/0/0
R1(config-if)# ip nat outside
// - Tento příkaz nastavuje statický překlad adresy pro konkrétní interní IP adresu.
// - Adresa `adresa1` je interní adresa, která bude překládána.
// - Adresa `adresa2` je veřejná adresa, na kterou bude interní adresa překládána.
```

### TFTP


### NTP
