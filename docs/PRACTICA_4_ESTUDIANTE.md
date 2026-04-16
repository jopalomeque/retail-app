# 🐳 PRÁCTICA: De contenedor local a imagen en GitHub Container Registry

**Duración:** ~70 minutos  
**Nivel:** Intermedio  
**Prerequisito:** Git instalado, cuenta en GitHub, Docker Desktop corriendo  
**Resultado:** Imagen Docker publicada en `ghcr.io/TU_USUARIO/mi-app-docker` con pipeline CI/CD automatizado

---

## 🎯 Objetivo

Al finalizar esta práctica serás capaz de **construir** una imagen Docker personalizada con un Dockerfile, **ejecutarla** localmente, y **publicarla automáticamente** en GitHub Container Registry (GHCR) mediante GitHub Actions cada vez que hagas `git push`.

---

## El "Por Qué"

Imagina que eres desarrollador en una startup. Tu aplicación funciona en tu computadora, pero al pasarla al servidor de producción… ¡falla! El clásico problema: *"en mi máquina sí funciona"*.

Docker resuelve esto empaquetando tu app junto a todas sus dependencias en un **contenedor**. Y GitHub Container Registry (GHCR) te permite almacenar esas imágenes directamente junto a tu código, sin necesidad de una cuenta externa.

---

## FASE 1 — Activación (15 minutos)

### 1.1 Responde en tu cuaderno

Antes de escribir código, reflexiona:

1. ¿Cuál es la diferencia entre una **Máquina Virtual** y un **contenedor Docker**? Piensa en velocidad de arranque y uso de recursos.
2. ¿Por qué se dice que los contenedores son "ligeros"?
3. Relaciona cada comando con su función:

| Comando | ¿Qué hace? |
|---------|-----------|
| `docker run` | ? |
| `docker ps` | ? |
| `docker stop` | ? |
| `docker rm` | ? |

### 1.2 Verificar tu entorno

Abre la terminal y ejecuta:

```bash
docker --version
docker run hello-world
```

✅ Si ves un mensaje de bienvenida de Docker, tu entorno está listo.  
❌ Si falla, verifica que Docker Desktop esté abierto y corriendo.

---

## FASE 2 — Aplicación Guiada (40 minutos)

### Paso 1: Crear la aplicación (3 min)

```bash
mkdir mi-app-docker && cd mi-app-docker
```

Crea el archivo `index.html`:

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Mi App Docker</title>
</head>
<body>
  <h1>Hola desde un contenedor Docker 🐳</h1>
  <p>Esta app corre en un contenedor nginx publicado en GHCR.</p>
</body>
</html>
```

---

### Paso 2: Escribir el Dockerfile (5 min)

Crea un archivo llamado `Dockerfile` (sin extensión) en la misma carpeta:

```dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80
```

**¿Qué hace cada línea?**

- `FROM nginx:alpine` → Usa una imagen base ligera de Nginx (Alpine Linux, ~5 MB)
- `COPY` → Copia tu archivo HTML dentro del contenedor
- `EXPOSE 80` → Documenta que el contenedor escucha en el puerto HTTP

---

### Paso 3: Construir y probar localmente (10 min)

```bash
# Construir la imagen
docker build -t mi-app-docker:v1.0 .

# Verificar que la imagen existe
docker images

# Ejecutar el contenedor
docker run -d -p 8080:80 --name mi-sitio mi-app-docker:v1.0

# Verificar que está corriendo
docker ps
```

Abre `http://localhost:8080` en tu navegador. Deberías ver tu página HTML.

```bash
# Cuando termines, limpia:
docker stop mi-sitio
docker rm mi-sitio
```

✅ **Checkpoint:** Imagen construida y contenedor corriendo localmente.

---

### Paso 4: Subir el código a GitHub (5 min)

Crea un repositorio nuevo en GitHub llamado `mi-app-docker` (sin README, sin .gitignore).

```bash
git init
git add .
git commit -m "feat: agregar Dockerfile y app web"
git branch -M main
git remote add origin https://github.com/TU_USUARIO/mi-app-docker.git
git push -u origin main
```

---

### Paso 5: Automatizar con GitHub Actions + GHCR (17 min)

#### 5.1 Crear la estructura del workflow

```bash
mkdir -p .github/workflows
```

#### 5.2 Crear `.github/workflows/docker-ghcr.yml`

Copia exactamente el siguiente contenido (respeta la indentación):

```yaml
name: Build y Publish imagen Docker en GHCR

on:
  push:
    tags:
      - 'v*.*.*'

env:
  REGISTRY: ghcr.io
  IMAGE_NAME: ${{ github.repository }}

jobs:
  build-and-push:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write

    steps:
    - name: Checkout del repositorio
      uses: actions/checkout@v4

    - name: Login a GitHub Container Registry
      uses: docker/login-action@v3
      with:
        registry: ${{ env.REGISTRY }}
        username: ${{ github.actor }}
        password: ${{ secrets.GITHUB_TOKEN }}

    - name: Extraer metadata de la imagen
      id: meta
      uses: docker/metadata-action@v5
      with:
        images: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}
        tags: |
          type=semver,pattern={{version}}
          type=semver,pattern={{major}}.{{minor}}
          type=raw,value=latest

    - name: Build y Push imagen
      uses: docker/build-push-action@v5
      with:
        context: .
        push: true
        tags: ${{ steps.meta.outputs.tags }}
        labels: ${{ steps.meta.outputs.labels }}
```

**¿Qué hace este workflow?**

- `on: push: tags: 'v*.*.*'` → Solo se activa cuando creas un tag con formato `v1.0.0`, `v2.3.1`, etc. Un simple `git push` no publica nada.
- `permissions: packages: write` → Le da permiso a GitHub Actions para subir imágenes a GHCR
- `GITHUB_TOKEN` → Token automático de GitHub, **no necesitas crear secrets manualmente**
- `type=semver` → La imagen se etiqueta automáticamente con el número de versión del tag: si creas `v1.2.0`, la imagen tendrá las etiquetas `1.2.0`, `1.2` y `latest`
- `build-push-action` → Construye la imagen y la publica en `ghcr.io/TU_USUARIO/mi-app-docker`

> 💡 **¿Por qué tags y no push a main?** Esta es la práctica profesional para release de software. Puedes hacer muchos commits y pushes durante el desarrollo sin publicar una imagen nueva. Solo cuando decides que algo está listo para "lanzarse" creas un tag, y en ese momento se construye y publica la imagen con su número de versión. Tus 6 tags del proyecto anterior son exactamente este concepto.

#### 5.3 Commitear el workflow y crear el primer tag

Primero sube el workflow al repositorio:

```bash
git add .github/workflows/docker-ghcr.yml
git commit -m "ci: agregar workflow para publicar en GHCR con tags"
git push origin main
```

Ahora crea y sube tu primer tag para disparar el pipeline:

```bash
# Crear el tag localmente (mensaje descriptivo de la versión)
git tag -a v1.0.0 -m "release: primera imagen Docker publicada en GHCR"

# Subir el tag a GitHub (esto dispara el workflow)
git push origin v1.0.0
```

Ve a tu repositorio en GitHub → pestaña **Actions**. Verás el workflow ejecutándose con el nombre del tag.

Espera a que aparezca ✅ verde.

> ⚠️ **Importante:** `git push origin main` ya **no** dispara el workflow. Solo lo hace `git push origin v*.*.*`. Si haces push sin tag, el pipeline no se ejecuta — eso es exactamente lo que queremos.

✅ **Checkpoint:** Pipeline ejecutado al crear el tag `v1.0.0`.

---

### Paso 6: Ver tu imagen publicada (5 min)

Cuando el workflow termine:

1. Ve a tu perfil de GitHub
2. Haz clic en **Packages** (en la barra lateral o en `github.com/TU_USUARIO?tab=packages`)
3. Verás `mi-app-docker` listado como paquete

Verás que la imagen tiene **tres etiquetas automáticas** generadas a partir del tag `v1.0.0`:

- `1.0.0` → versión exacta
- `1.0` → versión mayor.menor
- `latest` → siempre apunta a la más reciente

Cualquier persona con Docker puede ahora ejecutar tu imagen:

```bash
# Por versión exacta (recomendado en producción)
docker pull ghcr.io/TU_USUARIO/mi-app-docker:1.0.0

# Por la más reciente
docker pull ghcr.io/TU_USUARIO/mi-app-docker:latest

docker run -p 8080:80 ghcr.io/TU_USUARIO/mi-app-docker:1.0.0
```

---

## FASE 3 — Transferencia (15 minutos)

Ahora que dominas el flujo completo, elige **una** extensión y documenta tu solución en un archivo `REFLEXION.md`:

**Opción A — Múltiples tags en un solo pipeline:**  
Crea una segunda versión de tu app (cambia el contenido de `index.html`), haz commit, y publica como `v1.1.0`. Verifica en Packages que ahora tienes ambas versiones disponibles (`1.0.0` y `1.1.0`) y que `latest` apunta a la nueva. ¿Cuándo sería peligroso usar siempre `latest` en producción?

**Opción B — Multi-stage build:**  
Modifica el Dockerfile para usar una etapa de construcción separada y reducir el tamaño de la imagen final. Publica como `v1.0.1` y mide con `docker images` cuántos MB ahorraste respecto a `v1.0.0`.

**Opción C — Hacer la imagen pública:**  
Por defecto, los paquetes en GHCR son privados. Investiga cómo hacer pública la imagen desde la interfaz de GitHub Packages y explica en qué casos querrías una imagen privada vs pública.

### Reflexión escrita (incluir en `REFLEXION.md`)

1. ¿Qué ventaja tiene disparar el pipeline con un tag en lugar de con cada push? ¿Cómo cambia el flujo de trabajo del equipo?
2. ¿Cuál es la diferencia entre usar `latest` y una versión específica como `1.0.0` al desplegar en producción?
3. El proyecto anterior ya tenía 6 tags creados. ¿Qué significaría haber tenido este workflow desde el principio?

---

## ✅ Checklist de entrega

- [ ] Repositorio `mi-app-docker` visible en GitHub con código
- [ ] `Dockerfile` en la raíz del repositorio
- [ ] Workflow `.github/workflows/docker-ghcr.yml` presente
- [ ] Tag `v1.0.0` creado y visible en el repositorio (pestaña **Tags**)
- [ ] Pipeline con ✅ verde en la pestaña Actions disparado por el tag
- [ ] Imagen visible en Packages con etiquetas `1.0.0`, `1.0` y `latest`
- [ ] `REFLEXION.md` con la extensión elegida y las 3 preguntas respondidas

---

## 🆘 Si algo falla

| Problema | Solución |
|----------|----------|
| `permission denied` al hacer push | Ve a Settings del repo → Actions → General → Workflow permissions → "Read and write permissions" |
| Workflow falla en el paso de login | Verifica que el archivo YAML tenga exactamente 2 espacios de indentación |
| Imagen no aparece en Packages | Espera 1-2 min y recarga. Si persiste, ve a Actions y lee el log de error |
| `docker: command not found` | Abre Docker Desktop y espera a que el daemon esté corriendo |
| El workflow no se ejecuta al hacer push | Correcto — solo se activa con tags. Usa `git push origin v1.0.0` |
| El tag ya existe y no puedo volver a crearlo | Borra el tag con `git tag -d v1.0.0` y `git push origin --delete v1.0.0`, luego vuelve a crearlo |

---

## 🔗 URLs importantes

```
Repositorio:  https://github.com/TU_USUARIO/mi-app-docker
Packages:     https://github.com/TU_USUARIO?tab=packages
Actions:      https://github.com/TU_USUARIO/mi-app-docker/actions
Imagen GHCR:  ghcr.io/TU_USUARIO/mi-app-docker:latest
```

---

**¡Felicidades! 🚀 Acabas de crear un pipeline DevOps profesional: código → imagen → registry → automatización. Esto es exactamente lo que hacen los equipos de ingeniería modernos.**

---

## 🔗 Integración con tu proyecto anterior

Esta práctica no vive sola. El objetivo es que **todo lo que has construido desde las prácticas anteriores conviva en un solo repositorio con un pipeline completo**: tu sitio web con deploy automático a GitHub Pages **y** su imagen Docker publicada en GHCR.

### El repositorio que ya tienes

Desde las prácticas anteriores tienes un repositorio con esta estructura:

```
tu-repo/
├── .github/
│   └── workflows/         ← ya tiene el workflow de deploy a GitHub Pages
├── docs/
├── src/                   ← aquí viven tu index.html, styles.css y script.js
├── tests/
├── .env.example
├── .gitignore
└── README.md
```

También tienes **ramas** y **tags** creados, y el historial de commits que acumulaste durante el curso. No debes tocar nada de eso — solo vas a agregar dos archivos nuevos.

### ¿Qué tienes hasta ahora?

| Práctica | Qué construiste | Dónde vive |
|----------|----------------|------------|
| Práctica 3.1 | Repositorio Git con ramas y estructura de carpetas | GitHub |
| Práctica 3.2 | Sitio web con deploy automático a GitHub Pages | `tu-usuario.github.io/tu-repo` |
| Esta práctica | Imagen Docker con pipeline CI/CD | `ghcr.io/TU_USUARIO/tu-repo:latest` |

### Qué debes agregar

#### 1. Dockerfile en la raíz del proyecto

```dockerfile
FROM nginx:alpine
COPY src/ /usr/share/nginx/html/
EXPOSE 80
```

Usamos `src/` porque ahí ya están tu `index.html`, `styles.css` y `script.js` desde la Práctica 3.2. Nginx los servirá exactamente igual que GitHub Pages, pero desde un contenedor.

#### 2. Nuevo workflow dentro de `.github/workflows/`

La carpeta `.github/workflows/` ya existe en tu proyecto. Agrega dentro un nuevo archivo `docker-ghcr.yml` con el workflow que construiste en esta práctica. **No modifiques el `deploy.yml` que ya tienes** — los dos workflows conviven sin conflicto.

```
.github/
  workflows/
    deploy.yml          ← ya existía, no lo toques (GitHub Pages)
    docker-ghcr.yml     ← nuevo (GHCR)
```

#### 3. Commitear respetando el flujo de ramas y publicar con un tag

Tienes 5 ramas activas y ya 6 tags creados. Respeta el flujo que ya practicaste, y ahora le agregas el paso de crear un tag para disparar la publicación de la imagen:

```bash
# Posiciónate en develop
git checkout develop

# Agrega los dos archivos nuevos
git add Dockerfile .github/workflows/docker-ghcr.yml
git commit -m "feat: agregar Dockerfile y workflow de GHCR"
git push origin develop

# Mergea a main (esto NO dispara el pipeline de Docker todavía)
git checkout main
git merge develop
git push origin main

# Crea el tag para publicar la imagen Docker (esto SÍ dispara el pipeline)
git tag -a v1.0.0 -m "release: primera imagen Docker del sitio web"
git push origin v1.0.0
```

> 💡 Nota que ya tienes 6 tags en tu proyecto. El siguiente debería ser `v1.0.0` o el que siga en tu secuencia. Revisa tus tags existentes con `git tag --list` antes de crear uno nuevo.

#### 4. Verificar que ambos pipelines corren

Ve a la pestaña **Actions** de tu repositorio. Al hacer `git push origin v1.0.0` deberías ver:

- ✅ `Build y Publish imagen Docker en GHCR` → publica `ghcr.io/TU_USUARIO/tu-repo:1.0.0`

El workflow de GitHub Pages (`deploy.yml`) se sigue disparando como siempre con cada push a `main`. Los dos workflows tienen triggers distintos y son completamente independientes.

### Resultado final del proyecto

Al completar la integración, tu repositorio queda así:

```
tu-repo/
├── .github/
│   └── workflows/
│       ├── deploy.yml          ← CI/CD → GitHub Pages  (ya existía)
│       └── docker-ghcr.yml     ← CI/CD → GHCR          (nuevo)
├── docs/
├── src/
│   ├── index.html
│   ├── styles.css
│   └── script.js
├── tests/
├── Dockerfile                  ← nuevo
├── .env.example
├── .gitignore
└── README.md
```

Cada flujo tiene su propio trigger:

- `git push origin main` → actualiza el sitio en GitHub Pages (automático)
- `git push origin vX.X.X` → construye y publica la imagen Docker en GHCR (controlado)

**Eso es un pipeline de entrega continua real, construido paso a paso durante todo el curso. El deploy del sitio es continuo; el release de la imagen es deliberado.**
