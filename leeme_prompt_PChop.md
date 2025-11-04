🧩 PRIMERA PARTE — PROYECTO: PChop
🔹 Datos Generales del Proyecto
Elemento	Descripción
Nombre del Proyecto:	PChop
Lenguaje de Programación:	Python
Framework:	Django
Editor:	Visual Studio Code
Tipo de Proyecto:	Sistema Web Administrativo para Tienda de Computadoras
⚙️ Configuración Inicial del Entorno
1. Creación de la Carpeta Principal del Proyecto

Crear una carpeta con el nombre:

UIII_PChop_1128


Esta carpeta será la raíz del proyecto.

2. Abrir Visual Studio Code

Abrir VS Code.

Seleccionar la carpeta creada:

Menú: Archivo → Abrir carpeta → UIII_PChop_1128

3. Abrir la Terminal Integrada en VS Code

Menú: Ver → Terminal

Confirmar que se está ubicado en la ruta del proyecto:

(base) C:\Users\<usuario>\UIII_PChop_1128>

4. Crear el Entorno Virtual

Ejecutar en la terminal:

python -m venv .venv

5. Activar el Entorno Virtual

Comando para Windows:

.venv\Scripts\activate


Una vez activado, la terminal mostrará algo como:

(.venv) C:\Users\<usuario>\UIII_PChop_1128>

6. Seleccionar el Intérprete de Python

En VS Code:

Presionar Ctrl + Shift + P

Buscar: Python: Select Interpreter

Elegir: .venv\Scripts\python.exe

7. Instalar Django

En la terminal (con el entorno activo):

pip install django

8. Crear el Proyecto Django (sin duplicar carpeta)

Ejecutar:

django-admin startproject backend_PChop .


La estructura generada será:

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

9. Ejecutar el Servidor en el Puerto 8017
python manage.py runserver 8017


Resultado en consola:

Starting development server at http://127.0.0.1:8017/

10. Probar el Servidor

Copiar y pegar en el navegador:

http://127.0.0.1:8017/


Verificar que la página de inicio de Django se muestre correctamente.

🧱 Creación y Configuración de la Aplicación
11. Crear la Aplicación app_PChop

Ejecutar:

python manage.py startapp app_PChop


Nueva estructura:

UIII_PChop_1128/
│
├── app_PChop/
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── tests.py
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

🧩 Definición de los Modelos — models.py
12. Agregar el siguiente código en app_PChop/models.py:
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

12.5. Migraciones del Proyecto

Ejecutar los comandos:

python manage.py makemigrations
python manage.py migrate

📦 Trabajo con el Modelo CATEGORÍA
13. Trabajar inicialmente con el modelo Categoria

Los modelos Producto y Pedido quedarán pendientes por ahora.

14. Crear Funciones en views.py

Dentro de app_PChop/views.py, definir las funciones:

inicio_PChop

agregar_categoria

actualizar_categoria

realizar_actualizacion_categoria

borrar_categoria

(Cada función representará una operación CRUD.)

🧭 Estructura de Plantillas (Templates)
15. Crear carpeta principal templates dentro de app_PChop:
app_PChop/
│
├── templates/
│   ├── base.html
│   ├── header.html
│   ├── navbar.html
│   ├── footer.html
│   └── inicio.html

16. Configurar los archivos base

base.html:

Incluir Bootstrap CSS y JS desde CDN.

Definir bloques {% block content %}{% endblock %}.

header.html:

Colocar el título principal del sistema.

navbar.html:

Barra de navegación con menús desplegables:

Inicio

Categoría → Agregar / Ver / Actualizar / Borrar

Producto → Agregar / Ver / Actualizar / Borrar

Pedido → Agregar / Ver / Actualizar / Borrar

Incluir íconos solo en los menús principales.

footer.html:

Mostrar:

© {{ fecha_actual }} | Creado por Adrian Hazael 5I, Cbtis 128


Fijo al final de la página.

inicio.html:

Mostrar información general del sistema PChop.

Incluir una imagen tomada desde la red (tienda de computadoras).

🗂️ Subcarpeta de Categoría
21. Crear carpeta interna para Categoría:
app_PChop/
│
└── templates/
    ├── categoria/
    │   ├── agregar_categoria.html
    │   ├── ver_categorias.html
    │   ├── actualizar_categoria.html
    │   └── borrar_categoria.html

22. Configuración de las páginas:

ver_categorias.html → Mostrar tabla de registros con botones:

Ver

Editar

Borrar

⚠️ No se utilizará forms.py (formularios serán creados manualmente con HTML y etiquetas <form>).

🌐 Configuración de Rutas y Enlaces
24. Crear archivo urls.py en app_PChop:

Enlazar las rutas a las funciones CRUD del views.py.

25. Registrar la app en settings.py:
INSTALLED_APPS = [
    ...
    'app_PChop',
]

26. Configurar urls.py de backend_PChop:

Incluir las rutas de app_PChop mediante include().

🧾 Administración y Migraciones Finales
27. Registrar los modelos en admin.py:
from django.contrib import admin
from .models import Categoria, Producto, Pedido

admin.site.register(Categoria)
admin.site.register(Producto)
admin.site.register(Pedido)


Luego ejecutar nuevamente:

python manage.py makemigrations
python manage.py migrate

🎨 Diseño y Estilo
28. Indicaciones Generales de Diseño

Usar colores suaves y modernos (paletas pastel o neutras).

Páginas sencillas y limpias.

No validar datos (entradas libres).

Utilizar íconos de Bootstrap (bi bi-...).

29. Crear toda la estructura completa desde el inicio.

Asegurarse de que todas las carpetas y archivos existan antes de comenzar el desarrollo.

30. Verificar el funcionamiento general del proyecto.

Comprobar que las rutas, plantillas y vistas de categoría funcionen correctamente.

31. Ejecutar nuevamente el servidor
python manage.py runserver 8017


Verificar en navegador:

http://127.0.0.1:8017/
