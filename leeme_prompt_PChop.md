# 🧩 Proyecto PChop — Primera Parte

## 📘 Datos Generales del Proyecto
- **Proyecto:** PChop  
- **Lenguaje:** Python  
- **Framework:** Django  
- **Editor:** Visual Studio Code  
- **Tipo:** Sistema Web Administrativo para una Tienda de Computadoras  

---

## ⚙️ Configuración Inicial del Entorno

### 1️⃣ Crear carpeta del proyecto
```bash
mkdir UIII_PChop_1128
cd UIII_PChop_1128
2️⃣ Abrir Visual Studio Code
Abrir VS Code

Menú: Archivo → Abrir carpeta → UIII_PChop_1128

3️⃣ Abrir la terminal integrada
Menú: Ver → Terminal

Confirmar ruta actual:

makefile
Copiar código
C:\Users\<usuario>\UIII_PChop_1128>
4️⃣ Crear el entorno virtual
bash
Copiar código
python -m venv .venv
5️⃣ Activar el entorno virtual
bash
Copiar código
.venv\Scripts\activate
💡 Si se activó correctamente, la terminal mostrará:
(.venv) C:\Users\<usuario>\UIII_PChop_1128>

6️⃣ Seleccionar el intérprete de Python
En VS Code:
Ctrl + Shift + P → Python: Select Interpreter → Selecciona .venv

7️⃣ Instalar Django
bash
Copiar código
pip install django
8️⃣ Crear el proyecto Django (sin duplicar carpeta)
bash
Copiar código
django-admin startproject backend_PChop .
Estructura generada:

markdown
Copiar código
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
9️⃣ Ejecutar el servidor en el puerto 8017
bash
Copiar código
python manage.py runserver 8017
🔟 Abrir en el navegador
cpp
Copiar código
http://127.0.0.1:8017/
🧱 Creación de la Aplicación
11️⃣ Crear aplicación app_PChop
bash
Copiar código
python manage.py startapp app_PChop
Estructura del proyecto:

markdown
Copiar código
UIII_PChop_1128/
│
├── app_PChop/
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── views.py
│   ├── tests.py
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
🧩 Modelos — models.py
12️⃣ Código del archivo app_PChop/models.py
python
Copiar código
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
12.5️⃣ Realizar migraciones
bash
Copiar código
python manage.py makemigrations
python manage.py migrate
📦 Trabajo con el Modelo CATEGORÍA
13️⃣ Solo trabajar inicialmente con Categoria
14️⃣ En views.py crear funciones:
inicio_PChop

agregar_categoria

actualizar_categoria

realizar_actualizacion_categoria

borrar_categoria

🧭 Estructura de Plantillas (Templates)
15️⃣ Crear carpeta principal templates
css
Copiar código
app_PChop/
│
└── templates/
    ├── base.html
    ├── header.html
    ├── navbar.html
    ├── footer.html
    └── inicio.html
16️⃣ Configuración de las plantillas
base.html → incluir Bootstrap (CSS + JS) y bloques {% block content %}

header.html → título del sistema

navbar.html → menú con submenús

footer.html → derechos de autor, fecha y creador

inicio.html → información general + imagen sobre PChop

🗂️ Subcarpeta para Categoría
21️⃣ Crear estructura:
markdown
Copiar código
app_PChop/
│
└── templates/
    └── categoria/
        ├── agregar_categoria.html
        ├── ver_categorias.html
        ├── actualizar_categoria.html
        └── borrar_categoria.html
22️⃣ Configuración de las páginas:
ver_categorias.html → mostrar tabla con botones Ver, Editar, Borrar

No usar forms.py

🌐 Configuración de URLs y Enlaces
24️⃣ Crear urls.py en app_PChop
Enlazar las rutas a las funciones CRUD del views.py.

25️⃣ Registrar la app en settings.py
python
Copiar código
INSTALLED_APPS = [
    ...
    'app_PChop',
]
26️⃣ Enlazar urls.py del proyecto
En backend_PChop/urls.py agregar:

python
Copiar código
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app_PChop.urls')),
]
🧾 Administración y Migraciones Finales
27️⃣ Registrar modelos en admin.py
python
Copiar código
from django.contrib import admin
from .models import Categoria, Producto, Pedido

admin.site.register(Categoria)
admin.site.register(Producto)
admin.site.register(Pedido)
Ejecutar nuevamente:
bash
Copiar código
python manage.py makemigrations
python manage.py migrate
🎨 Diseño y Estilo
28️⃣ Recomendaciones
Usar colores suaves y modernos

Diseño simple y limpio

No validar entradas de datos

Íconos de Bootstrap solo en los menús principales

🧰 Estructura Completa del Proyecto
css
Copiar código
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
🚀 Ejecución Final
31️⃣ Ejecutar el servidor
bash
Copiar código
python manage.py runserver 8017
Abrir en el navegador:

cpp
Copiar código
http://127.0.0.1:8017/
✅ Proyecto completamente funcional (solo módulo Categoría por ahora)

👨‍💻 Creado por:
Adrian Hazael — 5I — CBTIS 128
