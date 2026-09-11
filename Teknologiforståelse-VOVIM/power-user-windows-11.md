---
marp: true
theme: gaia
paginate: true
header: 'Teknologiforståelse VOVIM'
style: |
  section {
    font-size: 28px;
    overflow-y: auto;
  }
  h1, h2 {
    color: #0b5cab;
  }
  code {
    color: #7a2e00;
  }
---

<!-- _class: lead -->

# Windows 11
## Fra vanlig bruker til power user

**Hvordan tenker en IT-drifter?**

---

# Mål for økta

Etter presentasjonen skal du kunne:

- jobbe raskere og mer presist i Windows 11
- finne ut hva som faktisk skjer på en PC
- feilsøke systematisk i stedet for å gjette
- gjøre endringer kontrollert og reverserbart
- dokumentere slik at andre kan overta arbeidet

---

# To ulike roller

| Vanlig bruker | IT-drifter |
| --- | --- |
| Vil at det skal virke | Vil forstå hvorfor det virker |
| Prøver tilfeldige løsninger | Tester én hypotese om gangen |
| Endrer innstillinger | Vurderer konsekvens og risiko |
| Løser bare sin egen PC | Lager en løsning som kan gjentas |
| Husker kanskje løsningen | Dokumenterer løsningen |

**Power user** betyr ikke å kunne flest triks. Det betyr å ha bedre kontroll.

---

# Grunnregelen: observer før du endrer

Når noe er galt:

1. **Avgrens problemet** – hvem, hva, hvor og når?
2. **Samle fakta** – feilmelding, tidspunkt, endringer og symptomer.
3. **Lag en hypotese** – hva tror du er årsaken?
4. **Test minst mulig først** – velg en trygg test.
5. **Endre én ting om gangen**.
6. **Bekreft og dokumenter** resultatet.

> En omstart kan være en test. Den er ikke en forklaring.

---

# Tastatursnarveier som faktisk sparer tid

- `Win + E` – Filutforsker
- `Win + I` – Innstillinger
- `Win + X` – hurtigmeny for administrasjon
- `Win + V` – utklippstavlehistorikk
- `Win + Shift + S` – skjermklipp
- `Alt + Tab` – bytt mellom vinduer
- `Win + Ctrl + D` – nytt virtuelt skrivebord
- `Ctrl + Shift + Esc` – Oppgavebehandling

**Tips:** Lær tre snarveier godt før du lærer ti nye.

---

# Søk og start verktøy raskt

Trykk `Win` og skriv navnet på verktøyet:

- **Terminal** – PowerShell og kommandolinje
- **Oppgavebehandling** – prosesser, ytelse og oppstart
- **Tjenester** – bakgrunnstjenester
- **Hendelsesliste** – logger og systemhendelser
- **Enhetsbehandling** – maskinvare og drivere
- **Ressursovervåking** – mer detaljert ressursbruk
- **Datamaskinbehandling** – samlet administrasjonsflate

Søk er ofte raskere enn å lete i menyer.

---

# Oppgavebehandling: mer enn «Avslutt oppgave»

Bruk fanene til å undersøke:

- **Prosesser:** Hva bruker CPU, minne, disk eller nettverk?
- **Ytelse:** Er flaskehalsen prosessor, minne, disk eller nettverk?
- **Oppstartsapper:** Hva starter automatisk?
- **Brukere:** Hvilke økter og prosesser er aktive?
- **Tjenester:** Hvilke tjenester kjører?

Se etter mønster over tid. Et høyt tall i ett sekund er ikke nødvendigvis et problem.

---

# Ressursovervåking: `resmon`

Åpne **Ressursovervåking** ved å trykke `Win + R`, skrive `resmon` og trykke Enter.

Bruk verktøyet når Oppgavebehandling viser et symptom, men du trenger mer detaljer:

- **Oversikt:** hvilke prosesser bruker ressurser akkurat nå?
- **CPU:** hvilke tjenester og prosesser venter eller belaster prosessoren?
- **Minne:** er minnet fullt, eller brukes det effektivt som hurtigbuffer?
- **Disk:** hvilken prosess leser eller skriver mye?
- **Nettverk:** hvilke prosesser har aktive forbindelser?

`resmon` passer godt for en rask og detaljert undersøkelse av en treg PC.

---
<!--- 
# Ytelsesovervåking: `perfmon`

Åpne **Ytelsesovervåking** ved å trykke `Win + R`, skrive `perfmon` og trykke Enter.

Bruk `perfmon` når du vil måle og dokumentere utviklingen over tid:

- legg til tellere for prosessor, minne, disk og nettverk
- sammenlign normal drift med tidspunktet problemet oppstår
- lag en **datainnsamlersett** for en lengre måling
- se etter mønstre før du konkluderer

Eksempler på tellere:

- **Processor(_Total)\\% Processor Time**
- **Memory\\Available MBytes**
- **PhysicalDisk(_Total)\\% Disk Time**

`perfmon` svarer på: *Hva skjer over tid, ikke bare akkurat nå?*

---

-->

# Pålitelighetsovervåking: `perfmon /rel`

Åpne **Pålitelighetsovervåking** ved å trykke `Win + R`, skrive `perfmon /rel` og trykke Enter.

Bruk verktøyet for å se en tidslinje over hva som har skjedd på PC-en:

- program- og Windows-krasj
- maskinvarefeil og uventede avslutninger
- installasjoner og avinstallasjoner
- oppdateringer og driverendringer
- når problemene startet og hvor ofte de skjer

Start med å finne tidspunktet da problemet oppstod. Sammenlign deretter hendelsen med endringer som skjedde rett før.

Pålitelighetsindeksen er et hint, ikke en ferdig diagnose. Bruk detaljene i hendelsen og andre logger for å finne årsaken.

---

# Filutforsker med kontroll

Power users:

- viser filendelser: **Vis → Vis → Filtyper**
- bruker meningsfulle mappenavn og en ryddig struktur
- skiller arbeidsfiler, arkiv, installasjon og sikkerhetskopi
- sjekker filbane og dato før de overskriver noe
- unngår å lagre viktige filer bare på skrivebordet

**Viktig:** En synkronisert mappe er ikke automatisk en sikkerhetskopi.

---

# Terminalen: spør systemet direkte

Åpne Windows Terminal og prøv:

```powershell
Get-ComputerInfo | Select-Object WindowsProductName, WindowsVersion
Get-Process | Sort-Object CPU -Descending | Select-Object -First 10
Get-Service | Where-Object Status -eq 'Running'
Get-PSDrive -PSProvider FileSystem
```

PowerShell gir informasjon som kan leses, filtreres og brukes på nytt.

**Tenk:** Hva vil jeg vite? Hvilket verktøy kan svare på det?

---

# Nettverk: skill mellom symptom og årsak

Ved «internett virker ikke»:

1. Er problemet bare på én enhet?
2. Har enheten IP-adresse?
3. Fungerer kommunikasjon til lokal gateway?
4. Fungerer DNS-oppslag?
5. Fungerer forbindelsen til en ekstern tjeneste?

Nyttige kommandoer:

```powershell
ipconfig /all
ping 192.168.1.1
nslookup nrk.no
Test-NetConnection nrk.no -Port 443
```

Ikke hopp rett til å bytte passord eller reinstallere.

---

# Tillatelser og administratorrollen

Windows skiller mellom:

- vanlig bruker og administrator
- lese-, skrive- og kjøretilgang
- brukerens profil og hele maskinen
- applikasjonens behov og faktisk tilgang

**Least privilege:** Bruk minst mulig tilgang for å løse oppgaven.

Før du kjører noe som administrator, spør:

- Hvorfor kreves det?
- Hva endres?
- Kan endringen rulles tilbake?
- Hvordan bekrefter jeg at den virket?

---

# Sikkerhet er en del av god drift

En power user:

- oppdaterer Windows og programmer
- bruker unike passord og multifaktorautentisering
- låser skjermen når arbeidsplassen forlates
- vurderer lenker, vedlegg og popup-vinduer kritisk
- installerer bare programvare fra pålitelige kilder
- beskytter personopplysninger og elevdata

**Bekvemmelighet er ikke et godt argument for å fjerne sikkerhet.**

---

# Hendelsesliste: systemets loggbok

Åpne **Hendelsesliste** og se under:

- **Windows-logger → System** – drivere, tjenester og maskinvare
- **Windows-logger → Program** – applikasjonsfeil
- **Windows-logger → Sikkerhet** – innlogging og tilgang, avhengig av policy

Se etter:

- tidspunkt
- kilde
- hendelses-ID
- alvorlighetsgrad
- hva som skjedde rett før feilen

En logg betyr ikke automatisk at noe er alvorlig.

---

# Feilsøking som en liten vitenskap

| Steg | Spørsmål |
| --- | --- |
| Problem | Hva er det som ikke fungerer? |
| Omfang | Gjelder det én bruker eller mange? |
| Tid | Når startet det? Hva endret seg? |
| Hypotese | Hvilken årsak passer med fakta? |
| Test | Hvilken trygg test skiller årsakene? |
| Tiltak | Hva endrer vi, og hvorfor? |
| Kontroll | Virker det fortsatt etterpå? |

Skriv ned resultatet, også når hypotesen var feil.

---

# Endringskontroll på én PC

Før en endring:

- noter nåsituasjonen
- ta sikkerhetskopi av viktig data
- avtal hva «tilbake» betyr
- velg et tidspunkt med lav risiko

Etter endringen:

- test den opprinnelige funksjonen
- test relevante bivirkninger
- noter innstilling, tidspunkt og resultat
- rydd opp i midlertidige filer og verktøy

En god løsning kan forklares og gjentas av andre.

---

# Dokumentasjon: gjør kunnskapen delbar

En kort driftslogg bør inneholde:

```text
Dato og tidspunkt:
Problem og omfang:
Observasjoner og feilmeldinger:
Hypotese:
Tiltak og kommandoer:
Resultat:
Tilbakeføring:
Neste steg / hvem må informeres:
```

Skriv fakta først. Skill tydelig mellom **observert**, **antatt** og **bekreftet**.

---

# Automatiser det som gjentas

Automatisering passer når oppgaven:

- skjer ofte
- følger faste regler
- har lav risiko ved feil
- kan testes på et lite utvalg først

Eksempel: finne store filer i hjemmemappen:

```powershell
Get-ChildItem $HOME -File -Recurse -ErrorAction SilentlyContinue |
  Sort-Object Length -Descending |
  Select-Object -First 10 FullName, Length
```

Automatisering uten kontroll kan gjøre feil raskere.

---

<!-- 

# Praktisk oppgave: drift en «problem-PC»

Scenario: En bruker sier at PC-en er treg, og at nettleseren «ikke finner nettet».

Arbeid i par:

1. Still fem avklarende spørsmål.
2. Samle fakta uten å endre systemet.
3. Lag to mulige hypoteser.
4. Velg én trygg test per hypotese.
5. Gjør ett kontrollert tiltak.
6. Lever en kort driftslogg.

**Krav:** Ingen reinstallasjon, tilfeldig sletting eller «prøv alt»-metode.

---

-->

# Sjekkliste for en power user

Før du avslutter en sak, kan du svare ja på dette?

- Jeg vet hva problemet faktisk var.
- Jeg har skilt fakta fra antakelser.
- Jeg vet hva som ble endret.
- Jeg har kontrollert at løsningen virker.
- Jeg har vurdert sikkerhet og tilgang.
- En annen person kan følge dokumentasjonen.

Dette er forskjellen på å ha flaks og å drive IT.

---

<!-- _class: lead -->

# Oppsummering

## Power user = kontrollert nysgjerrighet

Bruk Windows-verktøyene til å **observere**.

Bruk driftsmetoden til å **forstå**.

Bruk dokumentasjon og sikkerhet til å **skape tillit**.

**Neste gang noe feiler: stopp, spør, mål, test og skriv ned.**
