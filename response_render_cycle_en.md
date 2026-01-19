# Data Management Documentation: `ApartmentList` Component

This document describes the information flow, from the asynchronous request to the final render in the React component, explaining how Hooks and the Axios library interact.

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
    
    Note over Parser: Business Logic:<br/>1. Date formatting<br/>2. Property name mapping<br/>3. Null cleanup
    Parser-->>Hook: Returns clean/parsed object
    
    Hook->>UI: updateState(parsedData)
    Hook->>UI: setStatus('success')
```

---

## 2. Apartment Data Sequence Diagram

The following diagram illustrates the data lifecycle:

```mermaid
sequenceDiagram
    participant Browser as Browser (DOM)
    participant Component as ApartmentList (React)
    participant Effect as useEffect Hook
    participant Axios as Axios (HTTP Client)
    participant API as API Server (/getAll)

    Note over Component: Initial Render
    Component->>Browser: Renders "Loading..."
    Component->>Effect: Triggers effect (deps: [])
    
    rect rgb(240, 248, 255)
    Note right of Effect: Asynchronous Request Phase
    Effect->>Axios: get('/api/apartment/getAll')
    Axios->>API: GET Request
    API-->>Axios: Returns JSON (Raw Data)
    Axios-->>Effect: Returns response.data (JS Object)
    end

    Note over Effect: Data Handling Phase
    Effect->>Effect: Assignment to 'apartmentsData'
    
    Note over Effect: State Update
    Effect->>Component: setApartments(apartmentsData)
    Effect->>Component: setIsLoading(false)
    
    Note over Component: Second Render (Re-render)
    Component->>Browser: Maps 'apartments' to <li> list
```

---

## 3. Technical Flow Analysis

### A. State Initialization (`useState`)

Three critical states are defined to manage the data lifecycle:

* **`apartments`**: Initialized as an empty array `[]`. This is the final container for the data.
* **`isLoading`**: A boolean that controls the user experience (UX) while data is being fetched over the network.
* **`isAxiosError`**: A flag used for exception handling.

---

### B. Effect Trigger (`useEffect`)

By using an empty dependency array `[]`, the data flow starts exactly once, immediately after the component is mounted to the DOM.

---

### C. Automatic Consumption and Parsing (Axios)

When the API responds, Axios performs the first level of parsing internally:

1. It receives the response body as plain text (JSON).
2. It automatically transforms it into a JavaScript Object Literal.
3. It exposes it through the `response.data` variable.

---

### D. Data Transformation (Data Mapping)

At the line:

```js
const apartmentsData = response.data;
```

the data is ready to be manipulated.

> **Note:** This is the recommended point to apply cleanup or transformation functions if the API property names (e.g. `prefarea`) do not match the desired UI naming conventions (e.g. `isPreferredArea`).

---

### E. Re-rendering and Projection

When `setApartments(apartmentsData)` is executed, React detects a state change and triggers a re-render. During this process:

* The component function runs again.
* The `{isLoading ? ... : ...}` condition changes to display the list.
* The `.map()` method iterates over the new data array, projecting each object into an `<li>` element in the DOM.
