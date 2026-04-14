# 🚀 PRÁCTICA 3.2 - Guía del Estudiante: Deploy Automático (60 minutos)

**Duración:** 60 minutos  
**Prerrequiso:** Práctica 3.1
**Objetivo:** Sitio web en vivo con deploy automático  
**Resultado:** `https://tu-usuario.github.io/mi-sitio-web` funcional

---

## ⚡ Instrucción Crítica

**SIGUE EXACTAMENTE ESTE ORDEN.** No saltees pasos. 60 minutos es ajustado.

---

## PASO 1: Estructura Básica (3 minutos)

```bash
mkdir mi-sitio-web && cd mi-sitio-web
mkdir -p .github/workflows src tests docs
```

Abre en VS Code: `code .`

---

## PASO 2: Crear 5 Archivos (15 minutos)

### 2.1 `.gitignore`

```
node_modules/
.env
.DS_Store
```

### 2.2 `package.json`

```json
{
  "name": "mi-sitio-web",
  "version": "1.0.0",
  "description": "Sitio web con DevOps",
  "scripts": {
    "test": "echo 'Tests'",
    "start": "python3 -m http.server 8000"
  }
}
```

### 2.3 `src/index.html` (Principal)

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mi Sitio - DevOps</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <header>
        <h1>🚀 Mi Sitio Web</h1>
    </header>
    <main>
        <section class="hero">
            <h2>Bienvenido a DevOps</h2>
            <p>Este sitio está completamente automatizado</p>
            <button id="btn">Haz Clic</button>
        </section>
        <section class="features">
            <h3>Características</h3>
            <article>
                <h4>✅ Validación Automática</h4>
                <p>GitHub valida el código</p>
            </article>
            <article>
                <h4>🚀 Deploy Automático</h4>
                <p>Cambios en vivo en minutos</p>
            </article>
        </section>
    </main>
    <footer>
        <p>&copy; 2024 DevOps</p>
    </footer>
    <script src="script.js"></script>
</body>
</html>
```

### 2.4 `src/styles.css`

```css
* { margin: 0; padding: 0; box-sizing: border-box; }
body { font-family: Arial, sans-serif; background: #667eea; color: #333; min-height: 100vh; }
header { background: #333; color: white; padding: 1rem; text-align: center; }
main { max-width: 1200px; margin: 0 auto; padding: 2rem; }
.hero { background: white; border-radius: 8px; padding: 3rem; text-align: center; margin-bottom: 2rem; }
.hero h2 { color: #667eea; font-size: 2rem; margin-bottom: 1rem; }
#btn { background: #667eea; color: white; border: none; padding: 0.75rem 2rem; border-radius: 4px; cursor: pointer; font-size: 1rem; }
#btn:hover { background: #764ba2; }
.features { background: white; border-radius: 8px; padding: 2rem; margin-bottom: 2rem; }
.features article { padding: 1rem; border-left: 4px solid #667eea; margin-bottom: 1rem; }
footer { background: #333; color: white; text-align: center; padding: 1rem; margin-top: 3rem; }
@media (max-width: 768px) { .hero h2 { font-size: 1.5rem; } }
```

### 2.5 `src/script.js`

```javascript
document.getElementById('btn').addEventListener('click', () => {
    alert('✅ ¡El sitio funciona!');
});
console.log('🚀 Sitio cargado');
```

✅ **Checkpoint:** 5 archivos creados. Estructura lista.

---

## PASO 3: Probar Localmente (2 minutos)

```bash
npm start
```

Abre navegador: `http://localhost:8000`

Verifica:

- Página bonita con gradiente ✅
- Botón funciona ✅
- Responsive (redimensiona) ✅

Presiona `Ctrl+C` para detener.

---

## PASO 4: Git + GitHub (10 minutos)

### 4.1 Inicializar Git

```bash
git init
git config user.name "Tu Nombre"
git config user.email "tu@email.com"
git add .
git commit -m "chore: sitio web inicial"
```

### 4.2 Crear Repositorio en GitHub

1. Ve a github.com
2. Clic en **"+"** → **"New repository"**
3. Nombre: `mi-sitio-web`
4. **NO** inicialices con README
5. Clic en **"Create repository"**

### 4.3 Conectar Local a GitHub

Copia estos comandos (reemplaza `TU_USUARIO`):

```bash
git branch -M main
git remote add origin https://github.com/TU_USUARIO/mi-sitio-web.git
git push -u origin main
```

### 4.4 Crear rama develop

```bash
git checkout -b develop
git push -u origin develop
```

### 4.5 Verificar en GitHub

Abre tu repositorio. Deberías ver todos tus archivos.

✅ **Checkpoint:** Código en GitHub.

---

## PASO 5: Crear Workflow de Deploy (10 minutos)

### 5.1 Crear `.github/workflows/deploy.yml`

Copia EXACTAMENTE (atención a indentación):

```yaml
name: Deploy a GitHub Pages

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    
    - name: Preparar archivos
      run: mkdir -p build && cp -r src/* build/
    
    - name: Deploy a GitHub Pages
      uses: peaceiris/actions-gh-pages@v3
      with:
        github_token: ${{ secrets.GITHUB_TOKEN }}
        publish_dir: ./build
```

**¿Qué hace?**

- Line 3: Se ejecuta con cada push a main
- Line 10-12: Descarga tu código
- Line 14-15: Copia archivos a carpeta `build/`
- Line 17-21: Publica en GitHub Pages automáticamente

### 5.2 Activar GitHub Pages

1. Ve a tu repositorio
2. **Settings** → **Pages** (en el menú izquierdo)
3. **Build and deployment → Source:** Selecciona **"GitHub Actions"**
4. Guarda

### 5.3 Commitear y Push

```bash
git checkout main
git merge develop
git add .github/workflows/deploy.yml
git commit -m "ci: agregar deploy a GitHub Pages"
git push origin main
```

### 5.4 Mira el Workflow

1. Ve a tu repositorio en GitHub
2. Haz clic en **"Actions"** (arriba)
3. Verás "Deploy a GitHub Pages" ejecutándose
4. **Espera a que termine** (debe verse ✅ verde)

Toma ~1-2 minutos.

✅ **Checkpoint:** Workflow ejecutándose.

---

## PASO 6: Ver Sitio en Vivo (8 minutos)

### 6.1 Ir a GitHub Pages

Abre en el navegador:

```
https://tu-usuario.github.io/mi-sitio-web
```

Reemplaza `tu-usuario` con tu nombre de usuario de GitHub.

**¡Deberías ver tu sitio!**

Prueba:

- Haz clic en el botón → aparece alerta
- Redimensiona la ventana → se adapta al teléfono
- Ves el gradiente morado

### 6.2 Demostración Mágica

Ahora viene la magia. Haz un cambio en vivo:

**Edita `src/index.html`:**

Cambia esta línea:

```html
<h2>Bienvenido a DevOps</h2>
```

A esto:

```html
<h2>🎉 ¡DevOps en Acción!</h2>
```

**Commitea y push:**

```bash
git add src/index.html
git commit -m "feat: actualizar mensaje"
git push origin main
```

**Ahora la magia:**

1. Ve a GitHub → **Actions**
2. Mira el workflow ejecutándose
3. **Cuando termine (✅ verde)**, recarga tu sitio en el navegador
4. **¡El cambio está ahí!** En vivo. Automáticamente.

---

## PASO 7: Validación Final (2 minutos)

Checklist de entrega:

- [ ] Repositorio en GitHub: `mi-sitio-web`
- [ ] Código visible en main
- [ ] Sitio en vivo: `https://tu-usuario.github.io/mi-sitio-web`
- [ ] Botón funciona
- [ ] Cambio en vivo demostrado
- [ ] Workflow ✅ verde en Actions

---

## 🎯 Resultado Final

**Tienes:**
✅ Sitio web profesional  
✅ Código versionado en GitHub  
✅ Deploy automático  
✅ Cambios en vivo en minutos  
✅ **DevOps completamente funcional**

---

## 🔗 URLs Importantes

```
Repositorio: https://github.com/TU_USUARIO/mi-sitio-web
Sitio en vivo: https://TU_USUARIO.github.io/mi-sitio-web
Actions: https://github.com/TU_USUARIO/mi-sitio-web/actions
```

---

## 🆘 Si Algo Falla

### "GitHub Pages no muestra sitio"

```
1. Settings → Pages
2. Source debe ser "GitHub Actions"
3. Espera 2 minutos
4. Recarga (Ctrl+Shift+R)
```

### "Workflow falla"

```
1. Ve a Actions
2. Haz clic en el workflow rojo
3. Lee el error
4. El error usual: indentación YAML (debe ser 2 espacios)
```

### "No veo los cambios"

```
1. Espera a que el workflow termine (✅ verde)
2. Recarga el navegador: Ctrl+Shift+R (limpia cache)
3. Si aún no funciona, ve a Actions y verifica que deploy.yml completó
```

---

## ✨ ¡Felicidades

Acabas de:

1. ✅ Crear un sitio web profesional
2. ✅ Versionarlo en GitHub
3. ✅ Automatizar el deploy
4. ✅ Ver cambios en vivo

**Esto es lo que hacen los desarrolladores profesionales todos los días.**

---

**¡Sigue aprendiendo DevOps! 🚀**
