# 📘 Guía del Estudiante — Sesión 2: CI/CD y Automatización con GitHub Actions

**Curso:** Fundamentos de DevOps potenciado con IA
**Duración:** 90 minutos

---

## 📖 Conceptos Clave — Antes de empezar

| Término | ¿Qué es? |
|---------|----------|
| 🏷️ **Git Tag** | Una *"fotografía inmutable"* del código en un momento exacto. A diferencia de una rama, no avanza. |
| 📦 **GitHub Release** | El entregable empaquetado generado a partir de un tag. Puede incluir assets descargables. |
| ⚙️ **GitHub Actions** | Sistema de CI/CD integrado en GitHub. Ejecuta workflows automáticos ante eventos en el repositorio. |
| 📋 **Workflow YAML** | Archivo de configuración declarativo que define cuándo y cómo correr el pipeline. |
| 🔐 **Secret** | Variable cifrada que GitHub almacena de forma segura. Nunca aparece en logs — se reemplaza por `***`. |
| 🔀 **Pull Request** | Solicitud de fusión de una rama a otra. Puede disparar pipelines automáticos de validación. |

---

## 🏷️ Bloque 1: Versionado Semántico, Tags y Releases `(00:00 – 00:30)`

**🎯 Objetivo:** Aprende a marcar versiones estables y publicar releases en GitHub.

### ¿Qué significa `v1.0.0`?

El **Versionado Semántico** sigue el formato `MAYOR.MENOR.PARCHE`:

- **MAYOR** → cambio que rompe compatibilidad con versiones anteriores.
- **MENOR** → nueva funcionalidad compatible con la versión anterior.
- **PARCHE** → corrección de bugs, sin cambio en funcionalidad.

### Paso 1 — Asegúrate de estar en `main` con todo actualizado

```bash
git checkout main
git pull origin main
```

### Paso 2 — Crea el tag local

```bash
git tag -a v1.0.0 -m "Release v1.0.0: Estructura base y modo oscuro"
```

### Paso 3 — Sube el tag a GitHub

```bash
git push origin v1.0.0
```

> ⚠️ **Importante:** `git push origin main` NO sube los tags automáticamente. Debes hacerlo explícitamente con `git push origin v1.0.0` (o `git push origin --tags` para subir todos).

### Paso 4 — Publica el Release en GitHub

1. Ir al repositorio `retail-app` en GitHub.
2. Hacer clic en **Releases** (panel derecho).
3. Hacer clic en **Draft a new release**.
4. Seleccionar el tag `v1.0.0`.
5. Escribir un título atractivo (ej. *"Lanzamiento Inicial Data-Driven Retail"*).
6. Hacer clic en **Publish release**.

---

## ⚙️ Bloque 2: Nuestro Primer Pipeline con GitHub Actions `(00:30 – 01:10)`

**🎯 Objetivo:** Implementa CI/CD directamente en tu repositorio con un workflow YAML.

> 🧠 **La analogía del Chef:**
>
> - El `.yml` es la **receta** — instrucciones escritas.
> - El `job` es el **plato a preparar** — el objetivo completo.
> - Los `steps` son los **pasos** de la receta — acciones secuenciales.
> - El runner (`ubuntu-latest`) es la **cocina** — entorno aislado y limpio en la nube.

### Paso 1 — Crea la estructura de carpetas

```text
retail-app/
└── .github/
    └── workflows/
        └── ci.yml
```

> ⚠️ `.github` es una carpeta **oculta** (empieza con punto). Debe estar en la raíz del proyecto. El nombre `ci.yml` puede ser cualquiera, pero debe terminar en `.yml` o `.yaml`.

### Paso 2 — Escribe el pipeline en `ci.yml`

```yaml
name: Pipeline de Calidad Retail App

# ¿Cuándo corre el workflow?
on:
  push:
    branches: [ "main" ]

# Conjunto de tareas
jobs:
  build-and-test:
    runs-on: ubuntu-latest  # Runner aislado en la nube

    # Acciones secuenciales
    steps:
      - name: Clonar el repositorio
        uses: actions/checkout@v3

      - name: Simular Análisis de Calidad
        run: |
          echo "Iniciando análisis de SonarQube simulado..."
          echo "Revisando HTML y JS..."
          echo "✅ Código limpio y sin code smells."

      - name: Simular Pruebas Unitarias
        run: |
          echo "Ejecutando tests de la carpeta tests/..."
          echo "✅ logic.test.js - PASSED"
          echo "✅ html.test.js - PASSED"
```

### Paso 3 — Commit y push para activar el pipeline

```bash
git add .
git commit -m "ci: agrega pipeline básico de github actions"
git push origin main
```

### Paso 4 — Verifica la ejecución en GitHub

1. Ir a tu repositorio en GitHub.
2. Hacer clic en la pestaña **Actions**.
3. Ver el pipeline corriendo: ✅ verde (éxito) o ❌ rojo (error).
4. Hacer clic en el job `build-and-test` para ver los logs de cada step.

---

## 🔐 Bloque 3: TAREA CALIFICADA — Secrets y Triggers `(01:10 – 01:30)`

**🎯 Objetivo:** Demostrar dominio de branches, triggers condicionales y manejo seguro de secretos.

> ✅ **Al final de esta tarea habrás probado que:**
>
> - Tu pipeline se activa automáticamente en un Pull Request.
> - GitHub protege datos sensibles ocultándolos con `***` en los logs.
> - Sabes configurar triggers y secrets en GitHub Actions.

---

### Paso 1 — Crea la rama `feature-seguridad`

```bash
git checkout -b feature-seguridad
```

---

### Paso 2 — Configura el secreto en GitHub

1. Ir a tu repositorio en GitHub.
2. Hacer clic en **Settings** (pestaña superior).
3. En el menú izquierdo: **Secrets and variables → Actions**.
4. Hacer clic en **New repository secret**.
5. **Nombre:** `API_KEY_RETAIL`
6. **Valor:** `super-secreto-123` (o cualquier texto de prueba).
7. Hacer clic en **Add secret**.

---

### Paso 3 — Modifica `.github/workflows/ci.yml`

**Cambio A — Agrega `pull_request` al trigger `on:`:**

```yaml
on:
  push:
    branches: [ "main" ]
  pull_request:        # <-- AGREGAR ESTA LÍNEA
    branches: [ "main" ]
```

**Cambio B — Agrega un nuevo step al final del job:**

```yaml
      - name: Probar Secret
        run: |
          echo "Probando acceso al secreto..."
          echo "El secreto es: ${{ secrets.API_KEY_RETAIL }}"
          echo "✅ Si ves *** arriba, el secreto está protegido."
```

---

### Paso 4 — Sube la rama y crea el Pull Request

```bash
git add .
git commit -m "ci: prueba de secretos y trigger de PR"
git push origin feature-seguridad
```

Luego en GitHub:

1. Aparecerá un banner para crear el Pull Request — hacer clic en **Compare & pull request**.
2. Título: `feat: configuración de seguridad y secrets`.
3. Hacer clic en **Create pull request**.

---

### Paso 5 — Toma la evidencia 📸

1. En el Pull Request, bajar a la sección **Checks**.
2. Hacer clic en **Details** del pipeline.
3. Abrir el step **Probar Secret**.
4. Verificar que el log muestra: `El secreto es: ***`
5. **Tomar captura de pantalla** — esa es tu evidencia.

> 📸 **La captura debe mostrar:**
>
> - El nombre del step `Probar Secret` en el log de GitHub Actions.
> - La línea: `El secreto es: ***` (con asteriscos, **NO** el valor real).
> - El estado ✅ del pipeline.

---

## ✅ Checklist de Entrega

Antes de cerrar, verifica que completaste todo:

- [ ] Creé la rama `feature-seguridad` correctamente.
- [ ] Creé el secret `API_KEY_RETAIL` en GitHub Settings → Actions.
- [ ] Modifiqué `ci.yml` para agregar el trigger `pull_request`.
- [ ] Agregué el step que intenta imprimir el secret.
- [ ] Hice commit con mensaje claro y empujé la rama.
- [ ] Creé el Pull Request hacia `main`.
- [ ] El pipeline se ejecutó automáticamente en el PR.
- [ ] Tomé la captura donde se ve `***` en lugar del valor real.
- [ ] Subí la captura al sistema de calificaciones.

---

## 📌 Referencia Rápida de Comandos

| Acción | Comando |
|--------|---------|
| Crear tag anotado | `git tag -a v1.0.0 -m "mensaje"` |
| Subir tag | `git push origin v1.0.0` |
| Ver todos los tags | `git tag` |
| Crear rama nueva | `git checkout -b nombre-rama` |
| Ver rama actual | `git branch` |
| Subir rama nueva | `git push origin nombre-rama` |
| Ver estado de archivos | `git status` |

---

*Curso: Fundamentos de DevOps potenciado con IA  •  Jordan Murillo & Diego Quisi*
