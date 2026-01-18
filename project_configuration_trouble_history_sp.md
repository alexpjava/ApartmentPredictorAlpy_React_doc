# Historial de Configuración y Resolución de Errores – Proyecto React

Este documento detalla los pasos seguidos para configurar el entorno de desarrollo en Windows y macOS (Big Sur 11.7), incluyendo la resolución de conflictos de versiones.

## 1. Entorno Windows

**Problema inicial:** Error de seguridad de PowerShell al ejecutar comandos `npm`.

* **Error:** `UnauthorizedAccess – Script execution is disabled`.
* **Causa:** Política de ejecución restrictiva de Windows PowerShell.
* **Solución:** Ejecutar PowerShell como administrador y ejecutar:

  ```powershell
  Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope LocalMachine
  ```

## 2. Entorno macOS (Big Sur 11.7)

El principal desafío fue la incompatibilidad entre versiones modernas de Node.js y Vite con macOS 11.7.

### A. Gestión de Versiones con NVM

Se instaló **NVM (Node Version Manager)** para evitar conflictos de permisos y gestionar versiones de Node.js.

* **Versión estable recomendada:** Node v18 (LTS).
* **Comandos:**

  ```zsh
  nvm install 18
  nvm use 18
  nvm alias default 18
  ```

---

### B. Error de Compatibilidad del Sistema (`dyld: Symbol not found`)

* **Error:** `Symbol not found: _SecTrustCopyCertificateChain`.
* **Causa:** Vite 7 y versiones modernas de `esbuild` requieren macOS 12 o superior.
* **Solución:** Realizar una degradación controlada de dependencias en `package.json` para garantizar la compatibilidad con Big Sur.

### C. Ajuste de Dependencias (`package.json`)

Las versiones se ajustaron para usar **Vite 4** y **React 18**:

```json
"dependencies": {
  "react": "^18.2.0",
  "react-dom": "^18.2.0"
},
"devDependencies": {
  "@vitejs/plugin-react": "^4.2.1",
  "vite": "^4.5.2"
}
```

---

### D. Conflicto del Compilador de React

* **Error:** `Missing "./compiler-runtime" specifier in "react" package`.
* **Causa:** El archivo `vite.config.js` incluía el nuevo compilador de React 19 (`babel-plugin-react-compiler`), mientras que el proyecto se había degradado a React 18 por compatibilidad con el sistema operativo.
* **Solución:** Editar `vite.config.js` para eliminar la configuración de Babel, dejando una configuración limpia del plugin de React:

  ```javascript
  // Antes
  react({
    babel: { plugins: [['babel-plugin-react-compiler']] }
  })

  // Después (Solución)
  react()
  ```

## 3. Comandos de Ejecución Diaria

Para iniciar el proyecto de forma consistente en cualquier sistema (Windows/macOS), sigue este flujo de trabajo en la terminal:

1. **Ubicación:** Abrir una terminal en la carpeta raíz del proyecto (`ApartmentPredictor-React`).
2. **Control de versión (solo macOS):** Asegurarse de usar la versión compatible de Node:

   ```bash
   nvm use 18
   ```
3. **Sincronización:** Instalar dependencias si se cambia de equipo o se descargan cambios desde Git:

   ```bash
   npm install
   ```
4. **Inicio:** Lanzar el servidor de desarrollo de Vite:

   ```bash
   npm run dev
   ```

---

## 4. Guía de Herramientas de Visualización en VS Code

Es esencial distinguir entre las herramientas para evitar pantallas en blanco o errores de compilación según el tipo de archivo.

### **Vite (`npm run dev`)**

* **Tipo:** Servidor de desarrollo especializado.
* **Uso:** **Obligatorio para React.** Procesa archivos `.jsx` y los sirve (normalmente en el puerto `5173`). Es el motor que traduce tu código para que el navegador pueda entenderlo.

### **Simple Browser**

* **Tipo:** Navegador interno de VS Code.
* **Uso:** Ideal para visualizar la app React sin salir del editor.
* **Cómo habilitarlo:** Presiona `Cmd + Shift + P` → busca `Simple Browser: Show` y pega la URL de Vite (por ejemplo `http://localhost:5173`).

### **Live Preview / Live Server**

* **Tipo:** Servidores de archivos estáticos.
* **Uso:** **Solo** para archivos HTML simples (como `test.html`).
* **Advertencia:** **No funcionan con React**, porque no pueden procesar la lógica de componentes ni los módulos modernos de JavaScript. Usarlos con React dará como resultado una página en blanco.

---

## 5. Configuración de Git (`.gitignore`)

Para mantener el repositorio limpio y profesional, crea un archivo `.gitignore` en la raíz del proyecto con el siguiente contenido:

```text
# Dependencias (regeneradas con npm install)
node_modules/
dist/
dist-ssr/
*.local

# Logs de errores
npm-debug.log*
yarn-debug.log*
yarn-error.log*

# Entorno y secretos (configuración local)
.env
.env.local
.env.development.local
.env.test.local
.env.production.local

# Archivos del sistema
.DS_Store
Thumbs.db

# Configuración del IDE
.vscode/
.idea/
*.suo
*.ntvs*
*.njsproj
```

## 6. Guía Completa: Configuración de Git y Despliegue en GitHub

Esta sección describe cómo inicializar el repositorio local, hacer el primer commit y enlazarlo con GitHub.

### Paso 1: Inicializar el Repositorio Local

Dentro de la carpeta del proyecto (`/Users/alex/Coding/WebApp26/ApartmentPredictorAlpy_React_doc`), ejecuta:

```bash
git init
```

### Paso 2: Crear el Repositorio en GitHub (Remoto)

Como el repositorio aún no existe en GitHub, créalo manualmente:

1. Ve a [GitHub.com](https://github.com).
2. Haz clic en el botón **"+"** (arriba a la derecha) y selecciona **"New repository"**.
3. Nómbralo: `ApartmentPredictorAlpy_React_doc`.
4. **IMPORTANTE:** Configúralo como Público o Privado, pero **NO marques**:

   * *Add a README*
   * *Add .gitignore*
   * *Choose a license*
     *(Estos ya existen localmente. Seleccionarlos causaría conflictos de historial al hacer push.)*
5. Haz clic en **"Create repository"**.

### Paso 3: Enlazar el Repositorio Local con GitHub y Hacer Push

Después de crear el repositorio, GitHub proporcionará una URL. Ejecuta los siguientes comandos:

1. **Renombrar la rama por defecto a `main`:**

   ```bash
   git branch -M main
   ```

2. **Conectar el repositorio local con GitHub**
   *(Reemplaza `YOUR_USERNAME` con tu nombre de usuario real de GitHub):*

   ```bash
   git remote add origin https://github.com/YOUR_USERNAME/ApartmentPredictorAlpy_React_doc
   ```

3. **Subir el proyecto por primera vez**
   *(La opción `-u` enlaza permanentemente las ramas local y remota):*

   ```bash
   git push -u origin main
   ```
