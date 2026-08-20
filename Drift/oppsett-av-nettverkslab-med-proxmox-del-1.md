Her er den oppdaterte oppgaveteksten og guiden for elevene. I denne versjonen skal elevene **installere Proxmox VE selv fra USB-penn**, samt **sette opp WireGuard VPN på UXG-Lite** slik at de kan koble seg sikkert til sine virtuelle maskiner og nettverket fra sin egen laptop når de er hjemme.

---

# Oppgave: Etablering av lokalt lab-nettverk med Proxmox, UniFi Gateway og VPN for fjernadgang

**Fag:** Driftsstøtte (Vg2 Informasjonsteknologi)

**Organisering:** Pararbeid (2 elever per lab-stasjon)

---

## Formål og læringsmål

I denne oppgaven skal dere installere og konfigurere en komplett IT-infrastruktur helt fra bunnen av. Dere skal installere operativsystemet/hypervisoren Proxmox VE på en fysisk stasjonær PC, koble opp og adoptere en dedikert ruter/brannmur (UniFi UXG-Lite), installere en nettverkskontroller, bygge virtuelle Windows-klienter, og til slutt sette opp en **WireGuard VPN-server** slik at dere trygt kan hente opp lab-miljøet deres fra laptopen når dere sitter hjemme.

**Kompetansemål i fokus:**

* Utforske og beskrive komponenter i en driftsarkitektur.
* Planlegge, implementere og drifte fysiske og virtuelle løsninger med segmenterte nettverk.
* Utforske og beskrive relevante nettverksprotokoller, nettverkstjenester og serverroller.
* Planlegge, drifte og implementere IT-løsninger som ivaretar informasjonssikkerhet.
* Planlegge og dokumentere arbeidsprosesser og IT-løsninger.

---

## Utstyrsliste per gruppe

| Utstyr | Antall | Beskrivelse |
| --- | --- | --- |
| **Stasjonær PC (Lab-vert)** | 1 stk. | Fysisk PC der Proxmox skal installeres |
| **USB-minnepenn** | 1 stk. | Minnepenn med Proxmox VE installasjonsfil |
| **UniFi Gateway UXG-Lite** | 1 stk. | Fysisk ruter og brannmur |
| **Fysisk Svitsj** | 1 stk. | Lokal svitsj på pulten |
| **Elev-laptoper** | 2 stk. | Elev A og Elev B sine maskiner |
| **Patchkabler (RJ-45)** | 5 stk. | Nettverkskabler |

---

## Nettverkstoppologi og IP-plan

Hver gruppe tildeles et unikt IP-område (f.eks. **Gruppe 1 = `192.168.10.0/24**`, **Gruppe 2 = `192.168.20.0/24**`).

```text
               [ Skolenett / Internett ]
                           │
                           ▼ (WAN)
               ┌──────────────────────┐
               │ UniFi Gateway (UXG)  │ (IP: 192.168.X.1)
               └───────────┬──────────┘ (VPN Server: 10.8.0.0/24)
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
                                                  ├─► LXC: UniFi Controller (192.168.X.3)
                                                  ├─► VM 101: Win11-Klient-A (DHCP)
                                                  └─► VM 102: Win11-Klient-B (DHCP)

```

---

## Del 1: Fysisk kabling og oppkobling

1. Kobl en patchkabel fra **skolens nettverksuttak** til **WAN-porten** på UXG-Lite.
2. Kobl en patchkabel fra **LAN-porten** på UXG-Lite til **Port 1** på den fysiske svitsjen.
3. Kobl nettverkskortet på **stasjonær PC** til **Port 2** på svitsjen.
4. Kobl **Elev A sin laptop** til **Port 3** på svitsjen.
5. Kobl **Elev B sin laptop** til **Port 4** på svitsjen.

---

## Del 2: Installasjon av Proxmox VE på stasjonær PC

1. Sett inn USB-minnepennen med Proxmox VE i den stasjonære PC-en og slå den på.
2. Trykk på F12, F11 eller Delete under oppstart for å åpne boot-menyen, og velg USB-minnepennen.
3. Velg **Install Proxmox VE (Graphical)**.
4. Gjennomfør installasjonsveiviseren:
* **Target Harddisk:** Velg PC-ens SSD/harddisk *(Obs: Alt på disken slettes)*.
* **Country / Timezone / Keyboard:** *Norway / Europe/Oslo / Norwegian*.
* **Password & Email:** Sett et sikkert root-passord og oppgi e-postadresse.
* **Management Network Configuration:**
* **Management Interface:** Velg det fysiske nettverkskortet.
* **Hostname:** `pve-gruppeX.lab` (Erstatt X med gruppenummer).
* **IP Address:** `192.168.X.2/24` (f.eks. `192.168.10.2/24` for Gruppe 1).
* **Gateway:** `192.168.X.1`
* **DNS Server:** `192.168.X.1` (eller `1.1.1.1`).




5. Klikk **Install**. Når installasjonen er ferdig, ta ut USB-pennen og start PC-en på nytt.
6. **Verifisering:** Åpne nettleseren på en av laptopene og gå til `[https://192.168.](https://192.168.)X.2:8006`. Logg inn med brukernavn `root` og passordet dere opprettet.

---

## Del 3: Opprettelse av UniFi Controller i Proxmox

1. I Proxmox web-grensesnittet, klikk på Proxmox-noden i venstremenyen og åpne **Shell**.
2. Opprett en LXC-container for UniFi Controller ved å lime inn følgende skript i Shell:
```bash
bash -c "$(wget -qLO - https://github.com/community-scripts/ProxmoxVE/raw/main/ct/unifi.sh)"

```


3. Velg **Advanced Settings** og oppgi følgende verdier:
* **Container ID:** `100`
* **Hostname:** `unifi-controller`
* **RAM:** `2048 MB`
* **Disk:** `8 GB`
* **IP Address:** Statisk IP `192.168.X.3/24`, Gateway `192.168.X.1`.


4. Fullfør skriptet og vent til containeren er ferdig installert og startet.

---

## Del 4: Adoptere UXG-Lite i UniFi Controller

1. Åpne en ny fane på laptopen og gå til: `[https://192.168.](https://192.168.)X.3:8443`.
2. Gjennomfør førstegangsoppsettet i UniFi Network (opprett lokal admin-bruker og gi nettet et navn).
3. Gå til **Devices** i venstremenyen.
4. Klikk på UXG-Lite (som står som *Pending Adoption*) og velg **Adopt Device**. Vent til den får status *Online*.
5. Verifiser under **Settings -> Networks** at LAN-subnettet matcher gruppens IP-plan.

---

## Del 5: Konfigurasjon av WireGuard VPN for Hjemmekontor / Fjernadgang

Nå skal dere sette opp en VPN-server på UXG-Lite slik at dere trygt kan koble laptopene til lab-nettverket når dere jobber hjemmefra.

1. I UniFi Controller-grensesnittet, gå til **Settings -> VPN -> VPN Server**.
2. Klikk **Create New** og velg **WireGuard**.
3. Oppgi følgende instillinger:
* **Name:** `Lab-VPN`
* **Client IP Subnet:** `10.8.0.0/24`
* **Port:** `51820` (Standard)


4. Opprett to klientprofiler under **Client Management**:
* Profil 1: `ElevA-Laptop`
* Profil 2: `ElevB-Laptop`


5. Last ned `.conf`-filen (eller generer QR-kode) for hver profil og lagre på laptopene deres.
6. **Installer WireGuard-klienten** på laptopene (kan lastes ned fra [wireguard.com](https://www.wireguard.com/install/)).
7. Importer `.conf`-filen i WireGuard på laptopen.

> **Obs for læreren/elevene ved testing hjemmefra:**
> For at VPN skal fungere utenfra skolen, må UXG-Lite ha en offentlig IP på sin WAN-port, ELLER skolens hovedbrannmur må videresende (port-forwarde) UDP-port `51820` til UXG-Lite sin WAN-IP. *(Dette avklares med IT-avdelingen/lærer).*

---

## Del 6: Opprette to virtuelle Windows-klienter i Proxmox

1. I Proxmox, klikk på **Create VM** øverst til høyre.
2. Opprett **VM 101 (`Win11-Klient-A`)**:
* **OS:** Velg Windows ISO-filen fra lagringsmediet.
* **System:** Hak av for TPM 2.0 og EFI (ved Windows 11).
* **Disks:** `50 GB`.
* **CPU / Memory:** `2 cores` og `4096 MB RAM`.
* **Network:** Bridge `vmbr0`.


3. Opprett **VM 102 (`Win11-Klient-B`)** med samme spesifikasjoner.
4. Start begge VM-ene og gjennomfør Windows-installasjonen via konsollen i Proxmox.

---

## Del 7: Aktivere Fjernskrivebord (RDP) i Windows

Gjør følgende på begge VM-ene når Windows er ferdig installert:

1. Opprett en lokal brukerkonto (`ElevA` på VM A, `ElevB` på VM B).
2. Gå til **Innstillinger -> System -> Fjernskrivebord (Remote Desktop)** og slå funksjonen **PÅ**.
3. Åpne Kommandotolke (`cmd`) i Windows og finn IP-adressen ved å skrive `ipconfig`. Noter ned IP-adressene (f.eks. `192.168.X.101` og `192.168.X.102`).

---

## Del 8: Verifisering, test og VPN-demonstrasjon

### Test 1: Lokal tilkobling (På skolen)

* Åpne **Tilkobling til fjernskrivebord (Mstsc)** fra laptopen mens du er plugget i svitsjen.
* Koble direkte til din VM sin IP-adresse (`192.168.X.101` eller `.102`).

### Test 2: Fjernadgang via WireGuard VPN (Hjemme/Mobilnett)

1. Koble laptopen til et eksternt nettverk (f.eks. del internett fra mobilen din).
2. Åpne **WireGuard** på laptopen og klikk **Aktiver / Tunnel On**.
3. Åpne nettleseren på laptopen og sjekk om du når Proxmox på `[https://192.168.](https://192.168.)X.2:8006`.
4. Start **Tilkobling til fjernskrivebord (Mstsc)** og koble til din Windows VM sin IP-adresse.

---

## Dokumentasjon og innlevering

Lever et felles dokument (PDF) på elevplattformen bestående av:

1. **Nettverkstoppologi:** En oversiktlig tegning av lab-oppsettet (f.eks. fra Draw.io) som viser fysiske enheter, virtuelle maskiner, portnummer, WAN/LAN-grensesnitt og tildelte IP-adresser (inkludert VPN-subnet).
2. **Skjermbilde-dokumentasjon:**
* Proxmox sitt webgrensesnitt med grønne piler som viser at Proxmox, UniFi-containeren og begge VM-ene kjører.
* UniFi Controller som viser at UXG-Lite er *Adopted* og at WireGuard VPN-serveren er *Active*.
* Skjermbilde av en aktiv Fjernskrivebord-sesjon (RDP) mot Windows-klienten **samtidig som WireGuard VPN er aktiv**.


3. **Faglig refleksjon:**
* *Hvorfor er det nødvendig med en statisk IP-adresse på Proxmox-vertens management-grensesnitt? Hva hadde skjedd om den brukte dynamisk DHCP?*
* *Beskriv hvordan datatrafikken krypteres og rutes når du sitter på et usikret offentlig Wi-Fi (f.eks. en kafé) og bruker WireGuard VPN for å styre Windows-klienten din i laben.*