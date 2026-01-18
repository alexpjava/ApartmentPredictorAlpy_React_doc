# Arquitectura y Herramientas del Ecosistema Frontend Moderno

## 1. Entorno de Ejecución y Gestión de Paquetes

### Node.js
Entorno de ejecución para JavaScript construido con el motor V8 de Chrome. Permite la ejecución de código JavaScript en el lado del servidor, facilitando la creación de herramientas de automatización, servidores web y sistemas de construcción.

### NVM (Node Version Manager)
Utilidad de línea de comandos diseñada para administrar múltiples instalaciones de Node.js. Permite al desarrollador alternar entre diferentes versiones del entorno de ejecución según los requisitos específicos de compatibilidad de cada proyecto.

### NPM (Node Package Manager)
El gestor de paquetes estándar para Node.js. Facilita la instalación, actualización y gestión de dependencias (librerías y frameworks), además de permitir la ejecución de scripts definidos en el archivo `package.json`.

---

## 2. Herramientas de Desarrollo y Frameworks

### Vite
Herramienta de construcción (build tool) de próxima generación que mejora significativamente la experiencia de desarrollo. Utiliza Native ES Modules para ofrecer un servidor de desarrollo extremadamente rápido y Rollup para la generación de bundles optimizados en producción.

### Next.js
Framework de desarrollo web basado en React que proporciona funcionalidades extendidas como el enrutamiento basado en el sistema de archivos, optimización de imágenes y, fundamentalmente, diversas estrategias de renderizado (SSR, SSG, ISR).

---

## 3. Estrategias de Renderizado (Rendering Patterns)

### CSR (Client-Side Rendering)
Patrón donde el renderizado de la interfaz de usuario se delega enteramente al navegador. El servidor entrega un documento HTML mínimo y el cliente ejecuta JavaScript para construir el DOM.

- **Ventajas:** Navegación fluida tipo SPA (Single Page Application).  
- **Desventajas:** Carga inicial pesada y desafíos en la indexación SEO.

### SSR (Server-Side Rendering)
Proceso mediante el cual cada solicitud al servidor genera una nueva página HTML de forma dinámica. El servidor ejecuta el código de la aplicación, recupera datos y entrega un HTML completamente formado al cliente.

- **Ventajas:** Optimización para motores de búsqueda (SEO) y menor tiempo de visualización inicial.  
- **Desventajas:** Mayor latencia por respuesta del servidor y mayor consumo de recursos computacionales.

### SSG (Static Site Generation)
Estrategia en la que el contenido HTML se genera durante el proceso de construcción (build time). Los archivos resultantes son estáticos y pueden ser servidos de forma eficiente a través de una CDN.

- **Ventajas:** Rendimiento excepcional y seguridad mejorada.  
- **Desventajas:** Falta de dinamismo en tiempo real para datos que cambian con frecuencia.

---

## 4. Ciclos de Vida del Servidor

### Servidor de Desarrollo (Development Server)
Entorno local optimizado para la productividad del programador. Implementa funciones como Hot Module Replacement (HMR) y depuración en tiempo real. No está diseñado para soportar tráfico de producción.

### Servidor de Producción (Production Server)
Entorno final donde reside la aplicación optimizada. En contextos de SSR, requiere un entorno de ejecución de Node.js para procesar peticiones dinámicas. En contextos de SSG o CSR, puede limitarse a ser un servidor de archivos estáticos.

# Entorno y Herramientas del Proyecto React

Este proyecto utiliza varias herramientas y runtimes de JavaScript para el desarrollo frontend ligero.

---

## Node.js

* **Qué es:** Runtime de JavaScript que permite ejecutar JS fuera del navegador.
* **Uso:** Crear servidores, APIs, scripts y herramientas de desarrollo.
* **Comando básico:**

```bash
node app.js
```

---

## npm (Node Package Manager)

* **Qué es:** Gestor de paquetes de Node.js.
* **Uso:** Instalar librerías y dependencias del proyecto.
* **Ejemplo:**

```bash
npm install axios
```

---

## nvm (Node Version Manager)

* **Qué es:** Administrador de versiones de Node.js.
* **Uso:** Permite instalar y cambiar entre diferentes versiones de Node.
* **Ejemplo:**

```bash
nvm install 18
nvm use 18
```

---

## Vite

* **Qué es:** Herramienta de desarrollo frontend para proyectos modernos.
* **Uso:** Crear proyectos web con React, Vue, Svelte, etc., y servirlos en desarrollo rápido.
* **Ejemplo:**

```bash
npm create vite@latest
```

---

## Axios

* **Qué es:** Librería para hacer peticiones HTTP.
* **Uso:** Consumir APIs y enviar datos al backend.
* **Ejemplo:**

```javascript
axios.get("/api/users")
  .then(response => console.log(response.data))
```

---

## Relación entre las herramientas

```
nvm → Node.js → npm → { Vite, Axios }
```

* **nvm**: gestiona Node
* **Node.js**: ejecuta JavaScript
* **npm**: instala librerías
* **Vite**: entorno de desarrollo frontend
* **Axios**: cliente HTTP para consumir APIs

---