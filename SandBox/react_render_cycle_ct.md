# React Rendering Process: Render and Commit Phases

This document summarizes the internal flow of React from the moment an update is requested until the user sees the changes on the screen.

---

## 1. Trigger Phase (The "Request")
The process starts with a **Trigger**, which notifies React that a component needs to be updated.
* **Initiators:** Usually caused by a state update (`useState` / `useReducer` setters) or changes in `props`.
* **Nature:** This phase is about scheduling the work. React keeps track of which components need to re-render.

## 2. Render Phase (The "Calculation")
In this phase, React determines what has changed. It is **pure** and has no visible side effects.
* **Component Execution:** React calls your function component to determine what the UI should look like based on current state and props.
* **Virtual DOM & Reconciliation:** React generates a new Virtual DOM tree and compares it with the previous one (an algorithm called **Diffing**).
* **Result:** A list of minimal changes (additions, updates, deletions) required to sync the UI.

## 3. Commit Phase (The "Update")
React applies the calculated changes to the browser's Real DOM.
* **Efficiency:** React only modifies the specific DOM nodes that changed. If the output of the Render phase is the same as before, React does nothing here.
* **Synchronous:** This phase is usually synchronous to ensure the UI stays consistent.

## 4. Browser Paint
Once the Real DOM is updated, the browser takes over:
* The browser re-calculates the layout and **paints** the pixels on the screen.
* The user finally sees the updated interface.

## 5. Effects Phase (Post-Paint)
After the browser has painted the screen, React executes side effects.
* **`useEffect`:** These run **asynchronously** after the paint to avoid blocking the UI.
* **Note:** If an effect triggers a new state update, the entire cycle starts again from the **Trigger** phase.

---

### Key Concepts to Remember
* **Render != Painting:** "Rendering" is React calling your function; "Painting" is the browser drawing pixels.
* **Immutability:** React relies on state snapshots. Each render sees a fixed version of state at that point in time.
* **Declarative UI:** You describe *what* the UI should look like (JSX), and React handles the *how* (Commit/Paint).

---

# React Lifecycle: The Render and Commit Phases

```mermaid
flowchart RL
 subgraph Fase_Render["Render Phase - Asíncrona / Pura"]
        B1("Transformació JSX")
        B["<b>2. RENDER</b><br>Fase de Renderitzat"]
        B2("Creació Virtual DOM")
        B3("Generació Snapshot")
  end
 subgraph Fase_Reconcile["Càlcul de Diferències"]
        C1("Comparació Nou vs Vell VDOM")
        C["<b>3. RECONCILIATION</b><br>Algoritme de Diffing"]
        C2("Identificació de canvis mínims")
  end
 subgraph Fase_Commit["Commit Phase - Síncrona"]
        D1("Actualització nodes DOM")
        D["<b>4. COMMIT</b><br>Aplicació al DOM Real"]
        D2("Només canvis necessaris")
  end
    A@{ label: "<b>1. TRIGGER</b><br>Canvi d'estat o Props" } --> B
    B --> C
    C --> D
    D --> E["<b>5. PAINT</b><br>El Navegador dibuixa"]
    B --- B1 & B2 & B3
    C --- C1 & C2
    D --- D1 & D2

    A@{ shape: rect}
     B1:::render
     B:::render
     B2:::render
     B3:::render
     C1:::render
     C:::render
     C2:::render
     D1:::commit
     D:::commit
     D2:::commit
     A:::trigger
     E:::browser
    classDef trigger fill:#f9f,stroke:#333,stroke-width:2px
    classDef render fill:#bbf,stroke:#333,stroke-width:2px
    classDef commit fill:#bfb,stroke:#333,stroke-width:2px
    classDef browser fill:#eee,stroke:#333,stroke-dasharray:
```

# React Lifecycle: The Render and Commit Phases (Hooks, UserState, UserEffect)

```mermaid
graph TD
    %% Style Definitions
    classDef hook fill:#ffcc00,stroke:#333,stroke-width:2px;
    classDef trigger fill:#f9f,stroke:#333,stroke-width:2px;
    classDef render fill:#bbf,stroke:#333,stroke-width:2px;
    classDef commit fill:#bfb,stroke:#333,stroke-width:2px;
    classDef browser fill:#eee,stroke:#333,stroke-dasharray: 5 5;

    %% Flow
    Start((Start)) --> A
    
    subgraph Trigger_Phase [1. TRIGGER PHASE]
        A[State or Props Change]
        H1{{<b>useState / useReducer</b><br/>Setter invoked}} -.-> A
    end

    A --> B

    subgraph Render_Phase [2. RENDER PHASE]
        B[Function Component Execution]
        B1[Virtual DOM Calculation]
        B2[Reconciliation / Diffing]
        B --- B1
        B1 --- B2
    end

    B2 --> D

    subgraph Commit_Phase [3. COMMIT PHASE]
        D[DOM Mutation]
        D1[React updates Real DOM nodes]
        D --- D1
    end

    D1 --> E

    subgraph Paint_Phase [4. BROWSER PAINT]
        E[Screen Painting]
    end

    E --> F

    subgraph Effects_Phase [5. PASSIVE EFFECTS]
        F{{<b>useEffect</b>}}
        F1[Asynchronous execution]
        F --- F1
    end

    F1 -.->|May trigger new| A

    %% Class Assignment
    class H1,F hook;
    class A trigger;
    class B,B1,B2 render;
    class D,D1 commit;
    class E browser;
    class F1 hook;
```