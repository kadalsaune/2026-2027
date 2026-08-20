Her er et forslag til en pedagogisk og logisk oppbygd **årsplan-outline** for programfaget **Driftsstøtte (Vg2 Informasjonsteknologi)**.

Årsplanen er strukturert i **fem hovedbolker/perioder**, hvor det faglige innholdet bygger stein på stein: fra fysisk/lokal infrastruktur til virtualisering, skytjenester, automatisering, og til slutt sikkerhet, personvern og helhetlig prosjektarbeid.

---

## Oversikt over periodene

| Periode | Hovedtema | Sentrale kompetansemål (KM) |
| --- | --- | --- |
| **1. Høst (Uke 34–41)** | Driftsarkitektur, maskinvare og segmenterte nettverk | KM 1, 2, 5, 6 |
| **2. Høst/Vinter (Uke 42–51)** | Virtualisering, serverroller og brukeradministrasjon | KM 2, 3, 4, 5, 9 |
| **3. Vinter (Uke 2–9)** | Skytjenester, automatisering og bærekraftig IT | KM 3, 9, 12 |
| **4. Vår (Uke 10–17)** | Informasjonssikkerhet, risikoanalyse og personvern | KM 7, 8, 10, 11 |
| **5. Vår (Uke 18–24)** | Helhetlig systemdrift i praksis og eksamensforberedelse | Alle (spesielt 2, 4, 6, 8, 10) |

---

## Detaljert periodeoppsett

### Periode 1: Driftsarkitektur, maskinvare og segmenterte nettverk

**Estimert tid:** Uke 34–41 (ca. 7–8 uker)

#### Sentralt innhold

* Gjennomgang av fysiske komponenter i bedriftsnettverk (rutere, switcher, brannmurer, aksesspunkter, servere).
* Grunnleggende nettverksprotokoller (TCP/IP, IPv4/IPv6, DHCP, DNS, VLAN, routing).
* Planlegging og oppsett av segmenterte nettverk (VLAN) for å skille f.eks. administrasjon, ansatte, gjester og IoT.
* Systematisk dokumentasjon og merking av fysisk/logisk infrastruktur (topologitegninger).

#### Kobling til læreplanen

* **Kompetansemål:**
* *Utforske og beskrive komponenter i en driftsarkitektur*
* *Planlegge, implementere og drifte fysiske og virtuelle løsninger med segmenterte nettverk*
* *Utforske og beskrive relevante nettverksprotokoller, nettverkstjenester og serverroller*
* *Planlegge og dokumentere arbeidsprosesser og IT-løsninger*


* **Kjerneelementer:** Løsningsarkitektur og systemutvikling, IT-støtte og kommunikasjon.
* **Praktisk lab/aktivitet:** Bygge et fysisk/skalert lab-nettverk med Managed Switch og ruter. Elevene konfigurerer VLAN og dokumenterer topologien i et verktøy som Draw.io eller Cisco Packet Tracer.

---

### Periode 2: Virtualisering, serverroller og brukeradministrasjon

**Estimert tid:** Uke 42–51 (ca. 8–9 uker)

#### Sentralt innhold

* Prinsipper for virtualisering og hypervisorer (f.eks. Proxmox VE, Hyper-V, VMware ESXi eller VirtualBox).
* Oppsett og konfigurering av sentrale serverroller (Active Directory / LDAP, DNS, DHCP, fildeling, print).
* Identity & Access Management (IAM): Brukeradministrasjon, gruppepolicyer (GPO), tilgangsstyring (RBAC / Least Privilege).
* Introduksjon til enkel automatisering og skripting (PowerShell / Bash) for brukeropprettelse og rutineoppgaver.

#### Kobling til læreplanen

* **Kompetansemål:**
* *Planlegge, implementere og drifte fysiske og virtuelle løsninger med segmenterte nettverk*
* *Administrere brukere, tilganger og rettigheter i relevante systemer*
* *Utforske og beskrive relevante nettverksprotokoller, nettverkstjenester og serverroller*
* *Forenkle og automatisere arbeidsprosesser i utvikling av IT-løsninger*


* **Kjerneelementer:** Løsningsarkitektur og systemutvikling, Utviklingsprosesser og kreativ problemløsing.
* **Praktisk lab/aktivitet:** Sette opp et virtuelt domenemiljø. Elevene skal skrive et skript som automatisk oppretter brukere ut fra en CSV-fil og tildeler riktige grupperettigheter.

---

### Periode 3: Skytjenester, automatisering og grønn IT

**Estimert tid:** Uke 2–9 (ca. 7 uker)

#### Sentralt innhold

* Skymodeller: IaaS, PaaS, SaaS, samt hybrid- og multi-sky-arkitekturer (Azure, AWS, GCP eller Microsoft 365).
* Sammenligning av lokal infrastruktur vs. skyinfrastruktur (skalerbarhet, kostnader, drift).
* Videreføring av automatisering (Infrastructure as Code / skripting for skyressurser).
* **Bærekraftig IT:** Dataindustriens miljøavtrykk, energiforbruk i datasentre, e-avfall (EE-avfall), gjenbruk og livssyklusanalyse for maskinvare.

#### Kobling til læreplanen

* **Kompetansemål:**
* *Gjøre rede for prinsipper og strukturer for skytjenester og virtuelle tjenester*
* *Forenkle og automatisere arbeidsprosesser i utvikling av IT-løsninger*
* *Utforske dataindustriens miljøavtrykk og vurdere tiltak for å sikre bærekraftige valg i IT-løsninger*


* **Kjerneelementer:** Løsningsarkitektur og systemutvikling, Etikk, lovverk og yrkesutøvelse.
* **Tverrfaglig tema:** *Bærekraftig utvikling*.
* **Praktisk lab/aktivitet:** Casestudie der elevene analyserer en bedrifts serverpark, vurderer overgang til sky/hybridløsning, og beregner både økonomisk og miljømessig gevinst (strømforbruk, levetid på hardware).

---

### Periode 4: Informasjonssikkerhet, risikoanalyse og personvern

**Estimert tid:** Uke 10–17 (ca. 7 uker)

#### Sentralt innhold

* **Trusselbildet:** Social engineering (phishing), skadevare (ransomware), DDoS, insider-trusler.
* **Demokrati og medborgerskap:** Hvordan trusler mot digital infrastruktur, valgsystemer, mediehus og offentlige tjenester kan påvirke tilliten til demokratiet og den åpne samfunnsdebatten.
* Risikoanalyse i praksis (ROS-analyse): Identifisere verdier, trusler, sårbarheter og foreslå sikringstiltak.
* **Personvern og lovverk:** GDPR, krav til behandling av personopplysninger, konsekvenser av personvernbrudd for individer, bedrifter og samfunnet.
* Sikkerhetsprinsipper: Defense in Depth, Multi-Factor Authentication (MFA), kryptering, sikker sikkerhetskopiering (backup-strategier).

#### Kobling til læreplanen

* **Kompetansemål:**
* *Utforske trusler mot datasikkerhet og gjøre rede for dagens trusselbilde og hvordan truslene kan påvirke en åpen samfunnsdebatt og tilliten til demokratiet*
* *Gjennomføre risikoanalyse av nettverk og tjenester i en virksomhets systemer og foreslå tiltak for å redusere risikoen*
* *Planlegge, drifte og implementere IT-løsninger som ivaretar informasjonssikkerhet og gjeldende regelverk for personvern*
* *Reflektere over og beskrive hvordan brudd på personvernet kan påvirke enkeltmennesker, virksomheter og samfunn*


* **Kjerneelementer:** Informasjonssikkerhet, Etikk, lovverk og yrkesutøvelse.
* **Tverrfaglig tema:** *Demokrati og medborgerskap*.
* **Praktisk lab/aktivitet:** Utføre en ROS-analyse for en fiktiv case-bedrift som har opplevd et datainnbrudd/feilkonfigurert personvern. Utarbeide handlingsplan med tiltak og brukerretningslinjer.

---

### Periode 5: Helhetlig prosjektarbeid, systemdrift i praksis og eksamensforberedelse

**Estimert tid:** Uke 18–24 (ca. 6 uker)

#### Sentralt innhold

* Tverrfaglig eller emneovergripende bedriftscase («Totalentreprenør for en fiktiv bedrift»).
* Planlegging, spesifikasjon, bygging, sikring, automatisering og dokumentasjon av en komplett IT-løsning.
* Brukeropplæring, IT-supportmetodikk (ITIL-prinsipper light, ticketing, feilsøking).
* Trening på muntlige og praktiske presentasjoner/eksamensformer.

#### Kobling til læreplanen

* **Kompetansemål:** Repetisjon og integrasjon av *alle* kompetansemål.
* **Kjerneelementer:** IT-støtte og kommunikasjon, Utviklingsprosesser og kreativ problemløsing, Løsningsarkitektur.
* **Vurdering:** Større praktisk/muntlig prosjekt med dokumentasjon og demonstrasjon av den operative løsningen.

---

## Tverrgående vurderinger og metodikk

1. **Dokumentasjon og språk:** I alle perioder kreves det at elevene skriver teknisk dokumentasjon (bruker manualer, systemdokumentasjon, nettverkskart) tilpasset enten fagfolk eller sluttbrukere (*Kjerneelement: IT-støtte og kommunikasjon*).
2. **Feilsøking og problemløsing:** Legg inn feilsøkingsscenarier («chaos engineering» i lab-miljøet) der du som lærer bevisst legger inn feil i nettverk eller servere som elevene må diagnostisere systematisk.