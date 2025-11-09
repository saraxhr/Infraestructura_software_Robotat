# 🌐 Proyecto Robotat Web — Universidad del Valle de Guatemala

Este repositorio contiene la **infraestructura web** del proyecto **Robotat UVG**, orientado al desarrollo de un laboratorio de robótica accesible localmente y con propuesta para conexión remota futura.  
Incluye tanto el **backend en Django** como el **frontend en React con Vite y TailwindCSS**.

---

## 🧭 Estructura del repositorio

```
pagina_web/
│
├── backend/               # Lógica del servidor (Django)
│   ├── robotat_web/       # Proyecto Django generado con 'django-admin startproject'
│   └── manage.py          # Utilidad administrativa de Django
│
├── frontend/              # Interfaz de usuario (React + Vite + TailwindCSS)
│   ├── package.json       # Dependencias del frontend
│   ├── package-lock.json  # Versiones exactas de dependencias
│
├── requirements.txt       # Dependencias de Python necesarias para el backend
└── README.md              # Este archivo
```

---

## ⚙️ Instalación del entorno

### 🔹 1. Clonar el repositorio
```bash
git clone https://github.com/tu_usuario/pagina_web.git
cd pagina_web
```

### 🔹 2. Configurar el entorno de Python (backend)
Crea y activa un entorno virtual, luego instala dependencias:
```bash
python -m venv venv
venv\Scripts\activate      # En Windows
source venv/bin/activate   # En Linux/macOS

pip install -r requirements.txt
```

### 🔹 3. Configurar el entorno Node.js (frontend)
Desde la carpeta `frontend`:
```bash
cd frontend
npm install
```

---

## 🚀 Ejecución de los servidores

### 🔸 **Backend (Django + Daphne)**
El proyecto fue creado con:
```bash
django-admin startproject robotat_web
```

Para ejecutar el servidor con **Daphne (ASGI)**:
```bash
cd backend
daphne -p 8000 robotat_web.asgi:application
```

### 🔸 **Frontend (React + Vite)**
Desde la carpeta `frontend`:
```bash
npm run dev
```

Luego abre tu navegador en [http://localhost:5173](http://localhost:5173)

---

## 🧩 Dependencias principales

### 📦 Backend (`requirements.txt`)
Algunas de las librerías clave:
- **Django 5.2.4** – Framework web principal.
- **Django REST Framework** – API RESTful.
- **Daphne** – Servidor ASGI compatible con WebSockets.
- **SimpleJWT** – Autenticación basada en tokens JWT.
- **OpenCV / NumPy / Requests** – Procesamiento de video, cálculos y comunicación HTTP.

### 📦 Frontend (`package.json`)
- **React** – Framework de interfaz.
- **Vite** – Herramienta de desarrollo rápida.
- **TailwindCSS** – Estilos modernos y responsivos.

---

## 🧠 Recursos útiles

- 📘 Documentación oficial de Django:  
  https://docs.djangoproject.com/en/5.2/

- ▶️ Tutorial para iniciar un proyecto Django:  
  [https://www.youtube.com/watch?v=KCrXgy8qtjM](https://www.youtube.com/watch?v=KCrXgy8qtjM)

- 💡 Crear un proyecto React con Vite y Tailwind:  
  https://vitejs.dev/guide/  
  https://tailwindcss.com/docs/guides/vite  

---

## 🧰 Comandos rápidos

| Acción | Comando |
|--------|----------|
| Crear nuevo proyecto Django | `django-admin startproject nombre_proyecto` |
| Crear nueva app en Django | `python manage.py startapp nombre_app` |
| Aplicar migraciones | `python manage.py migrate` |
| Crear superusuario | `python manage.py createsuperuser` |
| Ejecutar servidor ASGI | `daphne -p 8000 robotat_web.asgi:application` |
| Instalar dependencias del frontend | `npm install` |
| Ejecutar entorno React | `npm run dev` |

---

## 🧾 Licencia
Proyecto académico desarrollado en la **Universidad del Valle de Guatemala (UVG)** como parte del trabajo de graduación:  
**“Diseño e implementación de infraestructura de software para la conexión remota sincrónica con el laboratorio Robotat”**.

---
