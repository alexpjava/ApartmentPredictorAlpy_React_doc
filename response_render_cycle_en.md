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
    autonumber
    participant U as Usuario/Navegador
    participant R as React (ApartmentList)
    participant S as Estado (useState)
    participant A as Axios (HTTP Client)
    participant B as Backend (Spring Boot)

    Note over U, B: Inicio del ciclo de vida
    U->>R: 1. El componente se monta (Mount)
    R->>S: 2. Inicializa estados (apartments: [], isLoading: true)
    R->>R: 3. Ejecuta useEffect()
    
    Note over R, B: Petición Asíncrona
    R->>A: 4. Llama fetchApartments() -> axios.get("/api/apartment/getAll")
    A->>B: 5. Solicitud HTTP GET a la API
    B->>B: 6. Procesa en @RestController y busca en DB
    B-->>A: 7. Responde con JSON (List<Apartment>)
    
    Note over A, R: Gestión y Parseo
    A-->>R: 8. Axios recibe JSON y lo parsea automáticamente a JS Object
    R->>S: 9. setApartments(apartmentsData) (Línea 15)
    R->>S: 10. setIsLoading(false) (Línea 16)
    
    Note over R, U: Re-renderizado Final
    S-->>R: 11. El cambio de estado dispara un nuevo renderizado
    R->>U: 12. Mapea la lista (apartments.map) y muestra el HTML final
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
