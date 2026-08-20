```python
import weasyprint
print(weasyprint.__version__)


```

```text
62.3


```

```python
import weasyprint

html_content = """<!DOCTYPE html>
<html lang="no">
<head>
    <meta charset="UTF-8">
    <title>Prosjektoppgave: Lab-nettverk med Proxmox VE, UniFi og VPN</title>
    <style>
        @page {
            size: A4;
            margin: 18mm 15mm 20mm 15mm;
            background-color: #ffffff;
            @bottom-right {
                content: "Side " counter(page) " av " counter(pages);
                font-family: 'Segoe UI', Arial, sans-serif;
                font-size: 8.5pt;
                color: #64748b;
            }
            @bottom-left {
                content: "Vg2 IT — Driftsstøtte | Lab-prosjekt: Proxmox, UniFi & VPN";
                font-family: 'Segoe UI', Arial, sans-serif;
                font-size: 8.5pt;
                color: #64748b;
            }
        }

        *, *::before, *::after {
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', -apple-system, BlinkMacSystemFont, Roboto, Helvetica, Arial, sans-serif;
            font-size: 10pt;
            line-height: 1.5;
            color: #1e293b;
            margin: 0;
            padding: 0;
        }

        /* Header / Banner */
        .header-banner {
            background-color: #0f172a;
            color: #ffffff;
            padding: 20px 22px;
            border-radius: 6px;
            margin-bottom: 20px;
        }

        .header-banner h1 {
            font-size: 18pt;
            margin: 0 0 6px 0;
            color: #f8fafc;
            font-weight: 700;
            line-height: 1.2;
        }

        .header-banner .subtitle {
            font-size: 11pt;
            color: #94a3b8;
            margin: 0 0 12px 0;
        }

        .meta-table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 10px;
            border-top: 1px solid #334155;
            padding-top: 10px;
        }

        .meta-table td {
            font-size: 9pt;
            color: #cbd5e1;
            padding: 3px 0;
            vertical-align: top;
        }

        .meta-table td strong {
            color: #38bdf8;
        }

        /* Section Headings */
        h2 {
            font-size: 13pt;
            color: #0f172a;
            border-left: 4px solid #0284c7;
            padding-left: 10px;
            margin-top: 22px;
            margin-bottom: 10px;
            page-break-after: avoid;
        }

        h3 {
            font-size: 11pt;
            color: #0369a1;
            margin-top: 14px;
            margin-bottom: 6px;
            page-break-after: avoid;
        }

        p {
            margin-top: 0;
            margin-bottom: 10px;
        }

        ul, ol {
            margin-top: 0;
            margin-bottom: 10px;
            padding-left: 20px;
        }

        li {
            margin-bottom: 4px;
        }

        code {
            font-family: 'Consolas', 'Courier New', monospace;
            background-color: #f1f5f9;
            color: #0f172a;
            padding: 2px 5px;
            border-radius: 4px;
            font-size: 9pt;
        }

        pre {
            font-family: 'Consolas', 'Courier New', monospace;
            background-color: #0f172a;
            color: #f8fafc;
            padding: 10px 12px;
            border-radius: 6px;
            font-size: 8.5pt;
            line-height: 1.4;
            overflow-x: auto;
            margin-top: 6px;
            margin-bottom: 12px;
            white-space: pre-wrap;
            word-break: break-all;
        }

        /* Tables */
        table.data-table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 8px;
            margin-bottom: 14px;
            font-size: 9pt;
        }

        table.data-table th, table.data-table td {
            border: 1px solid #cbd5e1;
            padding: 6px 10px;
            text-align: left;
        }

        table.data-table th {
            background-color: #f1f5f9;
            color: #0f172a;
            font-weight: 600;
        }

        table.data-table tr:nth-child(even) {
            background-color: #f8fafc;
        }

        /* Callout Boxes */
        .doc-box {
            background-color: #eff6ff;
            border: 1px solid #bfdbfe;
            border-left: 4px solid #2563eb;
            padding: 10px 14px;
            border-radius: 4px;
            margin-top: 10px;
            margin-bottom: 14px;
            page-break-inside: avoid;
        }

        .doc-box .box-title {
            font-weight: 700;
            color: #1d4ed8;
            font-size: 9.5pt;
            margin-bottom: 6px;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }

        .reflect-box {
            background-color: #f0fdf4;
            border: 1px solid #bbf7d0;
            border-left: 4px solid #16a34a;
            padding: 10px 14px;
            border-radius: 4px;
            margin-top: 10px;
            margin-bottom: 14px;
            page-break-inside: avoid;
        }

        .reflect-box .box-title {
            font-weight: 700;
            color: #15803d;
            font-size: 9.5pt;
            margin-bottom: 6px;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }

        .info-box {
            background-color: #fffbebfb;
            background-color: #fefce8;
            border: 1px solid #fef08a;
            border-left: 4px solid #eab308;
            padding: 10px 14px;
            border-radius: 4px;
            margin-top: 10px;
            margin-bottom: 14px;
            page-break-inside: avoid;
        }

        .info-box .box-title {
            font-weight: 700;
            color: #a16207;
            font-size: 9.5pt;
            margin-bottom: 4px;
        }

        .module-header {
            background-color: #1e293b;
            color: #ffffff;
            padding: 8px 12px;
            border-radius: 4px;
            font-size: 11pt;
            font-weight: 700;
            margin-top: 22px;
            margin-bottom: 12px;
            page-break-after: avoid;
        }

        .badge {
            display: inline-block;
            padding: 2px 6px;
            border-radius: 3px;
            font-size: 8pt;
            font-weight: 600;
            text-transform: uppercase;
        }

        .badge-blue { background-color: #dbeafe; color: #1e40af; }
        .badge-green { background-color: #dcfce7; color: #166534; }
    </style>
</head>
<body>

    <div class="header-banner">
        <h1>PROSJEKT: LOKALT LAB-NETTVERK MED PROXMOX, UNIFI OG VPN</h1>
        <div class="subtitle">Helhetlig oppgavetekst med praktiske steg, kontinuerlig dokumentasjon og refleksjon</div>
        <table class="meta-table">
            <tr>
                <td><strong>Fag:</strong> Driftsstøtte (Vg2 Informasjonsteknologi)</td>
                <td><strong>Arbeidsform:</strong> Pararbeid (2 elever per lab-stasjon)</td>
            </tr>
            <tr>
                <td><strong>Varighet:</strong> Flerukers prosjekt (5 økter / moduler)</td>
                <td><strong>Sluttleveranse:</strong> Komplett lab-rapport (PDF)</td>
            </tr>
        </table>
    </div>

    <h2>1. Formål og læringsmål</h2>
    <p>I dette flerukers prosjektet skal dere bygge og drifte en komplett, isolert IT-infrastruktur fra grunnen av. Dere skal gjennomføre bare-metal installasjon av hypervisoren <strong>Proxmox VE</strong>, konfigurere en dedikert bedriftsgateway (<strong>UniFi UXG-Lite</strong>), distribuere nettverkstjenester via containere (LXC), installere virtuelle Windows 11-klienter, samt sette opp en kryptert <strong>WireGuard VPN-tunnel</strong> for fjernadgang fra hjemmekontor.</p>

    <p><strong>Sentralt kompetansemål i læreplanen:</strong></p>
    <ul>
        <li>Utforske og beskrive komponenter i en driftsarkitektur.</li>
        <li>Planlegge, implementere og drifte fysiske og virtuelle løsninger med segmenterte nettverk.</li>
        <li>Utforske og beskrive relevante nettverksprotokoller, nettverkstjenester og serverroller.</li>
        <li>Planlegge, drifte og implementere IT-løsninger som ivaretar informasjonssikkerhet.</li>
        <li>Planlegge og dokumentere arbeidsprosesser og IT-løsninger.</li>
    </ul>

    <h2>2. Utstyrsliste og Hovedtopologi</h2>
    <table class="data-table">
        <thead>
            <tr>
                <th>Utstyr / Komponent</th>
                <th>Antall</th>
                <th>Funksjon / Beskrivelse</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td><strong>Stasjonær PC (Lab-vert)</strong></td>
                <td>1 stk.</td>
                <td>Kraftig PC som Proxmox VE installeres direkte på ("bare-metal").</td>
            </tr>
            <tr>
                <td><strong>USB-minnepenn</strong></td>
                <td>1 stk.</td>
                <td>Oppstartbar USB med Proxmox VE ISO.</td>
            </tr>
            <tr>
                <td><strong>UniFi Gateway UXG-Lite</strong></td>
                <td>1 stk.</td>
                <td>Fysisk ruter, DHCP-server og VPN-gateway.</td>
            </tr>
            <tr>
                <td><strong>Fysisk Svitsj</strong></td>
                <td>1 stk.</td>
                <td>Lokal svitsj for kabling av lab-stasjonen.</td>
            </tr>
            <tr>
                <td><strong>Elev-laptoper</strong></td>
                <td>2 stk.</td>
                <td>Arbeidsstasjoner for Elev A og Elev B.</td>
            </tr>
            <tr>
                <td><strong>Patchkabler (RJ-45)</strong></td>
                <td>5 stk.</td>
                <td>Nettverkskabler til kabling.</td>
            </tr>
        </tbody>
    </table>

    <pre>
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
    </pre>

    <div class="info-box">
        <div class="box-title">ℹ IP-Adresseskjema per Gruppe</div>
        Hver gruppe tildeles et unikt subnett. Erstatt <strong>X</strong> med gruppenummeret ditt gjennom hele prosjektet (f.eks. Gruppe 1 = <code>192.168.10.0/24</code>, Gruppe 2 = <code>192.168.20.0/24</code>).
    </div>

    <!-- MODUL 1 -->
    <div class="module-header">ØKT 1: FYSISK OPPOBLING, IP-PLANLEGGING OG INSTALLASJON AV PROXMOX VE</div>

    <h3>Praktiske oppgaver</h3>
    <ol>
        <li><strong>Fysisk kabling:</strong>
            <ul>
                <li>Kobl RJ-45 kabel fra skolens nettverksuttak til <strong>WAN-porten</strong> på UXG-Lite.</li>
                <li>Kobl <strong>LAN-porten</strong> på UXG-Lite til Port 1 på den fysiske svitsjen.</li>
                <li>Kobl nettverkskortet på stasjonær PC til Port 2 på svitsjen.</li>
                <li>Kobl Elev A og Elev B sine laptoper til henholdsvis Port 3 og Port 4 på svitsjen.</li>
            </ul>
        </li>
        <li><strong>Bare-metal installasjon av Proxmox VE:</strong>
            <ul>
                <li>Sett inn Proxmox USB-penn i den stasjonære PC-en og start opp via Boot-menyen (F12/F11).</li>
                <li>Velg <em>Install Proxmox VE (Graphical)</em>.</li>
                <li>Sett Target Disk (SSD/HD), Timezone (Norway/Oslo) og sett et sikkert root-passord.</li>
                <li><strong>Nettverkskonfigurasjon:</strong>
                    <ul>
                        <li>Hostname: <code>pve-gruppeX.lab</code></li>
                        <li>IP Address: <code>192.168.X.2/24</code></li>
                        <li>Gateway / DNS: <code>192.168.X.1</code></li>
                    </ul>
                </li>
                <li>Fullfør installasjonen, ta ut USB-pennen og start PC-en på nytt.</li>
            </ul>
        </li>
        <li><strong>Verifisering av tilgang:</strong> Åpne nettleseren på en laptop og logg inn på Proxmox UI: <code>https://192.168.X.2:8006</code>.</li>
    </ol>

    <div class="doc-box">
        <div class="box-title">📷 Hva som skal dokumenteres i Økt 1</div>
        <ol>
            <li><strong>Fysisk kabling:</strong> Et skarpt bilde av pulten/oppsettet med fargekoding/merking av kablene.</li>
            <li><strong>IP-adressetabell:</strong> En strukturert tabell i rapporten som viser planlagte IP-adresser for WAN, LAN Gateway, Proxmox Vert, UniFi Controller og VM-er.</li>
            <li><strong>Installationskvitering:</strong> Skjermbilde av skrivebordet på laptopen som viser pålogget Proxmox VE webgrensesnitt (med synlig URL <code>https://192.168.X.2:8006</code> og node-status).</li>
        </ol>
    </div>

    <div class="reflect-box">
        <div class="box-title">🤔 Refleksjonsoppgaver — Økt 1</div>
        <ol>
            <li><strong>Statisk IP vs. DHCP:</strong> Hvorfor <em>må</em> hypervisoren (Proxmox-verten) konfigureres med en statisk IP-adresse i stedet for å motta dynamisk IP fra DHCP? Hvilke konsekvenser ville det hatt for driften dersom verten byttet IP-adresse etter en omstart?</li>
            <li><strong>Virtualiseringsbro (vmbr0):</strong> Hva er funksjonen til nettverksbroen <code>vmbr0</code> i Proxmox, og hvordan fungerer denne sammen med det fysiske nettverkskortet (NIC) i PC-en?</li>
        </ol>
    </div>

    <!-- MODUL 2 -->
    <div class="module-header">ØKT 2: INSTALLASJON AV UNIFI CONTROLLER (LXC) OG ADOPTION AV UXG-LITE</div>

    <h3>Praktiske oppgaver</h3>
    <ol>
        <li><strong>Opprette LXC-container for UniFi Controller:</strong>
            <ul>
                <li>Åpne <strong>Shell</strong> på Proxmox-noden i web-grensesnittet.</li>
                <li>Kjør installasjonsskriptet for UniFi Network Application:
                    <pre>bash -c "$(wget -qLO - https://github.com/community-scripts/ProxmoxVE/raw/main/ct/unifi.sh)"</pre>
                </li>
                <li>Velg <strong>Advanced Settings</strong> og oppgi parametere:
                    <ul>
                        <li>CT ID: <code>100</code> | Hostname: <code>unifi-controller</code></li>
                        <li>RAM: <code>2048 MB</code> | Disk: <code>8 GB</code></li>
                        <li>IP: Statisk IP <code>192.168.X.3/24</code> | Gateway: <code>192.168.X.1</code></li>
                    </ul>
                </li>
            </ul>
        </li>
        <li><strong>Førstegangsoppsett og Adoption:</strong>
            <ul>
                <li>Åpne nettleseren mot kontrolleren: <code>https://192.168.X.3:8443</code>.</li>
                <li>Opprett lokal admin-bruker (f.eks. <code>admin_lab</code>) og gi nettverket navnet <code>Lab-GruppeX</code>.</li>
                <li>Gå til <strong>Devices</strong>, finn UXG-Lite (står som <em>Pending Adoption</em>) og klikk <strong>Adopt Device</strong>.</li>
                <li>Kontroller under <strong>Settings -> Networks</strong> at LAN-subnettet matcher gruppens IP-plan.</li>
            </ul>
        </li>
    </ol>

    <div class="doc-box">
        <div class="box-title">📷 Hva som skal dokumenteres i Økt 2</div>
        <ol>
            <li><strong>LXC Container-status:</strong> Skjermbilde fra Proxmox som viser LXC ID 100 <code>unifi-controller</code> i aktiv drift (inkludert CPU/RAM-graf).</li>
            <li><strong>Device Adoption:</strong> Skjermbilde fra UniFi Network-oversikten der UXG-Lite har status <strong>Online / Getting Ready</strong>.</li>
            <li><strong>Nettverkskonfigurasjon:</strong> Skjermbilde fra UniFi Controller som viser oppsettet for LAN-nettverket (DHCP IP Range og Subnet Mask).</li>
        </ol>
    </div>

    <div class="reflect-box">
        <div class="box-title">🤔 Refleksjonsoppgaver — Økt 2</div>
        <ol>
            <li><strong>LXC vs. Fysisk/Virtuell Maskin:</strong> Hva er de viktigste forskjellene på en LXC-container og en full virtuell maskin (VM) i Proxmox når det gjelder ressurshåndtering (RAM/CPU) og isolasjon? Hvorfor egner UniFi Controller seg godt som LXC?</li>
            <li><strong>Prosess for Adoption:</strong> Hva skjer teknisk under "adoption"-prosessen mellom UXG-Lite og UniFi Controller? Hvordan vet gatewayen hvilken kontroller den skal rapportere til?</li>
        </ol>
    </div>

    <!-- MODUL 3 -->
    <div class="module-header">ØKT 3: VIRTUELLE WINDOWS-KLIENTER OG NETTVERKSVERIFISERING</div>

    <h3>Praktiske oppgaver</h3>
    <ol>
        <li><strong>Opprette Virtuelle Maskiner (VM 101 og VM 102):</strong>
            <ul>
                <li>Klikk <strong>Create VM</strong> i Proxmox UI.</li>
                <li><strong>VM 101 (Win11-Klient-A):</strong> CPU: 2 Cores | RAM: 4096 MB | Disk: 50 GB | Network Bridge: <code>vmbr0</code> | TPM 2.0 & EFI aktivert.</li>
                <li><strong>VM 102 (Win11-Klient-B):</strong> Samme spesifikasjoner, dedikert til Elev B.</li>
                <li>Start installasjonen og opprett lokale brukerkontoer (<code>ElevA</code> og <code>ElevB</code>).</li>
            </ul>
        </li>
        <li><strong>Nettverkskonfigurasjon og feilsøking i Windows:</strong>
            <ul>
                <li>Åpne Kommandotolke (<code>cmd</code>) på begge klientene og kjør <code>ipconfig /all</code>. Verifiser at de mottar IP-adresser fra UXG-Lite sine DHCP-tjeneste.</li>
                <li>Test nettverksforbindelse mellom Klient A og Klient B med <code>ping [IP-adresse]</code>.</li>
                <li>Dersom ping feiler: Konfigurer Windows Firewall til å tillate ICMPv4-Inbound echo request.</li>
            </ul>
        </li>
    </ol>

    <div class="doc-box">
        <div class="box-title">📷 Hva som skal dokumenteres i Økt 3</div>
        <ol>
            <li><strong>Proxmox VM Oversikt:</strong> Skjermbilde av Proxmox ressursoversikt som viser at både VM 101 og VM 102 kjører samtidig.</li>
            <li><strong>IP-konfigurasjon:</strong> Skjermbilde av <code>ipconfig /all</code> fra begge Windows-klientene side om side.</li>
            <li><strong>Vellykket Ping-test:</strong> Skjermbilde fra <code>cmd</code> som viser 0% pakketap ved ping mellom <code>Win11-Klient-A</code> og <code>Win11-Klient-B</code>.</li>
        </ol>
    </div>

    <div class="reflect-box">
        <div class="box-title">🤔 Refleksjonsoppgaver — Økt 3</div>
        <ol>
            <li><strong>Brannmur og sikkerhet:</strong> Hvorfor blokkerer Windows Firewall ICMP (ping) som standard på "Public/Private" nettverksprofiler? Hvilke sikkerhetsrisikoer er knyttet til at enheter svarer på ping i et bedriftsnettverk?</li>
            <li><strong>Ressursallokering (Overcommit):</strong> Dersom den stasjonære PC-en har 16 GB RAM totalt, og dere tildeler 4 GB til Elev A sin VM, 4 GB til Elev B sin VM, 2 GB til UniFi LXC og Proxmox bruker 2 GB selv — hva skjer dersom dere i tillegg starter opp to nye tunge server-VM-er? Forklar konseptet RAM Overcommit og Swapping.</li>
        </ol>
    </div>

    <!-- MODUL 4 -->
    <div class="module-header">ØKT 4: FJERNSTYRING (RDP) OG WIREGUARD VPN FOR HJEMMEKONTOR</div>

    <h3>Praktiske oppgaver</h3>
    <ol>
        <li><strong>Konfigurere Remote Desktop (RDP):</strong>
            <ul>
                <li>Inne på hver Windows-klient: Gå til <em>Innstillinger -> System -> Remote Desktop</em> og aktiver funksjonen.</li>
                <li>Gjør en lokal test: Åpne <em>Tilkobling til fjernskrivebord (mstsc)</em> på Elev A sin laptop og koble til IP-adressen til <code>Win11-Klient-A</code>.</li>
            </ul>
        </li>
        <li><strong>Oppsett av WireGuard VPN Server i UniFi:</strong>
            <ul>
                <li>Gå til UniFi Controller: <strong>Settings -> VPN -> VPN Server -> Create New</strong>.</li>
                <li>Velg VPN Protocol: <strong>WireGuard</strong>.</li>
                <li>Client IP Subnet: <code>10.8.0.0/24</code> | Server Port: <code>51820</code>.</li>
                <li>Under <strong>Client Management</strong>: Opprett to klientprofiler (<code>ElevA-Laptop</code> og <code>ElevB-Laptop</code>).</li>
                <li>Last ned <code>.conf</code>-profilfilene til laptopene.</li>
            </ul>
        </li>
        <li><strong>Klientkonfigurasjon og Ekstern Test:</strong>
            <ul>
                <li>Installer WireGuard-klientprogramvaren på laptopene ([wireguard.com](https://www.wireguard.com)).</li>
                <li>Importer <code>.conf</code>-filen og aktiver tunnelen.</li>
                <li><strong>Ekstern test (Simulert hjemmekontor):</strong> Koble laptopen til et eksternt nettverk (f.eks. mobilt internett / hotspot). Aktiver WireGuard VPN og sjekk at du når både Proxmox UI (<code>192.168.X.2:8006</code>) og kan starte en RDP-sesjon mot din virtuelle Windows-klient.</li>
            </ul>
        </li>
    </ol>

    <div class="doc-box">
        <div class="box-title">📷 Hva som skal dokumenteres i Økt 4</div>
        <ol>
            <li><strong>RDP I Drift:</strong> Skjermbilde fra Elev A sin laptop som viser at han/hun har en aktiv RDP-sesjon vindu-i-vindu mot sin virtuell Windows 11-maskin.</li>
            <li><strong>UniFi VPN Server Dashboard:</strong> Skjermbilde fra UniFi Controller som viser at WireGuard VPN Server er aktiv og at klientene er opprettet.</li>
            <li><strong>Ekstern VPN-tilkobling:</strong> Skjermbilde av WireGuard-klienten på laptopen i tilkoblet tilstand (med synlig Sendt/Mottatt data og VPN IP <code>10.8.0.x</code>) <em>samtidig</em> som RDP-vinduet mot Windows-klienten er åpent over eksternt nett.</li>
        </ol>
    </div>

    <div class="reflect-box">
        <div class="box-title">🤔 Refleksjonsoppgaver — Økt 4</div>
        <ol>
            <li><strong>VPN vs. Port Forwarding:</strong> Hvorfor er det en uakseptabel sikkerhetsrisiko å åpne RDP-porten (TCP 3389) direkte mot Internett i en bedrift? Hvordan beskytter WireGuard VPN-tunnelen bedriftens interne ressurser mot angripere?</li>
            <li><strong>UDP vs. TCP for VPN:</strong> WireGuard benytter seg av protokollen UDP (port 51820) i stedet for TCP. Hva er forskjellene på TCP og UDP, og hvorfor egner UDP seg spesielt godt for tunneler og VPN-trafikk?</li>
        </ol>
    </div>

    <!-- MODUL 5 -->
    <div class="module-header">ØKT 5: SLUTTDOKUMENTASJON, TOPOLOGIKART OG RAPPORTSAMMENSTILLING</div>

    <h3>Praktiske oppgaver</h3>
    <ol>
        <li><strong>Utarbeidelse av helhetlig nettverkskart (Topologi):</strong>
            <ul>
                <li>Bruk et profesjonelt verktøy som Draw.io, Cisco Packet Tracer eller Visio.</li>
                <li>Tegn opp hele infrastrukturen: Skolenett, WAN/LAN-grensesnitt på UXG-Lite, fysisk svitsj, Proxmox-vert, LXC-containere, VM-er og VPN-tunnel.</li>
                <li>Inkluder alle IP-adresser, portnummer, VLAN/subnett og brukernavn/roller.</li>
            </ul>
        </li>
        <li><strong>Kvalitetssikring og samling av rapporten:</strong>
            <ul>
                <li>Sammenstill alle dokumentasjonskrav og refleksjonssvar fra Økt 1–4 i ett felles PDF-dokument.</li>
                <li>Legg ved en egen <strong>Feilsøkingslogg</strong> (tabell) som beskriver utfordringer gruppen støyter på underveis og hvordan dere løste dem.</li>
            </ul>
        </li>
    </ol>

    <div class="doc-box">
        <div class="box-title">📷 Hva som skal levers i Sluttrapporten (Økt 5)</div>
        <ol>
            <li><strong>Komplett Nettverkstoppologi:</strong> Vektor/høyoppløselig bilde av det digitale nettverkskartet.</li>
            <li><strong>Feilsøkingslogg:</strong> Tabell over oppståtte feil (f.eks. sertifikatadvarsler, brannmurblokkering, manglende IP) med årsak og valgt tiltak.</li>
            <li><strong>Alle samlede dokumentasjonsbilder og besvarte refleksjonsoppgaver fra Økt 1 til 4.</strong></li>
        </ol>
    </div>

    <div class="reflect-box">
        <div class="box-title">🤔 Sluttrefleksjoner og Bærekraftsanalyse — Økt 5</div>
        <ol>
            <li><strong>Bærekraft og Grønn IT:</strong> Sammenlign dette virtualiserte oppsettet (1 kraftig fysisk PC som kjører Proxmox + 3 virtuell enheter) med å kjøpe 3 separate fysiske servere. Vurder forskjellene i energiforbruk, e-avfall (hardware livssyklus) og romoppvarming/kjøling i et datasenter.</li>
            <li><strong>Sikkerhet og Risikoanalyse:</strong> Tenk deg at denne lab-infrastrukturen var produksjonsnettverket til en liten bedrift med 20 ansatte. Identifiser minst to "Single Point of Failure" (SPOF) i designet og foreslå tiltak for å øke tilgjengeligheten (f.eks. redundans, backup-strategi, UPS).</li>
        </ol>
    </div>

</body>
</html>
"""

# Compile HTML to PDF using WeasyPrint
pdf_path = "Oppgave_Lab_Nettverk_Proxmox_UniFi_VPN.pdf"
weasyprint.HTML(string=html_content).write_pdf(pdf_path)
print("PDF generated successfully:", pdf_path)


```

```text
PDF generated successfully: Oppgave_Lab_Nettverk_Proxmox_UniFi_VPN.pdf


```

Your PDF file is ready

[file-tag: code-generated-file-d390e0c0-e9c9-437d-8f42-c099aeb74c46]

---

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