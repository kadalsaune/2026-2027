---
marp: true
theme: default
paginate: true
header: 'VG2 IT Driftstøtte | PowerShell'

style: |
  section {
    font-family: 'Segoe UI', sans-serif;
    background: #f7f4ed;
    color: #1f2933;
    padding: 44px;
    overflow-y: auto;
  }
  h1 { color: #173f5f; font-size: 2.2em; }
  h2 { color: #20639b; border-bottom: 3px solid #f6a01a; padding-bottom: 8px; }
  h3 { color: #3a506b; }
  strong { color: #b54a35; }
  code { background: #e7edf2; color: #173f5f; }
  pre { background: #102a43; color: #f0f4f8; border-radius: 8px; padding: 18px; }
  pre code { background: transparent; color: inherit; }
  header, footer { color: #52606d; font-size: 0.6em; }
  .box { background: #ffffff; border-left: 6px solid #20639b; padding: 14px 20px; border-radius: 6px; }
  .challenge { background: #fff3d6; border-left: 6px solid #f6a01a; padding: 14px 20px; border-radius: 6px; }
  .success { background: #e5f5ec; border-left: 6px solid #2a9d62; padding: 14px 20px; border-radius: 6px; }
  .warning { background: #fde9e5; border-left: 6px solid #b54a35; padding: 14px 20px; border-radius: 6px; }
  table { font-size: 0.78em; width: 100%; }
  th { background: #173f5f; color: white; }
---

<!-- _class: lead -->
<!-- _paginate: false -->

# Hvorfor PowerShell?
## Fra mus til supportskript

**VG2 IT Driftstøtte**

3–5 undervisningstimer · Klientfokus

---

## Målet med økten

Når økten er ferdig, skal du kunne:

- forklare hvorfor en drifter bruker kommandolinjen
- kjenne igjen mønsteret **Verb-Noun**
- hente, filtrere og sortere informasjon med rørgata
- lage et enkelt PowerShell-skript for IT-support

<div class="box">

**Sluttprodukt:** `Generer-Helserapport.ps1` samler nøkkelinformasjon fra PC-en og lagrer en rapport på skrivebordet.

</div>

---

## Kjøreplan

| Del | Tema | Arbeidsform |
| :--- | :--- | :--- |
| 1 | Mus vs. tastatur | Demonstrasjon og diskusjon |
| 2 | Verb-Noun | Mikroøvelser |
| 3 | Rørgata (`\|`) | Filtrering og sortering |
| 4 | Supportskriptet | Bygg steg for steg |
| 5 | Deling og feilsøking | Test, forklar og forbedre |

---

<!-- LÆRERNOTAT: Bruk 10–15 minutter her. Ikke start med å forklare PowerShell. La elevene først kjenne på forskjellen mellom en repeterende GUI-oppgave og en kommando som kan gjentas. Kjør kommandoene i en egen midlertidig mappe, ikke direkte på skrivebordet. -->

## Modul 1: Mus vs. tastatur

### Utfordring til klassen

Lag 10 mapper på skrivebordet med navnene `Elev_1` til `Elev_10`.

<div class="challenge">

**Gjør det med mus først.** Hvor lang tid bruker dere?

</div>

<!-- LÆRERNOTAT: Ta tiden, men la elevene først foreslå en strategi. Spør: Hva er det som faktisk gjentas? Hva måtte dere endret hvis oppgaven var 100 mapper? -->

### Før vi lager noe: hvor er vi?

Kjør dette i PowerShell:

```powershell
Get-Location
$demo = Join-Path $env:TEMP "PS-Kurs-Demo"
New-Item -ItemType Directory -Path $demo -Force
Set-Location $demo
```

**Stopp og se:** `Get-Location` viser arbeidsmappen. `$demo` er en variabel som gjør at vi slipper å skrive hele stien flere ganger.

---

## Modul 1: Bygg én ting først

Start med én mappe:

```powershell
New-Item -ItemType Directory -Name "Elev_1"
Get-ChildItem
```

### Spør klassen

- Hvilken del sier **hva** vi skal lage?
- Hvilken del sier **hva det skal hete**?
- Hva tror dere skjer hvis vi kjører samme linje en gang til?

<!-- LÆRERNOTAT: La en elev kjøre kommandoen på nytt. Bruk feilmeldingen til å introdusere `-Force` og hvorfor det er lurt å lese feilmeldinger i stedet for å gjette. -->

---

## Modul 1: Fra én til mange

Se tallrekken alene:

```powershell
1..5
```

Koble deretter tallrekken til en handling:

```powershell
1..5 | ForEach-Object {
  New-Item -ItemType Directory -Name "Elev_$_"
}
Get-ChildItem
```

### Endre én ting

La elevene gjøre `1..5` om til `1..10`, kjøre på nytt og forklare hva som endret seg.

---

## Modul 1: Rydd etter demoen

Når klassen har sett resultatet, rydd opp i testmappen:

```powershell
Set-Location $env:TEMP
Remove-Item -Path $demo -Recurse -Force
Test-Path $demo
```

`Test-Path` bør nå vise `False`.

<!-- LÆRERNOTAT: Dette er et godt tidspunkt for å modellere ansvarlig automatisering: avgrens arbeidsområdet, test først, og rydd etterpå. Spør hvorfor `-Recurse` og `-Force` kan være farlige på feil sti. -->

---

## Aha-opplevelsen

PowerShell kan lage 100 mapper med én kommando:

```powershell
1..100 | ForEach-Object { New-Item -ItemType Directory -Name "Elev_$_" }
```

### Hva ser du?

- `1..100` lager en tallrekke
- `ForEach-Object` gjør det samme for hvert tall
- `New-Item` oppretter mappen
- `$_` betyr «elementet vi jobber med akkurat nå»

---

## Hvorfor er dette nyttig i drift?

Tenk deg at du skal hente ut dette fra **50 maskiner**:

- serienummer
- IP-adresse
- ledig diskplass
- tjenester som kjører

<div class="warning">

**Spørsmål:** Klikke i 50 menyer, eller kjøre én kommando / ett skript?

</div>

PowerShell gjør repeterbare oppgaver raske, sporbare og enklere å standardisere.

---

<!-- LÆRERNOTAT: Bruk 25–35 minutter. Målet er ikke at elevene skal memorere kommandoer, men at de skal oppdage mønsteret og bruke PowerShell til å finne neste kommando selv. -->

## Modul 2: Byggeklossene

### PowerShell snakker «verb-substantiv»

| Handling | Kommando | Hva gjør den? |
| :--- | :--- | :--- |
| Hente | `Get-Process` | Viser prosesser |
| Hente | `Get-Service` | Viser tjenester |
| Hente | `Get-Disk` | Viser disker |
| Lage | `New-Item` | Lager filer eller mapper |
| Endre | `Set-Volume` | Endrer voluminnstillinger |
| Fjerne / stoppe | `Remove-Item` / `Stop-Process` | Fjerner eller stopper |

Du trenger ikke pugge alt. Lær deg mønsteret og finn resten.

### Les en kommando som en setning

`Get-Process` betyr omtrent: **hent prosesser**.

`Get-Service` betyr: **hent tjenester**.

```powershell
Get-Command -Verb Get | Select-Object -First 10 Name
Get-Command -Noun Process
```

<!-- LÆRERNOTAT: Kjør første linje og spør elevene om de ser flere substantiv enn `Process`. Kjør deretter `Get-Command -Noun Process` og la dem foreslå et nytt verb. -->

---

## Modul 2: Se hva kommandoen faktisk returnerer

Kjør først:

```powershell
$prosesser = Get-Process
$prosesser | Select-Object -First 3
```

Undersøk deretter én av verdiene:

```powershell
$prosesser[0].Name
$prosesser[0].Id
$prosesser[0].WS
```

`$prosesser` er en samling objekter. Hvert objekt har egenskaper som `Name`, `Id` og `WS`.

---

## Modul 2: Oppdag egenskapene med `Get-Member`

```powershell
Get-Process | Get-Member
Get-Process | Select-Object -First 1 | Format-List *
```

### Minioppgave

Finn en egenskap som kan være nyttig når du feilsøker en treg maskin.

<!-- LÆRERNOTAT: La elevene lese resultatet høyt. Pek på forskjellen mellom `TypeName`, egenskaper og metoder. Hold det enkelt: en egenskap er informasjon om objektet; en metode er noe objektet kan gjøre. -->

---

## Modul 2: Mikroøvelse – bli kjent med klienten

Kjør én kommando om gangen:

```powershell
Get-Process
Get-NetIPAddress
Get-Volume
```

Kjør også disse for å gjøre resultatet lettere å lese:

```powershell
Get-NetIPAddress -AddressFamily IPv4 |
  Select-Object InterfaceAlias, IPAddress

Get-Volume |
  Select-Object DriveLetter, FileSystemLabel, SizeRemaining, Size
```

### Snakk sammen

1. Hvilken informasjon får du tilbake?
2. Hvilken kommando gir mest nytte i en supportsituasjon?
3. Hva kan være sensitiv informasjon i resultatet?

---

## Modul 2: Finn veien selv

PowerShell har innebygd hjelp:

```powershell
Get-Help Get-Process -Online
Get-Command *network*
Get-Command -Verb Get
```

### Live jakt på en kommando

1. Finn kommandoer som handler om nettverk.
2. Velg én kommando som begynner med `Get-`.
3. Les hjelpen med `Get-Help`.
4. Kjør kommandoen uten å endre noe.

```powershell
$kommando = Get-Command *network* | Select-Object -First 1
$kommando.Name
Get-Help $kommando.Name -Examples
```

<div class="success">

**Arbeidsmåte:** Gjett et verb → søk etter substantivet → les hjelp → test forsiktig.

</div>

---

<!-- LÆRERNOTAT: Bruk 30–40 minutter. Tegn en enkel rørgate på tavlen: kilde → filter → sortering → utvalg. Kjør hvert trinn separat først, og sett dem sammen til slutt. -->

## Modul 3: PowerShells superkraft

### Rørgata: `|`

Rørgata sender resultatet fra én kommando videre til den neste.

```powershell
Get-Process | Sort-Object WS -Descending
```

PowerShell sender **objekter med egenskaper**, ikke bare rå tekst. Derfor kan neste kommando sortere på `WS` uten at vi først må klippe og lime tekst.

### Se rørgata i tre trinn

```powershell
$alle = Get-Process
$sortert = $alle | Sort-Object WS -Descending
$sortert | Select-Object -First 5 Name, WS
```

<!-- LÆRERNOTAT: Spør før hver linje: Hva ligger i variabelen nå? La elevene bruke `Get-Member` på `$sortert` for å vise at objektene fortsatt er prosessobjekter etter sorteringen. -->

---

## Modul 3: Filtrer, velg, mål

Filtrer tjenester:

```powershell
$aktive = Get-Service | Where-Object Status -eq "Running"
$aktive | Select-Object -First 10 Name, DisplayName, Status
```

Tell dem:

```powershell
$aktive.Count
```

### Endre filteret

Prøv å finne tjenester som **ikke** kjører:

```powershell
Get-Service | Where-Object Status -ne "Running"
```

---

## Modul 3: Lag en liten supportsjekk

Kjør hele kjeden:

```powershell
Get-Process |
  Sort-Object WS -Descending |
  Select-Object -First 5 Name, Id, WS
```

### Snakk sammen

- Hva er kilden?
- Hvor skjer sorteringen?
- Hvorfor ligger `Select-Object` til slutt?
- Hva skjer hvis vi flytter `Select-Object` først?

<!-- LÆRERNOTAT: Flytt gjerne `Select-Object` først som en bevisst demonstrasjon. Når `WS` ikke lenger er med i objektene, vil sorteringen ikke kunne bruke egenskapen på samme måte. -->

---

## Filtrer det du trenger

Finn bare tjenester som kjører:

```powershell
Get-Service | Where-Object Status -eq "Running"
```

Finn de fem prosessene som bruker mest CPU:

```powershell
Get-Process |
  Sort-Object CPU -Descending |
  Select-Object -First 5
```

<div class="challenge">

**Oppgave:** Hva må endres for å vise de fem prosessene som bruker mest minne?

</div>

---

<!-- LÆRERNOTAT: Sett av 60–90 minutter. Skriv skriptet live i små deler. Etter hver del kjører du den nye delen i konsollen, slik at elevene ser at skriptet vokser fra kjente enkeltkommandoer. -->

## Modul 4: Support-skriptet

### Situasjonen

En bruker sier: «Maskinen er treg.»

Du trenger et raskt første bilde av klienten. Skriptet skal hente:

- datamaskinnavn og innlogget bruker
- dato og klokkeslett
- IPv4-adresse
- ledig og total plass på `C:`

<div class="box">

**Oppdrag:** Lag `Generer-Helserapport.ps1` og lagre rapporten på skrivebordet.

</div>

### Før vi skriver: planlegg input og output

**Input:** informasjon fra klienten.

**Bearbeiding:** hente, filtrere og formatere.

**Output:** en tekstfil som en supporttekniker kan lese.

```text
Klientdata → PowerShell-objekter → rapporttekst → fil på skrivebordet
```

<!-- LÆRERNOTAT: Be elevene foreslå én ekstra verdi før du viser fasiten. Dette gjør sluttprosjektet til et designproblem, ikke bare avskrift. -->

---

## Modul 4: Steg 1 – hent én verdi

Start med en enkelt variabel:

```powershell
$datamaskin = $env:COMPUTERNAME
$datamaskin
```

Kjør deretter:

```powershell
$bruker = $env:USERNAME
$dato = Get-Date -Format "yyyy-MM-dd HH:mm"
"Maskin: $datamaskin | Bruker: $bruker | Tid: $dato"
```

### Stoppunkt

Hva er forskjellen på å skrive `$bruker` og å skrive teksten `bruker`?

---

## Modul 4: Steg 2 – hent nettverk og disk

```powershell
$datamaskin = $env:COMPUTERNAME
$bruker = $env:USERNAME
$dato = Get-Date -Format "yyyy-MM-dd HH:mm"
$ip = (Get-NetIPAddress -AddressFamily IPv4 |
  Where-Object InterfaceAlias -notlike "*Loopback*").IPAddress |
  Select-Object -First 1
$disk = Get-Volume -DriveLetter C
```

Test verdiene før de brukes i rapporten:

```powershell
$ip
$disk | Select-Object DriveLetter, SizeRemaining, Size
```

<!-- LÆRERNOTAT: På noen maskiner kan IPv4-uttrykket returnere flere adresser eller ingen adresse. Vis resultatet på lærerens maskin før elevene kjører det, og bruk det som inngang til hvorfor vi tester mellomsteg. -->

Alt som starter med `$` er en **variabel**. Variabelen lagrer en verdi vi kan bruke senere.

---

## Modul 4: Steg 3 – sett sammen rapporten

```powershell
$rapport = @"
==========================================
PC-HELSERAPPORT
==========================================
Dato:         $dato
Datamaskin:   $datamaskin
Innlogget:    $bruker
IP-adresse:   $ip

DISKSTATUS (C:)
------------------------------------------
Ledig plass:  $([math]::Round($disk.SizeRemaining / 1GB, 2)) GB
Total plass:  $([math]::Round($disk.Size / 1GB, 2)) GB
==========================================
"@
```

`@" ... "@` er en here-string: praktisk når teksten går over flere linjer.

### Test rapportteksten før du lagrer

```powershell
$rapport
```

<!-- LÆRERNOTAT: Be elevene finne én verdi i rapportteksten og spore den tilbake til variabelen som leverte den. Dette gjør koblingen mellom variabel og tekst konkret. -->

---

## Modul 4: Steg 4 – lagre og gi beskjed

```powershell
$sti = "$env:USERPROFILE\Desktop\PC_Rapport.txt"
$rapport | Out-File -FilePath $sti

Write-Host "Ferdig! Rapporten er lagret på skrivebordet." -ForegroundColor Green
```

Kontroller at filen faktisk finnes:

```powershell
Test-Path $sti
Get-Item $sti | Select-Object Name, Length, LastWriteTime
Get-Content $sti
```

### Endre rapporten

La elevene legge til én av disse linjene i here-stringen:

```powershell
Antall prosesser: $((Get-Process).Count)
```

### Test

1. Lagre filen som `Generer-Helserapport.ps1`.
2. Kjør skriptet fra PowerShell.
3. Åpne `PC_Rapport.txt`.
4. Sammenlign rapporten med verdier i Innstillinger og Oppgavebehandling.

---

## Modul 4: Hele skriptet

```powershell
Write-Host "Genererer rapport, vennligst vent..." -ForegroundColor Cyan

$datamaskin = $env:COMPUTERNAME
$bruker = $env:USERNAME
$dato = Get-Date -Format "yyyy-MM-dd HH:mm"
$ip = (Get-NetIPAddress -AddressFamily IPv4 |
  Where-Object InterfaceAlias -notlike "*Loopback*").IPAddress |
  Select-Object -First 1
$disk = Get-Volume -DriveLetter C

$rapport = @"
PC-HELSERAPPORT
Dato: $dato
Datamaskin: $datamaskin
Innlogget: $bruker
IP-adresse: $ip
Ledig plass: $([math]::Round($disk.SizeRemaining / 1GB, 2)) GB
Total plass: $([math]::Round($disk.Size / 1GB, 2)) GB
"@

$sti = "$env:USERPROFILE\Desktop\PC_Rapport.txt"
$rapport | Out-File -FilePath $sti
Write-Host "Ferdig! Rapporten er lagret på skrivebordet." -ForegroundColor Green
```

---

## Modul 4: Kjøring av skript steg for steg

Windows kan blokkere skript som standard. På skolens maskiner må dette avklares med lærer eller administrator.

```powershell
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
```

Kjør deretter skriptet fra mappen der filen ligger:

```powershell
Set-Location "C:\sti\til\mappen"
Get-ChildItem *.ps1
& .\Generer-Helserapport.ps1
```

`&` betyr at PowerShell skal kjøre stien som en kommando.

<!-- LÆRERNOTAT: Bytt ut eksempelstien med den faktiske mappen før timen. Demonstrer også at `Get-ChildItem *.ps1` er en trygg måte å kontrollere filnavnet på før kjøring. -->

### Viktig

- Bruk riktig scope: `CurrentUser`, ikke hele maskinen.
- Ikke kjør ukjent kode du ikke forstår.
- En policy-endring kan være styrt av skolens IT-administrator.

---

## Modul 4: Feilsøking med kontrollpunkter

| Problem | Første sjekk |
| :--- | :--- |
| Kommando finnes ikke | `Get-Command Get-Volume` |
| Ingen IPv4-adresse | Koble til nettverk og kjør `Get-NetIPAddress` |
| `C:` mangler | Kjør `Get-Volume` og se tilgjengelige stasjoner |
| Skriptet blokkeres | Les feilmeldingen og sjekk ExecutionPolicy |
| Rapporten havner feil | Skriv ut `$sti` og sjekk brukernavn / skrivebord |

**God feilsøking:** Les hele feilmeldingen. Den peker ofte på linjen og egenskapen som må undersøkes.

### Fire spørsmål ved hver feil

1. Hvilken linje stoppet?
2. Hvilken verdi hadde variabelen da?
3. Finnes kommandoen og egenskapen?
4. Kan vi teste akkurat dette trinnet alene?

```powershell
$ip
$disk
Test-Path $sti
```

<!-- LÆRERNOTAT: Ikke reparer skriptet for eleven med en gang. Be elevene isolere uttrykket som feiler og kjøre det alene. Det trener samme arbeidsmåte som ved reell klientfeilsøking. -->

---

## Utvid prosjektet

Velg minst to forbedringer:

- legg inn operativsystem-versjon
- tell hvor mange tjenester som kjører
- vis de fem tyngste prosessene i rapporten
- lagre rapporten med datamaskinnavn i filnavnet
- legg til en tydelig feilmelding hvis `C:` ikke finnes
- bruk `Export-Csv` for data som skal viderebehandles

```powershell
$filnavn = "$env:USERPROFILE\Desktop\PC_Rapport_$datamaskin.txt"
```

---

## Vurdering og deling

### Forklar skriptet til en medelev

1. Hvilke verdier hentes inn?
2. Hvorfor bruker vi variabler?
3. Hvor brukes rørgata?
4. Hva skjer hvis en kommando ikke returnerer noe?
5. Hvordan kan en supporttekniker bruke rapporten?

<div class="success">

Et godt skript er ikke bare kort. Det er forståelig, testbart og nyttig for andre enn den som skrev det.

</div>

---

## Kortformvideoer

Tre ideer til 30–60 sekunder i stående format:

### 1. Mus vs. PowerShell
Venstre side: mange klikk. Høyre side: én linje og 100 mapper.

### 2. Finn den trege prosessen
Oppgavebehandling mot:
`Get-Process | Sort-Object CPU -Descending | Select-Object -First 5`

### 3. Hemmeligheten er engelsk
Vis mønsteret: `Get-Disk`, `Stop-Process`, `New-Item`.

---

<!-- _class: lead -->

# Huskelapp

```powershell
Get-Help <kommando> -Online
Get-Command *network*
Get-Process | Sort-Object WS -Descending
Get-Service | Where-Object Status -eq "Running"
```

**Verb-Noun · Rørgata · Objekter · Test forsiktig**