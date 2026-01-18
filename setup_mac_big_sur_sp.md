# Historial de instalación — React + Vite en macOS Big Sur (11)

## 📌 Contexto del sistema

* **Sistema operativo:** macOS Big Sur (11)
* **Arquitectura:** Intel
* **Objetivo:** Crear proyectos React usando Vite
* **Editor:** Visual Studio Code

---

## 🔢 Versiones utilizadas (compatibles)

| Herramienta | Versión        |
| ----------- | -------------- |
| Node.js     | v18.20.8 (LTS) |
| npm         | v10.8.2        |
| Vite        | v4.x           |
| React       | 18             |

> ⚠️ Versiones más nuevas de Vite **NO son compatibles** con Big Sur.

---

## 🧪 Problemas encontrados

### ❌ Error 1 — `EBADENGINE Unsupported engine`

**Mensaje:**

```
@vitejs/plugin-react@5.x
required: node ^20.19.0 || >=22
current: node v18.20.8
```

**Causa:**

* Vite 7 y plugin-react 5 requieren Node 20+
* Node 18 no cumple el requisito

---

### ❌ Error 2 — `dyld: Symbol not found: _SecTrustCopyCertificateChain`

**Mensaje clave:**

```
esbuild (built for Mac OS X 12.0)
Symbol not found in Security.framework
```

**Causa:**

* `esbuild` compilado para macOS 12
* Big Sur (11) no contiene ese símbolo
* Error crítico al ejecutar `npm install`

---

## ✅ Solución aplicada (correcta y estable)

👉 **Usar Vite 4**, que:

* Es compatible con Node 18
* Funciona correctamente en macOS Big Sur
* No requiere actualizar el sistema operativo

---

## 🧹 Limpieza previa (si hubo intentos fallidos)

```bash
cd ..
rm -rf react-app
```

---

## ⚛️ Crear proyecto React con Vite (versión compatible)

### 1️⃣ Crear el proyecto

```bash
npm create vite@4
```

### 2️⃣ Responder al asistente

```
Project name: react-app
Framework: React
Variant: JavaScript
```

---

### 3️⃣ Entrar al proyecto

```bash
cd react-app
```

---

### 4️⃣ Instalar dependencias

```bash
npm install
```

---

### 5️⃣ Ejecutar servidor de desarrollo

```bash
npm run dev
```

Salida esperada:

```
Local: http://localhost:5173/
```

---

## 👀 Ver React dentro de VS Code

1. Copiar la URL (`http://localhost:5173`)
2. Presionar `Cmd + Shift + P`
3. Ejecutar **Simple Browser: Show**
4. Pegar la URL

---

## ✅ Estado final

* React funcionando correctamente
* Hot Reload activo
* Entorno estable para aprendizaje y proyectos
* Compatible con macOS Big Sur (11)

---

## 🧠 Lecciones aprendidas

* No siempre usar la última versión
* Revisar compatibilidad de Node + Vite + OS
* Node 18 + Vite 4 es una combinación segura en Mac antiguos
* Los errores de `esbuild` suelen ser de **sistema operativo**

---
