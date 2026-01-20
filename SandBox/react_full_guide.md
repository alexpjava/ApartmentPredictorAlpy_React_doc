# Guía Completa de React

Este documento reúne todos los puntos desde el 1️⃣ hasta el 6️⃣0️⃣ junto con un mini-proyecto de ejemplo. Está pensado como guía detallada para aprender y documentar React paso a paso.

---

## Índice

1. [JavaScript y React: Fundamentos](#punto-1)
2. [Componentes y JSX](#punto-2)
3. [Props y reutilización](#punto-3)
4. [Listas y map()](#punto-4)
5. [Fragment y contenedores](#punto-5)
6. [Div vs Section vs Article](#punto-6)
7. [Estado y useState](#punto-7)
8. [Renderizado condicional](#punto-8)
9. [Separación de lógica y presentación](#punto-9)
10. [Filtros y búsqueda](#punto-10)
11. [Mini-proyecto: Directorio de Personas](#punto-11)

---

## 1️⃣ JavaScript y React: Fundamentos <a name="punto-1"></a>

### 1. Funciones en JavaScript
```js
function saludar() {
  return "Hola";
}
```
- Funciones tienen nombre y pueden devolver algo.
- Nada de React todavía.

### 2. React usa funciones normales
```js
function Profile() {
  return "Hola";
}
```
- Sigue siendo JavaScript puro.

### 3. JSX
```jsx
function Profile() {
  return <h2>Hola</h2>;
}
```
- JSX parece HTML pero es JavaScript.
- React lo transforma internamente.

### 4. Regla clave: Mayúscula
- Función que empieza con mayúscula y devuelve JSX → React lo trata como componente.
- Ejemplo válido: `Profile`.
- Ejemplo inválido: `profile`.

### 5. <Profile />
- React interpreta `<Profile />` como una llamada a la función `Profile()`.
- No es HTML, es un componente.

### 6. Profile NO es especial
- Nombre elegido por ti.
- Puede ser cualquier nombre con mayúscula inicial.
- React solo exige mayúscula y retorno JSX.

### 7. Diferencia con HTML
| HTML | React |
|---|---|
| `<div>` | Etiqueta real |
| `<profile>` | No existe ❌ |
| `<Profile />` | Componente React ✔️ |

### 8. Props
```jsx
function Profile({ name }) {
  return <h2>{name}</h2>;
}
```
- Datos que entran al componente.
- Reutilización sencilla.

---

## 2️⃣ Componentes y JSX <a name="punto-2"></a>

### 9. Reutilización
```jsx
<Profile name="Ada" />
<Profile name="Marie" />
```
- Misma función, diferentes datos.
- React pinta cada uno por separado.

### 10. Relación con HTML real
```jsx
<div>
  <ul>
    <li>{name}</li>
  </ul>
</div>
```
- JSX genera HTML final en navegador.

### 11. JSX no es HTML
- Usar `className` en vez de `class`
- Usar `htmlFor` en vez de `for`

---

## 3️⃣ Listas y map() <a name="punto-3"></a>

### 12. Datos en array
```js
const people = [
  { id: 1, name: "Ada Lovelace", job: "Programadora" },
  { id: 2, name: "Marie Curie", job: "Química" }
];
```

### 13. Problema sin map
```jsx
<Profile name="Ada" />
<Profile name="Marie" />
```
- No escalable si hay muchas personas.

### 14. Solución: map()
```jsx
people.map(person => (
  <Profile key={person.id} name={person.name} job={person.job} />
))
```
- Devuelve un array de JSX.
- `key` obligatorio para identificar elementos.

### 15. Fragment
- Evita `div` extra.
```jsx
<>
  <h2>Hola</h2>
  <p>Mundo</p>
</>
```
- Útil en listas y elementos hermanos.

### 16. Div vs Section vs Article
- `div`: contenedor genérico.
- `section`: contenido relacionado.
- `article`: contenido independiente (cards, posts).

---

## 4️⃣ Estado y useState <a name="punto-4"></a>

### 17. Estado básico
```jsx
const [format, setFormat] = useState("card");
```
- React vuelve a renderizar al cambiar estado.

### 18. Botones interactivos
```jsx
<button onClick={() => setFormat("card")}>Cards</button>
<button onClick={() => setFormat("table")}>Tabla</button>
```
- Actualiza la UI dinámicamente.

### 19. Flujo mental
1. Usuario hace click
2. `setFormat` cambia estado
3. React ejecuta `App`
4. Renderiza vista correcta

---

## 5️⃣ Separación de lógica y presentación <a name="punto-5"></a>

### 20. Componente ProfileList
```jsx
function ProfileList({ people }) {
  return (
    <section className="grid">
      {people.map(person => (
        <Profile key={person.id} name={person.name} job={person.job} />
      ))}
    </section>
  );
}
```
- App decide qué mostrar
- Componentes deciden cómo se ve

### 21. App simplificado
```jsx
function App() {
  return <ProfileList people={people} />;
}
```
- Código limpio, responsabilidad clara.

---

## 6️⃣ Renderizado condicional <a name="punto-6"></a>

### 22. If simple
```jsx
if (format === "table") return <Table />;
return <ProfileList people={people} />;
```

### 23. Operador ternario
```jsx
{format === "card" ? <ProfileList people={people} /> : <ProfileTable people={people} />}
```
- React ama esta forma.

### 24. ProfileTable
```jsx
function ProfileTable({ people }) {
  return (
    <table>
      <thead>
        <tr><th>Nombre</th><th>Profesión</th></tr>
      </thead>
      <tbody>
        {people.map(p => <tr key={p.id}><td>{p.name}</td><td>{p.job}</td></tr>)}
      </tbody>
    </table>
  );
}
```
- `map()` también funciona en tablas.

---

## 7️⃣ Filtros y búsqueda <a name="punto-7"></a>

### 25. filter()
```js
people.filter(p => p.job === "Química")
```
- Devuelve un nuevo array.
- Nunca modificar el array original.

### 26. Estado para filtros
```js
const [profession, setProfession] = useState("all");
```
- Condición all muestra todos.

### 27. Búsqueda
```js
const [search, setSearch] = useState("");
const searchedPeople = filteredPeople.filter(p => p.name.toLowerCase().includes(search.toLowerCase()));
```
- Input controlado.

### 28. Flujo completo
```
people -> filter(profession) -> filter(search) -> render
```
- React dibuja el resultado final.

---

## 8️⃣ Mini-proyecto: Directorio de Personas <a name="punto-11"></a>

### Objetivo
- Mostrar perfiles en cards
- Buscar por nombre
- Filtrar por profesión

### Estructura de archivos
```
src/
 ├─ components/
 │   ├─ Profile.jsx
 │   ├─ ProfileList.jsx
 │   └─ SearchBar.jsx
 ├─ data.js
 ├─ App.jsx
 └─ index.css
```

### data.js
```js
export const people = [
  { id: 1, name: "Ada Lovelace", job: "Programadora" },
  { id: 2, name: "Marie Curie", job: "Química" },
  { id: 3, name: "Albert Einstein", job: "Físico" },
  { id: 4, name: "Grace Hopper", job: "Programadora" }
];
```

### Profile.jsx
```jsx
function Profile({ name, job }) {
  return (
    <article className="card">
      <h2>{name}</h2>
      <p>{job}</p>
    </article>
  );
}
export default Profile;
```

### ProfileList.jsx
```jsx
import Profile from "./Profile";
function ProfileList({ people }) {
  return (
    <section className="grid">
      {people.map(person => (
        <Profile key={person.id} name={person.name} job={person.job} />
      ))}
    </section>
  );
}
export default ProfileList;
```

### SearchBar.jsx
```jsx
function SearchBar({ search, setSearch }) {
  return (
    <input type="text" placeholder="Buscar por nombre..." value={search} onChange={e => setSearch(e.target.value)} />
  );
}
export default SearchBar;
```

### App.jsx
```jsx
import { useState } from "react";
import { people as data } from "./data";
import ProfileList from "./components/ProfileList";
import SearchBar from "./components/SearchBar";

export default function App() {
  const [search, setSearch] = useState("");
  const [profession, setProfession] = useState("all");

  const filteredPeople = data
    .filter(person => profession === "all" ? true : person.job === profession)
    .filter(person => person.name.toLowerCase().includes(search.toLowerCase()));

  return (
    <main>
      <h1>Directorio de Personas</h1>
      <SearchBar search={search} setSearch={setSearch} />
      <div className="filters">
        <button onClick={() => setProfession("all")}>Todos</button>
        <button onClick={() => setProfession("Programadora")}>Programadoras</button>
        <button onClick={() => setProfession("Química")}>Químicas</button>
        <button onClick={() => setProfession("Físico")}>Físicos</button>
      </div>
      <ProfileList people={filteredPeople} />
    </main>
  );
}
```

### index.css
```css
body { font-family: system-ui, sans-serif; padding: 2rem; }
.grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(200px, 1fr)); gap: 1rem; }
.card { border: 1px solid #ddd; padding: 1rem; border-radius: 8px; }
input { padding: 0.5rem; margin-bottom: 1rem; width: 100%; }
.filters button { margin-right: 0.5rem; }
```

### Flujo mental
1. Usuario escribe o hace click
2. Cambia el estado
3. React vuelve a ejecutar `App`
4. Se recalculan los filtros
5. Se renderiza la lista

---

**Este documento puede ser usado como guía completa para aprender y documentar React.**

