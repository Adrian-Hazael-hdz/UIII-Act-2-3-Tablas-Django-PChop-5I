🖥️ Primera Parte – Proyecto: PChop

Lenguaje: Python
Framework: Django
Editor: Visual Studio Code

🔧 Configuración Inicial

Crear la carpeta del proyecto:
Nombre: UIII_PChop_1128

Abrir Visual Studio Code sobre la carpeta UIII_PChop_1128.

Abrir la terminal integrada en Visual Studio Code.
(Menú: Ver → Terminal o Ctrl + ñ)

Crear el entorno virtual desde la terminal:

python -m venv .venv


Activar el entorno virtual:

En Windows (PowerShell):

.venv\Scripts\activate


En macOS/Linux:

source .venv/bin/activate


Seleccionar el intérprete de Python desde VS Code:

Comando: Ctrl + Shift + P → “Python: Select Interpreter” → elegir .venv.

Instalar Django:

pip install django


Crear el proyecto Django sin duplicar carpetas:

django-admin startproject backend_PChop .


Ejecutar el servidor en el puerto 8017:

python manage.py runserver 8017


Copiar y pegar el enlace en el navegador:

http://127.0.0.1:8017/

📦 Creación de la Aplicación

Crear la aplicación principal:

python manage.py startapp app_PChop

🧱 Modelo – models.py

Agregar el siguiente código en app_PChop/models.py:

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


12.5 Realizar las migraciones:

python manage.py makemigrations
python manage.py migrate

🧩 Trabajo con el modelo Categoría

Primero trabajar con el modelo: CATEGORIA.

En views.py de app_PChop, crear las funciones:

inicio_PChop

agregar_categoria

actualizar_categoria

realizar_actualizacion_categoria

borrar_categoria

🧱 Estructura de Templates

Crear la carpeta:

app_PChop/templates


Dentro de templates, crear los archivos HTML:

base.html

header.html

navbar.html

footer.html

inicio.html

Agregar Bootstrap (CSS y JS) en base.html.

Diseñar navbar.html con las opciones:

Sistema de Administración PChop

Inicio

Categoría

Agregar Categoría

Ver Categorías

Actualizar Categoría

Borrar Categoría

Producto

Agregar Producto

Ver Producto

Actualizar Producto

Borrar Producto

Pedido

Agregar Pedido

Ver Pedido

Actualizar Pedido

Borrar Pedido
(Agregar íconos solo a las opciones principales.)

Diseñar footer.html:

Incluir derechos de autor

Fecha del sistema

Texto: “Creado por Adrian Hazael 5I, CBTis 128”

Mantenerlo fijo al final de la página.

En inicio.html:

Colocar información general del sistema

Añadir una imagen desde la web sobre una tienda de computadoras.

📂 Sección Categorías

Crear subcarpeta:

app_PChop/templates/categoria


Crear los archivos HTML con su código correspondiente:

agregar_categoria.html

ver_categorias.html (mostrar en tabla con botones ver, editar, borrar)

actualizar_categoria.html

borrar_categoria.html

⚙️ Configuraciones Django

No utilizar forms.py.

Crear archivo urls.py en app_PChop con las rutas necesarias para el CRUD de categorías.

Agregar la app app_PChop en settings.py:

INSTALLED_APPS = [
    ...,
    'app_PChop',
]


Configurar urls.py de backend_PChop para enlazar con app_PChop.

Registrar los modelos en admin.py y volver a migrar:

python manage.py makemigrations
python manage.py migrate

🎨 Estilo y Presentación

Usar colores suaves, atractivos y modernos.
Las páginas deben ser sencillas y funcionales.

No validar entrada de datos.

Crear toda la estructura de carpetas y archivos al inicio.

Proyecto totalmente funcional.

Ejecutar el servidor nuevamente en el puerto 8017:

python manage.py runserver 8017
