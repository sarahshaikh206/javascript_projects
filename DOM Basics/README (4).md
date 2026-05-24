# Project 3 — DOM Basics

**Tech Stack:** HTML · CSS · Vanilla JavaScript · DOM API  
**Status:** ✅ Complete  
**File:** [index.html](./index.html)

---

## What It Does

A hands-on demonstration of core **Document Object Model (DOM)** operations using vanilla JavaScript. The project covers the four fundamental DOM skills every frontend developer needs — accessing elements, manipulating content, changing styles, and creating/removing elements dynamically.

**Live features:**
- Greet a user by name — reading input and updating text content
- Change the colour of a box — inline style manipulation
- Add a paragraph to the page dynamically — createElement + appendChild
- Remove that paragraph — element.remove()

---

## What is the DOM?

The DOM (Document Object Model) is the browser's representation of an HTML page as a tree of objects. JavaScript can read and manipulate this tree — changing text, styles, structure, and responding to user events — without reloading the page.

```
document
  └── html
       ├── head
       └── body
            ├── h1#mainHeading
            ├── input#nameInput
            ├── button#greetBtn
            ├── p#greeting
            ├── div#box
            └── div#paraContainer
```

Every element in this tree is a **node** that JavaScript can access, modify, create, or delete.

---

## Concepts Applied

### 1. Accessing Elements — getElementById

```javascript
const heading = document.getElementById("mainHeading");
```

`document.getElementById()` — finds and returns the element with the matching `id`. This is the most direct way to target a specific element.

**Other selection methods:**
| Method | Selects |
|--------|---------|
| `getElementById("id")` | Single element by ID |
| `querySelector(".class")` | First match by CSS selector |
| `querySelectorAll("p")` | All matches — returns a NodeList |
| `getElementsByClassName("name")` | All elements with that class |

---

### 2. Reading Input + Updating Text Content

```javascript
document.getElementById("greetBtn").addEventListener("click", function () {
  const name = document.getElementById("nameInput").value.trim();
  document.getElementById("greeting").textContent = "Hello, " + name + "!";
});
```

**`.value`** — reads the current text inside an input field  
**`.textContent`** — sets the text inside any element (safe — does not parse HTML)  
**`.trim()`** — strips leading/trailing whitespace from the input

**`addEventListener("click", function)`** — attaches an event listener to the button. When clicked, the callback function runs.

> `textContent` vs `innerHTML` — use `textContent` when setting plain text (safer, prevents XSS). Use `innerHTML` only when you need to insert actual HTML markup.

---

### 3. Style Manipulation

```javascript
document.getElementById("colorBtn").addEventListener("click", function () {
  document.getElementById("box").style.backgroundColor = "skyblue";
});
```

Every DOM element has a `.style` property that maps directly to CSS properties — written in camelCase instead of kebab-case:

| CSS | JavaScript |
|-----|-----------|
| `background-color` | `style.backgroundColor` |
| `font-size` | `style.fontSize` |
| `border-radius` | `style.borderRadius` |
| `display` | `style.display` |

The `transition: background-color 0.3s ease` in CSS means the colour change animates smoothly rather than snapping instantly.

---

### 4. Creating Elements — createElement + appendChild

```javascript
document.getElementById("addParaBtn").addEventListener("click", function () {
  const p = document.createElement("p");   // Create a new <p> element (not yet in the DOM)
  p.textContent = "I am a new paragraph."; // Set its text
  p.id = "newPara";                         // Give it an ID so it can be found later
  document.getElementById("paraContainer").appendChild(p); // Insert into the DOM
});
```

**Step by step:**
1. `createElement("p")` — creates a `<p>` element in memory, not yet visible
2. `.textContent` — gives it content
3. `.id` — assigns an ID so the remove function can find it
4. `appendChild()` — inserts it as the last child of `paraContainer` — now it appears on the page

---

### 5. Removing Elements — element.remove()

```javascript
document.getElementById("removeParaBtn").addEventListener("click", function () {
  const p = document.getElementById("newPara");
  if (p) p.remove(); // Guard: only remove if the element exists
});
```

`if (p)` — defensive check. If the paragraph hasn't been added yet (or was already removed), `getElementById` returns `null`. Calling `.remove()` on `null` would throw an error — the guard prevents that.

`element.remove()` — removes the element from the DOM entirely.

---

### 6. Keyboard Event — Enter to Greet

```javascript
document.getElementById("nameInput").addEventListener("keypress", function (e) {
  if (e.key === "Enter") document.getElementById("greetBtn").click();
});
```

Listening for the `keypress` event on the input and checking `e.key === "Enter"` — programmatically triggers the button click when the user presses Enter. Improves UX without duplicating logic.

---

## Data Flow

```
User types name → clicks Greet (or presses Enter)
      ↓
Event listener fires → reads input.value → sets greeting.textContent
      ↓
User clicks Change Color
      ↓
Event listener fires → box.style.backgroundColor = "skyblue"
      ↓
User clicks Add Paragraph
      ↓
createElement("p") → set content + id → appendChild to paraContainer
      ↓
User clicks Remove Paragraph
      ↓
getElementById("newPara") → if exists → .remove()
```

---

## What I Learned

- The DOM is the live representation of the page — JavaScript changes to it update the UI instantly
- `addEventListener` is the correct way to attach events (vs inline `onclick`) — keeps JS and HTML separate
- `textContent` vs `innerHTML` — always prefer `textContent` for plain text (security)
- `createElement` + `appendChild` is the safe, reusable pattern for dynamic UI
- Null-guarding (`if (p)`) prevents runtime errors when elements may not exist
- CSS `transition` works automatically on JavaScript style changes — no extra JS needed

---

## Comparison Across Projects

| Concept | CRUD App | To-Do List | DOM Basics |
|---------|----------|-----------|------------|
| Element access | `getElementById` | `getElementById` | `getElementById` |
| Rendering method | `innerHTML` template | `createElement` | `createElement` |
| Events | `onclick` inline | `addEventListener` | `addEventListener` |
| Style changes | CSS classes | CSS classes | `element.style` |
| Element removal | `splice` + re-render | `element.remove()` | `element.remove()` |
| Persistence | `localStorage` | None | None |

---

## Possible Improvements

- Add a colour picker instead of a hardcoded colour
- Allow multiple paragraphs to be added and removed individually
- Add a button to reset the heading text back to its original value
- Add `classList.add` / `classList.remove` to demonstrate class-based style toggling
