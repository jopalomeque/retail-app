# 🐙 PRÁCTICA 5: Docker Compose — Stack web + API local

**Duración:** ~25 minutos  
**Nivel:** Intermedio  
**Prerequisito:** Práctica Docker + GHCR completada, Docker Desktop corriendo  
**Resultado:** Dos contenedores comunicándose en red interna — nginx sirve el frontend, json-server responde la API REST

---

## 🎯 Objetivo

Definir dos servicios en un `docker-compose.yml`, conectarlos mediante una red interna, y hacer que el frontend consuma datos de la API usando el **nombre del servicio** como hostname — sin exponer el backend al exterior.

---

## El "Por Qué"

En la práctica anterior empaquetaste una app estática en un contenedor nginx. Pero una app real tiene dos piezas: un **frontend** que el usuario ve, y un **backend** que provee los datos. En producción esas dos piezas corren en contenedores separados que se comunican internamente.

Hoy vas a replicar exactamente eso. `nginx` servirá el HTML y actuará como **proxy** hacia `json-server` — un contenedor que levanta una REST API completa a partir de un archivo JSON, sin escribir ningún código de servidor.

```
navegador
    │
    ├── GET localhost:8080          → nginx (sirve index.html)
    │
    └── fetch('/api/productos')     → nginx → proxy → api:80
                                              │
                                         red interna
                                       (api nunca expuesta
                                        al exterior)
```

---

## Paso 1: Estructura del proyecto (2 min)

```bash
mkdir mi-stack-compose && cd mi-stack-compose
```

Tu proyecto tendrá esta estructura al final:

```
mi-stack-compose/
├── docker-compose.yml
├── nginx.conf
├── index.html
└── db.json
```

---

## Paso 2: Los datos — `db.json` (2 min)

Este archivo es toda la "base de datos" de la API. json-server lo leerá y expondrá cada clave como un endpoint REST.

```json
{
  "productos": [
    { "id": 1, "nombre": "Laptop", "precio": 999 },
    { "id": 2, "nombre": "Mouse", "precio": 25 },
    { "id": 3, "nombre": "Teclado", "precio": 45 }
  ],
  "categorias": [
    { "id": 1, "nombre": "Electrónica" },
    { "id": 2, "nombre": "Periféricos" }
  ]
}
```

json-server generará automáticamente estos endpoints:

- `GET /productos` → lista todos
- `GET /productos/1` → uno por ID
- `GET /categorias` → lista todas

---

## Paso 3: El frontend — `index.html` (5 min)

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>🛒 Tienda Docker Compose</title>
  <style>
    body { font-family: sans-serif; max-width: 600px; margin: 40px auto; padding: 0 20px; }
    h1 { color: #2d3748; }
    h2 { color: #4a5568; margin-top: 30px; }
    ul { list-style: none; padding: 0; }
    li { background: #f7fafc; border: 1px solid #e2e8f0; border-radius: 6px;
         padding: 10px 16px; margin: 8px 0; }
    .precio { float: right; font-weight: bold; color: #2b6cb0; }
    .error { color: #e53e3e; }
  </style>
</head>
<body>
  <h1>🛒 Tienda — Stack Docker Compose</h1>
  <p>Frontend: <strong>nginx</strong> · API: <strong>json-server</strong> · Red: interna</p>

  <h2>Productos</h2>
  <ul id="productos"><li>Cargando...</li></ul>

  <h2>Categorías</h2>
  <ul id="categorias"><li>Cargando...</li></ul>

  <script>
    async function cargar(endpoint, elementoId) {
      const lista = document.getElementById(elementoId);
      try {
        const res = await fetch(`/api/${endpoint}`);
        if (!res.ok) throw new Error(`HTTP ${res.status}`);
        const datos = await res.json();
        lista.innerHTML = datos.map(item =>
          `<li>${item.nombre}${item.precio !== undefined
            ? `<span class="precio">$${item.precio}</span>` : ''}</li>`
        ).join('');
      } catch (e) {
        lista.innerHTML = `<li class="error">Error al conectar con la API: ${e.message}</li>`;
      }
    }

    cargar('productos', 'productos');
    cargar('categorias', 'categorias');
  </script>
</body>
</html>
```

---

## Paso 4: El proxy — `nginx.conf` (3 min)

Este archivo le dice a nginx qué hacer con cada ruta:

```nginx
server {
  listen 80;

  # Sirve los archivos estáticos normalmente
  location / {
    root /usr/share/nginx/html;
    index index.html;
  }

  # Redirige /api/ hacia el servicio "api" en la red interna
  location /api/ {
    proxy_pass http://api:80/;
    proxy_set_header Host $host;
  }
}
```

**¿Qué hace `proxy_pass http://api:80/`?**

Cuando el navegador pide `/api/productos`, nginx recibe esa petición y la reenvía internamente a `http://api:80/productos`. El nombre `api` se resuelve automáticamente dentro de la red Docker — no es una IP, es el nombre del servicio definido en el Compose.

---

## Paso 5: El `docker-compose.yml` (5 min)

```yaml
version: "3.9"

services:

  web:
    image: nginx:alpine
    container_name: mi-web
    ports:
      - "8080:80"
    volumes:
      - ./index.html:/usr/share/nginx/html/index.html:ro
      - ./nginx.conf:/etc/nginx/conf.d/default.conf:ro
    networks:
      - mi-red-interna
    depends_on:
      - api

  api:
    image: clue/json-server
    container_name: mi-api
    volumes:
      - ./db.json:/data/db.json:ro
    networks:
      - mi-red-interna
    # ← sin "ports": la API solo es accesible desde dentro de la red

networks:
  mi-red-interna:
    driver: bridge
```

**Lo importante de este archivo:**

| Detalle | Por qué importa |
|---------|----------------|
| `api` no tiene `ports` | No está expuesta al exterior — solo nginx puede hablar con ella |
| `proxy_pass http://api:80/` en nginx.conf | `api` se resuelve por nombre de servicio, no por IP |
| Ambos en `mi-red-interna` | Sin esta red compartida, nginx no encontraría a `api` |
| `depends_on: api` | nginx espera a que `api` esté arriba antes de arrancar |

---

## Paso 6: Levantar y verificar (8 min)

```bash
# Levantar el stack
docker compose up -d

# Verificar que ambos servicios corren
docker compose ps
```

Abre `http://localhost:8080` — verás la lista de productos y categorías cargadas desde la API.

**Verificar la red interna:**

```bash
# Entrar al contenedor web
docker exec -it mi-web sh

# Intentar llegar a la API por nombre (solo funciona desde dentro de la red)
wget -q -O - http://api:80/productos
# Verás el JSON de productos

exit
```

**Verificar que la API NO está expuesta al exterior:**

Abre `http://localhost:80` en el navegador → no hay respuesta. El puerto 80 no existe para el exterior. Solo nginx puede llegar a él.

**Probar que los datos son un volumen vivo:**

1. Agrega un producto al `db.json` en tu máquina
2. Ejecuta `docker compose restart api`
3. Recarga `http://localhost:8080` → el nuevo producto aparece

**Limpiar:**

```bash
docker compose down
```

✅ **Hecho:** dos servicios comunicándose por red interna, API privada, datos como volumen.

---

## Reflexión rápida

Responde en tu cuaderno:

1. ¿Por qué `api` no tiene `ports` definidos? ¿Qué pasaría si lo expusieras directamente en el 80?
2. En el `nginx.conf`, `proxy_pass` usa `http://api:80/` — ¿cómo sabe nginx dónde está `api`?
3. Si cambias `db.json` y haces `docker compose restart api`, los datos cambian sin reconstruir la imagen. ¿Qué tipo de volumen hace eso posible y en qué se diferencia del named volume de la práctica con postgres?

---

## 🆘 Si algo falla

| Problema | Solución |
|----------|----------|
| Página carga pero dice "Error al conectar con la API" | Verifica que `nginx.conf` esté montado correctamente con `docker compose logs web` |
| `docker compose ps` muestra `api` como `Exit` | Revisa que `db.json` tenga JSON válido (sin comas finales ni errores de sintaxis) |
| `port is already allocated` | Cambia `"8080:80"` por `"8081:80"` en el compose |
| `http://localhost:80` sí responde | El servicio `api` tiene `ports` accidentalmente. Verifica el YAML |
| `wget: bad address 'api'` desde dentro del contenedor | Los dos servicios no están en la misma red. Verifica la sección `networks` en ambos servicios |

---

## ⚡ Extra: Self-Hosted Runner

> Esta sección es opcional. Si quieres ir un paso más allá y hacer que un `git push` despliegue este stack automáticamente en tu máquina, continúa aquí.

La idea es instalar el **GitHub Actions Runner** en tu computadora. Cuando hagas push, GitHub le enviará el trabajo a tu máquina y el pipeline ejecutará `docker compose up -d` localmente.

### Por qué es útil

Hasta ahora los workflows de GitHub Actions corrían en servidores de GitHub (`ubuntu-latest`). Esos servidores no tienen acceso a tu Docker local ni a tu red. Un self-hosted runner convierte tu máquina en el ejecutor del pipeline.

```
git push → GitHub Actions → "runs-on: self-hosted" → TU máquina → docker compose up -d
```

El runner *se conecta hacia* GitHub — no necesitas IP pública ni abrir puertos en tu router.

### Instalar el runner

En tu repositorio de GitHub: **Settings → Actions → Runners → New self-hosted runner**

Selecciona tu sistema operativo. GitHub te dará los comandos exactos con un token único. Síguelos paso a paso.

Durante la configuración, cuando te pida un label escribe `local`.

Inicia el runner:

```bash
# Linux / macOS
./run.sh

# Windows
./run.cmd
```

Cuando veas `Listening for Jobs` en la terminal, el runner está activo. En GitHub → Settings → Runners aparecerá como **Idle**.

### Workflow que usa el runner

Crea `.github/workflows/deploy-compose.yml`:

```yaml
name: Deploy Compose en máquina local

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: self-hosted

    steps:
    - name: Checkout
      uses: actions/checkout@v4

    - name: Bajar stack anterior
      run: docker compose down --remove-orphans

    - name: Levantar stack
      run: docker compose up -d

    - name: Estado del stack
      run: docker compose ps
```

Haz push y observa simultáneamente la terminal del runner, la pestaña Actions en GitHub, y `http://localhost:8080`.
