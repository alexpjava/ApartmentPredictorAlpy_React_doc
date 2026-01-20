# Guía React desde Cero hasta Mini Proyecto

> Documento de aprendizaje progresivo paso a paso. Desde JavaScript básico aplicado a React hasta un mini‑proyecto funcional.

---

## 📑 Índice

1. [Funciones en JavaScript](#1-funciones-en-javascript)

2. [React usa funciones](#2-react-usa-funciones)

3. [JSX](#3-jsx)

4. [Regla de las mayúsculas](#4-regla-de-las-mayúsculas)

5. [De dónde sale `<Profile />`](#5-de-dónde-sale-profile-)

6. [`Profile` no es propio de React](#6-profile-no-es-propio-de-react)

7. [HTML vs Componentes](#7-html-vs-componentes)

8. [Props](#8-props)

9. [Destructuring](#9-destructuring)

10. [Reutilización](#10-reutilización)

11. [HTML dentro de React](#11-html-dentro-de-react)

12. [Resumen mental](#12-resumen-mental)

13. [Arrays y datos](#13-arrays-y-datos)

14. [Problema de repetición](#14-problema-de-repetición)

15. [`map()`](#15-map)

16. [`map()` en React](#16-map-en-react)

17. [`key`](#17-key)

18. [Ejemplo completo](#18-ejemplo-completo)

19. [Fragment](#19-fragment)

20. [`div` vs Fragment](#20-div-vs-fragment)

21. [HTML semántico](#21-html-semántico)

22. [JSX no es HTML](#22-jsx-no-es-html)

23. [Qué hace React](#23-qué-hace-react)

24. [Errores normales](#24-errores-normales)

25. [Separar lógica y presentación](#25-separar-lógica-y-presentación)

26. [Componente lista](#26-componente-lista)

27. [App limpio](#27-app-limpio)

28. [Renderizado condicional](#28-renderizado-condicional)

29. [Operador ternario](#29-operador-ternario)

30. [Vista tabla](#30-vista-tabla)

31. [Cards o tabla](#31-cards-o-tabla)

32. [`useState`](#32-usestate)

33. [Flujo del estado](#33-flujo-del-estado)

34. [CSS por componentes](#34-css-por-componentes)

35. [Estructura de carpetas](#35-estructura-de-carpetas)

36. [Componentes grandes](#36-componentes-grandes)

37. [Pensar en componentes](#37-pensar-en-componentes)

38. [Progreso](#38-progreso)

39. [Siguientes pasos](#39-siguientes-pasos)

40. [Filtro](#40-filtro)

41. [`filter()`](#41-filter)

42. [`filter()` en React](#42-filter-en-react)

43. [Estado de filtro](#43-estado-de-filtro)

44. [Aplicar filtro](#44-aplicar-filtro)

45. [UI del filtro](#45-ui-del-filtro)

46. [Filtro + vista](#46-filtro--vista)

47. [Búsqueda](#47-búsqueda)

48. [Input controlado](#48-input-controlado)

49. [Filtro de búsqueda](#49-filtro-de-búsqueda)

50. [Flujo de datos](#50-flujo-de-datos)

51. [Responsive](#51-responsive)

52. [Grid](#52-grid)

53. [Separar visual](#53-separar-visual)

54. [Datos reales](#54-datos-reales)

55. [`useEffect`](#55-useeffect)

56. [Estado completo](#56-estado-completo)

57. [Lógica final](#57-lógica-final)

58. [Errores comunes](#58-errores-comunes)

59. [Cuándo parar](#59-cuándo-parar)

60. [Lo aprendido](#60-lo-aprendido)

---

## 🧩 Mini Proyecto: Directorio de Personas

### Objetivo

* Mostrar personas en cards
* Buscar por nombre
* Filtrar por profesión

### Estructura

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
    <input
      value={search}
      onChange={e => setSearch(e.target.value)}
      placeholder="Buscar por nombre"
    />
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
    .filter(p => profession === "all" || p.job === profession)
    .filter(p => p.name.toLowerCase().includes(search.toLowerCase()));

  return (
    <main>
      <h1>Directorio de Personas</h1>
      <SearchBar search={search} setSearch={setSearch} />
      <div>
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
body {
  font-family: system-ui, sans-serif;
  padding: 2rem;
}

.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 1rem;
}

.card {
  border: 1px solid #ddd;
  padding: 1rem;
  border-radius: 8px;
}
```

---

## ✅ Conclusión

Si entiendes este documento completo, **entiendes React a nivel junior sólido**.

> React no es memorizar. Es entender patrones.

Fin 🎉
