# Historial de Configuración y Resolución de Errores - Proyecto React

Este documento detalla los pasos seguidos para configurar el entorno de desarrollo en Windows y macOS (Big Sur 11.7), incluyendo la resolución de conflictos de versiones.

## 1. Entorno Windows
**Problema inicial:** Error de seguridad en PowerShell al ejecutar comandos `npm`.
* **Error:** `UnauthorizedAccess - La ejecución de scripts está deshabilitada`.
* **Causa:** Política de ejecución restrictiva de Windows PowerShell.
* **Solución:** Ejecutar PowerShell como administrador y aplicar:
    ```powershell
    Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope LocalMachine
    ```

## 2. Entorno macOS (Big Sur 11.7)
El mayor reto fue la incompatibilidad de las versiones modernas de Node y Vite con el sistema operativo macOS 11.7.

### A. Gestión de versiones con NVM
Se instaló **NVM (Node Version Manager)** para evitar conflictos de permisos y manejar versiones de Node.js.
* **Versión recomendada para estabilidad:** Node v18 (LTS).
* **Comandos:**
    ```zsh
    nvm install 18
    nvm use 18
    nvm alias default 18
    ```
---

### B. Error de Compatibilidad de Sistema (`dyld: Symbol not found`)
* **Error:** `Symbol not found: _SecTrustCopyCertificateChain`.
* **Causa:** Vite 7 y `esbuild` moderno requieren macOS 12 o superior.
* **Solución:** Realizar un "downgrade" controlado de las dependencias en `package.json` para asegurar compatibilidad con Big Sur.

### C. Ajuste de Dependencias (package.json)
Se modificaron las versiones para usar **Vite 4** y **React 18**:
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

### D. Conflicto con React Compiler
* **Error:** `Missing "./compiler-runtime" specifier in "react" package`.
* **Causa:** El archivo de configuración `vite.config.js` incluía el nuevo compilador de React 19 (`babel-plugin-react-compiler`), pero el proyecto se bajó a React 18 por compatibilidad con el sistema operativo.
* **Solución:** Se editó `vite.config.js` para eliminar la configuración de Babel, dejando el plugin de React limpio:
  ```javascript
  // Antes
  react({
    babel: { plugins: [['babel-plugin-react-compiler']] }
  })
  
  // Después (Corrección)
  react()

## 3. Comandos de Ejecución Diaria

Para arrancar el proyecto de forma consistente en cualquier sistema (Windows/Mac), sigue este flujo en la terminal:

1.  **Ubicación:** Abrir la terminal en la carpeta raíz del proyecto (`ApartmentPredictor-React`).
2.  **Control de Versión (Solo Mac):** Asegurar que se usa la versión compatible:
    ```bash
    nvm use 18
    ```
3.  **Sincronización:** Instalar dependencias si hay cambios en el equipo o se descargan cambios de Git:
    ```bash
    npm install
    ```
4.  **Encendido:** Lanzar el servidor de desarrollo de Vite:
    ```bash
    npm run dev
    ```
---
## 4. Guía de Herramientas de Visualización en VS Code

Es fundamental distinguir las herramientas para evitar pantallas en blanco o errores de compilación según el tipo de archivo:

### **Vite (`npm run dev`)**
* **Tipo:** Servidor de desarrollo especializado.
* **Uso:** **Obligatorio para React.** Procesa archivos `.jsx` y los sirve normalmente en el puerto `5173`. Es el "motor" que traduce tu código para que el navegador lo entienda.

### **Simple Browser**
* **Tipo:** Visor interno de VS Code.
* **Uso:** Ideal para ver la App de React sin salir del editor. 
* **Cómo activarlo:** Pulsa `Cmd + Shift + P` -> busca `Simple Browser: Show` y pega la URL que te da Vite (ej. `http://localhost:5173`).

### **Live Preview / Live Server**
* **Tipo:** Servidores de archivos estáticos.
* **Uso:** **Solo** para archivos HTML simples (como `prueba.html`).
* **Advertencia:** **No funcionan con React** porque no pueden procesar la lógica de los componentes ni los módulos de JavaScript moderno. Si los usas con React, verás la página en blanco.



---
## 5. Configuración de Git (.gitignore)

Para mantener el repositorio ligero y profesional, se debe crear un archivo llamado `.gitignore` en la raíz del proyecto con el siguiente contenido:

```text
# Dependencias (Se regeneran con npm install)
node_modules/
dist/
dist-ssr/
*.local

# Logs de errores
npm-debug.log*
yarn-debug.log*
yarn-error.log*

# Entorno y Secretos (Configuraciones locales)
.env
.env.local
.env.development.local
.env.test.local
.env.production.local

# Archivos del Sistema
.DS_Store
Thumbs.db

# Configuración de IDEs
.vscode/
.idea/
*.suo
*.ntvs*
*.njsproj
``````