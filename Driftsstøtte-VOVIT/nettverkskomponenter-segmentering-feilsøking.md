---
marp: true
theme: default
paginate: true
header: 'VG2 IT Driftstøtte | Nettverkskomponenter & Segmentering'
footer: 'Periode 1 – Lærerveiledning & Elevgjennomgang'
style: |
  section {
    font-family: 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
    background-color: #f8fafc;
    color: #1e293b;
    padding: 40px;
  }
  h1 {
    color: #0f172a;
    font-size: 2.2em;
  }
  h2 {
    color: #1d4ed8;
    font-size: 1.6em;
    border-bottom: 2px solid #93c5fd;
    padding-bottom: 8px;
  }
  h3 {
    color: #0f766e;
    font-size: 1.2em;
  }
  footer {
    font-size: 0.55em;
    color: #64748b;
  }
  header {
    font-size: 0.55em;
    color: #64748b;
  }
  code {
    background-color: #e2e8f0;
    color: #0f172a;
    border-radius: 4px;
    padding: 2px 6px;
  }
  .box {
    background-color: #ffffff;
    border-left: 5px solid #2563eb;
    padding: 15px;
    border-radius: 6px;
    box-shadow: 0 4px 6px -1px rgba(0,0,0,0.1);
    margin-top: 15px;
  }
  .alert {
    background-color: #fef2f2;
    border-left: 5px solid #ef4444;
    padding: 15px;
    border-radius: 6px;
    margin-top: 15px;
  }
  .success {
    background-color: #f0fdf4;
    border-left: 5px solid #22c55e;
    padding: 15px;
    border-radius: 6px;
    margin-top: 15px;
  }
  table {
    font-size: 0.8em;
    width: 100%;
  }
  th {
    background-color: #1d4ed8;
    color: white;
  }
---

<!-- _class: lead -->
<!-- _paginate: false -->

# VG2 IT DRIFTSTØTTE – PERIODE 1
## Nettverkskomponenter, Segmentering & Feilsøking

**4-Timers Økt:**
- 50 min: Dynamisk fellesgjennomgang
- 3 timer: Veiledet caseoppgave (Elevpar)

---

## Kjøreplan for økten

<div class="box">

| Tid | Kvartel / Del | Tema & Fokusområder |
| :--- | :--- | :--- |
| **00:00 - 00:15** | **Kvartel 1** | Fysisk infrastruktur, Managed switch, ruter, brannmur & AP |
| **00:15 - 00:30** | **Kvartel 2** | Protokoller: DHCP (DORA), DNS, Default Gateway, APIPA |
| **00:30 - 00:45** | **Kvartel 3** | VLAN-segmentering, Access vs. Trunk (802.1Q) & Ruting |
| **00:45 - 00:55** | **Kvartel 4** | Feilsøking med Bottom-Up CLI-metodikk + Launch av Case |
| **01:00 - 04:00** | **Del 2** | Case: Fjordfrakt Logistikk AS (Arbeid i par) |

</div>

---

## Kvartel 1: Fysisk Infrastruktur & Komponentroller

### 💡 Tenk-Par-Del (60 sekunder)

> *"Daglig leder i en bedrift med 50 ansatte spør hvorfor vi må betale 18 000 kr for en Managed Switch og en dedikert brannmur, når han kan kjøpe en trådløs ruter på Elkjøp til 1200 kr. Hva forklarer du ham som IT-driftstekniker?"*

<div class="box">

**Forventede nøkkelpunkter:**
1. **Kapasitet & CPU/NAT:** Forbrukerruteren kveles av 50 samtidige brukere og tunge dataøkter.
2. **Managed Switch:** Nødvendig for **VLAN-støtte**, prioritering av trafikk (**QoS**), og portsikkerhet (**802.1X**).
3. **Dedikert Brannmur:** Gir tilstandskontroll (*Stateful Inspection*), sonesikkerhet (DMZ / Gjest / Intern) og sentralisert logging/overvåking.

</div>

---

## Rask tavle-sjekk: Fra gata til klient

### Hva er korrekt rekkefølge på de 5 leddene i signalveien?

<div class="success">

1. **Fiberterminering / Modem** (Signal inn fra ISP)
2. **Brannmur / Ruter** (Sikkerhet, NAT, ruting)
3. **Managed Core / Distribusjonsswitch** (Segmentering & rygggrad)
4. **Patchpanel ➔ Vegguttak / AP** (Kabling og spredning)
5. **Klient / Enhet** (Sluttbruker-PC, printer, IP-telefon)

</div>

---

## Kvartel 2: Kjerneprotokoller under panseret

### Hva skjer idet en nettverkskabel plugges inn i PC-en?

### 🔄 DORA-prosessen (DHCP)

<div class="box">

* **`D` - Discover:** Klienten sender **Broadcast** (`0.0.0.0` ➔ `255.255.255.255`): *"Finnes det en DHCP-server her?"*
* **`O` - Offer:** Serveren svarer med tilbud: IP, Subnet Mask, Default Gateway & DNS.
* **`R` - Request:** Klienten bekrefter: *"Jeg ønsker å låne den tilbudte IP-adressen."*
* **`A` - Acknowledge (ACK):** Serveren låser leieavtalen (*Lease*) i databasen og bekrefter til klienten.

</div>

---

## Viktig Sertifiseringsknagg (Karakter 4+)

### ⚠️ Hva skjer hvis DHCP-serveren IKKE svarer?

<div class="alert">

Maskinen tildeler seg selv en **APIPA-adresse (`169.254.x.x`)**.

**Hva forteller dette en IT-tekniker?**
* **Lag 1 & Lag 2 fysisk link er OK** (kabelen og nettverkskortet fungerer).
* Det er **ingen kontakt med DHCP-serveren på Lag 2/3** (feil VLAN, full DHCP-pool eller nede server).

</div>

---

## Default Gateway vs. DNS

### 💡 Spørsmål i plenum:
> *"Hva er forskjellen på Default Gateway og DNS? Hvem av dem trengs for å pinge `195.88.55.16`, og hvem trengs for å åpne `vg.no`?"*

<div class="box">

* **Default Gateway:**
  * Ruterens interne port (utveien fra lokalnettet).
  * **Nødvendig for all trafikk ut av eget subnett** (også direkte til IP `195.88.55.16`).
* **DNS (Domain Name System):**
  * Oversetter domenenavn til IP-adresser.
  * **Kun nødvendig for navneoppslag** (f.eks. omsette `vg.no` ➔ `195.88.55.16`).

</div>

---

## Kvartel 3: Segmentering & VLAN

### 🔐 Sikkerhetsscenario (Tenk-Par-Del: 45 sekunder)

> *"Gjestenettet og serverne med lønnssystemet er koblet i samme fysiske switch. Hvis vi IKKE bruker VLAN, men bare gir gjestene en annen IP-serie, er vi da trygge? Hvorfor / Hvorfor ikke?"*

<div class="alert">

**Svar: NEI! Dere er IKKE trygge!**
Uten VLAN befinner alle seg i **samme Broadcast-domene (Lag 2)**. 

En ondsinnet gjest kan enkelt:
1. Sette en **statisk IP** i ledelsens subnett manuelt.
2. **Sniffe ukryptert trafikk** med Wireshark.
3. Utføre **ARP-spoofing** og Man-in-the-Middle-angrep.

</div>

---

## Begrepsavklaring: Porttyper & Ruting

| Begrep | Teknisk Definisjon | Typisk Bruksområde |
| :--- | :--- | :--- |
| **Access-port** | **Utagget port.** All trafikk som går inn/ut tilhører kun ett bestemt VLAN ID. | Tilkobling av PC, printer, kassaapparat, fasttelefon. |
| **Trunk-port (802.1Q)** | **Tagget port.** Legger til en 4-byte 802.1Q-header i Ethernet-rammen som angir VLAN ID. | Link mellom switcher, eller mellom switch og brannmur/ruter (*Router-on-a-Stick*). |
| **Inter-VLAN Routing** | Ruting av trafikk mellom ulike VLAN via Lag 3 (Ruter eller L3-switch). | Gjør det mulig for autoriserte ansatte (VLAN 20) å nå servere (VLAN 10). |

---

## Kvartel 4: Feilsøking med Bottom-Up Metodikk

### En god driftstekniker gjetter aldri – test fra bunnen og opp!