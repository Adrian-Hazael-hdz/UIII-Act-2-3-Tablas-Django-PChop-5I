# Proyecto PChop - Guía de Implementación

## Primera Parte: Configuración Inicial del Proyecto

### 1. Crear Carpeta del Proyecto
```bash
mkdir UIII_PChop_1128
cd UIII_PChop_1128
```

### 2. Abrir VS Code en la Carpeta
```bash
code .
```

### 3. Abrir Terminal en VS Code
- `Ctrl + `` (backtick) o
- Menú: `Terminal` → `New Terminal`

### 4. Crear Entorno Virtual
```bash
python -m venv .venv
```

### 5. Activar Entorno Virtual
**Windows:**
```bash
.venv\Scripts\activate
```

**Mac/Linux:**
```bash
source .venv/bin/activate
```

### 6. Activar Intérprete de Python
- `Ctrl + Shift + P`
- Buscar: "Python: Select Interpreter"
- Seleccionar: `./.venv/Scripts/python.exe`

### 7. Instalar Django
```bash
pip install django
```

### 8. Crear Proyecto Django
```bash
django-admin startproject backend_PChop .
```

### 9. Ejecutar Servidor en Puerto 8017
```bash
python manage.py runserver 8017
```

### 10. Verificar en Navegador
Abrir: `http://localhost:8017`

### 11. Crear Aplicación
```bash
python manage.py startapp app_PChop
```

---

## Segunda Parte: Configuración de Modelos y Vistas

### 12. Modelo `models.py`
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

### 12.5 Migraciones
```bash
python manage.py makemigrations
python manage.py migrate
```

### 13. Trabajar con MODELO: CATEGORÍA

### 14. Vistas `views.py`
```python
from django.shortcuts import render, redirect, get_object_or_404
from .models import Categoria

def inicio_PChop(request):
    return render(request, 'inicio.html')

def agregar_categoria(request):
    if request.method == 'POST':
        nombre = request.POST.get('nombre')
        descripcion = request.POST.get('descripcion')
        imagen = request.POST.get('imagen')
        slug = request.POST.get('slug')
        prioridad = request.POST.get('prioridad')
        
        Categoria.objects.create(
            nombre=nombre,
            descripcion=descripcion,
            imagen=imagen,
            slug=slug,
            prioridad=prioridad
        )
        return redirect('ver_categorias')
    
    return render(request, 'categoria/agregar_categoria.html')

def ver_categorias(request):
    categorias = Categoria.objects.all()
    return render(request, 'categoria/ver_categorias.html', {'categorias': categorias})

def actualizar_categoria(request, categoria_id):
    categoria = get_object_or_404(Categoria, id=categoria_id)
    
    if request.method == 'POST':
        categoria.nombre = request.POST.get('nombre')
        categoria.descripcion = request.POST.get('descripcion')
        categoria.imagen = request.POST.get('imagen')
        categoria.slug = request.POST.get('slug')
        categoria.prioridad = request.POST.get('prioridad')
        categoria.save()
        return redirect('ver_categorias')
    
    return render(request, 'categoria/actualizar_categoria.html', {'categoria': categoria})

def borrar_categoria(request, categoria_id):
    categoria = get_object_or_404(Categoria, id=categoria_id)
    
    if request.method == 'POST':
        categoria.delete()
        return redirect('ver_categorias')
    
    return render(request, 'categoria/borrar_categoria.html', {'categoria': categoria})
```

---

## Tercera Parte: Templates y Estructura

### 15-16. Crear Estructura de Templates
```
app_PChop/
└── templates/
    ├── base.html
    ├── header.html
    ├── navbar.html
    ├── footer.html
    ├── inicio.html
    └── categoria/
        ├── agregar_categoria.html
        ├── ver_categorias.html
        ├── actualizar_categoria.html
        └── borrar_categoria.html
```

### 17. `base.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PChop - Sistema de Administración</title>
    <!-- Bootstrap CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <style>
        body {
            background-color: #f8f9fa;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        .navbar {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        }
        .sidebar {
            background-color: #2c3e50;
            min-height: 100vh;
        }
        .footer {
            background-color: #343a40;
            color: white;
            position: fixed;
            bottom: 0;
            width: 100%;
        }
        .card {
            border: none;
            border-radius: 15px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            transition: transform 0.3s ease;
        }
        .card:hover {
            transform: translateY(-5px);
        }
    </style>
</head>
<body>
    {% include 'navbar.html' %}
    
    <div class="container-fluid">
        <div class="row">
            <main class="col-md-9 ms-sm-auto col-lg-10 px-md-4 py-4">
                {% block content %}
                {% endblock %}
            </main>
        </div>
    </div>
    
    {% include 'footer.html' %}
    
    <!-- Bootstrap JS -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>
```

### 18. `navbar.html`
```html
<nav class="navbar navbar-expand-lg navbar-dark">
    <div class="container-fluid">
        <a class="navbar-brand" href="{% url 'inicio' %}">
            🖥️ Sistema de Administración PChop
        </a>
        
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav">
            <span class="navbar-toggler-icon"></span>
        </button>
        
        <div class="collapse navbar-collapse" id="navbarNav">
            <ul class="navbar-nav me-auto">
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'inicio' %}">
                        🏠 Inicio
                    </a>
                </li>
                
                <li class="nav-item dropdown">
                    <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown">
                        📁 Categoría
                    </a>
                    <ul class="dropdown-menu">
                        <li><a class="dropdown-item" href="{% url 'agregar_categoria' %}">Agregar Categoria</a></li>
                        <li><a class="dropdown-item" href="{% url 'ver_categorias' %}">Ver Categoria</a></li>
                        <li><a class="dropdown-item" href="#">Actualizar Categoria</a></li>
                        <li><a class="dropdown-item" href="#">Borrar Categoria</a></li>
                    </ul>
                </li>
                
                <li class="nav-item dropdown">
                    <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown">
                        📦 Producto
                    </a>
                    <ul class="dropdown-menu">
                        <li><a class="dropdown-item" href="#">Agregar Producto</a></li>
                        <li><a class="dropdown-item" href="#">Ver Producto</a></li>
                        <li><a class="dropdown-item" href="#">Actualizar Producto</a></li>
                        <li><a class="dropdown-item" href="#">Borrar Producto</a></li>
                    </ul>
                </li>
                
                <li class="nav-item dropdown">
                    <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown">
                        🛒 Pedido
                    </a>
                    <ul class="dropdown-menu">
                        <li><a class="dropdown-item" href="#">Agregar Pedido</a></li>
                        <li><a class="dropdown-item" href="#">Ver Pedido</a></li>
                        <li><a class="dropdown-item" href="#">Actualizar Pedido</a></li>
                        <li><a class="dropdown-item" href="#">Borrar Pedido</a></li>
                    </ul>
                </li>
            </ul>
        </div>
    </div>
</nav>
```

### 19. `footer.html`
```html
<footer class="footer mt-auto py-3">
    <div class="container text-center">
        <span class="text-white">
            &copy; {% now "Y" %} PChop - Todos los derechos reservados | 
            Fecha del sistema: {% now "d/m/Y H:i" %} | 
            Creado por Adrian Hazael 5I, Cbtis 128
        </span>
    </div>
</footer>
```

### 20. `inicio.html`
```html
{% extends 'base.html' %}

{% block content %}
<div class="row">
    <div class="col-12">
        <div class="card">
            <div class="card-body">
                <h1 class="card-title text-center mb-4">Bienvenido a PChop</h1>
                <div class="row">
                    <div class="col-md-6">
                        <h3>Sistema de Administración</h3>
                        <p class="lead">
                            Gestiona categorías, productos y pedidos de manera eficiente 
                            con nuestro sistema de administración PChop.
                        </p>
                        <ul>
                            <li>Gestión de Categorías</li>
                            <li>Control de Productos</li>
                            <li>Administración de Pedidos</li>
                            <li>Reportes y Estadísticas</li>
                        </ul>
                    </div>
                    <div class="col-md-6">
                        <img src="https://images.unsplash.com/photo-1563013544-824ae1b704d3?ixlib=rb-4.0.3&auto=format&fit=crop&w=500&q=60" 
                             alt="Tienda de Computadoras PChop" 
                             class="img-fluid rounded">
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>
{% endblock %}
```

---

## Cuarta Parte: Templates de Categoría

### 22. Templates de Categoría

#### `agregar_categoria.html`
```html
{% extends 'base.html' %}

{% block content %}
<div class="row justify-content-center">
    <div class="col-md-8">
        <div class="card">
            <div class="card-header bg-primary text-white">
                <h4 class="mb-0">Agregar Nueva Categoría</h4>
            </div>
            <div class="card-body">
                <form method="POST">
                    {% csrf_token %}
                    <div class="mb-3">
                        <label for="nombre" class="form-label">Nombre</label>
                        <input type="text" class="form-control" id="nombre" name="nombre" required>
                    </div>
                    <div class="mb-3">
                        <label for="descripcion" class="form-label">Descripción</label>
                        <textarea class="form-control" id="descripcion" name="descripcion" rows="3" required></textarea>
                    </div>
                    <div class="mb-3">
                        <label for="imagen" class="form-label">URL de Imagen</label>
                        <input type="url" class="form-control" id="imagen" name="imagen">
                    </div>
                    <div class="mb-3">
                        <label for="slug" class="form-label">Slug</label>
                        <input type="text" class="form-control" id="slug" name="slug" required>
                    </div>
                    <div class="mb-3">
                        <label for="prioridad" class="form-label">Prioridad</label>
                        <input type="number" class="form-control" id="prioridad" name="prioridad" value="1" min="1">
                    </div>
                    <div class="d-grid gap-2">
                        <button type="submit" class="btn btn-success">Guardar Categoría</button>
                        <a href="{% url 'ver_categorias' %}" class="btn btn-secondary">Cancelar</a>
                    </div>
                </form>
            </div>
        </div>
    </div>
</div>
{% endblock %}
```

#### `ver_categorias.html`
```html
{% extends 'base.html' %}

{% block content %}
<div class="row">
    <div class="col-12">
        <div class="card">
            <div class="card-header bg-info text-white d-flex justify-content-between align-items-center">
                <h4 class="mb-0">Lista de Categorías</h4>
                <a href="{% url 'agregar_categoria' %}" class="btn btn-light">➕ Agregar Nueva</a>
            </div>
            <div class="card-body">
                <div class="table-responsive">
                    <table class="table table-striped table-hover">
                        <thead class="table-dark">
                            <tr>
                                <th>ID</th>
                                <th>Nombre</th>
                                <th>Descripción</th>
                                <th>Fecha Creación</th>
                                <th>Prioridad</th>
                                <th>Acciones</th>
                            </tr>
                        </thead>
                        <tbody>
                            {% for categoria in categorias %}
                            <tr>
                                <td>{{ categoria.id }}</td>
                                <td>{{ categoria.nombre }}</td>
                                <td>{{ categoria.descripcion|truncatewords:10 }}</td>
                                <td>{{ categoria.fecha_creacion|date:"d/m/Y H:i" }}</td>
                                <td>{{ categoria.prioridad }}</td>
                                <td>
                                    <a href="{% url 'actualizar_categoria' categoria.id %}" class="btn btn-warning btn-sm">✏️ Editar</a>
                                    <a href="{% url 'borrar_categoria' categoria.id %}" class="btn btn-danger btn-sm">🗑️ Borrar</a>
                                </td>
                            </tr>
                            {% empty %}
                            <tr>
                                <td colspan="6" class="text-center">No hay categorías registradas</td>
                            </tr>
                            {% endfor %}
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>
</div>
{% endblock %}
```

#### `actualizar_categoria.html`
```html
{% extends 'base.html' %}

{% block content %}
<div class="row justify-content-center">
    <div class="col-md-8">
        <div class="card">
            <div class="card-header bg-warning text-dark">
                <h4 class="mb-0">Actualizar Categoría: {{ categoria.nombre }}</h4>
            </div>
            <div class="card-body">
                <form method="POST">
                    {% csrf_token %}
                    <div class="mb-3">
                        <label for="nombre" class="form-label">Nombre</label>
                        <input type="text" class="form-control" id="nombre" name="nombre" value="{{ categoria.nombre }}" required>
                    </div>
                    <div class="mb-3">
                        <label for="descripcion" class="form-label">Descripción</label>
                        <textarea class="form-control" id="descripcion" name="descripcion" rows="3" required>{{ categoria.descripcion }}</textarea>
                    </div>
                    <div class="mb-3">
                        <label for="imagen" class="form-label">URL de Imagen</label>
                        <input type="url" class="form-control" id="imagen" name="imagen" value="{{ categoria.imagen|default:'' }}">
                    </div>
                    <div class="mb-3">
                        <label for="slug" class="form-label">Slug</label>
                        <input type="text" class="form-control" id="slug" name="slug" value="{{ categoria.slug }}" required>
                    </div>
                    <div class="mb-3">
                        <label for="prioridad" class="form-label">Prioridad</label>
                        <input type="number" class="form-control" id="prioridad" name="prioridad" value="{{ categoria.prioridad }}" min="1">
                    </div>
                    <div class="d-grid gap-2">
                        <button type="submit" class="btn btn-primary">Actualizar Categoría</button>
                        <a href="{% url 'ver_categorias' %}" class="btn btn-secondary">Cancelar</a>
                    </div>
                </form>
            </div>
        </div>
    </div>
</div>
{% endblock %}
```

#### `borrar_categoria.html`
```html
{% extends 'base.html' %}

{% block content %}
<div class="row justify-content-center">
    <div class="col-md-6">
        <div class="card">
            <div class="card-header bg-danger text-white">
                <h4 class="mb-0">Confirmar Eliminación</h4>
            </div>
            <div class="card-body text-center">
                <h5>¿Estás seguro de que deseas eliminar la categoría?</h5>
                <p class="lead"><strong>{{ categoria.nombre }}</strong></p>
                <p class="text-muted">Esta acción no se puede deshacer.</p>
                
                <form method="POST">
                    {% csrf_token %}
                    <div class="d-grid gap-2 d-md-flex justify-content-md-center">
                        <button type="submit" class="btn btn-danger me-md-2">✅ Sí, Eliminar</button>
                        <a href="{% url 'ver_categorias' %}" class="btn btn-secondary">❌ Cancelar</a>
                    </div>
                </form>
            </div>
        </div>
    </div>
</div>
{% endblock %}
```

---

## Quinta Parte: Configuración URLs y Settings

### 24. `app_PChop/urls.py`
```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.inicio_PChop, name='inicio'),
    path('categoria/agregar/', views.agregar_categoria, name='agregar_categoria'),
    path('categoria/ver/', views.ver_categorias, name='ver_categorias'),
    path('categoria/actualizar/<int:categoria_id>/', views.actualizar_categoria, name='actualizar_categoria'),
    path('categoria/borrar/<int:categoria_id>/', views.borrar_categoria, name='borrar_categoria'),
]
```

### 25. Configurar `settings.py`
```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'app_PChop',  # Agregar esta línea
]
```

### 26. Configurar `backend_PChop/urls.py`
```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app_PChop.urls')),
]
```

### 27. Registrar Modelos en `admin.py`
```python
from django.contrib import admin
from .models import Categoria, Producto, Pedido

@admin.register(Categoria)
class CategoriaAdmin(admin.ModelAdmin):
    list_display = ['nombre', 'activa', 'prioridad', 'fecha_creacion']
    list_filter = ['activa', 'fecha_creacion']
    search_fields = ['nombre']

@admin.register(Producto)
class ProductoAdmin(admin.ModelAdmin):
    list_display = ['nombre', 'categoria', 'precio', 'stock', 'marca']
    list_filter = ['categoria', 'marca']
    search_fields = ['nombre', 'marca']

@admin.register(Pedido)
class PedidoAdmin(admin.ModelAdmin):
    list_display = ['id', 'cliente', 'total', 'estado', 'fecha_pedido']
    list_filter = ['estado', 'fecha_pedido']
    search_fields = ['cliente']
```

### 27.5 Migraciones Finales
```bash
python manage.py makemigrations
python manage.py migrate
```

---

## Sexta Parte: Ejecución Final

### 31. Ejecutar Servidor
```bash
python manage.py runserver 8017
```

### Estructura Final del Proyecto
```
UIII_PChop_1128/
├── .venv/
├── backend_PChop/
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── app_PChop/
│   ├── migrations/
│   ├── templates/
│   │   ├── base.html
│   │   ├── header.html
│   │   ├── navbar.html
│   │   ├── footer.html
│   │   ├── inicio.html
│   │   └── categoria/
│   │       ├── agregar_categoria.html
│   │       ├── ver_categorias.html
│   │       ├── actualizar_categoria.html
│   │       └── borrar_categoria.html
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── tests.py
│   ├── urls.py
│   └── views.py
├── manage.py
└── db.sqlite3
```

El proyecto está completamente configurado y funcional en el puerto 8017 con operaciones CRUD completas para el modelo Categoría.
