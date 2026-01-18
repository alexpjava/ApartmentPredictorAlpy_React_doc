# Project Configuration & Troubleshooting History - ApartmentPredictor-React

This document serves as a comprehensive log of the setup process, environment challenges, and solutions implemented to ensure compatibility across Windows and macOS (Big Sur 11.7).

---

## 1. Windows Environment Setup
**Issue:** PowerShell security error when executing `npm` commands.
* **Error:** `UnauthorizedAccess - Script execution is disabled on this system`.
* **Cause:** Default restrictive Execution Policy in Windows PowerShell.
* **Solution:** Ran PowerShell as Administrator and updated the policy:
    ```powershell
    Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope LocalMachine
    ```

---

## 2. macOS Environment (Big Sur 11.7) Compatibility
The primary challenge was the incompatibility of modern tools (Vite 7+, Node 20+) with macOS 11.7.

### A. Version Management with NVM
**NVM (Node Version Manager)** was used to manage Node.js versions without permission issues.
* **Target Version:** Node v18 (LTS) - highest stable for Big Sur.
* **Commands:**
    ```zsh
    nvm install 18
    nvm use 18
    nvm alias default 18
    ```

### B. System Compatibility Error (`dyld: Symbol not found`)
* **Error:** `Symbol not found: _SecTrustCopyCertificateChain`.
* **Cause:** Vite 7 and its `esbuild` dependency require macOS 12+.
* **Solution:** Downgraded core dependencies in `package.json` to **Vite 4** and **React 18**:
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

### C. React Compiler Specifier Conflict
* **Error:** `Missing "./compiler-runtime" specifier in "react" package`.
* **Cause:** `vite.config.js` was trying to use the React 19 Babel compiler on a React 18 project.
* **Solution:** Cleaned `vite.config.js` to remove the Babel plugin:
    ```javascript
    // Corrected configuration
    export default defineConfig({
      plugins: [react()],
      // ... proxy settings
    })
    ```

---

## 3. Daily Workflow Commands
Follow this sequence to start the project consistently on any system:

1.  **Location:** Open terminal in the root folder (`ApartmentPredictor-React`).
2.  **Version Check (Mac Only):** Ensure Node 18 is active:
    ```bash
    nvm use 18
    ```
3.  **Sync:** Install/update dependencies:
    ```bash
    npm install
    ```
4.  **Run:** Start the development server:
    ```bash
    npm run dev
    ```

---

## 4. VS Code Visualization Guide
To avoid "blank screen" errors, use the correct tool for the correct file type:

* **Vite (`npm run dev`)**: **Mandatory for React.** It compiles `.jsx` files. Default URL: `http://localhost:5173`.
* **Simple Browser**: Internal VS Code viewer. Use it by pressing `Cmd+Shift+P` -> `Simple Browser: Show` and pasting the Vite URL.
* **Live Preview / Live Server**: **For static HTML only** (e.g., `test.html`). Do not use for React components.

---

## 5. Git Configuration (.gitignore)
Create a `.gitignore` file in the root to prevent uploading unnecessary files:

```text
# Dependencies
node_modules/
dist/
dist-ssr/
*.local

# Logs
npm-debug.log*
yarn-debug.log*

# Environment / Secrets
.env
*.local

# OS / IDE Files
.DS_Store
.vscode/
.idea/