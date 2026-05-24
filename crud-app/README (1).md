# Project 1 — Simple CRUD App

**Tech Stack:** HTML · CSS · Vanilla JavaScript · localStorage API  
**Status:** ✅ Complete  
**File:** [index.html](./index.html)

---

## What It Does

A fully functional **Create, Read, Update, Delete (CRUD)** application that manages a list of users — each with a name and email. All data persists in the browser via `localStorage`, meaning records survive page refreshes.

**Live features:**
- Add a new user (name + email)
- View all users in a table
- Edit an existing user inline
- Delete a user with a confirmation prompt
- Data persists across page refreshes via localStorage

---

## Concepts Applied

### 1. DOM Manipulation
Dynamically building and re-rendering the HTML table from a JavaScript array using `innerHTML`.

```javascript
users.forEach((user, index) => {
  table.innerHTML += `
    <tr>
      <td>${user.name}</td>
      <td>${user.email}</td>
      <td>
        <button onclick="editUser(${index})">Edit</button>
        <button onclick="deleteUser(${index})">Delete</button>
      </td>
    </tr>`;
});
```

**Why it matters:** The table never exists in the HTML source — it is built entirely by JavaScript at runtime. This is the foundation of dynamic UI rendering.

---

### 2. localStorage Persistence
Browser storage that keeps data alive after page reload — no backend or database required.

```javascript
// Save
localStorage.setItem("users", JSON.stringify(users));

// Load
let users = JSON.parse(localStorage.getItem("users")) || [];
```

**`JSON.stringify`** — converts the JavaScript array to a string for storage  
**`JSON.parse`** — converts it back to an array when retrieved  
**`|| []`** — fallback to empty array if nothing is stored yet

---

### 3. Create (Add New User)

```javascript
if (index === "") {
  users.push(user); // Append to array
}
```

Checks if `editIndex` is empty (no edit in progress) → pushes the new user object to the array → saves to localStorage → re-renders the table.

---

### 4. Read (Render Table)

The `renderUsers()` function loops through the `users` array and builds the table fresh each time — called after every Create, Update, or Delete operation to keep the UI in sync with the data.

---

### 5. Update (Edit Existing User)

```javascript
function editUser(index) {
  const user = users[index];
  document.getElementById("name").value = user.name;
  document.getElementById("email").value = user.email;
  document.getElementById("editIndex").value = index; // Track which record is being edited
}
```

Pre-fills the input fields with the user's existing data and stores the array index in a hidden field. When Save is clicked, the hidden index tells `saveUser()` to update rather than create.

---

### 6. Delete

```javascript
function deleteUser(index) {
  if (confirm("Are you sure?")) {
    users.splice(index, 1); // Remove 1 element at position index
    localStorage.setItem("users", JSON.stringify(users));
    renderUsers();
  }
}
```

`Array.splice(index, 1)` removes exactly one element at the given position. The confirm dialog prevents accidental deletions.

---

## Data Flow

```
User fills form → saveUser() runs
      ↓
New user? → push to array → save to localStorage → renderUsers()
Edit user? → replace at index → save to localStorage → renderUsers()
      ↓
renderUsers() rebuilds the table from the array
```

---

## What I Learned

- How CRUD maps to real data operations (Create → push, Read → render, Update → splice+replace, Delete → splice)
- Why `JSON.stringify` / `JSON.parse` are needed for localStorage (it only stores strings)
- How a hidden input field can track state without a backend
- The pattern of: **update data → save → re-render** is the foundation of all frontend state management

---

## Possible Improvements

- Add input validation (email format check, duplicate detection)
- Add CSS transitions for smoother row additions/removals
- Add search/filter functionality
- Replace localStorage with a backend API (Node.js + Express)
