# Blog Web con Django

Aplicación web desarrollada con Python y Django para la gestión integral de publicaciones (blog). El proyecto incluye un sistema completo de autenticación de usuarios, perfiles públicos y editables, carga y visualización de imágenes multimedia, búsqueda avanzada por título y contenido, y un sistema robusto de control de acceso y protección de rutas en el servidor.

---

## 🚀 Tecnologías Utilizadas

- **Python** (3.12+)
- **Django** (6.1+)
- **Pillow** (Gestión y procesamiento de imágenes)
- **python-decouple** (Configuración segura mediante variables de entorno)
- **django-guardian** (Permisos a nivel de objeto)
- **SQLite** (Base de datos relacional para desarrollo)
- **HTML5 & CSS3** (Diseño limpio, responsivo y adaptativo)

---

## 🌟 Funcionalidades Principales

### 1. Publicaciones (CRUD desde la Interfaz Web)
- **Listado y Detalle:** Vista de publicaciones con información del autor, estado y contador de likes.
- **Creación y Edición:** Formularios para redactar y editar posts, tanto mediante vistas basadas en funciones (FBV) como vistas basadas en clases (CBV con `CreateView` y `UpdateView`).
- **Eliminación:** Confirmación de borrado protegida (`DeleteView` / vista con función).
- **Carga de Imágenes:** Cada post permite adjuntar una imagen. El template maneja de forma segura las publicaciones con o sin imagen sin romper la interfaz.
- **Slugs amigables:** URLs semánticas generadas automáticamente a partir del título.

### 2. Autenticación y Perfiles de Usuario
- **Registro de usuarios:** Formulario con validación de correo y contraseñas seguras hasheadas.
- **Inicio y Cierre de Sesión:** Login con redirección inteligente y logout protegido mediante método POST (prevención CSRF).
- **Diferenciación de estado:** La interfaz distingue de forma clara a usuarios anónimos de usuarios autenticados.
- **Perfil de usuario:** Visualización de perfil público con listado de posts propios y edición de bio y sitio web personal.
- **Recuperación y cambio de contraseña:** Flujo integrado de cambio y reseteo de claves.

### 3. Rutas y Vistas Protegidas
- **Protección en el servidor:** Crear, editar y eliminar posts, así como editar el perfil, requieren autenticación obligatoria mediante `@login_required` o `LoginRequiredMixin` en las vistas (no solo ocultando enlaces en el template).
- **Control de autoría:** Solo el autor original del post o usuarios con permisos específicos pueden editarlo o eliminarlo.
- **Permisos granulares por objeto (`django-guardian`):** Posibilidad de compartir un post con un usuario colaborador específico para edición conjunta.
- **Permiso personalizado:** Permiso `puede_publicar` en el modelo para diferenciar redactores de borradores frente a publicadores.

### 4. Búsqueda y Filtros
- **Búsqueda por título o contenido:** Búsqueda en tiempo real mediante operador `Q`.
- **Filtro por autor:** Posibilidad de filtrar publicaciones por nombre de usuario.
- **Feedback al usuario:** Indicador de cantidad de resultados encontrados y mensaje claro de aviso cuando no existen coincidencias.

### 5. Estadísticas y Contacto
- **Métricas:** Resumen del total de posts, promedio de likes y publicación más leída utilizando agregaciones del ORM de Django.
- **Formulario de contacto:** Formulario validado con confirmación en pantalla.

---

## 📋 Instrucciones de Instalación y Ejecución

Seguí estos pasos para clonar, instalar y ejecutar el proyecto en tu entorno local:

### 1. Clonar el repositorio
```bash
git clone https://github.com/Javi1101-spec/Proyecto_Final_Python_Javier_Ruiz.git
cd Proyecto_Final_Python_Javier_Ruiz
```

### 2. Crear y activar el entorno virtual

**En macOS / Linux:**
```bash
python3 -m venv .venv
source .venv/bin/activate
```

**En Windows:**
```bash
python -m venv .venv
.venv\Scripts\activate
```

### 3. Instalar las dependencias
```bash
pip install -r requirements.txt
```

### 4. Configurar variables de entorno (Opcional)
El proyecto funciona por defecto en modo desarrollo sin requerir configuración adicional. Si deseás personalizar variables de entorno, podés crear un archivo `.env` basándote en el ejemplo provisto:
```bash
cp .env.example .env
```

### 5. Ejecutar migraciones
Prepara la base de datos SQLite ejecutando:
```bash
python manage.py migrate
```

### 6. Cargar datos de prueba (Opcional)
Para cargar usuarios de prueba con diferentes roles y publicaciones de ejemplo, ejecutá:
```bash
python manage.py cargar_demo_auth
```
> **Usuarios demo creados:** `ana`, `beto` (autores comunes), `carla` (grupo Editores) y `dani` (grupo Publicadores). Contraseña por defecto para pruebas: `clave-demo-123`.

### 7. Crear un superusuario (para el panel de administración)
Para acceder a `/admin/` podés crear tu propio superusuario:
```bash
python manage.py createsuperuser
```

### 8. Iniciar el servidor de desarrollo
```bash
python manage.py runserver
```
Abrí tu navegador e ingresá a: **[http://127.0.0.1:8000/](http://127.0.0.1:8000/)**

---

## 🧪 Ejecución de Pruebas Unitarias

El proyecto cuenta con una amplia suite de pruebas automatizadas que verifican el CRUD, la carga y visualización de imágenes, la búsqueda, la autenticación y las reglas de protección de rutas:

```bash
python manage.py test
```

---

## 📁 Estructura del Proyecto

```text
├── config/                  # Configuración global del proyecto Django
│   ├── settings.py          # Configuración de base de datos, apps, media y seguridad
│   ├── urls.py              # Enrutador principal y servicio de media en desarrollo
│   ├── wsgi.py
│   └── asgi.py
├── posts/                   # App principal de gestión del blog
│   ├── models.py            # Modelo Post (título, contenido, imagen, autor, estado, slug)
│   ├── forms.py             # PostForm, BuscarForm, CompartirForm, ContactoForm
│   ├── views.py             # Vistas basadas en funciones (CRUD, búsqueda, protección)
│   ├── views_cbv.py         # Vistas basadas en clases con Mixins
│   ├── urls.py              # Rutas de la app posts
│   ├── templates/posts/     # Templates HTML (base, lista, detalle, form, etc.)
│   ├── tests.py             # Tests unitarios del CRUD, imágenes y búsqueda
│   └── test_proteccion.py   # Tests unitarios de permisos y autenticación
├── cuentas/                 # App de usuarios y perfiles
│   ├── models.py            # Modelo Perfil enlazado al User de Django
│   ├── forms.py             # RegistroForm, UsuarioForm, PerfilForm
│   ├── views.py             # Registro, edición de perfil y perfil público
│   ├── urls.py              # Rutas de autenticación
│   └── templates/           # Templates de login, registro y perfil
├── .env.example             # Plantilla de variables de entorno seguras
├── .gitignore               # Exclusión de venv, db.sqlite3, media, pycache, etc.
├── requirements.txt         # Dependencias fijadas del proyecto
├── manage.py
└── README.md
```

---

## 👤 Autor

- **Javier Ruiz** - Estudiante de Diplomatura en Data Science
