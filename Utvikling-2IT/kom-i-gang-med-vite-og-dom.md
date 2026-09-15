<!-- ---
marp: true
title: Kom i gang med Vite og DOM
author: Grunnleggende webutvikling
paginate: true
theme: uncover
style: |
  :root {
    --accent: #e4572e;
    --ink: #172121;
    --muted: #52605f;
    --paper: #fffaf2;
  }
  section {
    background: var(--paper);
    color: var(--ink);
    font-family: "Aptos", "Segoe UI", sans-serif;
    overflow-y: auto;
  }
  h1, h2 { color: var(--ink); }
  strong { color: var(--accent); }
  code { color: #8f2d1f; }
  pre { font-size: 0.62em; }
  table { font-size: 0.75em; }
--- -->

# Lag en nettside, og gjør den interaktiv

## En nybegynnervennlig vei fra HTML og CSS til DOM

Vite + vanilla TypeScript

<!-- _footer: Lær én interaksjon om gangen -->

---

## Modellen i hodet

En nettside består av tre lag som samarbeider:

| Lag | Oppgave | Ditt første verktøy |
| --- | --- | --- |
| **HTML** | Innhold og struktur | `index.html` |
| **CSS** | Utseende og layout | `src/style.css` |
| **JavaScript** | Oppførsel og data | `src/main.ts` |

**DOM** er nettleserens levende og programmerbare representasjon av HTML-en din.

> Start med en side som fungerer uten JavaScript. Legg til JavaScript når brukeren trenger en interaksjon.

---

## Start et Vite-prosjekt

```bash
npm create vite@latest my-site -- --template vanilla-ts
cd my-site
npm install
npm run dev
```

Vite gir deg en rask utviklingsserver og bygger prosjektet ditt for produksjon.

De viktigste filene:

```text
index.html       sidens inngangspunkt
src/main.ts      oppførsel
src/style.css    stiler
public/          statiske ressurser
```

---

## Hva gjør Vite med `index.html`?

`index.html` er siden nettleseren åpner. Den laster TypeScript-modulen din:

```html
<!doctype html>
<html lang="no">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Min nettside</title>
  </head>
  <body>
    <header>
      <h1>Min første nettside</h1>
    </header>

    <main id="app"></main>

    <script type="module" src="/src/main.ts"></script>
  </body>
</html>
```

Du kan, og bør, redigere denne filen direkte.

---

## La den første versjonen være HTML-fokusert

Hvis innholdet er kjent på forhånd, skriver du semantisk HTML:

```html
<main>
  <section aria-labelledby="welcome-title">
    <h2 id="welcome-title">Velkommen</h2>
    <p>Her er informasjonen besøkeren din trenger.</p>
    <a href="/contact.html">Kontakt oss</a>
  </section>
</main>
```

God HTML gir deg struktur, tilgjengelighet og en nyttig side før du skriver DOM-kode.

**Ikke la TypeScript opprette hvert eneste element som standard.**

---

## Legg styling i CSS

```css
/* src/style.css */
:root {
  font-family: system-ui, sans-serif;
  color: #172121;
  background: #fffaf2;
}

body {
  max-width: 60rem;
  margin: 0 auto;
  padding: 2rem;
}

button {
  border: 0;
  border-radius: 0.4rem;
  padding: 0.7rem 1rem;
  cursor: pointer;
}
```

Importer stilarket én gang:

```ts
// src/main.ts
import './style.css'
```

---

## DOM: finn et element

Nettleseren gjør HTML-en din om til et tre av noder. TypeScript kan finne disse nodene:

```ts
const button = document.querySelector<HTMLButtonElement>('#theme-button')
const heading = document.querySelector<HTMLHeadingElement>('h1')
```

Nyttige selektorer:

```ts
document.querySelector('#app')       // id
 document.querySelector('.card')      // klasse
 document.querySelector('button')      // element
 document.querySelectorAll('li')      // flere elementer
```

`?` i `button?.` betyr: kjør dette bare når elementet finnes.

---

## Første interaksjon: en event listener

HTML:

```html
<button id="theme-button" type="button">Bytt tema</button>
```

TypeScript:

```ts
const button = document.querySelector<HTMLButtonElement>('#theme-button')

button?.addEventListener('click', () => {
  document.body.classList.toggle('dark')
})
```

CSS:

```css
.dark {
  background: #172121;
  color: #fffaf2;
}
```

Mønsteret er:

**finn -> lytt -> endre**

---

## Endre tekst og attributter

Bruk egenskaper når du endrer én verdi:

```ts
const message = document.querySelector<HTMLParagraphElement>('#message')

if (message) {
  message.textContent = 'Takk for at du klikket!'
  message.classList.add('success')
  message.setAttribute('aria-live', 'polite')
}
```

Bruk `textContent` for tekst fra brukeren. Da behandles verdien som tekst i stedet for at den tolkes som HTML.

---

## `innerHTML`: nyttig for små maler

For en liten, kontrollert mal kan en tekststreng være tydelig:

```ts
const app = document.querySelector<HTMLDivElement>('#app')

if (app) {
  app.innerHTML = `
    <h2>Oppgaver</h2>
    <ul>
      <li>Velg et tema</li>
      <li>Skisser siden</li>
      <li>Bygg den første versjonen</li>
    </ul>
  `
}
```

Bruk det med omtanke:

- Flott for markup du selv kontrollerer
- Praktisk når du viser en liste
- Ikke legg ukontrollert input fra brukeren direkte inn i den
- Når du erstatter `innerHTML`, erstatter du også lytterne på barna

---

## `createElement`: presis DOM-oppbygging

Den samme listen kan opprettes med DOM-metoder:

```ts
const list = document.createElement('ul')

for (const task of ['Velg et tema', 'Skisser siden']) {
  const item = document.createElement('li')
  item.textContent = task
  list.append(item)
}

document.querySelector('#app')?.append(list)
```

Dette er mer omfattende, men gjør teksthåndtering og forholdet mellom elementene tydelig.

Bruk det når du trenger detaljert kontroll. Du trenger ikke bruke det på hvert statiske element.

---

## Vis data på siden

En enkel funksjon gjør det lettere å forstå gjentakende grensesnitt:

```ts
type Task = { title: string; done: boolean }

const tasks: Task[] = [
  { title: 'Velg et tema', done: true },
  { title: 'Bygg den første versjonen', done: false },
]

function renderTasks(items: Task[]) {
  const list = document.querySelector<HTMLUListElement>('#task-list')
  if (!list) return

  list.innerHTML = items
    .map((task) => `<li>${task.done ? '[x]' : '[ ]'} ${task.title}</li>`)
    .join('')
}

renderTasks(tasks)
```

Dette eksempelet forutsetter at `task.title` er til å stole på. For tekst skrevet av brukeren, bruk `textContent` eller opprett elementene på en trygg måte.

---

## Skjemaer: lytt etter `submit`

HTML:

```html
<form id="task-form">
  <label for="task-input">Ny oppgave</label>
  <input id="task-input" name="task" required />
  <button type="submit">Legg til oppgave</button>
</form>
```

TypeScript:

```ts
const form = document.querySelector<HTMLFormElement>('#task-form')
const input = document.querySelector<HTMLInputElement>('#task-input')

form?.addEventListener('submit', (event) => {
  event.preventDefault()
  if (!input || input.value.trim() === '') return

  console.log('Ny oppgave:', input.value.trim())
  input.value = ''
})
```

Nettleseren håndterer skjemahendelsen. Koden din bestemmer hva som skjer etterpå.

---

## Hvor hører ekstra sider hjemme?

For en liten nettside med flere sider:

```text
index.html
about.html
contact.html
src/
  main.ts
  style.css
public/
  images/
  downloads/
```

Bruk HTML-filer på rotnivå for sider som Vite skal behandle:

```html
<a href="/about.html">Om oss</a>
```

Bruk `public/` for filer som kopieres uendret og leveres fra roten av nettsiden:

```html
<img src="/images/logo.png" alt="Bedriftens logo" />
```

Én side med flere seksjoner er også helt gyldig. Velg den enkleste strukturen som passer prosjektet.

---

## En nybegynnervennlig arbeidsflyt

1. **Planlegg:** finn brukeren, målet og innholdet.
2. **Strukturer:** skriv semantisk HTML i `index.html`.
3. **Style:** gjør layouten lesbar og responsiv.
4. **Legg til interaksjon:** legg til én event listener i `main.ts`.
5. **Vis data:** bruk en liten funksjon når data endrer seg.
6. **Sjekk:** test tastatur, mobilbredde og tomme tilstander.
7. **Commit:** lagre et lite og meningsfullt steg i Git.

Etter hvert steg kan du oppdatere siden og spørre: *Hva endret seg?*

---

## Feilsøking av DOM

Bruk nettleserens DevTools:

```ts
console.log('main.ts er lastet')
console.log({ button, input })
```

Sjekk dette først:

- Er selektoren skrevet nøyaktig som id-en i HTML-en?
- Er scriptet lastet med `type="module"`?
- Finnes elementet når koden kjører?
- Ble event listener koblet til riktig element?
- Skjuler CSS-endringen?

TypeScript-feil peker vanligvis på en skrivefeil eller en type som ikke passer. Les den første feilen før du endrer koden.

---

## Din første DOM-øvelse

Lag en liten **leseliste**:

- HTML: overskrift, skjema, input, knapp og tom liste
- CSS: god avstand og et mobiltilpasset layout
- DOM: legg til en bok når skjemaet sendes inn
- DOM: marker en bok som lest når den klikkes
- Ekstra: vis antallet bøker

Foreslåtte milepæler:

```text
1. Statisk side
2. Knappen skriver en melding i konsollen
3. Skjemaet leser input.value
4. Ett element vises
5. Elementene kommer fra en array
6. Arrayen og siden holdes synkronisert
```

Ikke legg til et rammeverk før du forstår denne løkken:

**hendelse -> oppdater data -> vis grensesnittet**

---

## Kort oppsummert

- HTML beskriver siden.
- CSS gjør den brukervennlig og uttrykksfull.
- TypeScript legger til oppførsel.
- DOM kobler TypeScript til HTML.
- Start med `querySelector` og `addEventListener`.
- Foretrekk statisk HTML for statisk innhold.
- Bruk `innerHTML` for små, kontrollerte maler.
- Bruk `createElement` når du trenger presis kontroll.
- Bygg én liten interaksjon om gangen.

## Start enkelt. Få det til å fungere. Gjør det bedre etterpå.
