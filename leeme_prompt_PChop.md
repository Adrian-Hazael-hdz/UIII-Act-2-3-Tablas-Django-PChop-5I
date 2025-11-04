🖥️ PROYECTO: PChop

Lenguaje: Python
Framework: Django
Editor: Visual Studio Code

🧱 1. Creación y preparación del entorno

1️⃣ Crear la carpeta del proyecto

mkdir UIII_PChop_1128


2️⃣ Abrir Visual Studio Code sobre la carpeta

Abre VS Code

Menú: Archivo > Abrir carpeta

Selecciona: UIII_PChop_1128

3️⃣ Abrir la terminal integrada

En VS Code:
Menú Terminal > Nueva terminal
o presiona Ctrl + ñ

4️⃣ Crear el entorno virtual “.venv”

python -m venv .venv


5️⃣ Activar el entorno virtual

En Windows:

.venv\Scripts\activate


En Linux/Mac:

source .venv/bin/activate


6️⃣ Seleccionar el intérprete de Python en VS Code

Presiona Ctrl + Shift + P

Escribe: Python: Select Interpreter

Selecciona el que termine con .venv

7️⃣ Instalar Django

pip install django

⚙️ 2. Crear el proyecto y aplicación Django

8️⃣ Crear el proyecto sin duplicar carpeta

django-admin startproject backend_PChop .


9️⃣ Ejecutar servidor en el puerto 8017

python manage.py runserver 8017


🔟 Copiar y pegar el enlace en el navegador

http://127.0.0.1:8017/


1️⃣1️⃣ Crear la aplicación

python manage.py startapp app_PChop

💾 3. MODELOS (models.py)

Ubicado en: app_PChop/models.py

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
    categoria = models.ForeignKey(Categoria, on_delete=models.CASCADE, related_name='productos')  # 1 a muchos
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
    productos = models.ManyToManyField(Producto, related_name='pedidos')  # muchos a muchos
    fecha_pedido = models.DateTimeField(auto_now_add=True)
    cliente = models.CharField(max_length=100)
    direccion_envio = models.CharField(max_length=255)
    total = models.DecimalField(max_digits=10, decimal_places=2)
    estado = models.CharField(max_length=50, default='Pendiente')
    metodo_pago = models.CharField(max_length=50)
    observaciones = models.TextField(blank=True, null=True)

    def __str__(self):
        return f"Pedido #{self.id} - {self.cliente}"

🧩 4. Migraciones

12.5️⃣ Crear y aplicar migraciones

python manage.py makemigrations
python manage.py migrate

📁 5. Estructura de plantillas HTML

15️⃣ Crear carpeta templates dentro de app_PChop

app_PChop/
 ├── templates/
 │   ├── base.html
 │   ├── header.html
 │   ├── navbar.html
 │   ├── footer.html
 │   └── inicio.html


17️⃣ Agregar Bootstrap a base.html (CSS + JS)

18️⃣ En navbar.html incluir menús y submenús:

Sistema de Administración PChop

Inicio

Categoría → Agregar, Ver, Actualizar, Borrar

Producto → Agregar, Ver, Actualizar, Borrar

Pedido → Agregar, Ver, Actualizar, Borrar
(Con íconos solo en los menús principales)

19️⃣ En footer.html:

Derechos de autor, fecha del sistema y “Creado por Adrian Hazael 5I, CBTIS 128”.
Fijo al final de la página.

20️⃣ En inicio.html:

Información sobre el sistema y una imagen de PChop (tienda de computadoras).

📂 6. Carpeta Categoría

21️⃣ Crear subcarpeta:

app_PChop/templates/categoria/


22️⃣ Crear archivos:

agregar_categoria.html

ver_categorias.html (tabla con botones ver, editar, borrar)

actualizar_categoria.html

borrar_categoria.html

Sin usar forms.py, solo con HTML y views.

⚡ 7. Vistas y Rutas

14️⃣ En views.py crear funciones:

inicio_PChop

agregar_categoria

actualizar_categoria

realizar_actualizacion_categoria

borrar_categoria

24️⃣ Crear urls.py en app_PChop
Configura las rutas para conectar las vistas CRUD.

26️⃣ Configurar urls.py de backend_PChop
Incluye la app:

path('', include('app_PChop.urls')),

🧭 8. Configuraciones finales

25️⃣ Registrar la app en settings.py

INSTALLED_APPS = [
    ...,
    'app_PChop',
]


27️⃣ Registrar modelos en admin.py y volver a migrar

from django.contrib import admin
from .models import Categoria, Producto, Pedido

admin.site.register(Categoria)
admin.site.register(Producto)
admin.site.register(Pedido)

python manage.py makemigrations
python manage.py migrate

🎨 9. Recomendaciones de diseño

28️⃣ Colores suaves, atractivos y modernos
28️⃣ Sin validación de datos
29️⃣ Crear toda la estructura completa desde el inicio
30️⃣ Proyecto totalmente funcional

🚀 10. Ejecución final

31️⃣ Ejecutar el servidor

python manage.py runserver 8017


👉 Abre en navegador:

http://127.0.0.1:8017/
