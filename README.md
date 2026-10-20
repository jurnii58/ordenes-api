# Tema 1: Programación del Lado del Servidor (ordenes-api)

Repositorio con el desarrollo de las prácticas de desarrollo backend en Python usando **FastAPI**, **Pydantic**, **Pytest** y **Ruff**.

---

## Estructura del Proyecto

```text
├── practica1/                     # Práctica 1: Primer servidor con FastAPI
│   ├── app/
│   │   ├── __init__.py
│   │   └── main.py
│   ├── pyproject.toml
│   └── requirements.txt
├── practica2/                     # Práctica 2: CRUD de Órdenes
│   ├── app/
│   │   ├── routers/
│   │   │   ├── __init__.py
│   │   │   └── orders.py
│   │   ├── __init__.py
│   │   ├── main.py
│   │   ├── repository.py
│   │   └── schemas.py
│   ├── tests/
│   │   ├── __init__.py
│   │   └── test_orders.py
│   ├── pyproject.toml
│   └── requirements.txt
├── practica3-inicial-alumnos/     # Práctica 3: Catálogo de Productos
│   ├── app/
│   │   ├── routers/
│   │   │   ├── __init__.py
│   │   │   └── productos.py
│   │   ├── __init__.py
│   │   ├── main.py
│   │   ├── repository.py
│   │   └── schemas.py
│   ├── tests/
│   │   ├── __init__.py
│   │   └── test_productos.py
│   ├── pyproject.toml
│   └── requirements.txt
└── evidencias/                    # Capturas de pantalla de ejecución y pruebas
```

---

## Instalación y Configuración

1. **Crear y activar el entorno virtual:**
   ```powershell
   python -m venv .venv
   .\.venv\Scripts\Activate.ps1
   ```

2. **Instalar dependencias:**
   ```powershell
   pip install "fastapi[standard]" pytest ruff
   ```

---

## Ejecución de Prácticas y Pruebas

### Práctica 1: Primer servidor web
```powershell
cd practica1
fastapi dev app/main.py
```
- Health check: `http://127.0.0.1:8000/health`
- Documentación Swagger: `http://127.0.0.1:8000/docs`

### Práctica 2: CRUD de Órdenes
```powershell
cd practica2
# Servidor:
fastapi dev app/main.py

# Pruebas automatizadas (4 passed):
pytest -v
```

### Práctica 3: Catálogo de Productos
```powershell
cd practica3-inicial-alumnos
# Servidor:
fastapi dev app/main.py

# Pruebas automatizadas (16 passed):
pytest -v
```

---

## Evidencias y Respuestas Teóricas

### Tabla del Paso 4 (Práctica 1)

| Petición | Código de estado | Mensaje (`detail`) | ¿Quién generó el error? |
| :--- | :--- | :--- | :--- |
| `GET /cuadrado/abc` | **422** | `Input should be a valid integer...` | **Pydantic / FastAPI** (error de tipo en parámetro de ruta) |
| `GET /no-existe` | **404** | `Not Found` | **Starlette / FastAPI** (ruta no existente) |
| `POST /health` | **405** | `Method Not Allowed` | **Starlette / FastAPI** (método no admitido para este endpoint) |
| `GET /saludo/Ana?mayusculas=quizas` | **422** | `Input should be a valid boolean...` | **Pydantic / FastAPI** (error en query parameter booleano) |

### Respuestas a Preguntas (Práctica 2)

1. **¿Por qué `OrderIn` no tiene los campos `id`, `total` ni `status`?**  
   Son campos generados, calculados y gestionados por el servidor para garantizar la integridad y evitar manipulación por parte del cliente.
2. **¿Qué pasaría en las pruebas si no existiera `app.dependency_overrides`?**  
   Las pruebas compartirían el mismo estado en memoria, provocando que los datos creados en un test interfieran con los siguientes.
3. **¿Qué líneas aparecen al arrancar y detener el servidor? ¿Quién las produce?**  
   `"API de Órdenes iniciada"` y `"API de Órdenes detenida"`. Son producidas por la función `@asynccontextmanager lifespan(app: FastAPI)` en `app/main.py`.
