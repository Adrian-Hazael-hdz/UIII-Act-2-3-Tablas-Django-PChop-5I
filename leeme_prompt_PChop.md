Aquí tienes el **listado completo paso a paso** SIN saltar nada y siguiendo EXACTAMENTE tu orden:

---

## ✅ **Primera Parte — Proyecto: PChop (Django + VS Code)**

**Lenguaje:** Python
**Framework:** Django
**Editor:** Visual Studio Code

---

### **1. Procedimiento para crear carpeta del Proyecto `UIII_PChop_1128`**

* Ir a la carpeta donde guardarás tu proyecto
* Clic derecho → **Nuevo → Carpeta**
* Nombre: **UIII_PChop_1128**

---

### **2. Procedimiento para abrir VS Code sobre la carpeta `UIII_PChop_1128`**

* Abrir Visual Studio Code
* Menú **File / Archivo**
* Seleccionar **Open Folder / Abrir carpeta**
* Elegir: **UIII_PChop_1128**

---

### **3. Procedimiento para abrir terminal en VS Code**

* Ir al menú **Terminal**
* Seleccionar **New Terminal / Nueva Terminal**

---

### **4. Procedimiento para crear entorno virtual `.venv` desde terminal**

En la terminal:

```bash
python -m venv .venv
```

---

### **5. Procedimiento para activar el entorno virtual**

```bash
.\.venv\Scripts\activate
```

> Si al activarse aparece `(.venv)` al inicio de la línea, está correcto ✅

---

### **6. Procedimiento para activar intérprete de Python**

* Presionar **Ctrl + Shift + P**
* Escribir: **Python: Select Interpreter**
* Seleccionar el que diga `.venv`

---

### **7. Procedimiento para instalar Django**

```bash
pip install django
```

---

### **8. Procedimiento para crear proyecto `backend_PChop` sin duplicar carpeta**

Desde la terminal (dentro de `UIII_PChop_1128`):

```bash
django-admin startproject backend_PChop .
```

> El punto `.` evita crear carpeta doble ✅

---

### **9. Procedimiento para ejecutar servidor en puerto 8017**

```bash
python manage.py runserver 8017
```

---

### **10. Procedimiento para copiar y pegar el link en el navegador**

* Copiar el enlace que muestra la terminal:
  **[http://127.0.0.1:8017/](http://127.0.0.1:8017/)**
* Pegarlo en tu navegador

---

### **11. Procedimiento para crear aplicación `app_PChop`**

```bash
python manage.py startapp app_PChop
```

---

### **12. Modelo `models.py`**

Pegar **exactamente este código** dentro de `app_PChop/models.py`:

```python
from django.db import models

#MODELO: CATEGORIA

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

#MODELO: PRODUCTO
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

#MODELO: PEDIDO
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

### **12.5 Procedimiento para migraciones**

```bash
python manage.py makemigrations
python manage.py migrate
```

---

### **13. Primero trabajamos con el MODELO: CATEGORÍA**

> Los modelos Producto y Pedido se usarán después ✅

---

### **14. En `views.py` de `app_PChop` crear funciones:**

* `inicio_PChop`
* `agregar_categoria`
* `actualizar_categoria`
* `realizar_actualizacion_categoria`
* `borrar_categoria`

*(El código lo harás después, por ahora solo la indicación)*

---

### **15. Crear carpeta `templates` dentro de `app_PChop`**

En VS Code → click derecho → New Folder
Nombre: **templates**

---

### **16. Crear archivos HTML dentro de `templates`:**

* base.html
* header.html
* navbar.html
* footer.html
* inicio.html

---

### **17. En `base.html` agregar Bootstrap CSS y JS**

(CDN de Bootstrap)

---

### **18. `navbar.html` con menú:**

* Sistema de Administración PChop (con icono)
* Inicio
* Categoría (submenu)

  * Agregar
  * Ver
  * Actualizar
  * Borrar
* Producto (submenu: agregar, ver, actualizar, borrar)
* Pedido (submenu: agregar, ver, actualizar, borrar)

> **Iconos solo en menú principal, NO en submenú**

---

### **19. `footer.html`:**

* Derechos de autor
* Fecha del sistema
* Texto: **"Creado por Adrian Hazael 5I, Cbtis 128"**
* Footer fijo abajo

---

### **20. `inicio.html`:**

* Información del sistema
* Imagen de tienda de computadoras desde internet

---

### **21. Crear carpeta `categoria` dentro de `app_PChop/templates/`**

---

### **22. Crear páginas dentro de `templates/categoria/`:**

* `agregar_categoria.html`
* `ver_categorias.html` (tabla + botones Ver, Editar, Borrar)
* `actualizar_categoria.html`
* `borrar_categoria.html`

---

### **23. No utilizar `forms.py`**

*(Todo con HTML y métodos en views)*

---

### **24. Crear `urls.py` en `app_PChop`**

> Con rutas para el CRUD de categoría

---

### **25. Agregar `app_PChop` en `settings.py`**

En `INSTALLED_APPS`

---

### **26. Configurar `urls.py` en `backend_PChop`**

Importar paths y enlazar a `app_PChop.urls`

---

### **27. Registrar modelos en `admin.py` y migrar**

```bash
python manage.py makemigrations
python manage.py migrate
```

> Solo se usará por ahora **Categoría**

---

### **28. Diseños**

* Colores suaves y modernos
* Código simple
* ❌ Sin validación de datos

---

### **29. Crear toda la estructura desde el inicio**

Carpetas + archivos

---

### **30. Proyecto totalmente funcional**

---

### **31. Ejecutar servidor en puerto 8017 nuevamente**

```bash
python manage.py runserver 8017
```

---

Si deseas, ahora puedo darte:

✅ Código completo de views
✅ Código de urls
✅ Plantillas HTML modernas con Bootstrap
✅ Screenshots de ejemplo del flujo CRUD
✅ Base de datos precargada

---

¿Quieres que continúe con la **segunda parte: creación del CRUD Categoría con código real**? 👇🔥
