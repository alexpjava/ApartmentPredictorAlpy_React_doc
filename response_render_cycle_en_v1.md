# Data Management Documentation: `ApartmentList` Component

This document details the information flow, from the asynchronous request to the final rendering in the React component, explaining how Hooks and the Axios library interact.

## 1. React Data Flow Diagram

```mermaid
sequenceDiagram
    participant UI as Component (UI)
    participant Hook as Custom Hook / useEffect
    participant Axios as Axios Client
    participant API as External API
    participant Parser as Transformer / Parser

    Note over UI, API: The process starts when the component mounts or due to an event
    UI->>Hook: Invokes action (e.g. fetchData)
    Hook->>Hook: setStatus('loading')
    
    Hook->>Axios: get('/endpoint')
    Axios->>API: HTTP Request (JSON)
    API-->>Axios: HTTP Response (Raw JSON)
    
    Note right of Axios: Interceptors (optional)
    Axios->>Parser: Sends raw data (response.data)
    
    Note over Parser: Business Logic:<br/>1. Date formatting<br/>2. Name mapping<br/>3. Null cleanup
    Parser-->>Hook: Returns Clean/Parsed Object
    
    Hook->>UI: updateState(parsedData)
    Hook->>UI: setStatus('success')
```

## 2. Apartment Data Sequence Diagram

The following diagram illustrates the data lifecycle:

```mermaid
sequenceDiagram
    autonumber
    participant U as User/Browser
    participant R as React (ApartmentList)
    participant S as State (useState)
    participant A as Axios (HTTP Client)
    participant B as Backend (Spring Boot)

    Note over U, B: Lifecycle start
    U->>R: 1. Component mounts
    R->>S: 2. Initializes state (apartments: [], isLoading: true)
    R->>R: 3. Executes useEffect()
    
    Note over R, B: Asynchronous Request
    R->>A: 4. Calls fetchApartments() -> axios.get("/api/apartment/getAll")
    A->>B: 5. HTTP GET request to the API
    B->>B: 6. Processes request in @RestController and queries the DB
    B-->>A: 7. Responds with JSON (List<Apartment>)
    
    Note over A, R: Handling and Parsing
    A-->>R: 8. Axios receives JSON and automatically parses it into a JS Object
    R->>S: 9. setApartments(apartmentsData) (Line 15)
    R->>S: 10. setIsLoading(false) (Line 16)
    
    Note over R, U: Final Re-render
    S-->>R: 11. State change triggers a new render
    R->>U: 12. Maps the list (apartments.map) and displays the final HTML
```

## 3. Detailed Step-by-Step Explanation

### 1. Initialization and Mounting

When the **`ApartmentList`** component loads, the first thing that happens is the initialization of the Hooks.

* **Lines 5–7**: The initial states are defined.
  `isLoading` starts as `true`, so the user initially sees the **"Loading..."** message on the screen.

---

### 2. Effect Trigger (`useEffect`)

* **Line 9**: The `useEffect` with an empty dependency array `[]` tells React:
  *"Run this code only once, right after the component appears on the screen."*

* **Line 10**: The asynchronous function `fetchApartments()` is defined and immediately invoked.

---

### 3. The HTTP Request (Axios)

* **Line 12**:

  ```js
  axios.get("/api/apartment/getAll")
  ```

  sends a request to the Spring Boot server.

> **Important:** Axios automatically parses the response when the backend returns JSON.
> There is no need to use `JSON.parse()`, as the result is already a JavaScript object accessible via `response.data`.

---

### 4. Backend Handling (Spring Boot)

Although the Java code is not shown, the flow assumes that:

* A **`@RestController`** receives the request at the corresponding endpoint.
* A **Repository** retrieves the data from the database.
* Spring Boot serializes Java objects into JSON format using the **Jackson** library.

---

### 5. State Update and Re-rendering

* **Line 15**:

  ```js
  setApartments(apartmentsData)
  ```

  stores the actual data in the component state.

* **Line 16**:

  ```js
  setIsLoading(false)
  ```

  switches off the loading indicator.

**The trigger:**
In React, every time a state setter (`set...`) is executed, the component automatically re-renders.

---

### 6. Data Transformation to UI (`.map()`)

* **Line 33**: Since `isLoading` is now `false`, React executes the main render block.

* **Line 35**:

  ```js
  apartments.map(...)
  ```

  acts as the rendering engine, iterating over each object in the array and transforming it into an `<li>` element.

* **Line 36**:

  ```jsx
  key={apartment.id}
  ```

  allows React to efficiently track each element when the list changes.

---

### A Key Point About “Parsing”

In this code, the data travels through the following formats:

**DB (SQL/NoSQL)** → **Java Object (POJO)** → **JSON (Text)** → **JS Object** → **DOM (HTML)**

This flow ensures a clear separation between persistence, business logic, data transport, and UI presentation.
