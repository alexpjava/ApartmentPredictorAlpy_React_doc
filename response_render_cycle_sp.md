# Documentación de Gestión de Datos: Componente ApartmentList

Este documento detalla el flujo de información, desde la petición asíncrona hasta el renderizado final en el componente React, explicando cómo interactúan los Hooks y la librería Axios.

## 1. Diagrama Flujo de Datos en React

```mermaid
sequenceDiagram
    participant UI as Componente (UI)
    participant Hook as Custom Hook / useEffect
    participant Axios as Axios Client
    participant API as API Externa
    participant Parser as Transformador / Parser

    Note over UI, API: El proceso inicia al montar el componente o por un evento
    UI->>Hook: Invoca acción (ej. fetchData)
    Hook->>Hook: setStatus('loading')
    
    Hook->>Axios: get('/endpoint')
    Axios->>API: Solicitud HTTP (JSON)
    API-->>Axios: Respuesta HTTP (Raw JSON)
    
    Note right of Axios: Interceptores (opcional)
    Axios->>Parser: Envía datos crudos (response.data)
    
    Note over Parser: Lógica de Negocio:<br/>1. Formateo de fechas<br/>2. Mapeo de nombres<br/>3. Limpieza de nulos
    Parser-->>Hook: Retorna Objeto Limpio/Parseado
    
    Hook->>UI: updateState(parsedData)
    Hook->>UI: setStatus('success')
   
```   
    Note over UI: Re-render con datos
---

## 2. Diagrama de Secuencia Datos Apartment
El siguiente diagrama ilustra el ciclo de vida de los datos:

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

## 3. Análisis Técnico del Flujo

### A. Inicialización del Estado (`useState`)

Se definen tres estados críticos para gestionar el ciclo de vida de la información:

* **`apartments`**: Inicializado como un array vacío `[]`. Es el contenedor final de los datos.
* **`isLoading`**: Un booleano que controla la experiencia de usuario (UX) mientras los datos viajan por la red.
* **`isAxiosError`**: Bandera para la gestión de excepciones.

---

### B. Disparador de Efecto (`useEffect`)

Al utilizar un array de dependencias vacío `[]`, el flujo de datos se inicia exactamente una vez inmediatamente después de que el componente se monta en el DOM.

---

### C. Consumo y Parseo Automático (Axios)

Cuando la API responde, Axios realiza el primer nivel de parseo de forma interna:

1. Recibe el cuerpo de la respuesta en formato de texto plano (JSON).
2. Lo transforma automáticamente en un Objeto Literal de JavaScript.
3. Lo entrega a la variable `response.data`.

---

### D. Transformación de Datos (Data Mapping)

En la línea:

```js
const apartmentsData = response.data;
```

los datos están listos para ser manipulados.

> **Nota:** En este punto es donde se recomienda aplicar funciones de limpieza o transformación si los nombres de las propiedades de la API (ej: `prefarea`) no coinciden con los nombres deseados para la UI (ej: `isPreferredArea`).

---

### E. Re-renderizado y Proyección

Al ejecutar `setApartments(apartmentsData)`, React detecta un cambio de estado y dispara un re-render. Durante este proceso:

* El componente vuelve a ejecutarse.
* La condición `{isLoading ? ... : ...}` cambia para mostrar la lista.
* El método `.map()` itera sobre el nuevo array de datos, proyectando cada objeto en un elemento `<li>` del DOM.
