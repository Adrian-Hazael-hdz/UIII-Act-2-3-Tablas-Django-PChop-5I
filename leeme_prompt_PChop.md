Perfecto 👌 Aquí tienes **todas las especificaciones completas y ordenadas** para tu nuevo proyecto **“PChop”**, hechas con el **mismo formato y nivel de detalle** que el primer proyecto que te di, pero adaptadas **exactamente** a los nuevos datos que me diste.

---

# 💻 PROYECTO DJANGO COMPLETO: **PChop**

## 🧩 Datos generales

* **Proyecto:** PChop
* **Lenguaje:** Python
* **Framework:** Django
* **Editor:** Visual Studio Code (VS Code)
* **Puerto de ejecución:** 8017
* **Descripción:** Sistema de administración para una tienda de computadoras (categorías, productos y pedidos).

---

## 🧱 Primera parte: creación del entorno y estructura del proyecto

### 1️⃣ Crear la carpeta del proyecto principal

**Nombre de carpeta:**
`UIII_PChop_1128`

📍 *Procedimiento:*

* Crear la carpeta desde el explorador de archivos o con el comando:

```bash
mkdir UIII_PChop_1128
```

---

### 2️⃣ Abrir VS Code sobre la carpeta creada

📍 *Procedimiento:*

* Abrir VS Code.
* Desde el menú superior:
  **Archivo → Abrir carpeta → Seleccionar “UIII_PChop_1128”**

---

### 3️⃣ Abrir terminal integrada en VS Code

📍 *Procedimiento:*

* En VS Code, ir a:
  **Ver → Terminal**
* Se abrirá la terminal en la ruta del proyecto.

---

### 4️⃣ Crear el entorno virtual “.venv”

📍 *Comando en la terminal:*

```bash
python -m venv .venv
```

---

### 5️⃣ Activar el entorno virtual

📍 *Comando:*

```bash
.venv\Scripts\activate
```

*(Debe aparecer “(.venv)” al inicio de la línea en la terminal.)*

---

### 6️⃣ Activar el intérprete de Python en VS Code

📍 *Procedimiento:*

* Presionar **Ctrl + Shift + P**
* Buscar “**Python: Select Interpreter**”
* Elegir el que indique la ruta de tu entorno virtual:
  `.venv\Scripts\python.exe`

---

### 7️⃣ Instalar Django

📍 *Comando:*

```bash
pip install django
```

---

### 8️⃣ Crear el proyecto Django sin duplicar carpeta

📍 *Comando:*

```bash
django-admin startproject backend_PChop .
```

(El punto final evita que se cree una carpeta adicional.)

---

### 9️⃣ Ejecutar el servidor en el puerto **8017**

📍 *Comando:*

```bash
python manage.py runserver 8017
```

---

### 🔟 Copiar el link generado y abrir en navegador

**URL:**
[`http://127.0.0.1:8017`](http://127.0.0.1:8017)

---

### 1️⃣1️⃣ Crear la aplicación “app_PChop”

📍 *Comando:*

```bash
python manage.py startapp app_PChop
```

---

## 🧩 Segunda parte: creación de los modelos

### 1️⃣2️⃣ Archivo **models.py** en `app_PChop`

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
```

---

### 1️⃣2️⃣.5️⃣ Realizar migraciones

📍 *Comandos:*

```bash
python manage.py makemigrations
python manage.py migrate
```

---

## 🧩 Tercera parte: vistas, templates y rutas

### 1️⃣3️⃣ Trabajar primero con el modelo **Categoría**

---

### 1️⃣4️⃣ En `views.py` crear funciones:

* inicio_PChop
* agregar_categoria
* actualizar_categoria
* realizar_actualizacion_categoria
* borrar_categoria

*(Cada función mostrará, agregará, editará o borrará categorías según corresponda.)*

---

### 1️⃣5️⃣ Crear carpeta `templates` dentro de `app_PChop`

Ruta:

```
app_PChop/templates/
```

---

### 1️⃣6️⃣ Dentro de `templates`, crear los archivos:

* **base.html**
* **header.html**
* **navbar.html**
* **footer.html**
* **inicio.html**

---

### 1️⃣7️⃣ En `base.html`, agregar Bootstrap para CSS y JS

Usar los CDN oficiales de Bootstrap.

---

### 1️⃣8️⃣ En `navbar.html`, incluir las siguientes opciones:

**Menú principal:**

* Sistema de Administración PChop
* Inicio
* Categoría

  * Agregar categoría
  * Ver categorías
  * Actualizar categoría
  * Borrar categoría
* Producto

  * Agregar producto
  * Ver producto
  * Actualizar producto
  * Borrar producto
* Pedido

  * Agregar pedido
  * Ver pedido
  * Actualizar pedido
  * Borrar pedido

📍 *Nota:* Incluir íconos en las opciones principales (no en los submenús).

---

### 1️⃣9️⃣ En `footer.html` incluir:

* Derechos de autor
* Fecha del sistema
* Texto: **“Creado por Adrian Hazael 5I, CBTIS 128”**
  📍 Mantener el footer fijo al final de la página.

---

### 2️⃣0️⃣ En `inicio.html`:

Colocar información del sistema y una imagen tomada de internet sobre **PChop (tienda de computadoras)**.

---

### 2️⃣1️⃣ Crear subcarpeta **categoria** dentro de `templates`

Ruta:

```
app_PChop/templates/categoria/
```

---

### 2️⃣2️⃣ Dentro de esa carpeta, crear los siguientes archivos:

* agregar_categoria.html
* ver_categorias.html *(mostrar tabla con botones “ver”, “editar” y “borrar”)*
* actualizar_categoria.html
* borrar_categoria.html

---

### 2️⃣3️⃣ **No utilizar forms.py**

👉 Los formularios se crean directamente en HTML.

---

### 2️⃣4️⃣ Crear `urls.py` dentro de `app_PChop`

Debe contener las rutas necesarias para acceder a las funciones CRUD de `views.py`.

---

### 2️⃣5️⃣ Agregar la app en `settings.py` de `backend_PChop`

📍 En la sección `INSTALLED_APPS`:

```python
'app_PChop',
```

---

### 2️⃣6️⃣ Configurar `urls.py` del proyecto (`backend_PChop/urls.py`)

Enlazarlo con las rutas de `app_PChop`.

---

### 2️⃣7️⃣ Registrar los modelos en `admin.py`

📍 *Ejemplo:*

```python
from django.contrib import admin
from .models import Categoria, Producto, Pedido

admin.site.register(Categoria)
admin.site.register(Producto)
admin.site.register(Pedido)
```

Luego ejecutar nuevamente las migraciones:

```bash
python manage.py makemigrations
python manage.py migrate
```

---

### 2️⃣8️⃣ Diseño y estilo

* Usar **colores suaves, atractivos y modernos.**
* Mantener un diseño **limpio y sencillo.**

---

### 2️⃣9️⃣ Al inicio crear toda la estructura de carpetas y archivos

Antes de comenzar a escribir código.

---

### 3️⃣0️⃣ El proyecto debe ser **totalmente funcional**

CRUD completo de Categoría (Producto y Pedido se dejarán pendientes por ahora).

---

### 3️⃣1️⃣ Finalmente, ejecutar el servidor en el puerto **8017**

📍 *Comando final:*

```bash
python manage.py runserver 8017
```

---

¿Quieres que te prepare también los **códigos base de las vistas (views.py)** y **urls.py** de “Categoría” como en el primer proyecto (ya listos para copiar y pegar)?
Así quedaría completamente funcional el CRUD.


