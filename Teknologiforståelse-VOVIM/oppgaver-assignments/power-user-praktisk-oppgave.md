# 🛠️ Lab-oppgave: Power-User i Windows 11

**Fag:** Teknologiforståelse

**Tidsramme:** 60–90 minutter

**Formål:** Lære å bruke Windows sine innebygde administrasjons- og feilsøkingsverktøy som en IT-tekniker, samt dokumentere funn systematisk.

> 📌 **Dokumentasjonskrav:**
> Du skal levere et Lite **Word-dokument eller PDF** der du svarer på alle spørsmålene og legger inn **skjermdumper (Screenshots)** på angitte steder.
> *(Tips for skjermdump: Bruk `Win + Shift + S` for å ta utsnitt).*

---

### Del 1: Lynrask navigasjon med `Win + R` (15 min)

I denne delen skal du utforske systemkommandoer som IT-teknikere bruker for å hoppe rett til administrasjon uten å gå omveien via Innstillinger-appen.

1. Trykk `Win + R` for å åpne **Kjør (Run)**.
2. Test følgende kommandoer én etter én:
* `sysdm.cpl` (Systemegenskaper)
* `ncpa.cpl` (Nettverkstilkoblinger)
* `devmgmt.msc` (Enhetsbehandling)
* `compmgmt.msc` (Datamaskinbehandling)


3. **Endre maskinnavn:** I `sysdm.cpl`, trykk på *Endre...* (Change) og gi maskinen navnet: `ELEV-PC-[DittNavn]`. *(Ikke start på nytt ennå).*
4. **📸 Dokumentasjon for Del 1:**
* **Skjermdump 1:** Ta en skjermdump av vinduet `sysdm.cpl` som viser ditt nye datamaskinnavn.
* **Skriftlig svar:** Åpne `ncpa.cpl`. Hva er det eksakte navnet på det aktive nettverkskortet ditt, og hvilken status har det (f.eks. Wi-Fi eller Ethernet)?



---

Del 2: Prosessidentifikasjon og Ressursstyring (20 min)
Når en bruker opplever at PC-en er treg, eller at et program har «hengt seg», bruker IT-driftere Resource Monitor til å isolere og analysere én bestemt prosess om gangen.

Åpne Kalkulator-appen og Notisblokk (Notepad) fra Start-menyen på PC-en din.

Åpne Resource Monitor ved å trykke Win + R og skrive: resmon

Velg fanen CPU / Prosessor øverst i vinduet.

Se i listen Processes (Prosesser). Finn prosessen CalculatorApp.exe (eller Calculator.exe / Notepad.exe) og hukk av i boksene til venstre for disse prosessene.

Legg merke til at de andre seksjonene nederst på skjermen (som Associated Handles og Services) nå kun viser informasjon som tilhører de programmene du har huket av for.

Utvid seksjonen Associated Handles nederst på CPU-fanen (mens du fortsatt har huket av for prosessen din).

📸 Dokumentasjon for Del 2:
Skriftlig svar 1: Hvilken PID (Process ID – prosess-ID) har applikasjonen du huket av for i listen? (Obs: PID endrer seg hver gang et program startes!)

Skriftlig svar 2: Velg fanen Nettverk øverst i Resource Monitor. Åpne nettleseren din (f.eks. Edge eller Chrome) og last inn en nettside med mye innhold (f.eks. vg.no eller en HD-video på YouTube). Hvilken prosess oppnår øverst verdi under Total (B/sek) i listen over nettverksaktivitet?

Skjermdump 2: Ta en skjermdump av Resource Monitor der du har huket av for en prosess i CPU-listen, slik at man ser at visningen nederst filtreres på din valgte prosess.

---

### Del 3: Logganalyse og Stabilitetsindeks (25 min)

Når en bruker rapporterer at "maskinen krasjet på tirsdag", må du undersøke historikken.

1. Åpne **Reliability Monitor** via `Win + R` ved å skrive:
`perfmon /rel`
2. Studer grafen som viser maskinens stabilitetsscore over tid (skala fra 1 til 10).
3. Åpne deretter **Event Viewer** ved å skrive `eventvwr.msc` i `Win + R`.
4. Naviger til **Windows Logs -> System**.
5. Klikk på **Filter Current Log...** i høyre panel, og hukk av for å kun vise **Critical** og **Error**.
6. **📸 Dokumentasjon for Del 3:**
* **Skriftlig svar 1:** Hva er din nåværende stabilitetsscore (1-10) i Reliability Monitor?
* **Skriftlig svar 2:** Velg én feilmelding fra **Event Viewer** (System loggen). Noter ned:
* **Kilde (Source):**
* **Event ID (Hendelses-ID):**
* **Kort forklaring på hva feilen gjaldt:**


* **Skjermdump 3:** Ta skjermdump av grafen i Reliability Monitor (`perfmon /rel`).



---

### Del 4: Systemfilkontroll via Terminal (15 min)

Siste steg for en IT-drifter når Windows oppfører seg merkelig, er å verifisere at systemfilene ikke er skadet.

1. Høyreklikk på Start-menyen (`Win + X`) og velg **Terminal (Admin)** eller **PowerShell (Administrator)**.
2. Kjør kommandolinjeverktøyet System File Checker ved å skrive:

```cmd
   sfc /scannow
   

```

3. Vent til skanningen når 100%.
4. **📸 Dokumentasjon for Del 4:**
* **Skjermdump 4:** Ta en skjermdump av Terminal-vinduet etter at skanningen er ferdig, der du får frem resultatmeldingen (f.eks. *"Windows Resource Protection did not find any integrity violations"*).



---

### 📤 Sjekkliste før innlevering

Dokumentet ditt skal inneholde totalt **4 skjermdumper** og svar på de **5 skriftlige spørsmålene** fra Del 1–4.

