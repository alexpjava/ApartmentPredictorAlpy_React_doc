# Guía Completa de React Paso a Paso

Esta guía contiene **todos los puntos desde el 1 al 60**, junto con un **mini-proyecto completo**, y está diseñada para ser tu referencia detallada en el futuro.

## Índice

- [1️⃣ JavaScript primero](#1️⃣-javascript-primero)
- [2️⃣ React usa funciones](#2️⃣-react-usa-funciones)
- [3️⃣ Qué añade React → JSX](#3️⃣-qué-añade-react-→-jsx)
- [4️⃣ Regla clave de React](#4️⃣-regla-clave-de-react)
- [5️⃣ De dónde sale `<Profile />`](#5️⃣-de-dónde-sale-profile-)
- [6️⃣ ¿Profile es especial de React?](#6️⃣-profile-especial-de-react)
- [7️⃣ Tú decides el nombre](#7️⃣-tú-decides-el-nombre)
- [8️⃣ Qué hace React con esto](#8️⃣-qué-hace-react-con-esto)
- [9️⃣ Comparación con HTML](#9️⃣-comparación-con-html)
- [🔟 Frase para memorizar](#🔟-frase-para-memorizar)
- [1️⃣1️⃣ Relación con HTML real](#1️⃣1️⃣-relación-con-html-real)
- [1️⃣2️⃣ JSX no es HTML](#1️⃣2️⃣-jsx-no-es-html)
- [1️⃣3️⃣ Listas en React (`map()`)](#1️⃣3️⃣-listas-en-react-map)
- [1️⃣4️⃣ Problema: escribir `<Profile />` muchas veces](#1️⃣4️⃣-problema-escribir-profile--muchas-veces)
- [1️⃣5️⃣ Solución: `map()`](#1️⃣5️⃣-solución-map)
- [1️⃣6️⃣ `map()` en React](#1️⃣6️⃣-map-en-react)
- [1️⃣7️⃣ `key` en React](#1️⃣7️⃣-key-en-react)
- [1️⃣8️⃣ Ejemplo completo: App + Profile](#1️⃣8️⃣-ejemplo-completo-app--profile)
- [1️⃣9️⃣ Dónde entra `<Fragment>`](#1️⃣9️⃣-dónde-entra-fragment)
- [2️⃣0️⃣ `div` vs `Fragment`](#2️⃣0️⃣-div-vs-fragment)
- [2️⃣1️⃣ `div`, `section`, `article`](#2️⃣1️⃣-div-section-article)
- [2️⃣2️⃣ JSX no es HTML (detalle)](#2️⃣2️⃣-jsx-no-es-html-detalle)
- [2️⃣3️⃣ Qué hace React al final](#2️⃣3️⃣-qué-hace-react-al-final)
- [2️⃣4️⃣ Errores normales](#2️⃣4️⃣-errores-normales)
- [2️⃣5️⃣ Separar lógica y presentación](#2️⃣5️⃣-separar-lógica-y-presentación)
- [2️⃣6️⃣ Crear un componente de lista](#2️⃣6️⃣-crear-un-componente-de-lista)
- [2️⃣7️⃣ App.jsx más simple](#2️⃣7️⃣-appjsx-más-simple)
- [2️⃣8️⃣ Renderizado condicional](#2️⃣8️⃣-renderizado-condicional)
- [2️⃣9️⃣ Operador ternario](#2️⃣9️⃣-operador-ternario)
- [3️⃣0️⃣ Crear vista en tabla](#3️⃣0️⃣-crear-vista-en-tabla)
- [3️⃣1️⃣ App.jsx final](#3️⃣1️⃣-appjsx-final)
- [3️⃣2️⃣ Estado (`useState`) interactivo](#3️⃣2️⃣-estado-usestate-interactivo)
- [3️⃣3️⃣ Flujo mental del estado](#3️⃣3️⃣-flujo-mental-del-estado)
- [3️⃣4️⃣ CSS profesional](#3️⃣4️⃣-css-profesional)
- [3️⃣5️⃣ Estructura de carpetas recomendada](#3️⃣5️⃣-estructura-de-carpetas-recomendada)
- [3️⃣6️⃣ Evitar demasiada lógica en un componente](#3️⃣6️⃣-evitar-demasiada-lógica-en-un-componente)
- [3️⃣7️⃣ React no es difícil, es estructural](#3️⃣7️⃣-react-no-es-difícil-es-estructural)
- [3️⃣8️⃣ Tu progreso](#3️⃣8️⃣-tu-progreso)
- [3️⃣9️⃣ Próximos pasos](#3️⃣9️⃣-próximos-pasos)
- [4️⃣0️⃣ Filtro por profesión](#4️⃣0️⃣-filtro-por-profesión)
- [4️⃣1️⃣ Qué es `filter()`](#4️⃣1️⃣-qué-es-filter)
- [4️⃣2️⃣ `filter()` + React](#4️⃣2️⃣-filter--react)
- [4️⃣3️⃣ Estado para el filtro](#4️⃣3️⃣-estado-para-el-filtro)
- [4️⃣4️⃣ Aplicar el filtro](#4️⃣4️⃣-aplicar-el-filtro)
- [4️⃣5️⃣ UI del filtro](#4️⃣5️⃣-ui-del-filtro)
- [4️⃣6️⃣ Conectar filtro + vista](#4️⃣6️⃣-conectar-filtro--vista)
- [4️⃣7️⃣ Buscar por nombre (input controlado)](#4️⃣7️⃣-buscar-por-nombre-input-controlado)
- [4️⃣8️⃣ Input controlado](#4️⃣8️⃣-input-controlado)
- [4️⃣9️⃣ Aplicar búsqueda](#4️⃣9️⃣-aplicar-búsqueda)
- [5️⃣0️⃣ Flujo completo de datos](#5️⃣0️⃣-flujo-completo-de-datos)
- [5️⃣1️⃣ Responsive básico (CSS mental)](#5️⃣1️⃣-responsive-básico-css-mental)
- [5️⃣2️⃣ Grid para cards](#5️⃣2️⃣-grid-para-cards)
- [5️⃣3️⃣ Separar lógica visual](#5️⃣3️⃣-separar-lógica-visual)
- [5️⃣4️⃣ Conectar con datos reales (API)](#5️⃣4️⃣-conectar-con-datos-reales-api)
- [5️⃣5️⃣ Qué es `useEffect`](#5️⃣5️⃣-qué-es-useeffect)
- [5️⃣6️⃣ Estado completo final](#5️⃣6️⃣-estado-completo-final)
- [5️⃣7️⃣ App mental final](#5️⃣7️⃣-app-mental-final)
- [5️⃣8️⃣ Errores comunes aquí](#5️⃣8️⃣-errores-comunes-aquí)
- [5️⃣9️⃣ Cuándo parar](#5️⃣9️⃣-cuándo-parar)
- [6️⃣0️⃣ Lo que ya sabes](#6️⃣0️⃣-lo-que-ya-sabes)
- [Mini-proyecto: Directorio de Personas](#mini-proyecto-directorio-de-personas)

---

# 1️⃣ JavaScript primero

En JavaScript puedes crear **funciones**:
```js
function saludar() {
  return "Hola";
}
```
Esto es **JavaScript puro**, nada de React todavía.

- Una función tiene un nombre
- Puede devolver algo (`return`)

---
# 2️⃣ React usa funciones

React **no inventa funciones nuevas**. Solo usa **funciones JS normales**.
```js
function Profile() {
  return "Hola";
}
```
Sigue siendo JS puro.

---
# 3️⃣ Qué añade React → JSX

React permite que la función **devuelva HTML dentro de JS**:
```jsx
function Profile() {
  return <h2>Hola</h2>;
}
```
- JSX parece HTML
- Es JS
- React lo transforma internamente

---
# 4️⃣ Regla clave de React

> Si una función empieza con **mayúscula** y devuelve JSX, React la trata como un **componente**

```jsx
function Profile() {} // ✔ componente
function profile() {} // ❌ no componente
```

---
# 5️⃣ De dónde sale `<Profile />`

Cuando escribes:
```jsx
<Profile />
```
React llama internamente a:
```js
Profile()
```
- Llama a la función
- Renderiza lo que devuelve

No es HTML.

---
# 6️⃣ Profile es especial de React?

❌ No, `Profile` es solo un nombre que tú eliges.
React lo interpreta como componente porque empieza con mayúscula y devuelve JSX.

---
# 7️⃣ Tú decides el nombre

```jsx
function Card() {}
function UserInfo() {}
<Pepe />
```
Todo funciona igual.

---
# 8️⃣ Qué hace React con esto

React: ve mayúscula → ejecuta función → renderiza JSX

---
# 9️⃣ Comparación con HTML

| Tipo | Ejemplo | Qué es |
|---|---|---|
| Minúscula | `<div>` | HTML real |
| Minúscula | `<profile>` | HTML inexistente ❌ |
| Mayúscula | `<Profile />` | Componente React ✔ |

---
# 🔟 Frase para memorizar

> Un componente React es una función JS con mayúscula que devuelve JSX y recibe props.

---
# 1️⃣1️⃣ Relación con HTML real

Dentro de un componente puedes usar HTML normal:
```jsx
<div>
  <h2>{name}</h2>
  <ul><li>React</li></ul>
</div>
```

---
# 1️⃣2️⃣ JSX no es HTML

- `className` en vez de `class`
- `htmlFor` en vez de `for`
Porque JSX es JS.

---
# 1️⃣3️⃣ Listas en React (`map()`)

Si tenemos un array:
```js
const people = [ ... ];
```
No queremos escribir `<Profile />` muchas veces.

---
# 1️⃣4️⃣ Problema: escribir `<Profile />` muchas veces

❌ No escalable:
```jsx
<Profile name="Ada" />
<Profile name="Marie" />
```

---
# 1️⃣5️⃣ Solución: `map()`

```js
people.map(person => <Profile key={person.id} name={person.name} />)
```

---
# 1️⃣6️⃣ `map()` en React

- Recorre array
- Devuelve JSX
- React pinta todos los elementos

---
# 1️⃣7️⃣ `key` en React

```jsx
<Profile key={person.id} />
```
- Identifica cada elemento
- Evita errores y mejora rendimiento

---
# 1️⃣8️⃣ Ejemplo completo: App + Profile

```jsx
function Profile({ name, job }) {
  return <div>{name} - {job}</div>;
}

function App() {
  return (
    <div>
      {people.map(p => <Profile key={p.id} name={p.name} job={p.job} />)}
    </div>
  );
}
```

---
# 1️⃣9️⃣ Dónde entra `<Fragment>`

- Un componente no puede devolver 2 elementos hermanos sin contenedor.
- Soluciones: `<div>` o `<Fragment>` (`<> ... </>`)

---
# 2️⃣0️⃣ `div` vs `Fragment`

| Usa `div` | Usa `Fragment` |
|---|---|
| Necesitas estilos | No quieres HTML extra |
| Layout | Lists (`map`) |

---
# 2️⃣1️⃣ `div`, `section`, `article`

- `<div>`: genérico
- `<section>`: contenido relacionado
- `<article>`: contenido independiente (cards, posts)

---
# 2️⃣2️⃣ JSX no es HTML (detalle)

- `className` en vez de `class`
- `htmlFor` en vez de `for`

---
# 2️⃣3️⃣ Qué hace React al final

1. JSX → JS
2. React crea HTML real
3. Navegador pinta

---
# 2️⃣4️⃣ Errores normales

- Confundir HTML y JSX
- Olvidar `key`
- Componentes en minúscula

---
# 2️⃣5️⃣ Separar lógica y presentación

- `App` → decide qué mostrar
- `Componentes` → cómo se ve

---
# 2️⃣6️⃣ Crear un componente de lista

`ProfileList.jsx` que recorra `people` y devuelva `<Profile />` por cada uno

---
# 2️⃣7️⃣ App.jsx más simple

```jsx
<ProfileList people={people} />
```

---
# 2️⃣8️⃣ Renderizado condicional

```jsx
if (format === "table") return <Table />
else return <ProfileList />
```

---
# 2️⃣9️⃣ Operador ternario

```jsx
{format === "card" ? <ProfileList /> : <ProfileTable />}
```

---
# 3️⃣0️⃣ Crear vista en tabla

`ProfileTable.jsx` con `<table>` y `map` dentro de `<tbody>`

---
# 3️⃣1️⃣ App.jsx final

```jsx
{format === "card" ? <ProfileList /> : <ProfileTable />}
```

---
# 3️⃣2️⃣ Estado (`useState`) interactivo

```jsx
const [format, setFormat] = useState("card");
```
Botones: `onClick={() => setFormat("table")}`

---
# 3️⃣3️⃣ Flujo mental del estado

1. Click → `setFormat`
2. React ejecuta App
3. Render correcto

---
# 3️⃣4️⃣ CSS profesional

- Separar por componente
- `Profile.css`, `ProfileTable.css`

---
# 3️⃣5️⃣ Estructura de carpetas recomendada

```
components/
 ├ Profile.jsx
 ├ ProfileList.jsx
 ├ ProfileTable.jsx
 └ Profile.css
App.jsx
```

---
# 3️⃣6️⃣ Evitar demasiada lógica en un componente

- Si hay muchos `if` o `map`, dividir componentes

---
# 3️⃣7️⃣ React no es difícil, es estructural

Pensar en **componentes y responsabilidades**.

---
# 3️⃣8️⃣ Tu progreso

✔ Componentes, JSX, props, map, filter, state, effects, CSS básico, responsive, UI dinámica

---
# 3️⃣9️⃣ Próximos pasos

- Filtro, búsqueda, CSS, datos reales, useEffect

---
# 4️⃣0️⃣ Filtro por profesión

`filter()` de JS para array de `people`

---
# 4️⃣1️⃣ Qué es `filter()`

Devuelve un nuevo array según condición

---
# 4️⃣2️⃣ `filter()` + React

Filtrar datos → renderizar → React pinta

---
# 4️⃣3️⃣ Estado para el filtro

```jsx
const [profession, setProfession] = useState("all");
```

---
# 4️⃣4️⃣ Aplicar el filtro

```jsx
const filteredPeople = profession === "all" ? people : people.filter(p => p.job === profession);
```

---
# 4️⃣5️⃣ UI del filtro

Botones que llaman `setProfession` para cambiar el estado

---
# 4️⃣6️⃣ Conectar filtro + vista

```jsx
<ProfileList people={filteredPeople} />
```

---
# 4️⃣7️⃣ Buscar por nombre (input controlado)

```jsx
const [search, setSearch] = useState("");
```

---
# 4️⃣8️⃣ Input controlado

```jsx
<input value={search} onChange={e => setSearch(e.target.value)} />
```

---
# 4️⃣9️⃣ Aplicar búsqueda

```jsx
const searchedPeople = filteredPeople.filter(p => p.name.toLowerCase().includes(search.toLowerCase()));
```

---
# 5️⃣0️⃣ Flujo completo de datos

`people → filter (profesión) → filter (search) → render`

---
# 5️⃣1️⃣ Responsive básico (CSS mental)

```css
.card { max-width: 300px; width: 100%; }
```

---
# 5️⃣2️⃣ Grid para cards

```css
.grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(250px, 1fr)); gap: 1rem; }
```

---
# 5️⃣3️⃣ Separar lógica visual

```jsx
<section className="grid"><ProfileList people={searchedPeople} /></section>
```

---
# 5️⃣4️⃣ Conectar con datos reales (API)

```jsx
useEffect(() => {
  fetch("https://api.example.com/people")
    .then(res => res.json())
    .then(data => setPeople(data));
}, []);
```

---
# 5️⃣5️⃣ Qué es `useEffect`

- Se ejecuta después del render
- Para fetch, timers, suscripciones

---
# 5️⃣6️⃣ Estado completo final

```jsx
const [people, setPeople] = useState([]);
const [format, setFormat] = use

