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

## 3. Explicación Detallada Paso a Paso

### 1. Inicialización y Montaje

Cuando el componente **`ApartmentList`** se carga, lo primero que ocurre es la inicialización de los Hooks.

* **Líneas 5–7**: Se definen los estados iniciales.
  `isLoading` comienza en `true`, por lo que el usuario ve el mensaje **"Loading..."** en la pantalla inicialmente.

---

### 2. Disparo del Efecto (`useEffect`)

* **Línea 9**: El `useEffect` con el array de dependencias vacío `[]` le indica a React:
  *"Ejecuta este código solo una vez, justo después de que el componente aparezca en pantalla"*.

* **Línea 10**: Se define y se llama inmediatamente a la función asíncrona `fetchApartments()`.

---

### 3. La Petición HTTP (Axios)

* **Línea 12**:

  ```js
  axios.get("/api/apartment/getAll")
  ```

  envía una solicitud al servidor Spring Boot.

> **Importante:** Axios realiza el parseo automáticamente cuando el backend responde con JSON.
> No es necesario usar `JSON.parse()`, ya que el resultado ya es un objeto de JavaScript accesible en `response.data`.

---

### 4. Gestión del Backend (Spring Boot)

Aunque no se muestra el código Java, el flujo asume que:

* Un **`@RestController`** recibe la petición en el endpoint correspondiente.
* Un **Repository** extrae los datos desde la base de datos.
* Spring Boot serializa los objetos Java a formato JSON mediante la librería **Jackson**.

---

### 5. Actualización del Estado y Re-renderizado

* **Línea 15**:

  ```js
  setApartments(apartmentsData)
  ```

  guarda los datos reales en el estado del componente.

* **Línea 16**:

  ```js
  setIsLoading(false)
  ```

  cambia el semáforo de carga.

**El trigger:**
En React, cada vez que se ejecuta un setter de estado (`set...`), el componente se vuelve a renderizar automáticamente.

---

### 6. Transformación de Datos a UI (`.map()`)

* **Línea 33**: Como `isLoading` ahora es `false`, React ejecuta el bloque de renderizado principal.

* **Línea 35**:

  ```js
  apartments.map(...)
  ```

  actúa como el motor de renderizado, recorriendo cada objeto del array y transformándolo en un elemento `<li>`.

* **Línea 36**:

  ```jsx
  key={apartment.id}
  ```

  permite que React identifique cada elemento de forma eficiente cuando la lista cambia.

---

### Un Punto Clave sobre el “Parseo”

En este código, el recorrido de los datos a través de distintos formatos es el siguiente:

**DB (SQL/NoSQL)** → **Java Object (POJO)** → **JSON (Texto)** → **JS Object** → **DOM (HTML)**

Este flujo garantiza una separación clara entre persistencia, lógica de negocio, transporte de datos y presentación en la UI.
