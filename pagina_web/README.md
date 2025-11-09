# 📦 Documentación de archivos clave — `pagina_web`

Este README explica **qué son**, **cómo se generan** y **cómo usar** los cuatro archivos que agregaste en tu repositorio raíz `pagina_web`:

- `manage.py`
- `package.json`
- `package-lock.json`
- `requirements.txt`

> **Contexto de estructura sugerida**
>
> ```txt
> pagina_web/
> ├─ backend/
> │  └─ robotat_web/        # Proyecto Django (creado con startproject)
> ├─ frontend/              # Proyecto React/Vite (npm)
> ├─ manage.py              # Utilidad administrativa (Django)
> ├─ package.json           # Manifiesto npm (si decides tener uno en la raíz)
> ├─ package-lock.json      # Lockfile npm
> └─ requirements.txt       # Dependencias Python
> ```
>
> *Si prefieres aislar frontend, también puedes mover `package.json`/`package-lock.json` a `frontend/`.*

---

## 1) `manage.py` (Django)

### 🧠 ¿Qué es?
Script de utilidad de Django para ejecutar tareas administrativas: migraciones, creación de superusuarios, ejecutar comandos de management, etc.

### 🛠️ ¿Cómo se genera?
Se **crea automáticamente** al iniciar un proyecto con:
```bash
# Dentro de pagina_web/backend/
django-admin startproject robotat_web
```
Esto produce:
```txt
backend/
├─ manage.py
└─ robotat_web/
   ├─ settings.py
   ├─ urls.py
   ├─ asgi.py
   └─ wsgi.py
```

> Tu `manage.py` actual apunta a `robotat_web.settings`, lo cual es correcto para un proyecto llamado `robotat_web`.

### ▶️ Comandos clave con `manage.py`
```bash
# Desde pagina_web/backend/
python manage.py migrate
python manage.py createsuperuser
python manage.py startapp nombre_app
# (si usaras runserver de desarrollo)
python manage.py runserver
```

### 💡 Nota si usas Daphne
Para producción/ASGI estás ejecutando:
```bash
# Desde pagina_web/backend/
daphne -p 8000 robotat_web.asgi:application
```
`manage.py` **se mantiene** para comandos administrativos.

---

## 2) `package.json` (Node.js / npm)

### 🧠 ¿Qué es?
Manifiesto de dependencias y scripts del entorno JavaScript. Define el nombre del paquete, dependencias (`dependencies`/`devDependencies`) y scripts (`npm run ...`).

### 🛠️ ¿Cómo se genera?
Opción A: **Inicialización vacía en una carpeta** (por ejemplo raíz o `frontend/`):
```bash
npm init -y
```
> Esto crea un `package.json` mínimo.

Opción B (**recomendada para React con Vite**): scaffold directo en `frontend/`:
```bash
# Desde pagina_web/
npm create vite@latest frontend -- --template react-ts
cd frontend
npm install
```
> Este flujo crea su propio `package.json` **dentro de `frontend/`** con React, Vite y TypeScript preconfigurados.

### ➕ Añadir dependencias
```bash
# Ejemplos:
npm install react react-dom
npm install -D @types/node typescript vite
```

### ▶️ Scripts típicos (si usas Vite)
```json
{
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
  }
}
```
```bash
# Uso
npm run dev
npm run build
npm run preview
```

---

## 3) `package-lock.json` (npm lockfile)

### 🧠 ¿Qué es?
Archivo **autogenerado** por `npm` que registra las **versiones exactas** de todas las dependencias instaladas (y sus árboles). Garantiza instalaciones reproducibles.

### 🛠️ ¿Cómo se genera? 
Se crea/actualiza al correr:
```bash
npm install
```
o al agregar/quitar dependencias (`npm install paquete`, `npm uninstall paquete`).

### 🔁 Reconstrucción exacta en otra máquina
```bash
# Con el lockfile presente:
npm ci
```
> `npm ci` usa el `package-lock.json` para instalar **exactamente** las versiones bloqueadas (ideal para CI/producción).

---

## 4) `requirements.txt` (Python)

### 🧠 ¿Qué es?
Lista de dependencias Python (con versiones) que tu backend necesita para ejecutarse.

### 🛠️ ¿Cómo se genera?
En tu entorno virtual (activado) después de instalar tus paquetes:
```bash
# Ejemplo: instalación de dependencias
pip install django djangorestframework djangorestframework-simplejwt daphne requests numpy opencv-python

# Congelar a requirements.txt
pip freeze > requirements.txt
```
> Esto captura versiones exactas. Si ya tienes `requirements.txt`, puedes instalarlo así:
```bash
pip install -r requirements.txt
```

### ✅ Paquetes típicos en tu caso
- `Django`, `djangorestframework`, `djangorestframework-simplejwt` (API y auth)
- `daphne` (servidor ASGI)
- `requests`, `numpy`, `opencv-python` (utilidades de video/cómputo)
- Otros utilitarios: `asgiref`, `sqlparse`, etc.

---

## 🧪 Comandos “de cero a funcionando” (receta resumida)

### A) Backend (Python/Django)
```bash
# 1) Crear/activar entorno virtual
python -m venv venv
# Windows
venv\Scripts\activate
# Linux/macOS
# source venv/bin/activate

# 2) Crear proyecto Django (si aún no existe)
cd backend
django-admin startproject robotat_web

# 3) Instalar dependencias
cd ..        # regresar a raiz si el requirements está en pagina_web/
pip install -r requirements.txt

# 4) Migraciones y superusuario
cd backend
python manage.py migrate
python manage.py createsuperuser

# 5) Ejecutar con Daphne (ASGI)
daphne -p 8000 robotat_web.asgi:application
```

### B) Frontend (Vite/React recomendado dentro de `frontend/`)
```bash
# 1) Scaffold del frontend (si no existe)
npm create vite@latest frontend -- --template react-ts

# 2) Instalar dependencias del frontend
cd frontend
npm install

# 3) Ejecutar entorno de desarrollo
npm run dev
```

---

## 📌 Buenas prácticas de versionado (Git)

**Sube al repo:**
- `manage.py`
- `requirements.txt`
- `package.json`
- `package-lock.json`

**Ignora en el repo (`.gitignore`):**
```
# Python
venv/
__pycache__/
*.pyc

# Node
node_modules/

# OS/IDE
.DS_Store
.idea
.vscode/
```

---

## ❓ Preguntas rápidas

- **¿Debe existir `package.json` en la raíz y también en `frontend/`?**  
  Lo habitual es mantener **uno dentro de `frontend/`** para aislar el mundo npm del frontend. Solo usa un `package.json` en la raíz si **realmente** necesitas scripts npm a nivel de monorepo.

- **¿Puedo regenerar `requirements.txt` si agrego/quito librerías?**  
  Sí. Tras instalar/desinstalar paquetes en tu venv, ejecuta nuevamente: `pip freeze > requirements.txt`.

- **¿Qué pasa si borro `package-lock.json`?**  
  Se recrea en la próxima instalación (`npm install`). Sin embargo, **conservarlo** asegura instalaciones reproducibles (y `npm ci`).

---

### ✅ Resumen
- **`manage.py`**: lo crea `startproject`, úsalo para tareas Django.
- **`package.json`**: lo crea `npm init` (o el scaffold de Vite); define dependencias/scripts.
- **`package-lock.json`**: lo crea `npm install`; bloquea versiones exactas.
- **`requirements.txt`**: lo creas con `pip freeze > requirements.txt`; instala con `pip install -r`.

¡Listo! Con esto puedes documentar exactamente **qué son** y **cómo se generan** estos archivos en tu repo.
