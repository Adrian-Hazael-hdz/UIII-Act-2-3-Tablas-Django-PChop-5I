

````markdown
# 🧩 Proyecto PChop — Primera Parte (31 pasos completos)

---

## 📘 Información general del proyecto
- **Proyecto:** PChop  
- **Lenguaje:** Python  
- **Framework:** Django  
- **Editor recomendado:** Visual Studio Code  
- **Objetivo:** Sistema web administrativo para una tienda de computadoras (módulo Categoría por ahora)

---



---

## ## 1. Crear carpeta del Proyecto: `UIII_PChop_1128`
```bash
mkdir UIII_PChop_1128
cd UIII_PChop_1128
````

---

## ## 2. Abrir VS Code sobre la carpeta `UIII_PChop_1128`

* Abrir Visual Studio Code → **Archivo → Abrir carpeta** → seleccionar `UIII_PChop_1128`.

---

## ## 3. Abrir terminal integrada en VS Code

* Menú: **Ver → Terminal**
* Asegúrate de que la terminal está en la ruta del proyecto:

```
C:\Users\<usuario>\UIII_PChop_1128>
```

---

## ## 4. Crear carpeta entorno virtual `.venv` desde terminal de VS Code

```bash
python -m venv .venv
```

> Esto crea la carpeta `.venv/` dentro de la raíz del proyecto.

---

## ## 5. Activar el entorno virtual

* En Windows (PowerShell o CMD):

```bash
.venv\Scripts\activate
```

* En Git Bash / WSL (Linux/macOS):

```bash
source .venv/bin/activate
```

La terminal debe mostrar el prefijo `(.venv)`.

---

## ## 6. Activar intérprete de Python (VS Code)

* `Ctrl + Shift + P` → `Python: Select Interpreter` → seleccionar el intérprete dentro de `.venv` (por ejemplo `.venv\Scripts\python.exe`).

---

## ## 7. Procedimiento para instalar Django

Con el entorno activo:

```bash
pip install django
```

Opcional: fijar versión si se requiere:

```bash
pip install "django>=4.2,<5"
```

---

## ## 8. Crear proyecto `backend_PChop` sin duplicar carpeta

Ejecutar **desde la raíz del proyecto**:

```bash
django-admin startproject backend_PChop .
```

(Este comando crea la carpeta `backend_PChop/` y los archivos `manage.py` ya existentes no se duplican.)

Estructura tras crear el proyecto:

```
UIII_PChop_1128/
├── backend_PChop/
│   ├── __init__.py
│   ├── asgi.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── manage.py
└── .venv/
```

---

## ## 9. Ejecutar servidor en el puerto 8017

```bash
python manage.py runserver 8017
```

Salida esperada:

```
Starting development server at http://127.0.0.1:8017/
```

---

## ## 10. Copiar y pegar el link en el navegador

Abrir:

```
http://127.0.0.1:8017/
```

Verificar que aparece la página de bienvenida de Django.

---

## ## 11. Crear aplicación `app_PChop`

En la raíz del proyecto:

```bash
python manage.py startapp app_PChop
```

Estructura:

```
app_PChop/
├── admin.py
├── apps.py
├── migrations/
├── models.py
├── tests.py
├── views.py
└── __init__.py
```

---

## ## 12. Aquí el modelo `models.py` (añadir a `app_PChop/models.py`)

Pega exactamente este código en `app_PChop/models.py`:

```python
from django.db import models

# MODELO: CATEGORIA
class Categoria(models.Model):
    nombre = models.CharField(max_length=100)
    descripcion = models.TextField()
    fecha_creacion = models.DateTimeField(auto_now_add=True)
    activa = models.BooleanField(default=True)
    imagen = models.URLField(blank=True, null=True)
    slug = models.SlugField(unique=True)
    prioridad = models.IntegerField(default=1)

    def __str__(self):
        return self.nombre

# MODELO: PRODUCTO
class Producto(models.Model):
    categoria = models.ForeignKey(Categoria, on_delete=models.CASCADE, related_name='productos')
    nombre = models.CharField(max_length=100)
    descripcion = models.TextField()
    precio = models.DecimalField(max_digits=10, decimal_places=2)
    stock = models.PositiveIntegerField()
    marca = models.CharField(max_length=100)
    fecha_agregado = models.DateTimeField(auto_now_add=True)
    imagen = models.URLField(blank=True, null=True)

    def __str__(self):
        return self.nombre

# MODELO: PEDIDO
class Pedido(models.Model):
    productos = models.ManyToManyField(Producto, related_name='pedidos')
    fecha_pedido = models.DateTimeField(auto_now_add=True)
    cliente = models.CharField(max_length=100)
    direccion_envio = models.CharField(max_length=255)
    total = models.DecimalField(max_digits=10, decimal_places=2)
    estado = models.CharField(max_length=50, default='Pendiente')
    metodo_pago = models.CharField(max_length=50)
    observaciones = models.TextField(blank=True, null=True)

    def __str__(self):
        return f"Pedido #{self.id} - {self.cliente}"
```

---

## ## 12.5. Procedimiento para realizar las migraciones (makemigrations y migrate)

Ejecuta:

```bash
python manage.py makemigrations
python manage.py migrate
```

Verifica que no haya errores y que se creen las tablas en la base de datos SQLite (por defecto).

---

## ## 13. Primero trabajamos con el MODELO: CATEGORÍA

* Dejar `Producto` y `Pedido` pendientes para fases siguientes.
* Implementar CRUD completo y plantillas para `Categoria` primero.

---

## ## 14. En `views.py` de `app_PChop` crear las funciones

Abrir `app_PChop/views.py` y añadir (ejemplo estructural — ajustar como prefieras):

```python
from django.shortcuts import render, redirect, get_object_or_404
from .models import Categoria

def inicio_PChop(request):
    return render(request, 'inicio.html')

def ver_categorias(request):
    categorias = Categoria.objects.all().order_by('-fecha_creacion')
    return render(request, 'categoria/ver_categorias.html', {'categorias': categorias})

def agregar_categoria(request):
    if request.method == 'POST':
        # recibir datos desde formulario HTML (sin forms.py)
        nombre = request.POST.get('nombre')
        descripcion = request.POST.get('descripcion')
        slug = request.POST.get('slug')
        imagen = request.POST.get('imagen')
        prioridad = request.POST.get('prioridad') or 1
        Categoria.objects.create(
            nombre=nombre, descripcion=descripcion, slug=slug,
            imagen=imagen, prioridad=int(prioridad)
        )
        return redirect('ver_categorias')
    return render(request, 'categoria/agregar_categoria.html')

def actualizar_categoria(request, pk):
    categoria = get_object_or_404(Categoria, pk=pk)
    return render(request, 'categoria/actualizar_categoria.html', {'categoria': categoria})

def realizar_actualizacion_categoria(request, pk):
    categoria = get_object_or_404(Categoria, pk=pk)
    if request.method == 'POST':
        categoria.nombre = request.POST.get('nombre')
        categoria.descripcion = request.POST.get('descripcion')
        categoria.slug = request.POST.get('slug')
        categoria.imagen = request.POST.get('imagen')
        categoria.prioridad = int(request.POST.get('prioridad') or 1)
        categoria.save()
        return redirect('ver_categorias')
    return redirect('actualizar_categoria', pk=pk)

def borrar_categoria(request, pk):
    categoria = get_object_or_404(Categoria, pk=pk)
    if request.method == 'POST':
        categoria.delete()
        return redirect('ver_categorias')
    return render(request, 'categoria/borrar_categoria.html', {'categoria': categoria})
```

> Nota: Este código asume uso de POST en formularios HTML — no hay validaciones.

---

## ## 15. Crear la carpeta `templates` dentro de `app_PChop`

Estructura:

```
app_PChop/
└── templates/
    ├── base.html
    ├── header.html
    ├── navbar.html
    ├── footer.html
    └── inicio.html
```

---

## ## 16. En `templates` crear archivos HTML (base.html, header.html, navbar.html, footer.html, inicio.html)

* `base.html` contendrá la base, carga Bootstrap y los bloques `{% block content %}`.
* `header.html`, `navbar.html`, `footer.html` serán fragmentos incluidos con `{% include %}`.
* `inicio.html` hereda de `base.html`.

Ejemplo muy corto de `base.html` (completa según diseño):

```html
<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>PChop - Administración</title>
  <!-- Bootstrap CSS CDN -->
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>
  {% include 'header.html' %}
  {% include 'navbar.html' %}
  <main class="container my-4">
    {% block content %}{% endblock %}
  </main>
  {% include 'footer.html' %}
  <!-- Bootstrap JS CDN -->
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>
```

---

## ## 17. En `base.html` agregar Bootstrap para CSS y JS

* Usar CDN de Bootstrap 5 (ejemplos arriba).
* Incluir `<meta>` responsive.
* Mantener bloques de plantilla claros.

---

## ## 18. En `navbar.html` incluir las opciones y submenu

Estructura del menú (ejemplo con Bootstrap):

* Título: **Sistema de Administración PChop**
* Menú principal:

  * Inicio
  * Categoría → (Agregar, Ver, Actualizar, Borrar)
  * Producto → (Agregar, Ver, Actualizar, Borrar)
  * Pedido → (Agregar, Ver, Actualizar, Borrar)
* Íconos solo en las opciones principales (p. ej. usar `bi bi-shop` de Bootstrap Icons).

Ejemplo de fragmento (simplificado):

```html
<nav class="navbar navbar-expand-lg navbar-light bg-light">
  <div class="container-fluid">
    <a class="navbar-brand" href="#">Sistema de Administración PChop</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#mainNav">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="mainNav">
      <ul class="navbar-nav ms-auto">
        <li class="nav-item"><a class="nav-link" href="{% url 'inicio' %}">Inicio</a></li>
        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" href="#" data-bs-toggle="dropdown">Categoría</a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="{% url 'agregar_categoria' %}">Agregar Categoria</a></li>
            <li><a class="dropdown-item" href="{% url 'ver_categorias' %}">Ver Categorias</a></li>
          </ul>
        </li>
        <!-- Producto y Pedido similar -->
      </ul>
    </div>
  </div>
</nav>
```

---

## ## 19. En `footer.html` incluir derechos de autor, fecha del sistema y texto fijo al final

Contenido sugerido:

```html
<footer class="bg-light text-center py-3 fixed-bottom">
  © {{ now|date:"Y" }} | Creado por Adrian Hazael 5I, Cbtis 128
</footer>
```

> Asegúrate que `now` esté en el contexto o usa `django.template.context_processors.request` y `{{ now }}` con template tag si es necesario.

---

## ## 20. En `inicio.html` colocar información del sistema + imagen desde la red

Ejemplo:

```html
{% extends 'base.html' %}
{% block content %}
  <h1>Bienvenido a PChop</h1>
  <p>Sistema administrativo para gestión de la tienda de componentes.</p>
  <img src="https://example.com/imagen-pchop.jpg" alt="PChop tienda" class="img-fluid">
{% endblock %}
```

(Reemplazar la URL por una imagen real en la web.)

---

## ## 21. Crear subcarpeta `categoria` dentro de `app_PChop/templates`

Estructura:

```
app_PChop/
└── templates/
    └── categoria/
        ├── agregar_categoria.html
        ├── ver_categorias.html
        ├── actualizar_categoria.html
        └── borrar_categoria.html
```

---

## ## 22. Crear los archivos HTML para categoría con su código correspondiente

* `agregar_categoria.html`: formulario `<form method="post">` con campos `nombre`, `descripcion`, `slug`, `imagen`, `prioridad`.
* `ver_categorias.html`: tabla `<table>` listando categorías con botones **Ver**, **Editar**, **Borrar** (cada botón enlaza a la ruta correspondiente).
* `actualizar_categoria.html`: formulario precargado con datos de la categoría.
* `borrar_categoria.html`: pantalla de confirmación con `POST` para eliminar.

> Ejemplos de formularios: usar inputs simples. No hay validación.

---

## ## 23. No utilizar `forms.py`

* Los formularios deben construirse con HTML puro y manejar `request.POST` en las vistas (tal como se ejemplificó en `views.py`).

---

## ## 24. Procedimiento para crear `urls.py` en `app_PChop` (rutas CRUD)

Crear `app_PChop/urls.py` con el siguiente contenido:

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.inicio_PChop, name='inicio'),
    path('categorias/', views.ver_categorias, name='ver_categorias'),
    path('categorias/agregar/', views.agregar_categoria, name='agregar_categoria'),
    path('categorias/<int:pk>/editar/', views.actualizar_categoria, name='actualizar_categoria'),
    path('categorias/<int:pk>/editar/guardar/', views.realizar_actualizacion_categoria, name='realizar_actualizacion_categoria'),
    path('categorias/<int:pk>/borrar/', views.borrar_categoria, name='borrar_categoria'),
]
```

---

## ## 25. Procedimiento para agregar `app_PChop` en `settings.py` de `backend_PChop`

En `backend_PChop/settings.py`, localizar `INSTALLED_APPS` y añadir:

```python
INSTALLED_APPS = [
    # apps por defecto...
    'app_PChop',
]
```

---

## ## 26. Configurar `urls.py` de `backend_PChop` para enlazar con `app_PChop`

Editar `backend_PChop/urls.py`:

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app_PChop.urls')),
]
```

---

## ## 27. Procedimiento para registrar los modelos en `admin.py` y volver a migrar

En `app_PChop/admin.py`:

```python
from django.contrib import admin
from .models import Categoria, Producto, Pedido

admin.site.register(Categoria)
admin.site.register(Producto)
admin.site.register(Pedido)
```

Luego:

```bash
python manage.py makemigrations
python manage.py migrate
```

---

## ## 28. Estética: utilizar colores suaves, atractivos y modernos

* Recomendar paleta: tonos pastel/neutral (ej.: #f5f7fa, #e6eef5, acentos #6c7ae0).
* Mantener diseño minimalista, espaciado y tipografía legible.
* Usar clases de Bootstrap para consistencia.

---

## ## 29. Al inicio crear la estructura completa de carpetas y archivos

Antes de programar, crea estas carpetas y archivos mínimos:

```
UIII_PChop_1128/
├── .venv/
├── backend_PChop/
├── app_PChop/
│   ├── migrations/
│   ├── templates/
│   │   ├── base.html
│   │   ├── header.html
│   │   ├── navbar.html
│   │   ├── footer.html
│   │   └── categoria/
│   │       ├── agregar_categoria.html
│   │       ├── ver_categorias.html
│   │       ├── actualizar_categoria.html
│   │       └── borrar_categoria.html
│   ├── models.py
│   ├── views.py
│   ├── urls.py
│   └── admin.py
├── manage.py
└── README.md
```

---

## ## 30. Verificar que el proyecto sea totalmente funcional

* Comprobar:

  * Migraciones aplicadas sin errores.
  * Rutas cargan correctamente.
  * Formularios envían y guardan datos.
  * Plantillas se renderizan sin excepción.
  * Admin muestra modelos registrados.

---

## ## 31. Ejecutar servidor en el puerto 8017 (final)

```bash
python manage.py runserver 8017
```

* Abrir en el navegador:

```
http://127.0.0.1:8017/
```

* Prueba completa: crear, editar y borrar categorías desde las vistas y verificar la persistencia en la base de datos.

---

## 👨‍💻 Autor

**Creado por Adrian Hazael — 5I — CBTIS 128**

```


```
