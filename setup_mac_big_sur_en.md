# Installation History — React + Vite on macOS Big Sur (11)

## 📌 System Context

* **Operating System:** macOS Big Sur (11)
* **Architecture:** Intel
* **Goal:** Create React projects using Vite
* **Editor:** Visual Studio Code

---

## 🔢 Compatible Versions Used

| Tool    | Version        |
| ------- | -------------- |
| Node.js | v18.20.8 (LTS) |
| npm     | v10.8.2        |
| Vite    | v4.x           |
| React   | 18             |

> ⚠️ Newer versions of Vite are **NOT compatible** with macOS Big Sur.

---

## 🧪 Issues Encountered

### ❌ Issue 1 — `EBADENGINE Unsupported engine`

**Message:**

```
@vitejs/plugin-react@5.x
required: node ^20.19.0 || >=22
current: node v18.20.8
```

**Cause:**

* Vite 7 and plugin-react 5 require Node 20+
* Node 18 does not meet the requirement

---

### ❌ Issue 2 — `dyld: Symbol not found: _SecTrustCopyCertificateChain`

**Key message:**

```
esbuild (built for Mac OS X 12.0)
Symbol not found in Security.framework
```

**Cause:**

* `esbuild` binary was compiled for macOS 12
* Big Sur (11) does not include this symbol
* Critical failure during `npm install`

---

## ✅ Applied Solution (Stable & Recommended)

👉 **Use Vite 4**, which:

* Is compatible with Node 18
* Works correctly on macOS Big Sur
* Does not require upgrading the OS

---

## 🧹 Cleanup (after failed attempts)

```bash
cd ..
rm -rf react-app
```

---

## ⚛️ Create a React Project with Vite (Compatible Version)

### 1️⃣ Create the project

```bash
npm create vite@4
```

### 2️⃣ Answer the setup prompts

```
Project name: react-app
Framework: React
Variant: JavaScript
```

---

### 3️⃣ Enter the project directory

```bash
cd react-app
```

---

### 4️⃣ Install dependencies

```bash
npm install
```

---

### 5️⃣ Start the development server

```bash
npm run dev
```

Expected output:

```
Local: http://localhost:5173/
```

---

## 👀 View React Inside VS Code

1. Copy the URL (`http://localhost:5173`)
2. Press `Cmd + Shift + P`
3. Run **Simple Browser: Show**
4. Paste the URL

---

## ✅ Final Status

* React running successfully
* Hot Reload enabled
* Stable development environment
* Fully compatible with macOS Big Sur (11)

---

## 🧠 Lessons Learned

* Do not always use the latest versions
* Always check compatibility between Node, Vite, and OS
* Node 18 + Vite 4 is a safe combination for older Macs
* `esbuild` errors are often **OS compatibility issues**

---