# 🚀 Práctica de Laboratorio 1: Control de Versiones y Capa de Calidad (QA)

## Contexto de la Práctica

Para que una aplicación web sea profesional, no basta con que "funcione en mi máquina". Necesitamos una **Capa de Calidad (QA)** que asegure que no haya etiquetas HTML rotas, CSS redundante o JavaScript con errores antes de salir a producción.

Hoy dejaremos atrás la era manual de los archivos `.zip`. Prepararemos la arquitectura de nuestro proyecto "Data-Driven Retail" para soportar validaciones (Linters) y Unit Tests, y aseguraremos todo este código en la nube usando Git y GitHub.

---

## 🛠️ Paso a Paso de la Práctica en Clase

### Paso 1: Estructura de Proyecto Profesional

No más archivos sueltos en el escritorio. Abre tu editor **VS Code**, crea una carpeta llamada `retail-app` y construye exactamente esta estructura de carpetas y archivos vacíos:

```text
retail-app/
├── public/                 <-- Lo que el usuario ve
│   ├── index.html
│   ├── css/
│   │   └── styles.css
│   └── js/
│       └── main.js         <-- Aquí vive la interactividad
├── tests/                  <-- Aquí vive el control de calidad
│   ├── html.test.js        <-- (Lo dejaremos vacío para implementarlo después)
│   └── logic.test.js       <-- (Lo dejaremos vacío para implementarlo después)
└── README.md
```

## Paso 2: Creando el Código Base

Abre el archivo `public/index.html` y pega el siguiente código:

### HTML

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Data-Driven Retail</title>
    <link rel="stylesheet" href="css/styles.css">
</head>
<body>
    <h1 id="titulo">Bienvenido a Data-Driven Retail</h1>
    <button id="btn-estado">Ver Estado</button>
    <script src="js/main.js"></script>
</body>
</html>
```

Abre el archivo `public/js/main.js` y agrega un poco de interactividad:

### JavaScript

```javascript
document.getElementById('btn-estado').addEventListener('click', () => {
    alert('Sistemas operativos y estables.');
});
```

Prueba tu web haciendo clic derecho en el archivo `index.html` y seleccionando **"Open with Live Server"**.

---

## Paso 3: El Control de Código (Git y Hooks Conceptuales)

En un entorno DevOps maduro, usamos herramientas llamadas **Git Hooks**. Esto significa que cuando intentas guardar tu código, el sistema ejecuta automáticamente los archivos de la carpeta `tests/`. Si hay un error, ¡Git bloquea el guardado! Por ahora, guardaremos nuestra estructura base manualmente.

Abre la terminal integrada en VS Code.

### Inicializa tu repositorio local

```bash
git init
```

### Prepara todos tus archivos

```bash
git add .
```

### Crea tu primer punto de guardado (commit)

```bash
git commit -m "chore: estructura profesional y capa de calidad"
```

---

## Paso 4: Trabajo Aislado con Ramas (Branches)

Vamos a simular que nos piden una nueva función visual, pero no queremos el riesgo de romper la rama principal (`main`).

### Crea una nueva rama y muévete a ella

```bash
git switch -b feature-darkmode
```

Modifica el archivo `public/css/styles.css` agregando este estilo:

### CSS

```css
body { 
    background-color: #333; 
    color: white; 
}
```

### Registra el cambio en tu rama experimental

```bash
git add .
git commit -m "style: agrega modo oscuro experimental"
```

### Regresa a la rama principal

Notarás en tu navegador que el fondo oscuro desaparece de inmediato:

```bash
git checkout main
```

### Fusiona tu nueva característica con el código principal

¡Todo se ve bien! Fusiona tu nueva característica:

```bash
git merge feature-darkmode
```

---

## Paso 5: Subiendo la Verdad a GitHub

El código en tu computadora es vulnerable. Vamos a aplicar el principio base de GitOps: **"El repositorio remoto es la única fuente de la verdad"**.

1. Entra a tu cuenta de GitHub y crea un repositorio público nuevo llamado `retail-app` (sin agregar README ni .gitignore).

2. Copia y ejecuta en tu terminal los comandos que GitHub te muestra para conectar un repositorio existente. Serán similares a estos (asegúrate de cambiar `TU_USUARIO` por tu usuario real):

```bash
git branch -M main
git remote add origin https://github.com/TU_USUARIO/retail-app.git
git push -u origin main
```
