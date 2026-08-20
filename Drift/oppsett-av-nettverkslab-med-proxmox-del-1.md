# Prosjektoppgave: Etablering av lokalt lab-nettverk med Proxmox VE, UniFi og WireGuard VPN

**Fag:** Driftsstøtte (Vg2 Informasjonsteknologi)

**Organisering:** Pararbeid (2 elever per lab-stasjon)

**Tidsramme:** Flerukers prosjekt oppdelt i **5 modulære økter**

**Sluttleveranse:** Samlet digital lab-rapport (PDF) med dokumentasjon, nettverkskart og besvarte refleksjonsoppgaver

---

## Læreplanforankring og kompetansemål

Gjennom dette prosjektet dekker elevene central kompetansemål i faget **Driftsstøtte**:

* Utforske og beskrive komponenter i en driftsarkitektur.
* Planlegge, implementere og drifte fysiske og virtuelle løsninger med segmenterte nettverk.
* Utforske og beskrive relevante nettverksprotokoller, nettverkstjenester og serverroller.
* Planlegge, drifte og implementere IT-løsninger som ivaretar informasjonssikkerhet.
* Planlegge og dokumentere arbeidsprosesser og IT-løsninger.

---

## Utstyrsliste og IP-plan (Per gruppe)

| Utstyr / Komponent | Antall | Beskrivelse |
| --- | --- | --- |
| **Stasjonær PC (Lab-vert)** | 1 stk. | Kraftig PC der Proxmox VE installeres direkte ("bare-metal"). |
| **USB-minnepenn** | 1 stk. | Oppstartbar USB med Proxmox VE ISO. |
| **UniFi Gateway UXG-Lite** | 1 stk. | Fysisk ruter, brannmur og VPN-gateway. |
| **Fysisk Svitsj** | 1 stk. | Lokal svitsj på pulten. |
| **Elev-laptoper** | 2 stk. | Elev A og Elev B sine maskiner. |
| **Patchkabler (RJ-45)** | 5 stk. | Nettverkskabler. |

> **IP-subnetskjema:** Hver gruppe tildeles et unikt IP-område. Erstatt **`X`** med ditt gruppenummer (f.eks. Gruppe 1 = `192.168.10.0/24`, Gruppe 2 = `192.168.20.0/24`).

---

## Topologioversikt

```text
                      [ Skolenett / Internett ]
                                  │
                                  ▼ (WAN)
                      ┌──────────────────────┐
                      │ UniFi Gateway (UXG)  │ (LAN Gateway: 192.168.X.1)
                      └───────────┬──────────┘ (VPN Pool: 10.8.0.0/24)
                                  │ (LAN)
                                  ▼
                      ┌──────────────────────┐
                      │   Fysisk Svitsj      │
                      └─┬─────────┬────────┬─┘
                        │         │        │
          ┌─────────────┘         │        └─────────────┐
          ▼                       ▼                      ▼
       Elev A Laptop        Elev B Laptop        Proxmox VE Vert
       (DHCP fra UXG)       (DHCP fra UXG)       (Statisk: 192.168.X.2)
                                                         │
                                                         ├─► LXC 100: UniFi Controller (192.168.X.3)
                                                         ├─► VM 101: Win11-Klient-A (DHCP)
                                                         └─► VM 102: Win11-Klient-B (DHCP)

```

---

## ØKT 1: Fysisk oppkobling, IP-planlegging og installasjon av Proxmox VE

### Praktiske oppgaver

1. **Fysisk kabling:**
* Kobl RJ-45 kabel fra skolens nettverksuttak til **WAN-porten** på UXG-Lite.
* Kobl **LAN-porten** på UXG-Lite til Port 1 på den fysiske svitsjen.
* Kobl nettverkskortet på den stasjonære PC-en til Port 2 på svitsjen.
* Kobl Elev A og Elev B sine laptoper til henholdsvis Port 3 og Port 4 på svitsjen.


2. **Bare-metal installasjon av Proxmox VE:**
* Sett inn Proxmox USB-pennen i den stasjonære PC-en og start opp via Boot-menyen (F12/F11).
* Velg *Install Proxmox VE (Graphical)*.
* Sett Target Disk, Tidssone (*Norway/Oslo*) og velg et sikkert root-passord.
* **Nettverkskonfigurasjon:**
* Hostname: `pve-gruppeX.lab`
* IP Address: `192.168.X.2/24`
* Gateway / DNS: `192.168.X.1`


* Fullfør installasjonen, ta ut USB-pennen og start PC-en på nytt.


3. **Verifisering:** Åpne nettleseren på en laptop og logg inn på Proxmox UI: `[https://192.168.](https://192.168.)X.2:8006`.

---

### 📷 Hva som skal dokumenteres i Økt 1

1. **Fysisk kabling:** Et skarpt bilde av oppsettet med fargekoding eller merking av kablene.
2. **IP-adressetabell:** En strukturert tabell som viser planlagte IP-adresser for WAN, LAN Gateway, Proxmox Vert, UniFi Controller og VM-er.
3. **Installationskvittering:** Skjermbilde fra laptopen som viser pålogget Proxmox VE webgrensesnitt (med synlig URL `[https://192.168.](https://192.168.)X.2:8006` og node-status).

---

### 🤔 Refleksjonsoppgaver — Økt 1

* **1.1 (Statisk IP vs. DHCP):** Hvorfor *må* hypervisoren (Proxmox-verten) konfigureres med en statisk IP-adresse i stedet for å motta dynamisk IP fra DHCP? Hvilke konsekvenser ville det hatt for driften dersom verten byttet IP-adresse etter en omstart?
* **1.2 (Virtualiseringsbro):** Hva er funksjonen til nettverksbroen `vmbr0` i Proxmox, og hvordan fungerer denne sammen med det fysiske nettverkskortet (NIC) i PC-en?

---

## ØKT 2: Installasjon av UniFi Controller (LXC) og adoption av UXG-Lite

### Praktiske oppgaver

1. **Opprette LXC-container for UniFi Controller:**
* Åpne **Shell** på Proxmox-noden i web-grensesnittet.
* Kjør hjelpeskriptet for UniFi Network Application:
```bash
bash -c "$(wget -qLO - https://github.com/community-scripts/ProxmoxVE/raw/main/ct/unifi.sh)"

```


* Velg **Advanced Settings** og oppgi parametere:
* CT ID: `100` | Hostname: `unifi-controller`
* RAM: `2048 MB` | Disk: `8 GB`
* IP: Statisk IP `192.168.X.3/24` | Gateway: `192.168.X.1`




2. **Førstegangsoppsett og Adoption:**
* Åpne nettleseren mot kontrolleren: `[https://192.168.](https://192.168.)X.3:8443`.
* Opprett lokal admin-bruker (`admin_lab`) og gi nettverket navnet `Lab-GruppeX`.
* Gå til **Devices**, finn UXG-Lite (står som *Pending Adoption*) og klikk **Adopt Device**.
* Kontroller under **Settings -> Networks** at LAN-subnettet matcher gruppens IP-plan.



---

### 📷 Hva som skal dokumenteres i Økt 2

1. **LXC Container-status:** Skjermbilde fra Proxmox som viser LXC ID 100 `unifi-controller` i aktiv drift (inkludert CPU/RAM-graf).
2. **Device Adoption:** Skjermbilde fra UniFi Network-oversikten der UXG-Lite har status **Online / Getting Ready**.
3. **Nettverkskonfigurasjon:** Skjermbilde fra UniFi Controller som viser oppsettet for LAN-nettverket (DHCP IP Range og Subnet Mask).

---

### 🤔 Refleksjonsoppgaver — Økt 2

* **2.1 (LXC vs. Fysisk/Virtuell Maskin):** Hva er de viktigste forskjellene på en LXC-container og en full virtuell maskin (VM) i Proxmox når det gjelder ressurshåndtering (RAM/CPU) og isolasjon? Hvorfor egner UniFi Controller seg godt som LXC?
* **2.2 (Prosess for Adoption):** Hva skjer teknisk under "adoption"-prosessen mellom UXG-Lite og UniFi Controller? Hvordan vet gatewayen hvilken kontroller den skal rapportere til?

---

## ØKT 3: Virtuelle Windows-klienter og nettverksverifisering

### Praktiske oppgaver

1. **Opprette Virtuelle Maskiner (VM 101 og VM 102):**
* Klikk **Create VM** i Proxmox UI.
* **VM 101 (`Win11-Klient-A`):** CPU: 2 Cores | RAM: 4096 MB | Disk: 50 GB | Network Bridge: `vmbr0` | TPM 2.0 & EFI aktivert.
* **VM 102 (`Win11-Klient-B`):** Samme spesifikasjoner, dedikert til Elev B.
* Start installasjonen og opprett lokale brukerkontoer (`ElevA` og `ElevB`).


2. **Nettverkskonfigurasjon og feilsøking i Windows:**
* Åpne Kommandotolke (`cmd`) på begge klientene og kjør `ipconfig /all`. Verifiser at de mottar IP-adresser fra UXG-Lite sin DHCP-tjeneste.
* Test nettverksforbindelse mellom Klient A og Klient B med `ping [IP-adresse]`.
* Dersom ping feiler: Konfigurer Windows Firewall til å tillate ICMPv4-Inbound echo request.



---

### 📷 Hva som skal dokumenteres i Økt 3

1. **Proxmox VM Oversikt:** Skjermbilde av Proxmox ressursoversikt som viser at både VM 101 og VM 102 kjører samtidig.
2. **IP-konfigurasjon:** Skjermbilde av `ipconfig /all` fra begge Windows-klientene side om side.
3. **Vellykket Ping-test:** Skjermbilde fra `cmd` som viser 0% pakketap ved ping mellom `Win11-Klient-A` og `Win11-Klient-B`.

---

### 🤔 Refleksjonsoppgaver — Økt 3

* **3.1 (Brannmur og sikkerhet):** Hvorfor blokkerer Windows Firewall ICMP (ping) som standard på "Public/Private" nettverksprofiler? Hvilke sikkerhetsrisikoer er knyttet til at enheter svarer på ping i et bedriftsnettverk?
* **3.2 (Ressursallokering / Overcommit):** Dersom den stasjonære PC-en har 16 GB RAM totalt, og dere tildeler 4 GB til Elev A sin VM, 4 GB til Elev B sin VM, 2 GB til UniFi LXC og Proxmox bruker 2 GB selv — hva skjer dersom dere i tillegg starter opp to nye tunge server-VM-er? Forklar konseptet RAM Overcommit og Swapping.

---

## ØKT 4: Fjernstyring (RDP) og WireGuard VPN for Hjemmekontor

### Praktiske oppgaver

1. **Konfigurere Remote Desktop (RDP):**
* Inne på hver Windows-klient: Gå til *Innstillinger -> System -> Remote Desktop* og aktiver funksjonen.
* Gjør en lokal test: Åpne *Tilkobling til fjernskrivebord (mstsc)* på Elev A sin laptop og koble til IP-adressen til `Win11-Klient-A`.


2. **Oppsett av WireGuard VPN Server i UniFi:**
* Gå til UniFi Controller: **Settings -> VPN -> VPN Server -> Create New**.
* Velg VPN Protocol: **WireGuard**.
* Client IP Subnet: `10.8.0.0/24` | Server Port: `51820`.
* Under **Client Management**: Opprett to klientprofiler (`ElevA-Laptop` og `ElevB-Laptop`).
* Last ned `.conf`-profilfilene til laptopene.


3. **Klientkonfigurasjon og Ekstern Test:**
* Installer WireGuard-klientprogramvaren på laptopene ([wireguard.com](https://www.wireguard.com)).
* Importer `.conf`-filen og aktiver tunnelen.
* **Ekstern test (Simulert hjemmekontor):** Koble laptopen til et eksternt nettverk (f.eks. mobilt internett / hotspot). Aktiver WireGuard VPN og sjekk at du når både Proxmox UI (`192.168.X.2:8006`) og kan starte en RDP-sesjon mot din virtuelle Windows-klient.



---

### 📷 Hva som skal dokumenteres i Økt 4

1. **RDP I Drift:** Skjermbilde fra Elev A sin laptop som viser at han/hun har en aktiv RDP-sesjon vindu-i-vindu mot sin virtuell Windows 11-maskin.
2. **UniFi VPN Server Dashboard:** Skjermbilde fra UniFi Controller som viser at WireGuard VPN Server er aktiv og at klientene er opprettet.
3. **Ekstern VPN-tilkobling:** Skjermbilde av WireGuard-klienten på laptopen i tilkoblet tilstand (med synlig Sendt/Mottatt data og VPN IP `10.8.0.x`) *samtidig* som RDP-vinduet mot Windows-klienten er åpent over eksternt nett.

---

### 🤔 Refleksjonsoppgaver — Økt 4

* **4.1 (VPN vs. Port Forwarding):** Hvorfor er det en uakseptabel sikkerhetsrisiko å åpne RDP-porten (TCP 3389) direkte mot Internett i en bedrift? Hvordan beskytter WireGuard VPN-tunnelen bedriftens interne ressurser mot angripere?
* **4.2 (UDP vs. TCP for VPN):** WireGuard benytter seg av protokollen UDP (port 51820) i stedet for TCP. Hva er forskjellene på TCP og UDP, og hvorfor egner UDP seg spesielt godt for tunneler og VPN-trafikk?

---

## ØKT 5: Sluttdokumentasjon, topologikart og rapportsammenstilling

### Praktiske oppgaver

1. **Utarbeidelse av helhetlig nettverkskart (Topologi):**
* Bruk et profesjonelt verktøy som Draw.io, Cisco Packet Tracer eller Visio.
* Tegn opp hele infrastrukturen: Skolenett, WAN/LAN-grensesnitt på UXG-Lite, fysisk svitsj, Proxmox-vert, LXC-containere, VM-er og VPN-tunnel.
* Inkluder alle IP-adresser, portnummer, VLAN/subnett og brukernavn/roller.


2. **Kvalitetssikring og samling av rapporten:**
* Sammenstill alle dokumentasjonskrav og refleksjonssvar fra Økt 1–4 i ett felles PDF-dokument.
* Legg ved en egen **Feilsøkingslogg** (tabell) som beskriver utfordringer gruppen støyter på underveis og hvordan dere løste dem.



---

### 📷 Hva som skal leveres i Sluttrapporten (Økt 5)

1. **Komplett Nettverkstoppologi:** Vektor/høyoppløselig bilde av det digitale nettverkskartet.
2. **Feilsøkingslogg:** Tabell over oppståtte feil (f.eks. sertifikatadvarsler, brannmurblokkering, manglende IP) med årsak og valgt tiltak.
3. **Alle samlede dokumentasjonsbilder og besvarte refleksjonsoppgaver fra Økt 1 til 4.**

---

### 🤔 Sluttrefleksjoner og Bærekraftsanalyse — Økt 5

* **5.1 (Bærekraft og Grønn IT):** Sammenlign dette virtualiserte oppsettet (1 kraftig fysisk PC som kjører Proxmox + 3 virtuell enheter) med å kjøpe 3 separate fysiske servere. Vurder forskjellene i energiforbruk, e-avfall (hardware livssyklus) og romoppvarming/kjøling i et datasenter.
* **5.2 (Sikkerhet og Risikoanalyse):** Tenk deg at denne lab-infrastrukturen var produksjonsnettverket til en liten bedrift med 20 ansatte. Identifiser minst to "Single Point of Failure" (SPOF) i designet og foreslå tiltak for å øke tilgjengeligheten (f.eks. redundans, backup-strategi, UPS).