

---

## Paso 1 — Crear la carpeta del proyecto

Crea la carpeta principal del proyecto: `UIII_PChop_1128`.
Ejemplo: `C:\ruta\a\proyectos\UIII_PChop_1128` o `~/proyectos/UIII_PChop_1128`.

## Paso 2 — Abrir VS Code sobre la carpeta

Abre VS Code en esa carpeta: desde el Explorador → **Abrir carpeta**, o por terminal:
`code C:\ruta\a\UIII_PChop_1128` (Windows) / `code ~/proyectos/UIII_PChop_1128` (Linux/Mac).

## Paso 3 — Abrir la terminal integrada en VS Code

Menú **Terminal → Nueva terminal** o atajo (`Ctrl+ñ` / `Ctrl+` `). La terminal se abrirá en la raíz `UIII_PChop_1128`.

## Paso 4 — Crear la carpeta del entorno virtual `.venv` desde la terminal

Crear el virtualenv dentro del proyecto para evitar rutas globales:
`python -m venv .venv`

## Paso 5 — Activar el entorno virtual

* PowerShell (Windows): `.\.venv\Scripts\Activate.ps1`
* cmd (Windows): `.\.venv\Scripts\activate`
* Bash / macOS / WSL: `source .venv/bin/activate`

## Paso 6 — Activar el intérprete de Python en VS Code

Abrir la Paleta (`Ctrl+Shift+P`) → **Python: Select Interpreter** → elegir `.venv` (la ruta `.venv/...`).

## Paso 7 — Instalar Django

Con el entorno activo:
`pip install django`
(Puedes fijar versión: `pip install "django>=4.2,<5"` si quieres consistencia).

## Paso 8 — Crear proyecto `backend_PChop` sin duplicar carpeta

Para no crear una carpeta extra dentro de la carpeta del proyecto, ejecuta desde la raíz:
`django-admin startproject backend_PChop .`
(la `.` evita crear otra carpeta `backend_PChop` dentro de `UIII_PChop_1128`).

## Paso 9 — Ejecutar servidor en el puerto **8017**

Desde la carpeta donde está `manage.py`:
`python manage.py runserver 8017`
(usa `0.0.0.0:8017` si quieres acceder desde otras máquinas: `python manage.py runserver 0.0.0.0:8017`).

## Paso 10 — Copiar y pegar el link en el navegador

Abrir en el navegador: `http://127.0.0.1:8017/` (o la IP/host indicado por `runserver`).

## Paso 11 — Crear la aplicación `app_PChop`

Desde la raíz del proyecto (donde está `manage.py`):
`python manage.py startapp app_PChop`

## Paso 12 — Añadir `models.py` en `app_PChop` (modelo inicial)

Coloca este `models.py` (ya provisto):

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

> Nota: por ahora trabajaremos solamente con **Categoria**; Producto y Pedido quedan pendientes según lo indicado.

## Paso 12.5 — Realizar migraciones iniciales (`makemigrations` y `migrate`)

1. `python manage.py makemigrations app_PChop`
2. `python manage.py migrate`

## Paso 13 — Empezar trabajando con el MODELO: **CATEGORÍA**

Definir campos, admin y vistas CRUD para `Categoria` primero; dejar Producto y Pedido para después.

## Paso 14 — En `views.py` de `app_PChop` crear funciones (con su responsabilidad)

Crear las funciones y su lógica mínima:

* `inicio_PChop(request)` → mostrar `inicio.html` con resumen del sistema.
* `agregar_categoria(request)` → mostrar formulario HTML (POST crea categoria).
* `actualizar_categoria(request, slug_o_id)` → mostrar formulario con datos.
* `realizar_actualizacion_categoria(request, slug_o_id)` → procesar POST y guardar cambios.
* `borrar_categoria(request, slug_o_id)` → confirmar y eliminar.

(Implementar con `request.method == "POST"` y templates sin usar `forms.py` — construir los `<form>` manualmente.)

## Paso 15 — Crear la carpeta `templates` dentro de `app_PChop`

Estructura: `app_PChop/templates/` y dentro subcarpetas según convenga.

## Paso 16 — Crear los archivos HTML: `base.html`, `header.html`, `navbar.html`, `footer.html`, `inicio.html`

* `base.html` será la plantilla base con bloques `{% block content %}{% endblock %}`.
* Incluir `header`, `navbar` y `footer` con `{% include %}` en `base.html`.

## Paso 17 — En `base.html` agregar Bootstrap para CSS y JS

Usa CDN (ejemplo): `<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.x/dist/css/bootstrap.min.css" rel="stylesheet">` y el script de bootstrap al final del `body`.

## Paso 18 — En `navbar.html` incluir las opciones principales y submenús

Menú principal con iconos (ej: usando Bootstrap icons o FontAwesome):

* “Sistema de Administración PChop” (marca)
* “Inicio”
* “Categoría” → submenu: Agregar, Ver, Actualizar, Borrar
* “Producto” → submenu: Agregar, Ver, Actualizar, Borrar
* “Pedido” → submenu: Agregar, Ver, Actualizar, Borrar
  (Iconos en opciones principales, no en submenú).

## Paso 19 — En `footer.html` incluir derechos y autor y mantenerla fija abajo

Texto: `© <año> Creado por Adrian Hazael 5I, Cbtis 128` y mostrar la fecha del sistema (`{{ now|date:"Y" }}`) o renderizar con `datetime` en template. Fijarla con CSS para que quede al final de la página.

## Paso 20 — `inicio.html`: información del sistema + imagen desde la web

Mostrar descripción de PChop y una imagen representativa tomada de la red (URL en `<img src="...">`).

## Paso 21 — Crear subcarpeta `categoria` dentro de `app_PChop/templates`

Ruta: `app_PChop/templates/categoria/`

## Paso 22 — Crear los templates de categoría: agregar, ver, actualizar, borrar

* `agregar_categoria.html` → formulario para crear.
* `ver_categorias.html` → tabla con columnas y botones **Ver | Editar | Borrar** por fila.
* `actualizar_categoria.html` → formulario prellenado para editar.
* `borrar_categoria.html` → confirmación antes de eliminar.

## Paso 23 — **No utilizar `forms.py`**

Crear formularios HTML manuales (`<form method="post">`) y procesarlos en las vistas (leer `request.POST`).

## Paso 24 — Crear el archivo `urls.py` en `app_PChop` con rutas CRUD para categorías

Ejemplo mínimo en `app_PChop/urls.py`:

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.inicio_PChop, name='inicio_PChop'),
    path('categoria/agregar/', views.agregar_categoria, name='agregar_categoria'),
    path('categoria/ver/', views.ver_categorias, name='ver_categorias'),
    path('categoria/editar/<int:id>/', views.actualizar_categoria, name='actualizar_categoria'),
    path('categoria/actualizar/<int:id>/', views.realizar_actualizacion_categoria, name='realizar_actualizacion_categoria'),
    path('categoria/borrar/<int:id>/', views.borrar_categoria, name='borrar_categoria'),
]
```

(Ajusta `id` o `slug` según tu preferencia.)

## Paso 25 — Agregar `app_PChop` en `INSTALLED_APPS` de `backend_PChop/settings.py`

`'app_PChop',` dentro de `INSTALLED_APPS`.

## Paso 26 — Configurar `urls.py` de `backend_PChop` para enlazar con `app_PChop`

En `backend_PChop/urls.py`:

```python
from django.urls import path, include

urlpatterns = [
    path('', include('app_PChop.urls')),
    # path('admin/', admin.site.urls)  <-- si quieres acceso admin
]
```

## Paso 27 — Registrar los modelos en `admin.py` y volver a migrar

En `app_PChop/admin.py` registrar `Categoria`, `Producto`, `Pedido`. Luego:

1. `python manage.py makemigrations app_PChop`
2. `python manage.py migrate`

> Por ahora trabajar sólo con **Categoria**; dejar `Producto` y `Pedido` para después (como indicaste).

## Paso 28 — Usar colores suaves, atractivos y modernos; diseño sencillo

Define un CSS básico (o usar Bootstrap + clases personalizadas) con paleta suave (ej. grises claros, azules pastel). Mantén el HTML limpio y legible.

## Paso 29 — Al inicio: crear la estructura completa de carpetas y archivos

Crea de antemano la estructura en disco (por ejemplo con `mkdir` o VS Code):

```
UIII_PChop_1128/
├─ backend_PChop/
├─ app_PChop/
│  ├─ templates/
│  │  ├─ categoria/
│  │  ├─ base.html
│  │  ├─ navbar.html
│  │  └─ ...
│  ├─ static/
│  ├─ migrations/
│  ├─ models.py
│  ├─ views.py
│  ├─ urls.py
│  └─ admin.py
└─ .venv/
```

## Paso 30 — Asegurarse que el proyecto quede totalmente funcional (mínimo CRUD para Categoría)

Verificar: crear categorías, listarlas, editar, borrar; templates cargan correctamente; rutas funcionan; admin opcional activado.

## Paso 31 — Finalmente ejecutar el servidor en el puerto **8017**

(Recordatorio final) Ejecuta:
`python manage.py runserver 8017`
Abre `http://127.0.0.1:8017/` y prueba todas las operaciones.

---

