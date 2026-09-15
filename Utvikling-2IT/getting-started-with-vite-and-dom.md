---
marp: true
title: Getting Started with Vite and the DOM
author: Web development foundations
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
---

# Build a site, then make it interactive

## A beginner's path from HTML and CSS into the DOM

Vite + vanilla TypeScript

<!-- _footer: Learn one interaction at a time -->

---

## The mental model

A web page is three layers working together:

| Layer | Job | Your first tool |
| --- | --- | --- |
| **HTML** | Content and structure | `index.html` |
| **CSS** | Appearance and layout | `src/style.css` |
| **JavaScript** | Behavior and data | `src/main.ts` |

The **DOM** is the browser's live, programmable representation of your HTML.

> Start with a page that works without JavaScript. Add JavaScript when the user needs an interaction.

---

## Start a Vite project

```bash
npm create vite@latest my-site -- --template vanilla-ts
cd my-site
npm install
npm run dev
```

Vite gives you a fast development server and builds your project for production.

The important files:

```text
index.html       page entry point
src/main.ts      behavior
src/style.css    styles
public/          static assets
```

---

## What Vite does with `index.html`

`index.html` is the page the browser opens. It loads your TypeScript module:

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>My site</title>
  </head>
  <body>
    <header>
      <h1>My first site</h1>
    </header>

    <main id="app"></main>

    <script type="module" src="/src/main.ts"></script>
  </body>
</html>
```

You can and should edit this file directly.

---

## Keep the first version HTML-first

If content is known in advance, write semantic HTML:

```html
<main>
  <section aria-labelledby="welcome-title">
    <h2 id="welcome-title">Welcome</h2>
    <p>Here is the information your visitor needs.</p>
    <a href="/contact.html">Contact us</a>
  </section>
</main>
```

Good HTML gives you structure, accessibility, and a useful page before you write any DOM code.

**Do not make TypeScript create every element by default.**

---

## Put styling in CSS

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

Then import the stylesheet once:

```ts
// src/main.ts
import './style.css'
```

---

## The DOM: find an element

The browser turns your HTML into a tree of nodes. TypeScript can find those nodes:

```ts
const button = document.querySelector<HTMLButtonElement>('#theme-button')
const heading = document.querySelector<HTMLHeadingElement>('h1')
```

Useful selectors:

```ts
document.querySelector('#app')       // id
 document.querySelector('.card')      // class
 document.querySelector('button')      // element
 document.querySelectorAll('li')      // many elements
```

The `?` in `button?.` means: run this only when the element exists.

---

## First interaction: an event listener

HTML:

```html
<button id="theme-button" type="button">Change theme</button>
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

The pattern is:

**find -> listen -> change**

---

## Change text and attributes

Prefer properties when you are changing one value:

```ts
const message = document.querySelector<HTMLParagraphElement>('#message')

if (message) {
  message.textContent = 'Thanks for clicking!'
  message.classList.add('success')
  message.setAttribute('aria-live', 'polite')
}
```

Use `textContent` for user-provided text. It treats the value as text instead of interpreting it as HTML.

---

## `innerHTML`: useful for small templates

For a small, controlled template, a string can be clear:

```ts
const app = document.querySelector<HTMLDivElement>('#app')

if (app) {
  app.innerHTML = `
    <h2>Tasks</h2>
    <ul>
      <li>Choose a topic</li>
      <li>Sketch the page</li>
      <li>Build the first version</li>
    </ul>
  `
}
```

Use it carefully:

- Great for markup you control
- Convenient when rendering a list
- Do not place untrusted user input directly inside it
- Replacing `innerHTML` also replaces listeners on those children

---

## `createElement`: precise DOM construction

The same list can be created with DOM methods:

```ts
const list = document.createElement('ul')

for (const task of ['Choose a topic', 'Sketch the page']) {
  const item = document.createElement('li')
  item.textContent = task
  list.append(item)
}

document.querySelector('#app')?.append(list)
```

This is more verbose, but it makes text handling and element relationships explicit.

Use it when you need fine-grained control. You do not need to use it for every static element.

---

## Render from data

A simple function makes repeated UI easier to understand:

```ts
type Task = { title: string; done: boolean }

const tasks: Task[] = [
  { title: 'Choose a topic', done: true },
  { title: 'Build the first version', done: false },
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

This example assumes `task.title` is trusted. For user-entered text, use `textContent` or create elements safely.

---

## Forms: listen for `submit`

HTML:

```html
<form id="task-form">
  <label for="task-input">New task</label>
  <input id="task-input" name="task" required />
  <button type="submit">Add task</button>
</form>
```

TypeScript:

```ts
const form = document.querySelector<HTMLFormElement>('#task-form')
const input = document.querySelector<HTMLInputElement>('#task-input')

form?.addEventListener('submit', (event) => {
  event.preventDefault()
  if (!input || input.value.trim() === '') return

  console.log('New task:', input.value.trim())
  input.value = ''
})
```

The browser handles the form event. Your code decides what happens next.

---

## Where do extra pages belong?

For a small multi-page site:

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

Use root-level HTML files for pages that Vite should process:

```html
<a href="/about.html">About</a>
```

Use `public/` for files copied as-is and served from the site root:

```html
<img src="/images/logo.png" alt="Company logo" />
```

A single page with several sections is also completely valid. Choose the simplest structure that matches the project.

---

## A beginner-friendly workflow

1. **Plan:** identify the user, goal, and content.
2. **Structure:** write semantic HTML in `index.html`.
3. **Style:** make the layout readable and responsive.
4. **Interact:** add one event listener in `main.ts`.
5. **Render:** use a small function when data changes.
6. **Check:** test keyboard, mobile width, and empty states.
7. **Commit:** save a small, meaningful step in Git.

After each step, refresh the page and ask: *What changed?*

---

## Debugging the DOM

Use the browser's DevTools:

```ts
console.log('main.ts loaded')
console.log({ button, input })
```

Check these first:

- Is the selector spelled exactly like the HTML id?
- Is the script loaded as `type="module"`?
- Does the element exist when the code runs?
- Did the event listener attach to the right element?
- Is CSS hiding the change?

TypeScript errors usually point to a typo or a mismatched type. Read the first error before changing code.

---

## Your first DOM exercise

Build a small **reading list**:

- HTML: heading, form, input, button, empty list
- CSS: readable spacing and a mobile layout
- DOM: add a book when the form submits
- DOM: mark a book as read when clicked
- Stretch: show the number of books

Suggested milestones:

```text
1. Static page
2. Button logs a message
3. Form reads input.value
4. One item appears
5. Items come from an array
6. The array and screen stay in sync
```

Do not add a framework until you understand this loop:

**event -> update data -> render UI**

---

## The short version

- HTML describes the page.
- CSS makes it usable and expressive.
- TypeScript adds behavior.
- The DOM connects TypeScript to HTML.
- Start with `querySelector` and `addEventListener`.
- Prefer static HTML for static content.
- Use `innerHTML` for small controlled templates.
- Use `createElement` when you need precise control.
- Build one small interaction at a time.

## Start small. Make it work. Then make it better.
