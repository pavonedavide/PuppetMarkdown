# Windows examen

---

# Table of contents
<!-- MarkdownTOC -->

  - [Wat is het verschil tussen role based en feature based installation bij WDS?](#user-content-wat-is-het-verschil-tussen-role-based-en-feature-based-installation-bij-wds)
    - [Het verschil tussen Role en Feature installation :](#user-content-het-verschil-tussen-role-en-feature-installation-)
  - [iSCSI shared storage:](#user-content-iscsi-shared-storage)
    - [Wat is een iSCSI storage? Waarvoor dient het iscsi protocol?](#user-content-wat-is-een-iscsi-storage-waarvoor-dient-het-iscsi-protocol)
      - [iSCSI storage:](#user-content-iscsi-storage)
      - [Waarvoor dient het:](#user-content-waarvoor-dient-het)
  - [Wat is het verschil tussen een iscsi target en een iscsi initiator? Wat is een iscsi connector?](#user-content-wat-is-het-verschil-tussen-een-iscsi-target-en-een-iscsi-initiator-wat-is-een-iscsi-connector)
  - [Kan je onder Windows met meerdere toestellen TEGELIJK aan dezelfde iscsi target? Waarom kan dit niet?](#user-content-kan-je-onder-windows-met-meerdere-toestellen-tegelijk-aan-dezelfde-iscsi-target-waarom-kan-dit-niet)
  - [Wat is het voordeel van shared storage ten opzichte van lokale storage?](#user-content-wat-is-het-voordeel-van-shared-storage-ten-opzichte-van-lokale-storage)
  - [Wat is een LUN? Waarvoor dient die LUN id? Hoe kan je ervoor zorgen dat je server niet alle luns ‘ziet’ in de storage management template?](#user-content-wat-is-een-lun-waarvoor-dient-die-lun-id-hoe-kan-je-ervoor-zorgen-dat-je-server-niet-alle-luns-%E2%80%98ziet%E2%80%99-in-de-storage-management-template)
- [LUN OPTIONEEL MAAR ZEER GOED LEZEN EN BEGRIJPEN](#user-content-lun-optioneel-maar-zeer-goed-lezen-en-begrijpen)
  - [Blade:](#user-content-blade)
    - [Wat is het voordeel van blades ten opzichte van aparte fysieke servers? Wat is het voordeel om elke component in een blade chassis redundant te maken \(denk aan koeling, voeding\)? Hoe gebeurt de stroomverdeling bij zo een blade chassis? Is elke voedingskabel gekoppeld aan maar één fysieke blade?](#user-content-wat-is-het-voordeel-van-blades-ten-opzichte-van-aparte-fysieke-servers-wat-is-het-voordeel-om-elke-component-in-een-blade-chassis-redundant-te-maken-denk-aan-koeling-voeding-hoe-gebeurt-de-stroomverdeling-bij-zo-een-blade-chassis-is-elke-voedingskabel-gekoppeld-aan-maar-%C3%A9%C3%A9n-fysieke-blade)
    - [Voordeel redundancy componenten](#user-content-voordeel-redundancy-componenten)
    - [STROOM](#user-content-stroom)
  - [Vswitches:](#user-content-vswitches)
    - [Wat is een vswitch? Hoe koppel je een iscsi target via vswitch aan je initiator? Heb je daar een externe of interne vswitch voor nodig?](#user-content-wat-is-een-vswitch-hoe-koppel-je-een-iscsi-target-via-vswitch-aan-je-initiator-heb-je-daar-een-externe-of-interne-vswitch-voor-nodig)
  - [Backup](#user-content-backup)
      - [Grandfather](#user-content-grandfather)
      - [Father](#user-content-father)
      - [Child](#user-content-child)
    - [Rotatie schema](#user-content-rotatie-schema)
  - [Monitoring](#user-content-monitoring)
- [Extra vragen](#user-content-extra-vragen)
  - [Extra info ISCSI](#user-content-extra-info-iscsi)
      - [iscsi initiator](#user-content-iscsi-initiator)
      - [iscsi target](#user-content-iscsi-target)
      - [iscsi controller](#user-content-iscsi-controller)
  - [ESXI](#user-content-esxi)

<!-- /MarkdownTOC -->

---

<a name="user-content-wat-is-het-verschil-tussen-role-based-en-feature-based-installation-bij-wds"></a>
## Wat is het verschil tussen role based en feature based installation bij WDS?
<a name="user-content-het-verschil-tussen-role-en-feature-installation-"></a>
### Het verschil tussen Role en Feature installation : 
een Role is een service die de server aan anderen “geeft” dus bijvoorbeeld DHCP waarbij de server IP-addressen verdeelt onder de clients. Een feature wordt door de server zelf gebruikt dus bijvoorbeeld Network Load Balancing of Failover Clustering.
Bij WDS installatie worden deze te samen geïnstalleerd omdat een role opzichzelf werkt met minimale controle van de admin en hierbij door de feature ondersteund wordt. Een ander voorbeeld van deze samenwerking is bijvoorbeeld DFS clustering hierbij is de DFS de role en de clustering feature is de feature.
http://philipflint.com/2009/11/03/what-is-the-difference-between-a-role-and-a-feature/

<a name="user-content-iscsi-shared-storage"></a>
## iSCSI shared storage: 
<a name="user-content-wat-is-een-iscsi-storage-waarvoor-dient-het-iscsi-protocol"></a>
### Wat is een iSCSI storage? Waarvoor dient het iscsi protocol?
<a name="user-content-iscsi-storage"></a>
#### iSCSI storage:
- iSCSI kan enkel op SAN gebruikt worden (niet op NAS). Een Storage Area Network is een toegewijde IT-infrastructuur, bedoeld voor transport van het SCSI protocol tussen servers/computers en een geconsolideerde opslagvoorziening. Het geheel van middelen dat gezamenlijk de geconsolideerde opslagvoorziening vormt, wordt vaak SAN genoemd, maar dit is feitelijk onjuist. Een SAN is (net zoals een LAN) een infrastructuur t.b.v. datatransport. in het Nederlands bestaat geen korte term voor geconsolideerde dataopslag. Een SAN is een cluster van opslag. Indien een deel ervan uitvalt blijft de SAN nog steeds bereikbaar. Indien meer ruimte nodig is dan beschikbaar is kan er eenvoudig een uitbreiding gebeuren door blokken met capaciteit bij te steken of bestaande te vervangen door grotere. SAN is ook mogelijk via FCoE (fibre) maar is veel duurder omwille dat Fibre aparte netwerkkaarten op de servers nodig heeft, dure Fibreswitchen en de SAN zelf is ook duurder met Fibreconnecties. De snelheid van Fibre is wel evenredig met het kostenplaatje en ligt dus veel hoger dan bij iSCSI. iSCSI Storage werkt over gewone Ethernet kabels, switchen en netwerkkaarten en is daardoor veel bereikbaarder qua prijs.

<a name="user-content-waarvoor-dient-het"></a>
#### Waarvoor dient het:
- Iscsi (Internet Small Computer Systems Interface) laat toe om scsi commando’s te verzenden over een IP-gebasseerd netwerk. Hierdoor kunnen servers en computers connecteren met een of meer hard drives. Het zal lijken dat deze HDD rechtstreeks geconnecteerd is met de server of computer, maar in werkelijkheid is deze niet geconnecteerd, maar is de server via een netwerkkabel geconnecteerd met de HDD. Het netwerk waarover de SCSI-commando’s uitgevoerd worden kan een LAN, een WAN of het internet zijn.

<a name="user-content-wat-is-het-verschil-tussen-een-iscsi-target-en-een-iscsi-initiator-wat-is-een-iscsi-connector"></a>
## Wat is het verschil tussen een iscsi target en een iscsi initiator? Wat is een iscsi connector?
De server die verbonden is met de ISCSI storage wordt de ISCSI Initiator genoemd in een
ISCSI storage netwerk. Deze kan connecteren met de storage, de ISCSI target. Als de Initiator
connecteert met het Target, stuurt de initiator SCSI-commando’s naar het Target. Deze
commando’s worden in een IP pakket gestoken en verstuurd. Het Target bevat één of meer
zogenaamde Logical Units. Deze worden onderscheiden van elkaar door een LUN (Logical
Unit Number).

<a name="user-content-kan-je-onder-windows-met-meerdere-toestellen-tegelijk-aan-dezelfde-iscsi-target-waarom-kan-dit-niet"></a>
## Kan je onder Windows met meerdere toestellen TEGELIJK aan dezelfde iscsi target? Waarom kan dit niet?
Dit is een limitatie van ISCSI. Zonder een cluster tussen de 2 toestellen, zal de data corrupt
worden omdat deze constant overschreven wordt. Wanneer de corruptie aan het licht komt,
is niet zeker.

<a name="user-content-wat-is-het-voordeel-van-shared-storage-ten-opzichte-van-lokale-storage"></a>
## Wat is het voordeel van shared storage ten opzichte van lokale storage?
- Men moet de storage niet meer direct koppelen aan de servers. Hierdoor zal het
onderbenutten van storage wegvallen. Bijvoorbeeld het IT departement heeft een
standaard set van server configuraties. Deze servers worden gebruikt voor apllicaties.
Sommige applicaties hebben minder storage nodig dan andere. In de standaard
configuratie hebben alle servers dezelfde hoeveelheid opslagruimte. Alle
opslagruimte die in een bepaalde server niet gebruikt wordt, gaat dan verloren.
- Het managen van shared storage is makkelijker omdat de opslagruimte word gezien
als een enkele logische resource die kan gebruikt worden door meerdere servers. De
servers krijgen opslagruimte toegewezen naargelang de ruimte die ze nodig hebben.
Als deze opslagruimte die ze nodig hebben groter of kleiner wordt, zal meer of
minder opslagruimte toegewezen worden.
- Shared storage maakt geavanceerde virtual machine management features mogelijk
in virtualisatie platformen. Een voorbeeld hiervan is automatic machine migration.
Hierdoor kan een VM verplaatst worden van een server naar een andere zonder de
lopende operaties te onderbreken. Dit kan nodig zijn wanneer er een hardware
failure is in een server. Ook kan deze migration plaatsvinden wanneer het
virtualisatie platform een VM verplaatst om een betere loadbalance te creëren over
alle servers. Met shared storage heeft de VM toegang tot de storage die hij gebruikt.
Hierbij maakt het niet uit op welke server de VM draait.

<a name="user-content-wat-is-een-lun-waarvoor-dient-die-lun-id-hoe-kan-je-ervoor-zorgen-dat-je-server-niet-alle-luns-%E2%80%98ziet%E2%80%99-in-de-storage-management-template"></a>
## Wat is een LUN? Waarvoor dient die LUN id? Hoe kan je ervoor zorgen dat je server niet alle luns ‘ziet’ in de storage management template?
Een LUN is een Logical Unit Number, deze dient om te verwijzen naar een individuele of
collectie van fysieke of virtuele storage device(s) die input/output commands uitvoeren met
een host computer zoals gedefinieerd door de Small System Computer Interface standard
(SCSI).
LUN setup varieert per systeem. Een LUN wordt toegekend wanneer een host een SCSI
device scant en een logische unit vindt. De LUN geeft die specifieke logische unit weer aan
de SCSI Initiator in combinatie met andere informatie zoals target port identifier. De logical
unit waar de LUN naar verwijst kan een deel van een storage drive zijn, een gehele storage
drive of allemaal delen van verschillende storage drives zoals hard disks, solid-state drives,…
in 1 of meerdere systemen. Een LUN kan refereren naar een volledige RAID set, een enkele
schijf of een partitie. In ieder geval wordt de Logical Unit behandeled als een enkel apparaat
en geidentificeerd met 1 LUN.
Een LUN gebruiken vereenvoudigd het onderhoud van storage resources omdat access en
control privileges toegekend kunnen worden aan een LUN.
Ervoor zorgen dat je server niet alle LUN’s ziet :
Gebeurt met LUN Masking : LUN masking gebeurt in de storage controller maar kan ook
versterkt worden aan de Host Bus Adapter of de switch layer. Met LUN masking kunnen
verschillende hosts en zones dezelfde poort gebruiken op een storage device, maar enkel de
specifieke SCSI targets en LUNS zien die aan hun toegewezen zijn.
(ook heb je LUN Zoning, dit voorziet geisoleerde paden waar I/O flow tussen end ports. Een
host is hierbij beperkt tot de zone waar hij aan toegekend is. Helpt het verbeteren van de
beveiliging en het elimineren van hot spots in het netwerk.)

<a name="user-content-lun-optioneel-maar-zeer-goed-lezen-en-begrijpen"></a>
# LUN OPTIONEEL MAAR ZEER GOED LEZEN EN BEGRIJPEN

```markdown
# LUNS and virtualization
A LUN constitutes a form of virtualization in the sense that it abstracts the hardware devices
behind it with a standard SCSI method of identification and communication. The storage
object represented by the LUN can be provisioned, compressed and/ordeduplicated as long
as the representation to the host does not change. A LUN can be migrated within and
between storage devices, as well as copied, replicated, snapshottedand tiered.
A virtual LUN can be created to map to multiple physical LUNs or a virtualized capacity
created in excess of the actual physical space available. Virtual LUNs created in excess of the
available physical capacity help to optimize storage use, because the physical storage is not
allocated until the data is written. Such a virtual LUN is sometimes referred to as a thin LUN.
A virtual LUN can be set up at the server operating system (OS), hypervisor or storage
controller. Because the virtual machine (VM) does not see the physical LUN on the storage
system, there is no need for LUN zoning.
Software applications can present LUNs to VMs running on guest OSes. Proprietary
technology such as VMware’s Virtual Volumes can provide the virtualization layer and the
storage devices to support them with fine-grain control of storage resources and services.

## Types of LUNs
The underlying storage structure and logical unit type may play a role in performance and
reliability. Examples include:
• **Mirrored LUN:** Fault-tolerant LUN with identical copies on two physical drives for
dataredundancy.
• **Concatenated LUN:** Consolidates several LUNs into a single logical unit or volume.
• **Striped LUN:** Writes data across multiple physical drives, potentially enhancing
performance by distributing I/O requests across the drives.
• **Striped LUN with parity:** Spreads data and parity information across three or more
physical drives. If a physical drive fails, the data can be reconstructed from the data
and parity information on the remaining drives. The parity calculation may have an
impact on write performance.
```


<a name="user-content-blade"></a>
## Blade:
<a name="user-content-wat-is-het-voordeel-van-blades-ten-opzichte-van-aparte-fysieke-servers-wat-is-het-voordeel-om-elke-component-in-een-blade-chassis-redundant-te-maken-denk-aan-koeling-voeding-hoe-gebeurt-de-stroomverdeling-bij-zo-een-blade-chassis-is-elke-voedingskabel-gekoppeld-aan-maar-%C3%A9%C3%A9n-fysieke-blade"></a>
### Wat is het voordeel van blades ten opzichte van aparte fysieke servers? Wat is het voordeel om elke component in een blade chassis redundant te maken (denk aan koeling, voeding)? Hoe gebeurt de stroomverdeling bij zo een blade chassis? Is elke voedingskabel gekoppeld aan maar één fysieke blade?

- **Densiteit:** Doordat blades weinig plaats innemen, is er meer processing power op een
kleiner oppervlak. Doordat deze een kleiner oppervlak in beslag nemen is de
bekabeling, de opslag en het onderhoud makkelijker. Voeding en koeling worden dan
ook achteraan in het chassis geïnstalleerd zodat optimaal gebruik gemaakt kan
worden van de plaats in de rack.
- **Load balancing en failover:** omdat blades een smaller en makkelijkere infrastructuur
hebben, zoals bekabeling, is het managen van loadbalancing en failover makkelijker.
Als er een hardware probleem ontstaat, start er een self-diagnostic proces waardoor
de blade, waarbij het probleem zit, makkelijk opgespoord kan worden en kan
vervangen of gerapreerd worden.
- **Power consumption en management:** het samenvoegen van de voedingen in het
chassis vermindert het aantal aparte voedingen en vermindert dan ook aanzienlijk
het stroomverbruik. De blades zijn gestripped en hebben enkel nog de componenten
die ze nodig hebben om te functioneren. Extra componenten zijn makkelijk toe te
voegen. Doordat de blades gestripped zijn, moeten ze enkel de benodigde
componenten van stroom voorzien.
- **Lagere management kost:** het chassis beschikt over een single interface dat gebruikt
wordt om alle blades individueel te managen. Hardware management en fault
monitoring worden allemaal centraal beheerd.
- **Flexibiliteit, modulariteit:** Blades kunnen uit het chassis gehaald worden en
toegevoegd worden zonder problemen. Ook de koelingmodules en de powermodules
kunnen verwijderd en terug ingestoken worden. Deze modules zijn hot-plugable.
(Een blade server is een ultra-compacte server ontworpen om geinstalleerd te worden in
een speciaal chassis dat zorgt voor ondersteuning aan blades via een backplane verbinding.
Blade servers hebben zelf geen stroomvoorziening of verkoeling, deze worden door het
chassis voorzien.
Voordelen : Ten opzichte van aparte fysieke servers is een bladesysteem goedkoper (initïele
aankoopkosten van servers, switches, stroomverdeling, gedeelde storage devices) en lagere
kosten om het systeem draaiende te houden (wattage, cabling, space) en in
onderhoudskosten om configuraties te voorzien , up te graden en aan te passen.
Verbeterd data center efficïentie terwijl de complexiteit en het risico onder controle
gehouden kan worden.

<a name="user-content-voordeel-redundancy-componenten"></a>
### Voordeel redundancy componenten
Door elk component in een chasis reduntant te maken zoals stroom invoer, voeding en
kooling zorg je ervoor dat er geen single point of failure is wat de downtime van een chasis
aanzienlijk verkleint. Indien er een onderdeel van een chasis niet naar behoren functioneert
blijft het toch nog werken op de reduntante infrastructuur, zo kunnen de
verantwoordelijken het chasis herstellen zonder dat de gebruikers hier veel hinder van
ondervinden.

<a name="user-content-stroom"></a>
### STROOM
Het chassis van de blades wordt van stroom voorzien via een PDU (Power Distribution Unit).
Het chassis waar we voor het project over beschikten had tot 6 connectoren voor de
voedingen aanwezig in het chassis. Zo wordt niet elke Blade apart voorzien van stroom maar
het hele chassis. De verdeling van deze stroom in het chassis gebeurt via power capping.
Met Power Capping en de Onboard administrator wordt het stroomverbruik van elke
individuele blade gemonitord. Als een blade zijn voorziene stroom bereikt en de prestaties
hierdoor beïnvloed worden, kan hij aan de OA (Onboard administrator) meer stroom vragen.
De OA zal dan nakijken of er in het in chassis blades zijn die minder stroom verbruiken dan
voorzien, indien er een blade gevonden wordt met een stroom overschot zal de OA deze
minder stroom geven en degene die stroom nodig heeft meer geven. Door het gebruik van
power capping kan de capaciteit van bijvoorbeeld een datacenter verdrievoudigd worden.
Omdat Power capping en de OA het stroomverbruik continu monitoren en bijsturen wordt
het opblazen van circuits uitgesloten.

<a name="user-content-vswitches"></a>
## Vswitches:
<a name="user-content-wat-is-een-vswitch-hoe-koppel-je-een-iscsi-target-via-vswitch-aan-je-initiator-heb-je-daar-een-externe-of-interne-vswitch-voor-nodig"></a>
### Wat is een vswitch? Hoe koppel je een iscsi target via vswitch aan je initiator? Heb je daar een externe of interne vswitch voor nodig?
<a name="user-content-wat"></a>
**WAT:**

Een virtuele switch is een softwarematige toepassing die communicatie tussen virtuele
machines toelaat.Deze is dus bedoeld om een mechanisme te voorzien dat de complexiteit
van de netwerk configuratie verminderd. Dit wordt gedaan door het aantal netwerkswitches
te verminderen (wat zorgt voor een verbetering van de bandbreedte) na het rekening
houden met de grootte van het netwerk, de data packets en de architectuur. Een vswitch
doet meer dan enkel data packets forwarden, hij leidt de communicatie op een intelligente
manier door data packets te controleren alvorens ze door te sturen naar de bestemming.
Doordat een virtuele switch intelligent is kan het ook de integriteit van de netwerk en
security settings van een virtueel machine verzekeren. Virtuele switches worden meestal
geinstalleerd in de software van de server maar kunnen ook als firmware in de hardeware
van de server geinstalleerd worden. Een vrituele switch is volledig virtueel en kan
connecteren met een NIC. De vswitch voegt meerdere fysieke switchen samen tot een
enkele logische switch.
Voordelen hiervan zijn : Helpt met het gemakkelijke opstellen en migreren van virtuele
servers, laat de netwerk administrator toe om de virtuele switch te beheren met een
hypervisor (Hyper-V bijvoorbeeld). In vergelijking met een fysieke switch is het veel
gemakkelijker om nieuwe functionaliteiten te implementeren die hardware of firmware
related kunnen zijn.

**HOE KOPPELEN:**

Om een iscsi target te koppelen aan je server moet de server beschikken over een externe
switch, zodat deze kan communiceren met ons target. Vervolgens start je de iscsi initiator op
en voeg je een nieuw target toe. Dit kan door het IP adres van het target in te vullen, en
vervolgens quick connect te klikken. Eenmaal dit proces doorlopen is zullen de schijven
zichtbaar worden op onze server. Om de juiste schijf te koppelen starten we computer
management op dit kan via administratieve tools. Kies voor device management en dan
schijven. Normaal zie je hier nu verschillende iscsi targets, om te beginnen schakel je alle
schijven die je niet gaat gebruiken uit door deze te disablen. Start vervolgens disk
management op en herlaad indien nodig de schijven zodat de gekopelde schijf zichtbaar
wordt. Hierna moet je de schijf nog online brengen en initialiseren dit kan telkens door
rechtermuisknop te klikken op de schijf.

---

<a name="user-content-backup"></a>
## Backup

<a name="user-content-grandfather"></a>
#### Grandfather
1 keer een FULL backup doen

<a name="user-content-father"></a>
#### Father
meerdere keren een full backup doen

<a name="user-content-child"></a>
#### Child
dagelijks backups (nooit een full backup)

<a name="user-content-rotatie-schema"></a>
### Rotatie schema

|M  | D  | W  | D  | V  | Z |
|:----:|:----:|:----:|:----:|:----:|:----:|
|GF | S  | S  | S  | S  | S |
|F  | S  | S  | S  | S  | S |
|F  | S  | S  | S  | S  | S |
|F  | S  | S  | S  | S  | S |

---

<a name="user-content-monitoring"></a>
## Monitoring

**SNMP:**
* Simple Network Management Protocol

**Trap:**
* Gaat steeds de waarden opvragen van SNMP en waarschuwen bij alerts

**Sensors:**
* Waarde gehaald van SNMP ofwel een client toepassing installeren op het toestel => Hiermee kan je BV: services mee monitoren in windows

**VOORBEELDEN van monitoring tools:**
* PRTG
* SCOM operations manager *(van Microsoft zelf)*
* ...

---

<a name="user-content-extra-vragen"></a>
# Extra vragen

**Downtime:**
* Tijd dat een dienst niet meer functioneert

**Redundancy:**
* Dienst of apparaat dat functie overneemt van een niet-functionerend apparaat
* Minimum in 2-voud
* Kostelijk in aankoop, onderhoud
* Geldbesparend bij falende dienst
- Enkele voorbeelden:
    + Backup route - additional ISP
    + STP bij switches
    + UPS
    + Fail-over bij blades
    + Fail-over bij servers
    + Fail-over bij ESX(i)

**Single point of failure:**
* 1 toestel of dienst dat een gans netwerk of service kan beperken
    - Stroompanne
    - Single switch of single router
    - 1 fysieke server met meerdere VM"s
    - 1 database server voor ERP netwerk

**Replication:**
* Gegevens op meerdere plaatsen continu synchroniseren met als doel redundancy te verkrijgen bij failure
    - Active Directory
    - DNS
    - SYSVOL
    - Distributed File Server
        + Namespace
        + Replication Group
            
**Fail-over clustering:**
* redundancy tussen 2 verschillende systemen om downtime te vermijden
- Cluster:
    + verschillende systemen die eenzelfde service hosten of gegevens bevatten
- Fail-over:
    + het automatisch opstarten van een identieke backup service op een ander systeem na een faling van het actieve systeem zonder manuele interventie, waarbij de gebruiker geen of weinig downtime ondervindt
            
**Cluster:**
* een cluster bevat meerdere fysieke of virtuele servers of services waarbij 1 systeem online is
    
**Node:**
* 1 totaal systeem of service dat autonoom kan opstarten
* Een cluster bevat minimum 2 nodes die onafhankelijk functioneren
* Een node is actief, de andere node staat in fail-over mode (klaar om over te nemen bij failing van actieve node)
    (2 nodes worden verbonden met een hartbeat netwerk)
    
**Nut van een cluster:**
* Services die ALTIJD online moeten blijven zonder downtime:
- ziekenhuizen (vb hart monitoring, patientengegevens)
- Datacenters (beschikbaarheid gegevens)
- Vliegtuigradars (luchtverkeer)
- Databanken naar belangrijke gegevens (politie, banken, grote bedrijven, ...)
- Systemen die altijd online moeten zijn (grote online shops, betalingssystemen, ...)

**Vereisten cluster:**
    *(Geen single point of failure!!!!)*
* Denk aan stroomvoorzieningen, storage, netwerkconnecties en netwerk devices, hardware devices, software drivers, scheiding ISP, locatiescheiding, ...
* Meerdere onafhankelijke systemen met privaat heartbeat network en redundante netwerkverbindingen tussen storage, nodes en (eventueel) ISP's

> Clustering is GEEN backup!!! gewiste bestanden worden doorgestuurd naar elke node!! 

**Heartbeat network:**
* privé netwerk tussen 2 nodes dat test of de actieve node nog functioneert en eventueel het fail-over commando doorstuurt
* Intern netwerk, volledig gescheiden van LAN of SAN netwerk (eventueel dms VLAN's en INTERVLAN-routing)
* 2 redundante lijnen om false fail-over te voorkomen

---

<a name="user-content-extra-info-iscsi"></a>
## Extra info ISCSI

<a name="user-content-iscsi-initiator"></a>
#### iscsi initiator
Degene die scsi commando's doorstuurt

<a name="user-content-iscsi-target"></a>
#### iscsi target
Degene die de scsi commando's aan krijgt

<a name="user-content-iscsi-controller"></a>
#### iscsi controller
Een kaart of een chip die toelaat dat de iscsi apparaat kan communiceren met het OS

---

<a name="user-content-esxi"></a>
## ESXI

* [How to use iSCSI targets on ESXI server with multipath support](https://www.synology.com/nl-nl/knowledgebase/DSM/tutorial/Virtualization/How_to_Use_iSCSI_Targets_on_VMware_ESXi_Server_with_Multipath_Support "How to use iSCSI targets on ESXI server with multipath support")
* ...