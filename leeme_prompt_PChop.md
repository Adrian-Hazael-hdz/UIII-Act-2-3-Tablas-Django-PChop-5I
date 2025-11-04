
---

````markdown
# 🧩 Proyecto PChop — Primera Parte

---

## 📘 Información General

**Nombre del Proyecto:** PChop  
**Lenguaje:** Python  
**Framework:** Django  
**Editor:** Visual Studio Code  
**Tipo:** Sistema Web Administrativo para Tienda de Computadoras  

---

## ⚙️ Configuración Inicial del Entorno

---

### 🥇 1. Crear Carpeta del Proyecto

```bash
mkdir UIII_PChop_1128
cd UIII_PChop_1128
````

---

### 🥈 2. Abrir la Carpeta en Visual Studio Code

* Abrir VS Code
* Ir a: **Archivo → Abrir carpeta → UIII_PChop_1128**

---

### 🥉 3. Abrir la Terminal Integrada

* Menú: **Ver → Terminal**
* Verifica la ruta actual:

  ```
  C:\Users\<usuario>\UIII_PChop_1128>
  ```

---

### 🔢 4. Crear el Entorno Virtual

```bash
python -m venv .venv
```

---

### 🧠 5. Activar el Entorno Virtual

```bash
.venv\Scripts\activate
```

> 💡 Si todo está correcto, verás:
> `(.venv) C:\Users\<usuario>\UIII_PChop_1128>`

---

### 🐍 6. Seleccionar el Intérprete de Python

En VS Code:

* Presiona `Ctrl + Shift + P`
* Escribe: `Python: Select Interpreter`
* Selecciona: `.venv\Scripts\python.exe`

---

### 💾 7. Instalar Django

```bash
pip install django
```

---

### 🏗️ 8. Crear el Proyecto Django (sin duplicar carpeta)

```bash
django-admin startproject backend_PChop .
```

**Estructura generada:**

```
UIII_PChop_1128/
│
├── backend_PChop/
│   ├── __init__.py
│   ├── asgi.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
│
├── manage.py
└── .venv/
```

---

### 🌐 9. Ejecutar el Servidor en el Puerto 8017

```bash
python manage.py runserver 8017
```

---

### 🔗 10. Abrir el Proyecto en el Navegador

```
http://127.0.0.1:8017/
```

---

## 🧱 Creación de la Aplicación

---

### ⚒️ 11. Crear la Aplicación `app_PChop`

```bash
python manage.py startapp app_PChop
```

**Estructura del Proyecto:**

```
UIII_PChop_1128/
│
├── app_PChop/
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── views.py
│   ├── migrations/
│   └── __init__.py
│
├── backend_PChop/
│   ├── settings.py
│   ├── urls.py
│   └── ...
│
├── manage.py
└── .venv/
```

---

## 🧩 Modelos — `models.py`

---

### 🧱 12. Crear Modelos en `app_PChop/models.py`

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

### 💽 12.5 Realizar Migraciones

```bash
python manage.py makemigrations
python manage.py migrate
```

---

## 🧩 Trabajo con el Modelo `Categoria`

---

### 🧮 13. Trabajar Inicialmente Solo con `Categoria`

Los modelos `Producto` y `Pedido` se dejarán pendientes.

---

### 🧰 14. Crear Funciones en `views.py`

Definir las vistas:

* `inicio_PChop`
* `agregar_categoria`
* `actualizar_categoria`
* `realizar_actualizacion_categoria`
* `borrar_categoria`

---

## 🧭 Estructura de Plantillas (Templates)

---

### 🗂️ 15. Crear Carpeta Principal `templates`

```
app_PChop/
│
└── templates/
    ├── base.html
    ├── header.html
    ├── navbar.html
    ├── footer.html
    └── inicio.html
```

---

### 🖋️ 16. Configurar las Plantillas Base

#### `base.html`

* Incluir Bootstrap (CSS + JS)
* Añadir bloques `{% block content %}{% endblock %}`

#### `navbar.html`

* Menú principal:

  * Inicio
  * Categoría → Agregar / Ver / Actualizar / Borrar
  * Producto → Agregar / Ver / Actualizar / Borrar
  * Pedido → Agregar / Ver / Actualizar / Borrar
* Íconos solo en las opciones principales

#### `footer.html`

* Mostrar derechos de autor, fecha y creador:

  ```
  © {{ fecha_actual }} | Creado por Adrian Hazael 5I, CBTIS 128
  ```

#### `inicio.html`

* Incluir texto informativo sobre el sistema y una imagen de tienda de computadoras.

---

## 🗂️ Subcarpeta para Categoría

---

### 🗃️ 21. Crear Subcarpeta

```
app_PChop/
│
└── templates/
    └── categoria/
        ├── agregar_categoria.html
        ├── ver_categorias.html
        ├── actualizar_categoria.html
        └── borrar_categoria.html
```

---

### 📄 22. Configuración de las Páginas

* `ver_categorias.html` → mostrar registros en tabla
  con botones **Ver**, **Editar** y **Borrar**

> ⚠️ No usar `forms.py` (formularios hechos con HTML puro).

---

## 🌐 Configuración de URLs y Enlaces

---

### 🔗 24. Crear Archivo `urls.py` en `app_PChop`

Enlazar las rutas de las funciones CRUD del `views.py`.

---

### ⚙️ 25. Registrar la App en `settings.py`

```python
INSTALLED_APPS = [
    ...
    'app_PChop',
]
```

---

### 🧭 26. Configurar `urls.py` del Proyecto

En `backend_PChop/urls.py` agregar:

```python
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app_PChop.urls')),
]
```

---

## 🧾 Administración y Migraciones Finales

---

### 🗃️ 27. Registrar Modelos en `admin.py`

```python
from django.contrib import admin
from .models import Categoria, Producto, Pedido

admin.site.register(Categoria)
admin.site.register(Producto)
admin.site.register(Pedido)
```

Luego ejecutar:

```bash
python manage.py makemigrations
python manage.py migrate
```

---

## 🎨 Diseño y Estilo

---

### 🧡 28. Recomendaciones de Diseño

* Colores suaves, atractivos y modernos
* Diseño simple y limpio
* No validar entrada de datos
* Íconos de Bootstrap solo en menús principales

---

## 🧰 Estructura Completa del Proyecto

---

```
UIII_PChop_1128/
│
├── .venv/
├── app_PChop/
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── views.py
│   ├── urls.py
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
│   ├── migrations/
│   └── __init__.py
│
├── backend_PChop/
│   ├── settings.py
│   ├── urls.py
│   ├── asgi.py
│   ├── wsgi.py
│   └── __init__.py
│
└── manage.py
```

---

## 🚀 Ejecución Final

---

### ▶️ 31. Ejecutar el Servidor

```bash
python manage.py runserver 8017
```

Abrir en el navegador:

```
http://127.0.0.1:8017/
```

---

✅ **Proyecto funcional — módulo Categoría completo**

---

### 👨‍💻 Autor

**Adrian Hazael — 5I — CBTIS 128**

```


```
