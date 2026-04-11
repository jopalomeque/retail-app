# 🚀 Configuración Inicial de GitHub en VS Code

> **Este documento te guía paso a paso para configurar GitHub en VS Code desde cero.**

---

## ✅ PASO 1: Configurar tu Identidad en Git

Antes de trabajar con GitHub, Git necesita conocer quién eres. Esta información aparecerá en todos tus commits.

### 1.1 Abre la Terminal en VS Code

Tienes 3 opciones:

**Opción 1:** Presiona `Ctrl + Ñ` (Windows/Linux) o `Cmd + Ñ` (Mac)

**Opción 2:** Usa el menú: **Terminal** → **New Terminal**

**Opción 3:** Haz clic en la esquina superior derecha de VS Code

**Opción 4:** Abre git config --config --list en el Terminal

### 1.2 Configura tu Nombre

Copia y pega este comando (cambia `Tu Nombre` por tu nombre real):

```bash
git config --global user.name "Tu Nombre"
```

**Ejemplo real:**

```bash
git config --global user.name "Juan García López"
```

Presiona **Enter**.

### 1.3 Configura tu Correo

Copia y pega este comando (cambia `tu-email@example.com` por tu correo **de GitHub**):

```bash
git config --global user.email "tu-email@example.com"
```

**Ejemplo real:**

```bash
git config --global user.email "juan.garcia@universidad.edu.ec"
```

### 1.4 Configura la rama main por default

```bash
git config --global init.defaultBranch main
```


Presiona **Enter**.

> ⚠️ **MUY IMPORTANTE:** Usa el MISMO correo que registraste en GitHub. Si no lo haces, GitHub no vinculará tus commits correctamente.

### 1.4 Verifica que Todo Quedó Bien

Ejecuta este comando para confirmar:

```bash
git config --global --list
```

Deberías ver algo así (desplázate con la flecha hacia abajo):

```
user.name=Juan García López
user.email=juan.garcia@universidad.edu.ec
core.autocrlf=true
...
```

Si ves tu nombre y correo, **¡perfecto!** Continúa con el siguiente paso.

---

## ⚡ PASO 2: Elige tu Método de Autenticación

GitHub requiere que demuestres quién eres cuando subes código. Hay **2 formas principales**:

### Opción A: HTTPS + Personal Access Token ⭐ **RECOMENDADO PARA PRINCIPIANTES**

- ✅ Más fácil de configurar
- ✅ No requiere generar claves
- ✅ Funciona en cualquier computadora

### Opción B: SSH

- ✅ Más seguro (sin contraseñas)
- ❌ Más complicado de configurar

**Recomendación:** Si es tu primera vez, **sigue la Opción A** (HTTPS + Token). Es más simple y igual de segura.

---

## 🔑 OPCIÓN A: Autenticación con HTTPS + Personal Access Token (RECOMENDADO)

### ¿Qué es un Personal Access Token (PAT)?

Un **Personal Access Token** es como una contraseña temporal especial que solo GitHub reconoce. Ventajas:

- 🔒 Más seguro que tu contraseña real
- 🔄 Puedes crear múltiples tokens (uno por dispositivo)
- 🚫 Puedes eliminarlos sin cambiar tu contraseña
- ⏰ Expiran automáticamente (máximo 90 días recomendado)

### Paso 2A.1: Crear tu Personal Access Token en GitHub

1. **Abre tu navegador** y ve a: <https://github.com/settings/tokens>

   > Si no estás logged in en GitHub, hazlo primero en <https://github.com>

2. Haz clic en el botón verde **"Generate new token"** → **"Generate new token (classic)"**

3. En el campo **"Note"** (notas), escribe:

   ```
   VS Code - Mi Computadora
   ```

4. En **"Expiration"** (vencimiento), selecciona:

   ```
   90 days
   ```

   > Esto es más seguro. Después de 90 días deberás crear uno nuevo.

5. En **"Select scopes"** (permisos), asegúrate de marcar:
   - ✅ **repo** (acceso completo a repositorios privados y públicos)

6. Haz clic en el botón verde **"Generate token"** al final

7. **COPIA EL TOKEN INMEDIATAMENTE** (aparece una sola vez)

   Verás algo como:

   ```
   ghp_16C7e42F292c6912E7710c838347Ae178B4a
   ```

   Cópialo con `Ctrl + C` o haz clic en el icono de copiar

### Paso 2A.2: Guarda el Token en un Lugar Seguro

Guarda el token en un archivo de texto temporal (en tu Escritorio):

1. Abre el Bloc de Notas
2. Pega el token
3. Guarda como `github-token-temporal.txt` en tu Escritorio

> ⚠️ **IMPORTANTE:** No compartas este token con nadie. Es como tu contraseña.

### Paso 2A.3: Configura VS Code para Usar el Token

Excelente noticia: VS Code puede guardar el token **automáticamente**.

Cuando hagas tu primer `git push`:

1. VS Code te pedirá autenticación
2. Aparecerá un cuadro de diálogo: **"Authenticate with GitHub"**
3. Haz clic en **"Sign in with your browser"** o **"Open"**
4. Se abrirá tu navegador y verás una página de GitHub
5. Haz clic en **"Authorize GitHub"**
6. GitHub te dirá que todo está bien
7. Vuelve a VS Code

**¡Listo!** VS Code guardó tu token automáticamente. Para futuros `git push`, no necesitarás ingresar nada.

### Paso 2A.4 (Alternativa Manual): Si el Paso Anterior No Funciona

Si la autenticación automática falla, configura Git manualmente:

En la terminal de VS Code, ejecuta:

```bash
git config --global credential.helper store
```

Luego, cuando hagas `git push` por primera vez:

1. Te pedirá: **Username for '<https://github.com>':**

   ```
   Ingresa tu usuario de GitHub (ejemplo: juan.garcia)
   ```

   Presiona **Enter**

2. Te pedirá: **Password for 'https://...':**

   ```
   Pega tu Personal Access Token aquí (el que copiaste antes)
   ```

   Presiona **Enter**

Git guardará el token para futuros usos. No tendrás que ingresar nada más.

---

## 🔐 OPCIÓN B: Autenticación con SSH (Alternativa Avanzada)

**Usa esta opción SOLO si:**

- La Opción A no funciona para ti
- Prefieres no usar tokens
- Quieres autenticación sin contraseñas

### Paso 2B.1: Verifica si ya tienes una Clave SSH

En la terminal de VS Code, ejecuta:

```bash
ls -la ~/.ssh
```

**Resultados posibles:**

**Si ves una lista con archivos `id_rsa` o `id_ed25519`:**

- Ya tienes una clave SSH, salta al **Paso 2B.3**

**Si ves "No such file or directory" o la carpeta está vacía:**

- Necesitas crear una clave nueva, continúa con **Paso 2B.2**

### Paso 2B.2: Genera una Nueva Clave SSH

Ejecuta este comando (cambia el email):

```bash
ssh-keygen -t ed25519 -C "tu-email@example.com"
```

**Ejemplo:**

```bash
ssh-keygen -t ed25519 -C "juan.garcia@universidad.edu.ec"
```

El sistema te hará 3 preguntas:

**Pregunta 1:** "Enter file in which to save the key..."

```
→ Presiona Enter (usa la ruta por defecto)
```

**Pregunta 2:** "Enter passphrase..."

```
→ Presiona Enter (o escribe una contraseña si quieres más seguridad)
```

**Pregunta 3:** "Enter same passphrase again..."

```
→ Presiona Enter (o repite la contraseña)
```

**Si todo funcionó, verás:**

```
Your identification has been saved in /home/usuario/.ssh/id_ed25519
Your public key has been saved in /home/usuario/.ssh/id_ed25519.pub
```

### Paso 2B.3: Copia tu Clave Pública SSH

Ejecuta este comando:

```bash
cat ~/.ssh/id_ed25519.pub
```

**Si usaste RSA en lugar de Ed25519:**

```bash
cat ~/.ssh/id_rsa.pub
```

Verás una línea larga que empieza con `ssh-ed25519`:

```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIKvK/... tu-email@example.com
```

**Cópiala toda entera** (desde `ssh-ed25519` hasta el final).

### Paso 2B.4: Agrega la Clave SSH a GitHub

1. Abre tu navegador en: <https://github.com/settings/keys>

2. Haz clic en **"New SSH key"** (botón verde)

3. En el campo **"Title"**, escribe:

   ```
   Mi Computadora - VS Code
   ```

4. En el campo **"Key"**, pega tu clave SSH completa

5. Haz clic en **"Add SSH key"**

6. Si GitHub te pide, confirma con tu contraseña de GitHub

### Paso 2B.5: Verifica que Funciona

En la terminal de VS Code, ejecuta:

```bash
ssh -T git@github.com
```

**Si ves esto (✅ CORRECTO):**

```
Hi juan-garcia! You've successfully authenticated, but GitHub does not provide shell access.
```

**Si ves un error (❌ PROBLEMA):**

- Verifica que copiaste toda la clave (incluyendo `ssh-ed25519` al inicio)
- Asegúrate de que está en GitHub settings
- Intenta de nuevo

---

## ⚠️ SOLUCIONAR PROBLEMAS COMUNES

### Problema: "Authentication failed" o "Permission denied"

**¿Qué significa?**
GitHub no reconoce quién eres.

**Solución:**

**Si usas HTTPS + Token:**

1. Ve a <https://github.com/settings/tokens>
2. Genera un nuevo token (si el anterior expiró)
3. Copia el nuevo token
4. En VS Code, presiona `Ctrl + Shift + P`
5. Escribe: "Credentials: Clear credentials from memory"
6. Presiona Enter
7. Intenta hacer `git push` de nuevo
8. Cuando te pida autenticación, ingresa tu token

**Si usas SSH:**

1. Verifica tu clave: `ssh -T git@github.com`
2. Si falla, regenera una clave (Paso 2B.2)
3. Agrégala a GitHub (Paso 2B.4)

### Problema: "Could not read from remote repository"

**¿Qué significa?**
VS Code no puede conectar con GitHub.

**Solución:**

1. Verifica que tienes internet
2. Verifica tu método de autenticación:

   ```bash
   git remote -v
   ```

3. Deberías ver algo como:

   ```
   origin  https://github.com/tu-usuario/mi-repo.git (fetch)
   origin  https://github.com/tu-usuario/mi-repo.git (push)
   ```

4. Si está incorrecto, cambialo:

   ```bash
   git remote set-url origin https://github.com/tu-usuario/mi-repo.git
   ```

### Problema: "Token expirado"

**¿Qué significa?**
Tu Personal Access Token tiene más de 90 días.

**Solución:**

1. Ve a <https://github.com/settings/tokens>
2. Genera un nuevo token (Paso 2A.1)
3. En VS Code:
   - Presiona `Ctrl + Shift + P`
   - Escribe: "Credentials: Clear credentials from memory"
   - Presiona Enter
4. Intenta `git push` de nuevo con el nuevo token

---

## 📝 PASO 3: Crear tu Primer Repositorio

### Paso 3.1: Crea el Repositorio en GitHub

1. Ve a <https://github.com/new>

2. En **"Repository name"**, escribe:

   ```
   mi-primer-proyecto
   ```

3. Deja todo lo demás por defecto

4. Haz clic en **"Create repository"** (botón verde)

5. **NO selecciones** "Initialize this repository with README"

### Paso 3.2: Crear la Carpeta Localmente

En tu terminal de VS Code:

```bash
mkdir mi-primer-proyecto
cd mi-primer-proyecto
git init
```

### Paso 3.3: Crear un Archivo

Abre VS Code en esa carpeta y crea un archivo `README.md`:

```markdown
# Mi Primer Proyecto

Este es mi primer proyecto con GitHub.
```

### Paso 3.4: Tu Primer Commit

En la terminal:

```bash
git add .
git commit -m "chore: inicio del proyecto"
```

### Paso 3.5: Conectar con GitHub

GitHub te mostró unos comandos. Cópialos en la terminal:

```bash
git branch -M main
git remote add origin https://github.com/tu-usuario/mi-primer-proyecto.git
git push -u origin main
```

**Ingresa tu token cuando te lo pida.**

### Paso 3.6: ¡Verifica en GitHub

1. Ve a <https://github.com/tu-usuario/mi-primer-proyecto>

2. Deberías ver:
   - Tu archivo `README.md`
   - Tu commit message "chore: inicio del proyecto"

**¡Felicidades! Acabas de hacer tu primer push a GitHub.** 🎉

---

## 📚 REFERENCIA RÁPIDA: Comandos Git Básicos

| Comando | ¿Qué hace? |
|---------|-----------|
| `git config --global user.name "Nombre"` | Configura tu nombre |
| `git config --global user.email "email"` | Configura tu correo |
| `git init` | Inicia un repositorio local |
| `git add .` | Prepara todos los cambios |
| `git commit -m "mensaje"` | Guarda los cambios con un mensaje |
| `git push` | Envía los cambios a GitHub |
| `git pull` | Descarga cambios desde GitHub |
| `git clone URL` | Descarga un repositorio completo |
| `git status` | Muestra qué cambió |
| `git log` | Muestra el historial de commits |
| `git branch` | Muestra todas las ramas |
| `git checkout -b nombre` | Crea una rama nueva (Deprecated) | 
| `git switch -c nombre` | Crea una rama nueva |

---

## ✅ CHECKLIST FINAL

Antes de empezar a trabajar, verifica que completaste todo:

- ✅ Configuré mi nombre: `git config --global --list`
- ✅ Configuré mi correo (el mismo de GitHub)
- ✅ Elegí autenticación HTTPS + Token (Opción A) **O** SSH (Opción B)
- ✅ Si usé Token: Creé un Personal Access Token en GitHub
- ✅ Si usé SSH: Agregué mi clave SSH a GitHub settings
- ✅ Probé la conexión (Token funciona **O** SSH funciona)
- ✅ Creé mi primer repositorio en GitHub
- ✅ Hice mi primer commit localmente
- ✅ Hice mi primer push a GitHub
- ✅ Verifiqué que mis archivos aparecen en GitHub

**Si marcaste todo, ¡estás listo!** 🚀

---

## 💡 TIPS ÚTILES

### Cambiar de HTTPS a SSH (o viceversa)

Si después cambias de opinión sobre el método de autenticación:

**De HTTPS a SSH:**

```bash
git remote set-url origin git@github.com:tu-usuario/tu-repo.git
```

**De SSH a HTTPS:**

```bash
git remote set-url origin https://github.com/tu-usuario/tu-repo.git
```

### Ver tus repositorios conectados

```bash
git remote -v
```

### Limpiar tu token guardado (si algo falla)

```bash
git config --global credential.helper store
```

Luego, el siguiente `git push` te pedirá el token de nuevo.

---

## 🆘 ¿TODAVÍA TIENES PROBLEMAS?

Si algo no funciona:

1. **Lee el mensaje de error** - Suelen ser muy descriptivos
2. **Copia el error y búscalo en Google** - 99% de las veces alguien ya lo resolvió
3. **Pide ayuda a tu profesor o compañeros** - Que vean qué sale en tu terminal
4. **No borres nada** - Si algo falla, no elimines archivos. Probablemente se pueda arreglar.
