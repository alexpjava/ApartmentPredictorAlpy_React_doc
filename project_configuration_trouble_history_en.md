# Configuration History and Error Resolution – React Project

This document details the steps followed to configure the development environment on Windows and macOS (Big Sur 11.7), including the resolution of version conflicts.

## 1. Windows Environment

**Initial issue:** PowerShell security error when running `npm` commands.

* **Error:** `UnauthorizedAccess – Script execution is disabled`.
* **Cause:** Restrictive Windows PowerShell execution policy.
* **Solution:** Run PowerShell as administrator and execute:

  ```powershell
  Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope LocalMachine
  ```

## 2. macOS Environment (Big Sur 11.7)

The main challenge was the incompatibility between modern versions of Node.js and Vite with macOS 11.7.

### A. Version Management with NVM

**NVM (Node Version Manager)** was installed to avoid permission conflicts and manage Node.js versions.

* **Recommended stable version:** Node v18 (LTS).
* **Commands:**

  ```zsh
  nvm install 18
  nvm use 18
  nvm alias default 18
  ```

---

### B. System Compatibility Error (`dyld: Symbol not found`)

* **Error:** `Symbol not found: _SecTrustCopyCertificateChain`.
* **Cause:** Vite 7 and modern versions of `esbuild` require macOS 12 or higher.
* **Solution:** Perform a controlled downgrade of dependencies in `package.json` to ensure compatibility with Big Sur.

### C. Dependency Adjustment (`package.json`)

Versions were adjusted to use **Vite 4** and **React 18**:

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

### D. React Compiler Conflict

* **Error:** `Missing "./compiler-runtime" specifier in "react" package`.
* **Cause:** The `vite.config.js` file included the new React 19 compiler (`babel-plugin-react-compiler`), while the project had been downgraded to React 18 for OS compatibility.
* **Solution:** Edit `vite.config.js` to remove the Babel configuration, leaving a clean React plugin setup:

  ```javascript
  // Before
  react({
    babel: { plugins: [['babel-plugin-react-compiler']] }
  })

  // After (Fix)
  react()
  ```

## 3. Daily Execution Commands

To start the project consistently on any system (Windows/macOS), follow this workflow in the terminal:

1. **Location:** Open a terminal in the project root folder (`ApartmentPredictor-React`).
2. **Version Control (macOS only):** Ensure the compatible Node version is in use:

   ```bash
   nvm use 18
   ```
3. **Synchronization:** Install dependencies if switching machines or pulling changes from Git:

   ```bash
   npm install
   ```
4. **Startup:** Launch the Vite development server:

   ```bash
   npm run dev
   ```

---

## 4. VS Code Visualization Tools Guide

It is essential to distinguish between tools to avoid blank screens or compilation errors depending on the file type.

### **Vite (`npm run dev`)**

* **Type:** Specialized development server.
* **Usage:** **Mandatory for React.** Processes `.jsx` files and serves them (usually on port `5173`). It is the engine that translates your code so the browser can understand it.

### **Simple Browser**

* **Type:** Internal VS Code browser.
* **Usage:** Ideal for viewing the React app without leaving the editor.
* **How to enable:** Press `Cmd + Shift + P` → search for `Simple Browser: Show` and paste the Vite URL (e.g. `http://localhost:5173`).

### **Live Preview / Live Server**

* **Type:** Static file servers.
* **Usage:** **Only** for simple HTML files (such as `test.html`).
* **Warning:** **They do not work with React**, because they cannot process component logic or modern JavaScript modules. Using them with React will result in a blank page.

---

## 5. Git Configuration (`.gitignore`)

To keep the repository clean and professional, create a `.gitignore` file at the project root with the following content:

```text
# Dependencies (regenerated with npm install)
node_modules/
dist/
dist-ssr/
*.local

# Error logs
npm-debug.log*
yarn-debug.log*
yarn-error.log*

# Environment and secrets (local configuration)
.env
.env.local
.env.development.local
.env.test.local
.env.production.local

# System files
.DS_Store
Thumbs.db

# IDE configuration
.vscode/
.idea/
*.suo
*.ntvs*
*.njsproj
```

## 6. Complete Guide: Git Setup and GitHub Deployment

This section describes how to initialize the local repository, make the first commit, and link it to GitHub.

### Step 1: Initialize the Local Repository

Inside the project folder (`/Users/alex/Coding/WebApp26/ApartmentPredictorAlpy_React_doc`), run:

```bash
git init
```

### Step 2: Create the GitHub Repository (Remote)

Since the repository does not yet exist on GitHub, create it manually:

1. Go to [GitHub.com](https://github.com).
2. Click the **"+"** button (top-right) and select **"New repository"**.
3. Name it: `ApartmentPredictorAlpy_React_doc`.
4. **IMPORTANT:** Set it as Public or Private, but **DO NOT check**:

   * *Add a README*
   * *Add .gitignore*
   * *Choose a license*
     *(These already exist locally. Selecting them would cause history conflicts when pushing.)*
5. Click **"Create repository"**.

### Step 3: Link Local Repository to GitHub and Push

After creating the repository, GitHub will provide a URL. Run the following commands:

1. **Rename the default branch to `main`:**

   ```bash
   git branch -M main
   ```

2. **Connect the local repository to GitHub**
   *(Replace `YOUR_USERNAME` with your actual GitHub username):*

   ```bash
   git remote add origin https://github.com/YOUR_USERNAME/ApartmentPredictorAlpy_React_doc
   ```

3. **Push the project for the first time**
   *(The `-u` flag permanently links the local and remote branches):*

   ```bash
   git push -u origin main
   ```

