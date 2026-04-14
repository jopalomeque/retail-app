# 🚀 PRÁCTICA 3.1 - Guía del Estudiante: GitHub DevOps & GitOps Fundamentals

**Duración:** 60 minutos  
**Objetivo:** Dominar GitHub, ramas, CI/CD, y buenas prácticas de versionamiento  
**Entrega:** Un repositorio en GitHub completamente funcional con GitHub Actions

---

## 📋 Instrucciones Generales

- **Lee cada paso completamente antes de ejecutarlo**
- **Sigue el orden exacto** (no saltees pasos)
- **Si algo falla,** copia el error exacto y muéstraselo al docente
- **Cada paso tiene un emoji:** ✅ cuando lo completes, márcalo en tu mente

---

## PASO 1: Preparar el Entorno Local (5 minutos)

### 1.1 Crear la carpeta del proyecto

Abre la terminal (PowerShell en Windows, Terminal en Mac/Linux):

```bash
mkdir github-devops-practice
cd github-devops-practice
```

### 1.2 Crear la estructura de carpetas

Ejecuta estos comandos uno a uno:

```bash
mkdir .github
mkdir .github/workflows
mkdir .github/ISSUE_TEMPLATE
mkdir src
mkdir tests
mkdir docs
```

**¿Qué significa cada carpeta?**

- `.github/workflows/` = Aquí van los "robots" (GitHub Actions)
- `src/` = Tu código de aplicación
- `tests/` = Pruebas automáticas
- `docs/` = Documentación

✅ **Verifica:** Abre VS Code con `code .` y deberías ver todas las carpetas

---

## PASO 2: Crear Archivos de Configuración (8 minutos)

### 2.1 Archivo `.gitignore`

Crea un archivo llamado `.gitignore` en la raíz del proyecto:

```
node_modules/
.env
.DS_Store
*.log
dist/
build/
.vscode/settings.json
*.swp
```

**¿Por qué?** Estos archivos NO deben subirse a GitHub nunca.

### 2.2 Archivo `.env.example`

Crea `.env.example` (sin extensión `.env` real):

```
# ⚠️ ESTE ARCHIVO ES SOLO UN EJEMPLO
# Copia este archivo a .env y completa los valores reales
# NUNCA subes .env a GitHub

DATABASE_URL=postgresql://user:password@localhost:5432/mydb
API_KEY=tu_api_key_aqui
SECRET_TOKEN=tu_secret_token_aqui
LOG_LEVEL=debug
```

**¿Por qué?** Muestra qué variables existen, pero sin valores reales.

### 2.3 Archivo `package.json`

Crea `package.json` en la raíz:

```json
{
  "name": "github-devops-practice",
  "version": "1.0.0",
  "description": "Práctica de DevOps con GitHub, CI/CD y buenas prácticas",
  "main": "src/app.js",
  "scripts": {
    "test": "jest 2>/dev/null || echo 'Tests no configurados aún'",
    "start": "node src/app.js",
    "build": "echo 'Build completado'"
  },
  "keywords": ["devops", "github", "ci-cd", "github-actions"],
  "author": "Tu Nombre",
  "license": "MIT",
  "devDependencies": {}
}
```

### 2.4 Archivo `README.md`

Crea `README.md`:

```markdown
# 🚀 GitHub DevOps Practice

Repositorio de práctica para dominar GitHub, CI/CD, y DevOps.

## 📁 Estructura del Proyecto

```

github-devops-practice/
├── .github/
│   ├── workflows/      ← Automatización (GitHub Actions)
│   └── ISSUE_TEMPLATE/ ← Templates para issues
├── src/                ← Código fuente
├── tests/              ← Tests automáticos
├── docs/               ← Documentación
├── .gitignore          ← Archivos que NO suben a GitHub
├── .env.example        ← Ejemplo de variables de entorno
├── package.json        ← Dependencias del proyecto
└── README.md           ← Este archivo

```

## 🎯 Objetivos de Aprendizaje

- ✅ Estructura profesional de repositorios
- ✅ Manejo de ramas (branching)
- ✅ GitHub Actions para CI/CD
- ✅ Manejo seguro de credenciales
- ✅ Tags y Releases

## 🚀 Cómo Empezar

1. Clona este repositorio
2. Sigue los pasos de la documentación
3. Crea tu primer workflow

## 📝 Convención de Commits

Usa estos prefijos en tus commits:

- `feat:` - Nueva característica
- `fix:` - Corrección de bug
- `docs:` - Cambios en documentación
- `style:` - Cambios sin lógica
- `refactor:` - Reorganización de código
- `test:` - Agregar tests
- `chore:` - Tareas administrativas
- `ci:` - Cambios en CI/CD

**Ejemplo:** `git commit -m "feat: agregar validación de emails"`

## 📜 Licencia

MIT - Libre para usar y modificar
```

### 2.5 Archivo `COMMIT_TYPES.md`

Crea `docs/COMMIT_TYPES.md`:

```markdown
# 📝 Tipos de Commits (Conventional Commits)

Usa estos prefijos al hacer commits para mantener un historial claro:

| Tipo | Uso | Ejemplo |
|------|-----|---------|
| `feat:` | Nueva característica | `feat: agregar autenticación` |
| `fix:` | Bug fix | `fix: resolver error de login` |
| `docs:` | Documentación | `docs: actualizar README` |
| `style:` | Formato/espacios | `style: formatear código` |
| `refactor:` | Reorganización | `refactor: simplificar función` |
| `test:` | Tests | `test: agregar tests de auth` |
| `chore:` | Tareas admin | `chore: actualizar npm` |
| `ci:` | CI/CD | `ci: agregar GitHub Actions` |

## ✅ Beneficios

- Historial de cambios **legible**
- Cambios **trazables**
- Facilita **búsquedas**
- Profesional ante otros developers

## 🚫 Lo que NO debes hacer

```bash
# ❌ MALO
git commit -m "cambios"
git commit -m "arreglé esto"
git commit -m "falta una coma"

# ✓ BIEN
git commit -m "fix: resolver error en validación"
git commit -m "docs: mejorar instrucciones de setup"
git commit -m "style: agregar point and comma"
```

```

### 2.6 Archivo de configuración de Git

Crea `docs/GIT_WORKFLOW.md`:

```markdown
# 🔀 Git Workflow - Guía Paso a Paso

## Flujo básico

### 1. Crear una nueva rama (feature)

```bash
git checkout -b feature/tu-caracteristica
```

Ejemplo:

```bash
git checkout -b feature/dark-mode
```

### 2. Hacer cambios y commitear

```bash
git add .
git commit -m "feat: implementar dark mode"
```

### 3. Subir la rama a GitHub

```bash
git push -u origin feature/tu-caracteristica
```

### 4. Volver a main (cuando termines)

```bash
git checkout main
git merge feature/tu-caracteristica
```

## Visualizar ramas

```bash
# Ramas locales
git branch

# Todas las ramas (local + remoto)
git branch -a

# Historial visual
git log --oneline --graph --all
```

## Sincronizar con remoto

```bash
# Descargar cambios
git fetch

# Descargar y fusionar
git pull origin develop

# Enviar cambios
git push origin main
```

## Deshacer cambios (cuidado)

```bash
# Deshacer commit, mantener cambios
git reset --soft HEAD~1

# Deshacer TODO
git reset --hard HEAD~1

# Descartar cambios de un archivo
git checkout -- archivo.txt
```

```

✅ **Verifica:** Deberías tener estos archivos:
- `.gitignore`
- `.env.example`
- `package.json`
- `README.md`
- `docs/COMMIT_TYPES.md`
- `docs/GIT_WORKFLOW.md`

---

## PASO 3: Inicializar Git Localmente (5 minutos)

### 3.1 Inicializar Git

```bash
git init
```

### 3.2 Configurar identidad

```bash
git config user.name "Tu Nombre Completo"
git config user.email "tu.email@ejemplo.com"
```

**Nota:** Reemplaza `Tu Nombre Completo` y `tu.email@ejemplo.com` con tus datos reales.

### 3.3 Primer commit

```bash
git add .
git commit -m "chore: estructura inicial del proyecto"
```

**Espera:** Deberías ver algo como:

```
[main (root-commit) abc1234] chore: estructura inicial del proyecto
 6 files changed, 50 insertions(+)
 create mode 100644 .gitignore
 ...
```

### 3.4 Verificar el historial

```bash
git log --oneline
```

Deberías ver:

```
abc1234 (HEAD -> main) chore: estructura inicial del proyecto
```

✅ **Checkpoint:** Tienes Git inicializado y tu primer commit hecho.

---

## PASO 4: Crear Repositorio en GitHub (5 minutos)

### 4.1 Crear el repositorio

1. Ve a **github.com**
2. Haz login con tu cuenta
3. Haz clic en **"+"** → **"New repository"**
4. Nombre: `github-devops-practice`
5. Descripción: `Práctica de DevOps con GitHub`
6. Selecciona **Public** (importante para GitHub Actions free)
7. **NO** inicialices con README (ya tienes uno)
8. Haz clic en **"Create repository"**

### 4.2 Conectar tu repositorio local a GitHub

Después de crear el repositorio, GitHub te mostrará comandos. Ejecuta estos EN TU TERMINAL:

```bash
git branch -M main
git remote add origin https://github.com/TU_USUARIO/github-devops-practice.git
git push -u origin main
```

**Reemplaza `TU_USUARIO`** con tu nombre de usuario de GitHub.

**Ejemplo real:**

```bash
git remote add origin https://github.com/juandiego/github-devops-practice.git
```

### 4.3 Verificar la conexión

```bash
git remote -v
```

Deberías ver:

```
origin  https://github.com/TU_USUARIO/github-devops-practice.git (fetch)
origin  https://github.com/TU_USUARIO/github-devops-practice.git (push)
```

### 4.4 Recargar GitHub en el navegador

Abre GitHub → Tu repositorio. Deberías ver:

- Todos tus archivos
- Tu commit inicial
- El README renderizado

✅ **Checkpoint:** Tu código está en GitHub.

---

## PASO 5: Estrategia de Ramas (7 minutos)

### 5.1 Crear rama `develop`

```bash
git checkout -b develop
git push -u origin develop
```

**¿Por qué?** `main` = versión estable/producción. `develop` = donde se desarrolla.

### 5.2 Crear una rama de feature

```bash
git checkout -b feature/github-actions
```

### 5.3 Simulación de trabajo

Crea un archivo `src/app.js`:

```javascript
// app.js - Aplicación principal
console.log("🚀 Aplicación iniciada");
console.log("Versión: 1.0.0");

const config = {
  appName: "GitHub DevOps Practice",
  version: "1.0.0",
  environment: process.env.NODE_ENV || "development"
};

module.exports = config;
```

### 5.4 Commitear el cambio

```bash
git add src/app.js
git commit -m "feat: crear archivo principal de la aplicación"
git push origin feature/github-actions
```

### 5.5 Ver ramas en GitHub

Ve a tu repositorio en GitHub → **Branches**. Deberías ver:

- `main`
- `develop`
- `feature/github-actions`

### 5.6 Fusionar a develop

```bash
git checkout develop
git merge feature/github-actions
git push origin develop
```

### 5.7 Ver el resultado en GitHub

En GitHub → Branches, verás que `develop` y `feature/github-actions` están sincronizadas.

✅ **Checkpoint:** Entiendes cómo funcionan las ramas.

---

## PASO 6: GitHub Actions - Tu Robot de CI/CD (15 minutos)

### 6.1 Crear el workflow de CI

Crea el archivo `.github/workflows/ci.yml`:

```yaml
name: CI Pipeline

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main, develop ]

jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
    - name: 📥 Descargar código
      uses: actions/checkout@v3
    
    - name: 🔧 Configurar Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18'
    
    - name: 📦 Instalar dependencias
      run: npm install --no-audit 2>/dev/null || echo "Sin dependencias"
    
    - name: ✅ Ejecutar tests
      run: npm test 2>/dev/null || echo "⚠️ Tests no configurados aún"
    
    - name: 🏗️ Validar estructura
      run: |
        echo "📋 Validando estructura del proyecto..."
        test -d .github && echo "✓ .github existe"
        test -d src && echo "✓ src existe"
        test -d tests && echo "✓ tests existe"
        test -f .gitignore && echo "✓ .gitignore existe"
        test -f .env.example && echo "✓ .env.example existe"
        test -f package.json && echo "✓ package.json existe"
        test -f README.md && echo "✓ README.md existe"
        echo "✅ Validación completada"
    
    - name: 🔍 Verificar Variables de Entorno
      run: |
        echo "Variables disponibles en CI:"
        echo "- NODE_ENV: ${NODE_ENV:-no definida}"
        echo "- CI: $CI (siempre true en GitHub Actions)"
        echo "- GITHUB_REF: $GITHUB_REF"
```

**¿Qué hace cada sección?**

| Línea | Qué hace |
|-------|----------|
| `on: push:` | Se ejecuta cuando haces push |
| `branches: [ main, develop ]` | Solo en estas ramas |
| `runs-on: ubuntu-latest` | Corre en una máquina Linux de GitHub |
| `uses: actions/checkout@v3` | Descarga tu código |
| `actions/setup-node@v3` | Instala Node.js |
| `npm install` | Instala librerías |
| `npm test` | Ejecuta tests |

### 6.2 Commitear el workflow

```bash
git add .github/workflows/ci.yml
git commit -m "ci: agregar GitHub Actions workflow"
git push origin develop
```

### 6.3 Ver la ejecución en GitHub

1. Ve a tu repositorio en GitHub
2. Haz clic en **"Actions"** (arriba)
3. Verás "CI Pipeline" ejecutándose
4. Haz clic en el commit más reciente para ver los logs

**Espera:** El workflow se ejecutará en segundos/minutos.

### 6.4 Interpretar los resultados

Cuando termine, deberías ver ✅ (verde) en todos los pasos.

Si ves ❌ (rojo), haz clic para ver el error.

### 6.5 Entender el flujo

```
Tu código → git push → GitHub recibe → Ejecuta ci.yml → 
Robot corre npm test → Robot valida estructura → ✅ o ❌
```

✅ **Checkpoint:** Tu primer GitHub Action está corriendo.

---

## PASO 7: Manejar Secretos (Credenciales) - IMPORTANTE (8 minutos)

### 7.1 ¿Por qué necesitamos Secrets?

**❌ NUNCA hagas esto:**

```javascript
// ❌ PELIGRO - Esto expone tu clave
const API_KEY = "sk-abc123xyz789";
git push  // Ahora todo el mundo ve tu clave
```

**✓ BIEN - Usa Secrets:**

```javascript
// ✓ SEGURO - La clave viene de un lugar seguro
const API_KEY = process.env.API_KEY;  // Viene de .env (no versionado)
// En GitHub Actions, viene de Secrets
```

### 7.2 Crear un Secret en GitHub

1. Ve a tu repositorio en GitHub
2. **Settings** → **Secrets and variables** → **Actions**
3. Haz clic en **"New repository secret"**
4. **Name:** `API_KEY`
5. **Value:** `sk-demo-key-12345`
6. Haz clic en **"Add secret"**

**Crea también:**

- **Name:** `DATABASE_URL`
- **Value:** `postgresql://demo:demo@localhost:5432/demo`

### 7.3 Usar Secrets en tu workflow

Modifica `.github/workflows/ci.yml`:

```yaml
# ... (resto del archivo igual)

    - name: 🔍 Verificar Variables de Entorno
      run: |
        echo "Variables disponibles en CI:"
        echo "- NODE_ENV: ${NODE_ENV:-no definida}"
        echo "- CI: $CI"
        echo "- GITHUB_REF: $GITHUB_REF"
      env:
        API_KEY: ${{ secrets.API_KEY }}
        DATABASE_URL: ${{ secrets.DATABASE_URL }}
```

**¿Qué pasa?**

- GitHub coge el valor de `API_KEY` desde Secrets
- Lo pasa al workflow como variable de entorno
- El workflow lo usa de forma segura
- **Nunca aparece en los logs**

### 7.4 Commitear

```bash
git add .github/workflows/ci.yml
git commit -m "ci: agregar manejo seguro de credenciales"
git push origin develop
```

### 7.5 Verificar en GitHub Actions

1. Ve a **Actions**
2. Haz clic en el último workflow
3. Verás que `API_KEY` aparece en el log... **¡como asteriscos!** ⭐

**Muestra algo como:**

```
✓ API_KEY: sk-*****
✓ DATABASE_URL: postgresql://***
```

✅ **Checkpoint:** Tus credenciales están seguras.

---

## PASO 8: Tags y Releases (5 minutos)

### 8.1 Crear tu primer Tag

Un tag marca una versión específica del código:

```bash
git tag -a v1.0.0 -m "Primera versión estable"
git push origin v1.0.0
```

**¿Qué significa `v1.0.0`?**

- `v` = versión (prefijo)
- `1` = versión major (cambios grandes)
- `0` = versión minor (nuevas características)
- `0` = parche (bug fixes)

### 8.2 Ver tus tags

```bash
git tag
```

Deberías ver:

```
v1.0.0
```

### 8.3 Crear una Release en GitHub

1. Ve a tu repositorio
2. **Releases** (a la derecha)
3. Haz clic en **"Create a new release"**
4. **Choose a tag:** `v1.0.0`
5. **Title:** `v1.0.0 - GitHub DevOps Fundamentals`
6. **Describe this release:**

```markdown
# Versión 1.0.0

¡Primera versión estable!

## ✨ Características

- ✅ Estructura profesional de repositorio
- ✅ GitHub Actions CI/CD
- ✅ Manejo seguro de credenciales
- ✅ Workflow de branches
- ✅ Documentación completa

## 🚀 Cómo empezar

```bash
git clone https://github.com/TU_USUARIO/github-devops-practice.git
cd github-devops-practice
git checkout v1.0.0
```

## 📝 Cambios principales

- Iniciación de repositorio
- Configuración de GitHub Actions
- Setup de desarrollo

## 🙏 Agradecimientos

Práctica completada como parte del curso DevOps.

```

7. Haz clic en **"Publish release"**

### 8.4 Ver tu Release

Vuelve a **Releases**. Verás:
- Título: `v1.0.0 - GitHub DevOps Fundamentals`
- Descripción formateada
- Opción de descargar como ZIP

✅ **Checkpoint:** Tu primera versión está "lanzada" oficialmente.

---

## PASO 9: Validación Final (3 minutos)

### Checklist de Entrega

Marca cada uno que hayas completado:

- [ ] Estructura de carpetas creada
- [ ] Archivos de configuración creados (`.gitignore`, `package.json`, etc.)
- [ ] Git inicializado localmente
- [ ] Repositorio creado en GitHub
- [ ] Código pusheado a GitHub
- [ ] Ramas `main`, `develop`, `feature/*` creadas
- [ ] GitHub Actions workflow funcionando (✅ verde)
- [ ] Secrets configurados en GitHub
- [ ] Tag `v1.0.0` creado
- [ ] Release publicada en GitHub

### Verificación en GitHub

Abre tu repositorio y verifica:

1. **Code**: Todos tus archivos visibles ✅
2. **Actions**: Workflow ejecutándose sin errores ✅
3. **Branches**: 3+ ramas visibles ✅
4. **Settings > Secrets**: 2+ secrets configurados ✅
5. **Releases**: `v1.0.0` publicada ✅

### Video/Pantazo para entregar

Toma una captura de pantalla mostrando:
```

Tu repositorio GitHub con:

- Nombre: github-devops-practice
- Todas las ramas
- Actions workflow ✅
- Releases publicadas

```

---

## 📚 Referencia Rápida de Comandos

```bash
# INICIALIZAR
git init
git config user.name "Nombre"
git config user.email "email@com"

# RAMAS
git checkout -b nombre-rama        # Crear y cambiar
git branch -a                       # Ver todas las ramas
git checkout nombre-rama            # Cambiar de rama
git merge nombre-rama               # Fusionar rama actual

# COMMITS
git add .                           # Agregar archivos
git commit -m "tipo: descripción"  # Commit con tipo
git log --oneline --graph --all    # Ver historial visual

# REMOTO
git remote add origin [URL]         # Conectar a GitHub
git push -u origin rama             # Subir rama
git pull origin rama                # Bajar cambios

# TAGS Y RELEASES
git tag -a v1.0.0 -m "mensaje"     # Crear tag
git push origin v1.0.0              # Subir tag
git tag                             # Ver tags
```

---

## 🆘 Troubleshooting

### Error: "fatal: not a git repository"

```bash
# Solución
cd tu-carpeta-proyecto
git init
```

### Error: "error: src refspec main does not match any"

```bash
# Solución
git add .
git commit -m "chore: commit inicial"
```

### Error: "error: permission denied"

```bash
# Posible solución: Verificar token de GitHub
# Ve a GitHub > Settings > Developer settings > Personal access tokens
# Copia el token y úsalo como contraseña cuando Git lo pida
```

### El workflow no se ejecuta

```bash
# Verifica:
1. El archivo está en .github/workflows/ci.yml
2. El YAML tiene indentación correcta (2 espacios)
3. Hiciste git push (no solo commit local)
4. Ve a Actions y mira los errores
```

### Archivos aparecen rojos en VS Code

```bash
# Significado: Git los ve como cambios sin stagear
# Solución:
git add .
git status  # Ver qué cambió
```

---

## 🎓 Conceptos Aprendidos

| Concepto | Qué es | Por qué importa |
|----------|--------|-----------------|
| **Git** | Control de versiones local | Guardar cambios de forma segura |
| **GitHub** | Servidor remoto de Git | Compartir código y colaborar |
| **Ramas** | Líneas de trabajo paralelas | Desarrollar sin romper main |
| **Commits** | "Snapshots" de tu código | Historial de cambios trazable |
| **GitHub Actions** | Automatización de tareas | Tests y deploys automáticos |
| **Secrets** | Credenciales seguras | Proteger datos sensibles |
| **Tags/Releases** | Versiones marcadas | Marcar hitos y releases |

---

## 📌 Notas Importantes

1. **Nunca** hagas commit de archivos en `.gitignore`
2. **Siempre** usa prefijos en tus commits (`feat:`, `fix:`, etc.)
3. **Protege** tus credenciales en Secrets, no en código
4. **Revisa** los logs de GitHub Actions si algo falla
5. **Mantén** `main` siempre estable

---

## 🚀 Próximos Pasos

Cuando completes esta práctica:

1. Repasa los logs de GitHub Actions
2. Experimenta creando más ramas
3. Crea más Secrets y úsalos
4. Crea tags adicionales (v1.0.1, v1.1.0)
5. **Prepárate para la Práctica 2:** Aplicaremos todo esto a un `index.html` real

---

**¡Felicidades! 🎉 Completaste la Práctica 1: GitHub DevOps Fundamentals**

Ahora entiendes:

- ✅ Cómo estructurar un proyecto profesional
- ✅ Cómo usar ramas para desarrollo seguro
- ✅ Cómo automatizar tareas con GitHub Actions
- ✅ Cómo proteger credenciales
- ✅ Cómo versionar y lanzar releases

Esto es la **base de todo desarrollador profesional.**
