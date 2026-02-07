# Project Notes

## General Setup

```bash
npm install
```

---

## How to Set Up Routing

In short:

1. Install the router  
2. Modify `main.jsx` to power it on  
3. Wrap `App.jsx` so routes can be defined  

```bash
npm install react-router-dom
npm run dev
```

---

## What Is the Function of the `main.jsx` File?

Think of `main.jsx` as the **ON / OFF power switch** of a React app.

**Analogy:**

- Your app (`App.jsx`) → the device  
- The browser page (`index.html`) → the wall socket  
- `main.jsx` → pressing the power button  

The code below **turns ON routing for the entire app**:

```jsx
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App";
import "./index.css";

import { BrowserRouter } from "react-router-dom";

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </React.StrictMode>
);
```

---

## What Is This?

```js
import { Routes, Route } from "react-router-dom";
```

`Routes` (plural) and `Route` (singular) are **traffic signs for your app**.

They decide **what screen shows up for which URL**.

---

### Ultra-Simple Analogy

Imagine your app is a building with rooms:

- The address bar (URL) tells you where you are  
- `Routes` is the **floor map**  
- `Route` is a **sign on each door**

The import line is saying:

> “I want to use the routing tools that let my app change screens **without** reloading the page.”

---

### Why This Matters

**Without routing:**
- One single screen forever  
- Navigation reloads the page like a 1999 website  

**With routing:**
- URL changes  
- Screen changes  
- App stays alive and fast  

**One-line summary:**

- `Routes` = the rulebook  
- `Route` = one rule inside it  

---

## How Do You Establish Routes?

See this `App.jsx` snippet:

```jsx
<Routes>
  <Route path="/" element={<HomePage />} />
  <Route path="/students/:studentId" element={<StudentDetailsPage />} />
  <Route path="/profile" element={<UserProfilePage />} />
</Routes>
```

---

### The Whole Thing in One Sentence

> Depending on the URL, show a different page — without reloading the site.

---

## Line by Line (Plain English)

### `<Routes>`

Think of this as:

> “Pay attention to the address bar and decide what page to show.”

`<Routes>` **listens to the browser URL** and decides what to render.

---

## What Actually Connects to the Browser URL

- **BrowserRouter** → talks to the browser’s address bar  
- **Routes** → reads that information  
- **Route** → matches a rule  

```
Browser URL → BrowserRouter → Routes → Route → Page
```

---

## Why Is the Navbar NOT Routed?

**Short answer:**  
React Router renders **components**, not whole pages.

---

### The Key Mental Model

```
[ Navbar ]   ← stays the same
-------------------------
[ Page Area ] ← this changes
```

React Router is responsible **only for the Page Area**.

---

## Final Analogy

Imagine a TV:

- TV frame + buttons → Navbar  
- Channel content → Routed components  

Changing the channel:
- Does not rebuild the TV  
- Only changes what’s on the screen  

**React Router changes the channel, not the TV.**

---

## App overview

```mermaid
graph LR
    A["👥 Student List<br/>(HomePage)"] -->|Click a Student| B["📄 Student Details<br/>(StudentDetailsPage)"]
    C["🏠 Home Button<br/>(Navbar)"] -->|Click| A
    D["👤 Profile Button<br/>(Navbar)"] -->|Click| E["👨‍🏫 My Profile<br/>(UserProfilePage)"]
    
    style A fill:#2d3748,stroke:#4a5568,stroke-width:2px,color:#e2e8f0
    style B fill:#1e3a1f,stroke:#2f5233,stroke-width:2px,color:#c6f6d5
    style C fill:#2d3748,stroke:#4a5568,stroke-width:2px,color:#e2e8f0
    style D fill:#2d3748,stroke:#4a5568,stroke-width:2px,color:#e2e8f0
    style E fill:#3f2f5e,stroke:#553399,stroke-width:2px,color:#dcd2e8
```




---

## App Data Flow Diagram (Rendering & Props)

```mermaid
graph TD
    A["students.json<br/>(Static Data)"]
    
    B["main.jsx<br/>(Entry Point)<br/>renders: Router"]
    
    B -->|wraps| C["App.jsx<br/>(renders: Navbar + Routes)"]
    
    C -->|renders| D["Navbar.jsx<br/>(renders: Home/Profile Links)"]
    
    C -->|renders| E["Routes Component<br/>(decides which page)"]
    
    A -->|imported| F["HomePage.jsx"]
    F -->|useState<br/>stores students| G["students state"]
    F -->|renders| H["StudentCard Component"]
    F -->|passes props:<br/>_id, firstName, lastName,<br/>email, phone, program, image| H
    
    H -->|Link element<br/>to /students/:id| I["Browser URL<br/>Changes"]
    
    I -->|triggers| E
    E -->|matches route<br/>/students/:id| J["StudentDetailsPage<br/>(renders: Student details)"]
    
    J -->|useParams<br/>extracts :studentId| K["studentId from URL"]
    A -->|imported into| J
    J -->|searches array<br/>for matching student| L["Found Student<br/>Object"]
    J -->|renders| M["Student Profile<br/>Display"]
    
    D -->|Link to /| F
    D -->|Link to /profile| N["UserProfilePage<br/>(renders: User info)"]
    
    style A fill:#2d3748,stroke:#4a5568,stroke-width:2px,color:#e2e8f0
    style B fill:#1a202c,stroke:#4a5568,stroke-width:2px,color:#cbd5e0
    style C fill:#2d3748,stroke:#4a5568,stroke-width:2px,color:#e2e8f0
    style D fill:#2d3748,stroke:#4a5568,stroke-width:2px,color:#e2e8f0
    style E fill:#2d3748,stroke:#4a5568,stroke-width:2px,color:#e2e8f0
    style F fill:#2d3748,stroke:#4a5568,stroke-width:2px,color:#e2e8f0
    style H fill:#1e3a1f,stroke:#2f5233,stroke-width:2px,color:#c6f6d5
    style J fill:#1e3a1f,stroke:#2f5233,stroke-width:2px,color:#c6f6d5
    style M fill:#0f172a,stroke:#1e293b,stroke-width:2px,color:#cbd5e0
    style N fill:#3f2f5e,stroke:#553399,stroke-width:2px,color:#dcd2e8
    style G fill:#2d3e5f,stroke:#4a5a7f,stroke-width:2px,color:#b8cce4
    style L fill:#0f3a1f,stroke:#1e5a2f,stroke-width:2px,color:#a8e6c1
    style I fill:#2d3748,stroke:#4a5568,stroke-width:2px,color:#e2e8f0
    style K fill:#2d3748,stroke:#4a5568,stroke-width:2px,color:#e2e8f0
```

