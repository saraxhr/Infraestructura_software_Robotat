# 📸 Aplicación Django — Control de Cámaras IP (Robotat UVG)

## 📘 Descripción general
La carpeta **`camaras/`** forma parte del backend del proyecto **Robotat UVG** y está desarrollada con **Django**.  
Su propósito es permitir el **control y monitoreo básico de cámaras IP Amcrest**, brindando endpoints que permiten:

- Consultar si una cámara está **en línea (online/offline)**.  
- Enviar **comandos PTZ (Pan, Tilt, Zoom, Home)** mediante solicitudes HTTP seguras.  
- Preparar la infraestructura para la transmisión en vivo, que se maneja por separado en `flask_video_server/`.

---

## ⚙️ Estructura de la carpeta

```
camaras/
│
├── apps.py        → Configura la app "camaras" dentro del proyecto Django.
├── config.py      → Define IPs, usuarios, contraseñas y URL RTSP de las cámaras Amcrest.
├── urls.py        → Declara los endpoints HTTP que exponen las funciones de la app.
└── views.py       → Implementa la lógica para control PTZ y verificación de conexión.
```

---

## 📦 Instalación de dependencias

Antes de ejecutar esta aplicación, asegúrate de tener instalado **Python 3.9 o superior**.  
Luego instala las dependencias necesarias ejecutando:

```bash
pip install django requests
```

| Librería | Descripción |
|-----------|-------------|
| **Django** | Framework principal del backend, maneja rutas, vistas y respuestas HTTP. |
| **Requests** | Permite enviar solicitudes HTTP hacia las cámaras Amcrest mediante autenticación Digest. |

---

## 🧱 Creación del proyecto Django y registro de la app

Si aún no tienes tu proyecto base configurado, puedes crearlo con los siguientes comandos:

```bash
# 1️⃣ Crear el proyecto principal
django-admin startproject robotat_web

# 2️⃣ Entrar al proyecto
cd robotat_web

# 3️⃣ Crear la aplicación de cámaras
python manage.py startapp camaras

# 4️⃣ Agregar la app al archivo settings.py
# Abre robotat_web/settings.py y agrega en INSTALLED_APPS:
# 'camaras',
```

✅ Esto crea la estructura base del proyecto Django y habilita la aplicación `camaras` para ejecutarse correctamente.

---

## 🧩 Descripción detallada de los archivos

### 📄 `apps.py`
Define la clase `CamarasConfig`, que registra la aplicación **“camaras”** dentro del proyecto Django.  
Esto permite que Django cargue e inicialice la app al arrancar el servidor.

---

### ⚙️ `config.py`
Contiene el diccionario `CAMERAS`, donde se definen las cámaras Amcrest disponibles.  
Cada entrada incluye la IP, usuario, contraseña y la URL RTSP para transmisión de video.

```python
CAMERAS = {
    "1": {
        "ip": "192.168.50.211",
        "user": "admin",
        "password": "UVG12345678",
        "rtsp": "rtsp://admin:UVG12345678@192.168.50.211:554/cam/realmonitor?channel=1&subtype=1",
    },
    "2": {
        "ip": "192.168.50.212",
        "user": "admin",
        "password": "12345678UVG",
        "rtsp": "rtsp://admin:12345678UVG@192.168.50.212:554/cam/realmonitor?channel=1&subtype=1",
    }
}
```

📌 **Notas importantes:**
- Puedes agregar más cámaras duplicando la estructura y cambiando el número de ID (`"3"`, `"4"`, etc.).  
- `subtype=1` usa el **substream** (flujo de menor resolución), ideal para transmisión fluida con baja latencia.  
- Este archivo es el único que debes modificar si cambian las IPs o contraseñas de las cámaras.

---

### 🌐 `urls.py`
Define las rutas HTTP (endpoints) que permiten interactuar con las cámaras.  
Cada endpoint está vinculado a una función dentro de `views.py`.

| Endpoint | Método | Función asociada | Descripción |
|-----------|---------|------------------|--------------|
| `/api/cameras/<id>/status/` | `GET` | `status_view` | Verifica si una cámara está en línea (online/offline). |
| `/api/cameras/<id>/ptz/` | `POST` | `ptz_view` | Envía comandos PTZ (movimiento de cámara). |

Para activar estas rutas dentro del proyecto principal (`robotat_web`), abre `robotat_web/urls.py` y agrega:

```python
from django.urls import path, include

urlpatterns = [
    path('', include('camaras.urls')),  # Habilita las rutas de la app "camaras"
]
```

---

### 🧠 `views.py`
Contiene la lógica principal de la aplicación.  
Aquí se implementan las funciones que gestionan las solicitudes y comunican Django con las cámaras Amcrest mediante HTTP Digest Authentication.

#### 🔹 1. `status_view(request, cam_id)`
- **Ruta:** `/api/cameras/<id>/status/`  
- **Método:** `GET`  
- **Función:** Comprueba si la cámara responde correctamente.  
- **Respuesta JSON:**
  ```json
  {"online": true}
  ```
- **Ejemplo de uso (desde navegador o Postman):**
  ```
  http://127.0.0.1:8000/api/cameras/1/status/
  ```

---

#### 🔹 2. `ptz_view(request, cam_id)`
- **Ruta:** `/api/cameras/<id>/ptz/`  
- **Método:** `POST`  
- **Función:** Envía comandos de movimiento PTZ a una cámara IP.  
- **Entrada esperada (JSON):**
  ```json
  {"cmd": "up", "speed": 5}
  ```
- **Comandos válidos:**  
  `up`, `down`, `left`, `right`, `home`, `center`, `reset`  

- **Ejemplo (terminal con cURL):**
  ```bash
  curl -X POST http://127.0.0.1:8000/api/cameras/1/ptz/   -H "Content-Type: application/json"   -d '{"cmd": "left", "speed": 4}'
  ```

- **Respuesta esperada:**
  ```json
  {"ok": true}
  ```

---

## 🚀 Ejecución del servidor Django

Desde la raíz del proyecto (`robotat_web/`):

```bash
python manage.py runserver
```

Por defecto el servidor se ejecutará en:

```
http://127.0.0.1:8000/
```

Luego podrás acceder a los endpoints:

```
http://127.0.0.1:8000/api/cameras/1/status/
http://127.0.0.1:8000/api/cameras/1/ptz/
```

---

## 🧰 Requisitos del sistema

| Componente | Versión recomendada |
|-------------|---------------------|
| **Python** | 3.9 o superior |
| **Django** | 5.0+ |
| **Requests** | 2.31+ |

---

## ⚠️ Notas importantes

- Esta aplicación **no transmite video**; su función es únicamente el **control y monitoreo** de las cámaras.  
  La transmisión MJPEG se maneja desde la carpeta `flask_video_server/`.
- Si las cámaras no responden:
  - Verifica que las IPs estén accesibles desde tu red local.
  - Confirma que las credenciales en `config.py` sean correctas.
  - Asegúrate de que el servicio **CGI** esté habilitado en las cámaras Amcrest.
- Los comandos PTZ se envían mediante **HTTP Digest Authentication**, un método seguro frente a ataques por texto plano.

---

## 🧾 Créditos

**Autora:** Sara Hernández  

**Asesor:** MSc. Miguel Zea  
**Colaboración:** ChatGPT (OpenAI)
