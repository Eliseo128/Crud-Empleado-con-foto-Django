# Crud-Empleado-con-foto-Django
proyecto crud empleado realizado con django
¡Claro que sí! Aquí tienes el proyecto completo, explicado paso a paso y diseñado para estudiantes de preparatoria que se inician en Django.

---

### **Proyecto: CRUD de Empleados con Foto en Django y Bootstrap**

#### **1. Objetivo del Proyecto**
Crear una aplicación web simple pero completa que permita administrar un registro de empleados. Las operaciones que podremos realizar son:
*   **C**rear (Create): Registrar nuevos empleados, incluyendo su foto.
*   **R**ecuperar (Read): Ver la lista completa de empleados y los detalles de un empleado específico.
*   **A**ctualizar (Update): Modificar los datos de un empleado existente, incluyendo la opción de cambiar su foto.
*   **D**eletar (Delete): Eliminar el registro de un empleado.

Utilizaremos Django para la lógica del servidor (backend) y Bootstrap para que la página se vea moderna y atractiva (frontend) sin necesidad de ser expertos en diseño web.

#### **2. Estructura de Carpetas y Archivos**
Antes de empezar, así es como se verá nuestro proyecto al final. Tener esta estructura clara te ayudará a entender dónde va cada cosa.

```
Crud_Empleado/
├── Backend_empleado/
│   ├── Backend_empleado/
│   │   ├── __init__.py
│   │   ├── asgi.py
│   │   ├── settings.py  <-- Archivo de configuración principal
│   │   ├── urls.py      <-- Archivo de URLs del proyecto
│   │   └── wsgi.py
│   ├── app_empleado/
│   │   ├── migrations/
│   │   ├── static/      <-- Carpeta para archivos estáticos (CSS, JS, etc.)
│   │   ├── templates/   <-- Carpeta para plantillas HTML
│   │   │   ├── app_empleado/
│   │   │   │   ├── detalle_empleado.html
│   │   │   │   ├── form_actualizar_empleado.html
│   │   │   │   ├── form_empleado.html
│   │   │   │   └── lista_empleados.html
│   │   │   ├── base.html
│   │   │   ├── footer.html
│   │   │   ├── header.html
│   │   │   └── inicio.html
│   │   ├── __init__.py
│   │   ├── admin.py
│   │   ├── apps.py
│   │   ├── models.py    <-- Aquí definimos la tabla de la base de datos
│   │   ├── tests.py
│   │   ├── urls.py      <-- Archivo de URLs de la aplicación
│   │   └── views.py     <-- Aquí va la lógica de nuestra aplicación
│   ├── db.sqlite3       <-- Nuestra base de datos
│   └── manage.py        <-- Utilidad para manejar el proyecto
├── media/               <-- Carpeta donde se guardarán las fotos
│   └── fotos_empleados/
└── requirements.txt     <-- Lista de dependencias del proyecto
```

---

### **Procedimiento Paso a Paso**

#### **Paso 1: Crear el Entorno Virtual**

Un entorno virtual es como una caja aislada para nuestro proyecto. Evita que las librerías de este proyecto se mezclen con las de otros.

1.  Abre una terminal o consola (CMD, PowerShell, Terminal de Linux/Mac).
2.  Navega hasta el lugar donde quieres crear tu proyecto.
3.  Crea la carpeta principal y entra en ella:
    ```bash
    mkdir Crud_Empleado
    cd Crud_Empleado
    ```
4.  Crea el entorno virtual. Le llamaremos `venv`.
    ```bash
    # En Windows
    python -m venv venv

    # En macOS / Linux
    python3 -m venv venv
    ```
5.  Activa el entorno virtual. Verás `(venv)` al inicio de la línea de tu terminal.
    ```bash
    # En Windows
    .\venv\Scripts\activate

    # En macOS / Linux
    source venv/bin/activate
    ```
    **¡Importante!** Siempre que trabajes en este proyecto, asegúrate de tener el entorno virtual activado.

#### **Paso 2: Seleccionar Intérprete de Python (en VS Code)**

Si usas Visual Studio Code, es una buena práctica decirle que use el Python de tu entorno virtual.

1.  Abre la carpeta `Crud_Empleado` en VS Code.
2.  Presiona `Ctrl+Shift+P` (o `Cmd+Shift+P` en Mac) para abrir la paleta de comandos.
3.  Escribe `Python: Select Interpreter`.
4.  Elige el que tiene `('venv': venv)` en su ruta. Suele aparecer como opción recomendada.

#### **Paso 3: Instalar Dependencias**

Instalaremos Django y una librería llamada `Pillow` que es necesaria para procesar imágenes.

```bash
pip install django pillow
```

#### **Paso 4: Crear el Proyecto y la Aplicación Django**

1.  Dentro de `Crud_Empleado` (con el entorno activado), crea el proyecto Django.
    ```bash
    django-admin startproject Backend_empleado .
    ```
    **Nota:** El punto `.` al final evita que se cree una carpeta extra, manteniendo la estructura limpia.

2.  Crea la aplicación `app_empleado`.
    ```bash
    python manage.py startapp app_empleado
    ```

#### **Paso 5: Crear Carpetas `static` y `templates`**

Dentro de la carpeta `app_empleado`, crea manualmente las siguientes carpetas para organizar nuestros archivos:

1.  `app_empleado/static/`
2.  `app_empleado/templates/`
3.  Dentro de `templates`, crea otra carpeta con el mismo nombre de la app: `app_empleado/templates/app_empleado/`
    *   Esto es una buena práctica de Django para evitar conflictos de nombres si tuvieras más aplicaciones.

#### **Paso 6: Configurar el Proyecto (`settings.py`)**

Abre el archivo `Backend_empleado/settings.py` y realiza las siguientes modificaciones.

1.  **Registrar la aplicación:** Dile a Django que existe `app_empleado`. Busca `INSTALLED_APPS` y añádela.
    ```python
    INSTALLED_APPS = [
        'django.contrib.admin',
        'django.contrib.auth',
        'django.contrib.contenttypes',
        'django.contrib.sessions',
        'django.contrib.messages',
        'django.contrib.staticfiles',
        'app_empleado',  # <-- Añadir esta línea
    ]
    ```

2.  **Configurar las plantillas (Templates):** Dile a Django dónde buscar los archivos HTML. Modifica la sección `TEMPLATES`.
    ```python
    TEMPLATES = [
        {
            'BACKEND': 'django.template.backends.django.DjangoTemplates',
            # Añade BASE_DIR / 'app_empleado/templates' a la lista DIRS
            'DIRS': [BASE_DIR / 'app_empleado/templates'],
            'APP_DIRS': True,
            'OPTIONS': {
                'context_processors': [
                    'django.template.context_processors.debug',
                    'django.template.context_processors.request',
                    'django.contrib.auth.context_processors.auth',
                    'django.contrib.messages.context_processors.messages',
                ],
            },
        },
    ]
    ```

3.  **Configurar archivos estáticos y de medios (Media):** Al final del archivo `settings.py`, añade estas líneas para manejar los archivos estáticos (CSS, JS) y los archivos subidos por el usuario (las fotos).
    ```python
    import os # Asegúrate que 'import os' está al inicio del archivo

    # ... (resto de configuraciones)

    # Configuración para archivos estáticos (CSS, JavaScript, Imágenes del sitio)
    STATIC_URL = '/static/'

    # Configuración para archivos de medios (subidos por el usuario)
    MEDIA_URL = '/media/'
    MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
    ```
    *   `MEDIA_URL`: La dirección web para acceder a las fotos.
    *   `MEDIA_ROOT`: La carpeta en tu computadora donde se guardarán las fotos.

#### **Paso 7: Configurar las URLs del Proyecto (`urls.py`)**

Ahora vamos a conectar las URLs del proyecto principal con las de nuestra aplicación.

1.  Abre `Backend_empleado/urls.py`.
2.  Importa `include` y añade la configuración para las fotos.
3.  Añade una ruta para que todas las URLs que empiecen con `/` sean manejadas por nuestra `app_empleado`.

```python
# Backend_empleado/urls.py
from django.contrib import admin
from django.urls import path, include  # <-- Importar 'include'

# Para servir archivos de medios en desarrollo
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    path('admin/', admin.site.urls),
    # Cuando alguien visite la URL raíz, Django buscará en app_empleado/urls.py
    path('', include('app_empleado.urls')),
]

# Esto es solo para el entorno de desarrollo (DEBUG=True)
if settings.DEBUG:
    urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

#### **Paso 8: Definir el Modelo (`models.py`)**

Abre `app_empleado/models.py` y pega el código que proporcionaste. Este código le dice a Django cómo será la tabla `empleados` en la base de datos.

```python
# app_empleado/models.py
from django.db import models

# Definir una tupla con los valores del select genero_empleado
generos = (
    ("Masculino", "Masculino"),
    ("Femenino", "Femenino"),
    ("Otro", "Otro"),
)

class Empleado(models.Model):
    nombre_empleado = models.CharField(max_length=200)
    apellido_empleado = models.CharField(max_length=100)
    email_empleado = models.EmailField(max_length=50)
    edad_empleado = models.IntegerField()
    genero_empleado = models.CharField(max_length=80, choices=generos)
    salario_empleado = models.DecimalField(
        max_digits=10, decimal_places=2, null=True, blank=True)
    foto_empleado = models.ImageField(
        upload_to='fotos_empleados/', null=True, blank=True)
    created_at = models.DateTimeField(auto_now_add=True, null=True, blank=True)
    updated = models.DateTimeField(auto_now_add=False, auto_now=True)

    def es_extension_valida(self):
        extensiones_validas = ['.jpg', '.jpeg', '.png', '.gif']
        return any(self.foto_empleado.name.lower().endswith(ext) for ext in extensiones_validas)

    """ la clase Meta dentro de un modelo se utiliza para proporcionar metadatos adicionales sobre el modelo."""
    class Meta:
        db_table = "empleados"
        ordering = ['-created_at']

    def __str__(self):
        return f"{self.nombre_empleado} {self.apellido_empleado}"
```
*He añadido el método `__str__` que es una buena práctica para que los objetos se muestren de forma legible en el panel de administrador de Django.*

#### **Paso 9: Realizar las Migraciones**

Las migraciones son la forma en que Django traduce tu `models.py` a instrucciones para la base de datos.

1.  **Crear los archivos de migración:**
    ```bash
    python manage.py makemigrations
    ```
    Verás que se crea un archivo en la carpeta `app_empleado/migrations/`.

2.  **Aplicar las migraciones a la base de datos:**
    ```bash
    python manage.py migrate
    ```
    Este comando crea la tabla `empleados` y otras tablas que Django necesita por defecto.

#### **Paso 10: Definir las URLs de la Aplicación (`urls.py`)**

Crea el archivo `urls.py` dentro de `app_empleado/` (si no existe) y pega el código que proporcionaste. Este archivo maneja las rutas específicas de nuestra aplicación.

```python
# app_empleado/urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('', views.inicio, name='inicio'),
    path('registrar-nuevo-empleado/', views.registrar_empleado,
         name='registrar_empleado'),
    path('lista-de-empleados/', views.listar_empleados, name='listar_empleados'),

    path('detalles-del-empleado/<int:id>/', # Cambiado a int:id para mejor práctica
         views.detalles_empleado, name='detalles_empleado'),

    path('formulario-para-actualizar-empleado/<int:id>/', # Cambiado a int:id
         views.view_form_update_empleado, name='view_form_update_empleado'),

    path('actualizar-empleado/<int:id>/', # Cambiado a int:id
         views.actualizar_empleado, name='actualizar_empleado'),
    path('eliminar-empleado/', views.eliminar_empleado, name='eliminar_empleado'),
]
```
**Nota:** He cambiado `<str:id>` a `<int:id>`. Como el ID de un empleado es un número, es mejor práctica usar `int`. Esto también proporciona una validación automática.

#### **Paso 11: Crear las Plantillas HTML**

Ahora crearemos la parte visual de nuestra aplicación.

**a) Plantillas Base (`base.html`, `header.html`, `footer.html`)**

Estas plantillas nos ayudarán a no repetir código.

**`app_empleado/templates/base.html`**
```html
<!doctype html>
<html lang="es">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>{% block title %}Sistema de Empleados{% endblock %}</title>
    <!-- Bootstrap CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
    <!-- Font Awesome (para iconos) -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css"/>
</head>
<body class="d-flex flex-column min-vh-100 bg-light">
    
    {% include 'header.html' %}

    <main class="container mt-4 flex-grow-1">
        {% block content %}
        <!-- El contenido específico de cada página irá aquí -->
        {% endblock %}
    </main>

    {% include 'footer.html' %}
    
    <!-- Bootstrap JS -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>
```

**`app_empleado/templates/header.html`**
```html
<header class="bg-primary text-white shadow-sm">
    <nav class="navbar navbar-expand-lg navbar-dark bg-primary">
        <div class="container">
            <a class="navbar-brand" href="{% url 'inicio' %}">
                <i class="fas fa-users-cog me-2"></i>
                Gestor de Empleados
            </a>
            <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
            </button>
            <div class="collapse navbar-collapse" id="navbarNav">
                <ul class="navbar-nav ms-auto">
                    <li class="nav-item">
                        <a class="nav-link active" aria-current="page" href="{% url 'inicio' %}">Inicio</a>
                    </li>
                </ul>
            </div>
        </div>
    </nav>
</header>
```

**`app_empleado/templates/footer.html`**
```html
<footer class="bg-dark text-white text-center p-3 mt-auto">
    <div class="container">
        &copy; {% now "Y" %} CRUD Empleados. Proyecto para Estudiantes de Preparatoria.
    </div>
</footer>
```

**b) Página de Inicio y Lista (`inicio.html`)**

Esta página mostrará la tabla con todos los empleados.

**`app_empleado/templates/inicio.html`**
```html
{% extends 'base.html' %}

{% block title %}Inicio - Lista de Empleados{% endblock %}

{% block content %}
<div class="card shadow-sm">
    <div class="card-header bg-white d-flex justify-content-between align-items-center">
        <h3 class="mb-0">Lista de Empleados</h3>
        <a href="{% url 'registrar_empleado' %}" class="btn btn-primary">
            <i class="fas fa-plus me-2"></i>Agregar Empleado
        </a>
    </div>
    <div class="card-body">
        {% if empleados %}
        <div class="table-responsive">
            <table class="table table-striped table-hover align-middle">
                <thead class="table-dark">
                    <tr>
                        <th>Foto</th>
                        <th>Nombre Completo</th>
                        <th>Email</th>
                        <th>Edad</th>
                        <th>Género</th>
                        <th>Acciones</th>
                    </tr>
                </thead>
                <tbody>
                    {% for empleado in empleados %}
                    <tr>
                        <td>
                            {% if empleado.foto_empleado %}
                                <img src="{{ empleado.foto_empleado.url }}" alt="Foto de {{ empleado.nombre_empleado }}" class="rounded-circle" width="50" height="50">
                            {% else %}
                                <i class="fas fa-user-circle fa-3x text-secondary"></i>
                            {% endif %}
                        </td>
                        <td>{{ empleado.nombre_empleado }} {{ empleado.apellido_empleado }}</td>
                        <td>{{ empleado.email_empleado }}</td>
                        <td>{{ empleado.edad_empleado }}</td>
                        <td>{{ empleado.genero_empleado }}</td>
                        <td>
                            <!-- Botones de Acción -->
                            <a href="{% url 'detalles_empleado' empleado.id %}" class="btn btn-sm btn-info" title="Ver Detalles">
                                <i class="fas fa-eye"></i>
                            </a>
                            <a href="{% url 'view_form_update_empleado' empleado.id %}" class="btn btn-sm btn-warning" title="Editar">
                                <i class="fas fa-pencil-alt"></i>
                            </a>
                            
                            <!-- Botón de Borrar con Modal de Confirmación -->
                            <button type="button" class="btn btn-sm btn-danger" data-bs-toggle="modal" data-bs-target="#confirmDeleteModal{{ empleado.id }}" title="Eliminar">
                                <i class="fas fa-trash-alt"></i>
                            </button>

                            <!-- Modal de Confirmación -->
                            <div class="modal fade" id="confirmDeleteModal{{ empleado.id }}" tabindex="-1" aria-labelledby="modalLabel{{ empleado.id }}" aria-hidden="true">
                                <div class="modal-dialog">
                                    <div class="modal-content">
                                        <div class="modal-header">
                                            <h5 class="modal-title" id="modalLabel{{ empleado.id }}">Confirmar Eliminación</h5>
                                            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                                        </div>
                                        <div class="modal-body">
                                            ¿Estás seguro de que quieres eliminar a <strong>{{ empleado.nombre_empleado }} {{ empleado.apellido_empleado }}</strong>?
                                        </div>
                                        <div class="modal-footer">
                                            <form action="{% url 'eliminar_empleado' %}" method="POST" style="display: inline;">
                                                {% csrf_token %}
                                                <input type="hidden" name="empleado_id" value="{{ empleado.id }}">
                                                <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cancelar</button>
                                                <button type="submit" class="btn btn-danger">Sí, eliminar</button>
                                            </form>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </td>
                    </tr>
                    {% endfor %}
                </tbody>
            </table>
        </div>
        {% else %}
        <div class="alert alert-info text-center">
            No hay empleados registrados todavía. ¡Agrega el primero!
        </div>
        {% endif %}
    </div>
</div>
{% endblock %}
```
*He incluido un "Modal" de Bootstrap para que aparezca una ventana de confirmación antes de borrar un empleado. Es una mejor experiencia de usuario.*

**c) Formularios y Detalles (dentro de `app_empleado/templates/app_empleado/`)**

**`app_empleado/templates/app_empleado/form_empleado.html`** (Para agregar)
```html
{% extends 'base.html' %}

{% block title %}Registrar Nuevo Empleado{% endblock %}

{% block content %}
<div class="row justify-content-center">
    <div class="col-md-8">
        <div class="card shadow-sm">
            <div class="card-header bg-primary text-white">
                <h3 class="mb-0">Formulario de Registro de Empleado</h3>
            </div>
            <div class="card-body">
                <form action="{% url 'registrar_empleado' %}" method="POST" enctype="multipart/form-data">
                    {% csrf_token %}
                    <div class="mb-3">
                        <label for="nombre" class="form-label">Nombre(s)</label>
                        <input type="text" class="form-control" id="nombre" name="nombre_empleado" required>
                    </div>
                    <div class="mb-3">
                        <label for="apellido" class="form-label">Apellido(s)</label>
                        <input type="text" class="form-control" id="apellido" name="apellido_empleado" required>
                    </div>
                    <div class="mb-3">
                        <label for="email" class="form-label">Email</label>
                        <input type="email" class="form-control" id="email" name="email_empleado" required>
                    </div>
                    <div class="row">
                        <div class="col-md-6 mb-3">
                            <label for="edad" class="form-label">Edad</label>
                            <input type="number" class="form-control" id="edad" name="edad_empleado" required>
                        </div>
                        <div class="col-md-6 mb-3">
                            <label for="genero" class="form-label">Género</label>
                            <select class="form-select" id="genero" name="genero_empleado" required>
                                <option value="Masculino">Masculino</option>
                                <option value="Femenino">Femenino</option>
                                <option value="Otro">Otro</option>
                            </select>
                        </div>
                    </div>
                    <div class="mb-3">
                        <label for="salario" class="form-label">Salario (Opcional)</label>
                        <input type="number" step="0.01" class="form-control" id="salario" name="salario_empleado">
                    </div>
                    <div class="mb-3">
                        <label for="foto" class="form-label">Foto del Empleado (Opcional)</label>
                        <input type="file" class="form-control" id="foto" name="foto_empleado" accept="image/*">
                    </div>
                    <div class="d-grid gap-2 d-md-flex justify-content-md-end">
                        <a href="{% url 'inicio' %}" class="btn btn-secondary me-md-2">Cancelar</a>
                        <button type="submit" class="btn btn-primary">Guardar Empleado</button>
                    </div>
                </form>
            </div>
        </div>
    </div>
</div>
{% endblock %}
```
**Importante:** La etiqueta `<form>` debe tener `enctype="multipart/form-data"` para que la subida de archivos funcione.

**`app_empleado/templates/app_empleado/detalle_empleado.html`**
```html
{% extends 'base.html' %}

{% block title %}Detalles de {{ empleado.nombre_empleado }}{% endblock %}

{% block content %}
<div class="card shadow-sm">
    <div class="card-header bg-info text-white">
        <h3 class="mb-0">Detalles del Empleado</h3>
    </div>
    <div class="card-body">
        <div class="row">
            <div class="col-md-4 text-center">
                {% if empleado.foto_empleado %}
                    <img src="{{ empleado.foto_empleado.url }}" class="img-fluid rounded-circle shadow mb-3" alt="Foto de {{ empleado.nombre_empleado }}" style="max-width: 200px;">
                {% else %}
                    <i class="fas fa-user-circle fa-8x text-secondary mb-3"></i>
                {% endif %}
                <h4 class="card-title">{{ empleado.nombre_empleado }} {{ empleado.apellido_empleado }}</h4>
            </div>
            <div class="col-md-8">
                <ul class="list-group list-group-flush">
                    <li class="list-group-item"><strong>Email:</strong> {{ empleado.email_empleado }}</li>
                    <li class="list-group-item"><strong>Edad:</strong> {{ empleado.edad_empleado }} años</li>
                    <li class="list-group-item"><strong>Género:</strong> {{ empleado.genero_empleado }}</li>
                    <li class="list-group-item"><strong>Salario:</strong> ${{ empleado.salario_empleado|default:"No especificado" }}</li>
                    <li class="list-group-item"><strong>Fecha de Registro:</strong> {{ empleado.created_at|date:"d/m/Y H:i" }}</li>
                    <li class="list-group-item"><strong>Última Actualización:</strong> {{ empleado.updated|date:"d/m/Y H:i" }}</li>
                </ul>
            </div>
        </div>
    </div>
    <div class="card-footer text-end">
        <a href="{% url 'inicio' %}" class="btn btn-secondary">
            <i class="fas fa-arrow-left me-2"></i>Volver a la lista
        </a>
    </div>
</div>
{% endblock %}
```

**`app_empleado/templates/app_empleado/form_actualizar_empleado.html`**
```html
{% extends 'base.html' %}

{% block title %}Actualizar Empleado{% endblock %}

{% block content %}
<div class="row justify-content-center">
    <div class="col-md-8">
        <div class="card shadow-sm">
            <div class="card-header bg-warning text-dark">
                <h3 class="mb-0">Formulario de Actualización</h3>
            </div>
            <div class="card-body">
                <form action="{% url 'actualizar_empleado' empleado.id %}" method="POST" enctype="multipart/form-data">
                    {% csrf_token %}
                    <div class="mb-3">
                        <label for="nombre" class="form-label">Nombre(s)</label>
                        <input type="text" class="form-control" id="nombre" name="nombre_empleado" value="{{ empleado.nombre_empleado }}" required>
                    </div>
                    <div class="mb-3">
                        <label for="apellido" class="form-label">Apellido(s)</label>
                        <input type="text" class="form-control" id="apellido" name="apellido_empleado" value="{{ empleado.apellido_empleado }}" required>
                    </div>
                    <div class="mb-3">
                        <label for="email" class="form-label">Email</label>
                        <input type="email" class="form-control" id="email" name="email_empleado" value="{{ empleado.email_empleado }}" required>
                    </div>
                    <div class="row">
                        <div class="col-md-6 mb-3">
                            <label for="edad" class="form-label">Edad</label>
                            <input type="number" class="form-control" id="edad" name="edad_empleado" value="{{ empleado.edad_empleado }}" required>
                        </div>
                        <div class="col-md-6 mb-3">
                            <label for="genero" class="form-label">Género</label>
                            <select class="form-select" id="genero" name="genero_empleado" required>
                                <option value="Masculino" {% if empleado.genero_empleado == 'Masculino' %}selected{% endif %}>Masculino</option>
                                <option value="Femenino" {% if empleado.genero_empleado == 'Femenino' %}selected{% endif %}>Femenino</option>
                                <option value="Otro" {% if empleado.genero_empleado == 'Otro' %}selected{% endif %}>Otro</option>
                            </select>
                        </div>
                    </div>
                    <div class="mb-3">
                        <label for="salario" class="form-label">Salario (Opcional)</label>
                        <input type="number" step="0.01" class="form-control" id="salario" name="salario_empleado" value="{{ empleado.salario_empleado }}">
                    </div>
                    <div class="mb-3">
                        <label class="form-label">Foto Actual</label>
                        <div>
                            {% if empleado.foto_empleado %}
                                <img src="{{ empleado.foto_empleado.url }}" alt="Foto actual" class="img-thumbnail" width="100">
                            {% else %}
                                <p class="text-muted">No hay foto.</p>
                            {% endif %}
                        </div>
                    </div>
                    <div class="mb-3">
                        <label for="foto" class="form-label">Cambiar Foto (Opcional)</label>
                        <input type="file" class="form-control" id="foto" name="foto_empleado" accept="image/*">
                    </div>
                    <div class="d-grid gap-2 d-md-flex justify-content-md-end">
                        <a href="{% url 'inicio' %}" class="btn btn-secondary me-md-2">Cancelar</a>
                        <button type="submit" class="btn btn-warning">Actualizar Empleado</button>
                    </div>
                </form>
            </div>
        </div>
    </div>
</div>
{% endblock %}
```
*El resto de archivos (`lista_empleados.html`) no son estrictamente necesarios ya que `inicio.html` cumple esa función, pero los crearemos para seguir la estructura de `urls.py`.*

**`app_empleado/templates/app_empleado/lista_empleados.html`**
```html
{% extends 'base.html' %}
{% block content %}
    <h1>Esta es una lista alternativa</h1>
    <!-- Podrías copiar el contenido de inicio.html aquí si quieres una vista idéntica -->
{% endblock %}
```

#### **Paso 12: Crear las Funciones en `views.py`**

Aquí es donde ocurre la magia. Abre `app_empleado/views.py` y reemplaza su contenido con lo siguiente.

```python
# app_empleado/views.py
from django.shortcuts import render, redirect, get_object_or_404
from .models import Empleado

# CREATE (Crear)
def registrar_empleado(request):
    if request.method == 'POST':
        # Obtener los datos del formulario
        nombre = request.POST.get('nombre_empleado')
        apellido = request.POST.get('apellido_empleado')
        email = request.POST.get('email_empleado')
        edad = request.POST.get('edad_empleado')
        genero = request.POST.get('genero_empleado')
        salario = request.POST.get('salario_empleado')
        foto = request.FILES.get('foto_empleado')

        # Crear y guardar el nuevo empleado
        Empleado.objects.create(
            nombre_empleado=nombre,
            apellido_empleado=apellido,
            email_empleado=email,
            edad_empleado=edad,
            genero_empleado=genero,
            salario_empleado=salario if salario else None,
            foto_empleado=foto
        )
        return redirect('inicio')
    
    # Si es GET, simplemente muestra el formulario
    return render(request, 'app_empleado/form_empleado.html')

# READ (Leer) - Vista de Inicio / Lista
def inicio(request):
    empleados = Empleado.objects.all()
    context = {'empleados': empleados}
    return render(request, 'inicio.html', context)

# Vista para la URL /lista-de-empleados/ que simplemente redirige a inicio
def listar_empleados(request):
    return redirect('inicio')

# READ (Leer) - Detalles de un empleado
def detalles_empleado(request, id):
    empleado = get_object_or_404(Empleado, id=id)
    context = {'empleado': empleado}
    return render(request, 'app_empleado/detalle_empleado.html', context)

# UPDATE (Actualizar) - Mostrar el formulario con datos existentes
def view_form_update_empleado(request, id):
    empleado = get_object_or_404(Empleado, id=id)
    context = {'empleado': empleado}
    return render(request, 'app_empleado/form_actualizar_empleado.html', context)

# UPDATE (Actualizar) - Procesar el formulario de actualización
def actualizar_empleado(request, id):
    empleado = get_object_or_404(Empleado, id=id)
    
    if request.method == 'POST':
        # Obtener los datos del formulario
        empleado.nombre_empleado = request.POST.get('nombre_empleado')
        empleado.apellido_empleado = request.POST.get('apellido_empleado')
        empleado.email_empleado = request.POST.get('email_empleado')
        empleado.edad_empleado = request.POST.get('edad_empleado')
        empleado.genero_empleado = request.POST.get('genero_empleado')
        salario = request.POST.get('salario_empleado')
        empleado.salario_empleado = salario if salario else None
        
        # Si se subió una nueva foto, la actualizamos. Si no, dejamos la anterior.
        if 'foto_empleado' in request.FILES:
            empleado.foto_empleado = request.FILES['foto_empleado']
            
        empleado.save()
        return redirect('inicio')
    
    # Si no es POST, redirigimos a la página de inicio (no debería pasar)
    return redirect('inicio')


# DELETE (Eliminar)
def eliminar_empleado(request):
    if request.method == 'POST':
        empleado_id = request.POST.get('empleado_id')
        empleado = get_object_or_404(Empleado, id=empleado_id)
        empleado.delete()
    return redirect('inicio')
```

#### **Paso 13: Ejecutar el Servidor**

¡Todo está listo! Ahora vamos a ver nuestra aplicación en acción.

1.  Asegúrate de estar en la carpeta `Crud_Empleado` con el entorno virtual activado.
2.  Ejecuta el servidor de desarrollo:
    ```bash
    python manage.py runserver
    ```
3.  Abre tu navegador web y ve a la siguiente dirección: **`http://127.0.0.1:8000/`**

Deberías ver la página principal con la tabla (vacía al principio) y el botón para agregar un nuevo empleado. ¡Prueba a agregar, ver, editar y eliminar empleados!

---

### **Finalizando el Proyecto**

#### **Crear el archivo de Requerimientos (`requirements.txt`)**
Este archivo es una lista de todas las librerías que tu proyecto necesita. Es muy útil para que otras personas (o tú en otra computadora) puedan instalar todo fácilmente.

```bash
pip freeze > requirements.txt
```
Este comando crea el archivo `requirements.txt` en la carpeta `Crud_Empleado`.

---

### **Resumen del Proyecto**

Hemos construido una aplicación web CRUD completa desde cero utilizando Django. Aprendimos a:
*   Configurar un proyecto Django, incluyendo una aplicación, plantillas y archivos estáticos/media.
*   Definir un modelo de datos (`Empleado`) y migrarlo a una base de datos.
*   Crear la lógica en `views.py` para manejar las peticiones del usuario (agregar, ver, editar, borrar).
*   Diseñar interfaces web limpias y funcionales con HTML y Bootstrap, incluyendo formularios, tablas y modales de confirmación.
*   Manejar la subida y visualización de imágenes, un requisito común en muchas aplicaciones web.

Este proyecto sienta las bases para construir aplicaciones mucho más complejas en el futuro.

---

### **Glosario de Términos**

*   **CRUD:** Acrónimo de Create (Crear), Read (Leer), Update (Actualizar), Delete (Eliminar). Son las operaciones básicas en la gestión de datos.
*   **Django:** Un framework de desarrollo web de alto nivel para Python, que promueve un desarrollo rápido y un diseño limpio y pragmático.
*   **Framework:** Un conjunto de herramientas y librerías que te dan una estructura base para construir aplicaciones, ahorrándote mucho trabajo.
*   **Backend:** La parte de una aplicación que se ejecuta en el servidor. Se encarga de la lógica, el acceso a la base de datos y la seguridad. En nuestro caso, es Django.
*   **Frontend:** La parte de una aplicación con la que el usuario interactúa directamente. Es lo que ves en el navegador (HTML, CSS, JavaScript). En nuestro caso, es HTML con Bootstrap.
*   **Bootstrap:** Un framework de CSS muy popular que facilita la creación de sitios web atractivos y responsivos (que se adaptan a móviles y computadoras).
*   **Entorno Virtual (venv):** Un directorio aislado que contiene una instalación de Python y librerías específicas para un proyecto, evitando conflictos.
*   **ORM (Object-Relational Mapping):** Una técnica que permite interactuar con la base de datos usando objetos de Python (como nuestra clase `Empleado`) en lugar de escribir consultas SQL directamente. El ORM de Django es una de sus características más potentes.
*   **Migraciones:** Archivos que genera Django para registrar los cambios en tus modelos (`models.py`) y traducirlos a comandos que modifican la estructura de tu base de datos.
*   **Plantilla (Template):** Un archivo HTML que contiene marcadores de posición (`{{ variable }}`) y lógica simple (`{% for %}`), que Django rellena con datos antes de enviarlo al navegador.
*   **URL (Uniform Resource Locator):** La dirección web que escribes en el navegador para acceder a una página específica. Django usa un archivo `urls.py` para dirigir cada URL a una función de vista (`views.py`) correspondiente.

---

### **Tecnologías Utilizadas**

*   **Backend:**
    *   Lenguaje: **Python**
    *   Framework: **Django**
    *   Base de Datos: **SQLite** (la base de datos por defecto de Django, perfecta para desarrollo y proyectos pequeños)
    *   Librería para imágenes: **Pillow**
*   **Frontend:**
    *   Lenguaje de maquetación: **HTML5**
    *   Framework de diseño: **Bootstrap 5**
    *   Iconos: **Font Awesome**

---

### **Links de Referencia**

*   **Documentación Oficial de Django:** [https://docs.djangoproject.com/en/stable/](https://docs.djangoproject.com/en/stable/)
*   **Documentación Oficial de Bootstrap:** [https://getbootstrap.com/docs/5.3/getting-started/introduction/](https://getbootstrap.com/docs/5.3/getting-started/introduction/)
*   **Página de Font Awesome (Iconos):** [https://fontawesome.com/](https://fontawesome.com/)
*   **Tutorial de MDN sobre Django (muy recomendado para principiantes):** [https://developer.mozilla.org/es/docs/Learn/Server-side/Django](https://developer.mozilla.org/es/docs/Learn/Server-side/Django)
