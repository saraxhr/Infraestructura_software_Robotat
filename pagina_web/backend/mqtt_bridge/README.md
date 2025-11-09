# 🧩 Backend – Interfaz (Robotat UVG)

## 📘 Descripción general

Esta carpeta contiene el módulo **`interfaz`**, una aplicación de Django que gestiona la **autenticación, administración de usuarios y registro de actividad** dentro del sistema Robotat UVG.

Integra funcionalidades de:
- Inicio y cierre de sesión con y sin JWT.  
- Control de roles (Administrador, Estudiante, Investigador).  
- Cambio de contraseñas con políticas seguras.  
- Registro de sesiones, logins y estadísticas de uso.  
- Comunicación con el broker MQTT (para control del Pololu).

---

## 📂 Estructura de archivos

```
interfaz/
│
├── admin.py                # Configura la vista de usuarios en el panel admin de Django.
├── apps.py                 # Define la clase principal de configuración de la app.
├── models.py               # Contiene los modelos de base de datos (usuarios, sesiones, estadísticas).
├── serializers.py          # Serializadores DRF para usuarios, contraseñas y registros.
├── serializers_jwt.py      # Serializer JWT personalizado (login con email y claims adicionales).
└── views.py                # Endpoints principales de autenticación, CRUD y comunicación MQTT.
```

---

## ⚙️ Dependencias necesarias

Asegúrate de tener instaladas las siguientes librerías antes de ejecutar el servidor Django:

```bash
pip install django djangorestframework djangorestframework-simplejwt paho-mqtt
```

---

## 🔐 Configuración importante

1. En el archivo `settings.py` del proyecto principal, incluir la app:
   ```python
   INSTALLED_APPS = [
       ...,
       'rest_framework',
       'rest_framework_simplejwt',
       'interfaz',
   ]
   ```

2. Configurar Django REST Framework y JWT (en `settings.py`):
   ```python
   REST_FRAMEWORK = {
       'DEFAULT_AUTHENTICATION_CLASSES': (
           'rest_framework_simplejwt.authentication.JWTAuthentication',
       ),
       'DEFAULT_PERMISSION_CLASSES': (
           'rest_framework.permissions.IsAuthenticated',
       ),
   }
   ```

3. Asegúrate de tener configurado el broker MQTT local o remoto (ver carpeta `mqtt_bridge`).

---

## 🔧 Ejecución del servidor

Para correr el backend desde la raíz del proyecto Django:

```bash
python manage.py runserver
```

El servidor se levantará por defecto en  
👉 `http://127.0.0.1:8000/`

---

## 🚀 Endpoints principales

| Tipo | Endpoint | Descripción |
|------|-----------|-------------|
| `POST` | `/api/login/` | Login con JWT personalizado |
| `POST` | `/api/login-simple/` | Login básico (sin token) |
| `POST` | `/api/logout/` | Cierra sesión y calcula tiempo de uso |
| `POST` | `/api/auth/password/change/` | Cambio de contraseña con sesión activa |
| `POST` | `/api/auth/password/change-direct/` | Cambio de contraseña con credenciales |
| `GET`  | `/api/mi-perfil/` | Devuelve información del usuario autenticado |
| `GET`  | `/api/logins/` | Lista de logins del día (solo admin) |
| `GET`  | `/api/statistics/` | Muestra estadísticas de uso diario |
| `POST` | `/api/enviar-comando/` | Envía comandos MQTT al robot Pololu |

---

## 💾 Modelos definidos

- **`UsuarioPersonalizado`** – Modelo de usuario principal, basado en `AbstractBaseUser`.  
- **`LoginRecord`** – Registra cada inicio de sesión.  
- **`UserStatistic`** – Acumula tiempo total de uso por día.  
- **`UserSession`** – Calcula duración de sesión activa.  

---

## 🧠 Notas adicionales

- El campo de autenticación principal es **`email`** (no `username`).  
- El serializer JWT incluye en el token los campos:
  - `email`
  - `nombre`
  - `role`
- Los endpoints están protegidos con autenticación JWT.  
- El sistema registra automáticamente las sesiones y actualiza métricas al cerrar sesión.

---

## ▶️ Cómo probar la API

Puedes usar **Postman** o **curl**.  
Ejemplo de login con JWT:

```bash
curl -X POST http://127.0.0.1:8000/api/login/      -H "Content-Type: application/json"      -d '{"email": "admin@uvg.edu.gt", "password": "tu_contraseña"}'
```

---

## 🧩 Integración con MQTT

El endpoint `/api/enviar-comando/` permite enviar paquetes MQTT estructurados hacia el robot Pololu.  
Depende del módulo `mqtt_bridge.mqtt_client` ubicado en  
`backend/mqtt_bridge/`.

Ejemplo de JSON enviado:

```json
{
  "src": 1,
  "pts": 5,
  "ptp": 10,
  "pid": 3,
  "cks": "a1b2c3",
  "pld": {"v_l": 0.2, "v_r": 0.2}
}
```

---

## 🧱 Autoría y contexto

Proyecto desarrollado como parte del sistema **Robotat UVG**,  
para la gestión local y futura conexión remota del laboratorio de robótica.
