# Prácticas de Python y Flask

Una colección de ejercicios para recorrer Python desde sus fundamentos hasta el desarrollo web con Flask. No es una única aplicación: cada carpeta reúne ejemplos independientes, y varias contienen programas que se ejecutan directamente desde la consola.

## Contenidos

| Carpeta | Qué se practica |
| --- | --- |
| [`01.-Introduccion`](01.-Introduccion) | Variables, tipos, entrada de datos y operadores lógicos y relacionales. |
| [`02.-Listas`](02.-Listas), [`03.-Tuplas`](03.-Tuplas), [`04.-Strings`](04.-Strings), [`05.-Diccionarios`](05.-Diccionarios) | Colecciones, matrices, empaquetado y desempaquetado, búsqueda y formato de texto. |
| [`06.-Condiciones`](06.-Condiciones) | Condicionales, valores booleanos, ciclos `for` y `while`, `range` y `break`. |
| [`07.-Funciones`](07.-Funciones) | Parámetros y argumentos, ámbitos, funciones anidadas, callbacks, closures, decoradores y lambdas. |
| [`08.-Clases`](08.-Clases) | Programación orientada a objetos: clases, métodos y herencia. |
| [`09.-Concurrencia`](09.-Concurrencia) | Hilos y procesos, pools, futuros, colas, pipes, sincronización y condiciones de carrera. |
| [`10.-Test unitarios`](10.-Test%20unitarios) | Pruebas con `unittest` y `pytest`: fixtures, parametrización, excepciones y marcadores. |
| [`11.-Bases de datos`](11.-Bases%20de%20datos) | Acceso a MySQL con consultas SQL y Peewee, y operaciones sobre documentos con PyMongo. |
| [`12.-Ejercicios Bootcamp`](12.-Ejercicios%20Bootcamp) | Desafíos de cadenas, números y listas; incluye [enunciados](12.-Ejercicios%20Bootcamp/README.md). |
| [`13.-Flask`](13.-Flask) | Un servidor WSGI, aplicaciones Flask sencillas y una aplicación web de tareas. |
| [`14.-Ejercicios codo a codo`](14.-Ejercicios%20codo%20a%20codo) | Ejercicios integradores de algoritmos, excepciones y clases; incluye un PDF con enunciados. |

Entre los desafíos hay ejemplos de FizzBuzz, Pig Latin, adivinanza de números, eliminación de duplicados, factorial, máximo común divisor y modelos de personas y cuentas.

## Stack y herramientas

- **Python 3** y su biblioteca estándar: entre otros módulos, `threading`, `multiprocessing`, `concurrent.futures`, `unittest` y `wsgiref`.
- **Flask y Jinja2** para rutas HTTP, formularios, sesiones y plantillas; HTML, CSS, JavaScript y Bootstrap para la interfaz.
- **Flask-Login, Flask-WTF, Flask-SQLAlchemy, Flask-Migrate y Flask-Mail** en la aplicación de tareas.
- **MySQL**, **Peewee** y **PyMongo** para explorar bases relacionales y de documentos.
- **pytest** para ejemplos de pruebas adicionales a `unittest`.

Las dependencias no son globales para todo el repositorio. Los dos archivos `requirements.txt` de Flask fijan versiones diferentes: conviene usar un entorno virtual por ejemplo. En particular, la aplicación de tareas usa dependencias antiguas, que pueden requerir una versión compatible de Python y bibliotecas de desarrollo de MySQL; no hay una versión de Python garantizada por el repositorio.

## Primeros pasos

Cloná el repositorio y ejecutá un ejercicio desde su raíz:

```bash
git clone https://github.com/FabianNorbertoEscobar/python-flask-ejercicios.git
cd python-flask-ejercicios
python3 "02.-Listas/listas.py"
```

Los ejemplos básicos suelen usar solo la biblioteca estándar. Algunos piden datos por consola, esperan durante su ejecución o tienen dependencias y servicios externos; revisá el archivo antes de ejecutarlo. Las rutas con espacios van entre comillas.

Para instalar dependencias aisladas, creá y activá un entorno virtual:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r "13.-Flask/server/requirements.txt"
```

En Windows, activalo con `.venv\Scripts\activate`. Para salir del entorno, ejecutá `deactivate`.

## Ejemplos web

### WSGI y Flask básico

Con las dependencias de `13.-Flask/server/requirements.txt` instaladas, ejecutá desde la carpeta del servidor:

```bash
cd "13.-Flask/server"
python main.py
```

El servidor WSGI escucha en <http://localhost:4000> y renderiza plantillas Jinja2 para `/` y `/users`. La carpeta de trabajo importa porque las plantillas se buscan en `templates`.

En la misma carpeta, `python flask_server/main.py` inicia un saludo con Flask en <http://localhost:5001/>.

Otro ejemplo, [`13.-Flask/flask`](13.-Flask/flask), practica formularios, mensajes flash, sesiones y redirecciones. Puede ejecutarse con las dependencias del ejemplo de servidor:

```bash
cd "13.-Flask/flask"
export flask_key="clave-local-de-ejemplo"
python main.py
```

Abrí <http://localhost:5000/login>. Los usuarios de ejemplo son `eduardo` y `cf`, ambos con contraseña `123`; viven en memoria y no representan un sistema de autenticación para producción.

### Aplicación de tareas

[`13.-Flask/web`](13.-Flask/web) reúne los conceptos en una aplicación con:

- Registro, inicio y cierre de sesión; contraseñas almacenadas como hashes y validación de formularios.
- Creación, listado paginado, visualización, edición y eliminación de tareas asociadas a usuarios. La edición y la eliminación comprueban que la tarea pertenezca al usuario autenticado.
- Plantillas Jinja2, mensajes flash, página 404 y protección CSRF.
- Modelos y relaciones con SQLAlchemy, configuración de migraciones con Flask-Migrate y envío de un correo de bienvenida mediante un hilo.

Para ejecutarla, usá **otro entorno virtual**, instalá sus dependencias y configurá una instancia local de MySQL:

```bash
cd "13.-Flask/web"
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
```

Creá previamente la base `project_web_facilito` en MySQL y ajustá la URI `SQLALCHEMY_DATABASE_URI` en `config.py` a tus credenciales locales. El valor incluido usa `root`, contraseña vacía y `localhost`. La aplicación ejecuta `db.create_all()` al iniciarse: crea las tablas que faltan, pero no actualiza esquemas existentes. Hay un directorio de migraciones; no es necesario volver a ejecutar `db init` sobre él.

El registro intenta enviar un correo de bienvenida por SMTP de Google. Para probarlo, configurá credenciales válidas para ese servicio:

```bash
export MAIL_USERNAME="tu-correo"
export MAIL_PASSWORD="tu-contraseña-de-aplicación"
python manage.py runserver
```

Abrí <http://localhost:5000/>. Sin una configuración SMTP válida, el envío en segundo plano puede fallar. No guardes credenciales reales en el código ni en el repositorio.

## Pruebas

Las pruebas de productos y carrito usan `unittest`, incluido en Python:

```bash
cd "10.-Test unitarios/unittest"
python -m unittest discover -s entities/test
```

Los ejemplos de `pytest` necesitan instalarlo en el entorno activo:

```bash
python -m pip install pytest
cd "10.-Test unitarios/pytest"
python -m pytest -c app/test/pytest.ini
```

La aplicación de tareas también incluye pruebas ejecutables desde `13.-Flask/web` con `python manage.py test`. Requieren las dependencias de esa aplicación y las bases MySQL de desarrollo y de prueba configuradas en `config.py`. **Usá únicamente bases descartables**: el inicio crea tablas y el `tearDown` de las pruebas elimina las tablas de la base de prueba.

## Bases de datos y precauciones

Los ejemplos de [`11.-Bases de datos`](11.-Bases%20de%20datos) requieren instalar por separado el controlador correspondiente (`mysqlclient` para `MySQLdb`, Peewee con un controlador MySQL, o PyMongo) y disponer del servicio local configurado en cada archivo. No tienen un archivo de dependencias propio y usan API de distintas épocas.

**Revisá los scripts antes de ejecutarlos:** los ejemplos SQL y Peewee eliminan y recrean tablas, y el de MongoDB inserta, modifica y elimina documentos. Utilizá bases aisladas y datos descartables.

Este es un repositorio educativo: contiene credenciales de ejemplo, configuración de desarrollo, versiones antiguas y ejercicios que pueden estar incompletos o contener errores. Las aplicaciones y sus configuraciones no están preparadas para exposición pública ni uso en producción.
