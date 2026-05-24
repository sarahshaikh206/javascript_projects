# Project 2 — To-Do List

**Tech Stack:** HTML · CSS · Vanilla JavaScript  
**Status:** ✅ Complete  
**File:** [index.html](./index.html)

---

## What It Does

An interactive task manager where you can add tasks, mark them as complete (strike-through toggle), and delete them. Built entirely with vanilla JavaScript — no libraries or frameworks.

**Live features:**
- Add a task by typing and clicking Add (or pressing Enter)
- Click a task to toggle it as complete (strike-through)
- Delete individual tasks with the Delete button
- Clean, minimal UI

---

## Concepts Applied

### 1. Dynamic Element Creation
Instead of hardcoding list items in HTML, every task is built programmatically in JavaScript using `createElement`.

```javascript
const li = document.createElement("li");
const span = document.createElement("span");
const delBtn = document.createElement("button");

li.appendChild(span);
li.appendChild(delBtn);
document.getElementById("taskList").appendChild(li);
```

**Why this matters:** This is how real applications work — the DOM is built dynamically from data, not written by hand. It's the same concept behind React components, just at the vanilla JS level.

---

### 2. Event Listeners — Click to Toggle Complete

```javascript
span.onclick = function () {
  li.classList.toggle("done");
};
```

`classList.toggle("done")` — adds the class if absent, removes it if present. The CSS class `done` applies `text-decoration: line-through`, making the task appear struck through.

**One line, two states** — this is a common pattern for toggle-based UI interactions.

---

### 3. Event Propagation — stopPropagation()

```javascript
delBtn.onclick = function (e) {
  e.stopPropagation(); // Prevent the click from bubbling up to the li
  li.remove();
};
```

Without `stopPropagation()`, clicking Delete would also fire the `li`'s click handler (toggling complete *and* deleting). This is a subtle but important DOM event concept — **event bubbling** means a click on a child element propagates up through all parent elements.

---

### 4. Keyboard Event — Enter to Submit

```javascript
document.getElementById("taskInput").addEventListener("keypress", function (e) {
  if (e.key === "Enter") addTask();
});
```

Improves UX — users can press Enter instead of clicking the Add button. A small detail that makes the app feel more polished.

---

### 5. Input Validation

```javascript
const taskText = input.value.trim();
if (taskText === "") return;
```

`trim()` removes leading/trailing whitespace — prevents blank or whitespace-only tasks from being added. `return` exits the function early if validation fails.

---

### 6. DOM Removal

```javascript
li.remove();
```

The simplest way to remove an element from the DOM entirely. No need to find the parent and call `removeChild` — `element.remove()` handles it directly.

---

## Data Flow

```
User types task → clicks Add (or presses Enter)
      ↓
addTask() runs → validates input
      ↓
createElement() builds li + span + button
      ↓
Event listeners attached to span (toggle) and button (delete)
      ↓
li appended to taskList → visible in UI
      ↓
Click task → classList.toggle("done") → strike-through applied/removed
Click Delete → e.stopPropagation() → li.remove() → gone from DOM
```

---

## What I Learned

- The difference between **innerHTML** (CRUD app) and **createElement** (To-Do) — createElement is safer and more reusable for building elements that need event listeners
- **Event bubbling** and why `stopPropagation()` matters when nesting clickable elements
- `classList.toggle()` as a clean pattern for binary state UI
- Keyboard event handling for better UX (`keypress` + `e.key`)
- Why `trim()` is always needed before processing user input

---

## Difference vs CRUD App

| Feature | CRUD App | To-Do List |
|---------|----------|-----------|
| Rendering method | `innerHTML` string template | `createElement` |
| Data persistence | `localStorage` | In-memory only (resets on refresh) |
| State tracking | Array + hidden input | DOM is the state |
| Complexity | Higher (edit mode, persistence) | Lower (focused on interactions) |

---

## Possible Improvements

- Add `localStorage` persistence so tasks survive page refresh
- Add task categories or priority levels
- Add a "Clear All Completed" button
- Add drag-and-drop reordering
- Add due dates with overdue highlighting
