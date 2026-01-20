
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
'''
