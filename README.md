# Asistencias

Aplicación web para registrar la asistencia diaria de empleados y consultar
el historial mes con mes. Los datos se guardan en la nube (Firebase), así
que puedes usarla desde cualquier computadora con internet.

## Qué incluye

- **Registrar asistencia**: una tarjeta por empleado para marcar el día
  como Presente / Falta / Retardo / Permiso, con hora automática.
- **Empleados**: alta y baja del personal.
- **Reportes mensuales**: filtra por mes y, opcionalmente, por empleado;
  muestra el detalle día por día y un resumen de totales.
- Acceso protegido con inicio de sesión (solo administradores).

## 1. Crear el proyecto de Firebase (la base de datos en la nube)

1. Entra a [console.firebase.google.com](https://console.firebase.google.com)
   e inicia sesión con tu cuenta de Google.
2. Clic en **Agregar proyecto**, ponle un nombre (por ejemplo `asistencias`)
   y termina el asistente. No necesitas Google Analytics, puedes desactivarlo.
3. Dentro del proyecto, clic en el ícono **</>** ("Web") para agregar una app web.
   Ponle un apodo y clic en **Registrar app**.
4. Firebase te mostrará un bloque `firebaseConfig = { ... }`. Copia esos valores.
5. Abre el archivo `js/firebase-config.js` de este proyecto y reemplaza los
   valores de ejemplo (`TU_API_KEY`, etc.) con los que copiaste.

### Activar autenticación

1. En el menú lateral de Firebase, ve a **Authentication** → **Comenzar**.
2. En la pestaña **Sign-in method**, activa **Correo electrónico/contraseña**.
3. Ve a la pestaña **Users** → **Add user** y crea tu usuario administrador
   (el correo y contraseña con los que vas a entrar a la app).

### Activar la base de datos

1. En el menú lateral, ve a **Firestore Database** → **Crear base de datos**.
2. Elige **Modo de producción** y la región más cercana a ti.
3. Ve a la pestaña **Reglas** y reemplaza el contenido con esto, para que
   solo usuarios que iniciaron sesión puedan leer y escribir datos:

   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /{document=**} {
         allow read, write: if request.auth != null;
       }
     }
   }
   ```

4. Clic en **Publicar**.

> La primera vez que uses la pestaña de Reportes, Firestore puede pedirte
> crear un "índice" (aparece un enlace en la consola del navegador, F12).
> Solo da clic en ese enlace y luego en **Crear índice**; tarda un par de
> minutos y no se vuelve a pedir.

## 2. Subir el proyecto a GitHub

1. Crea un repositorio nuevo en GitHub llamado `Asistencias` (puede ser privado).
2. Desde esta carpeta, en una terminal:

   ```
   git init
   git add .
   git commit -m "Primera versión de Asistencias"
   git branch -M main
   git remote add origin https://github.com/TU_USUARIO/Asistencias.git
   git push -u origin main
   ```

## 3. Publicar la app (GitHub Pages)

1. En tu repositorio de GitHub, ve a **Settings** → **Pages**.
2. En **Source**, elige la rama `main` y la carpeta `/ (root)`.
3. Guarda. En un par de minutos tu app estará disponible en
   `https://TU_USUARIO.github.io/Asistencias/`.

Cualquier administrador con el correo/contraseña que crearon en Firebase
Authentication podrá entrar a esa dirección y usar la app.

## Estructura del proyecto

```
Asistencias/
├── index.html          → pantalla de inicio de sesión
├── dashboard.html       → aplicación principal (3 pestañas)
├── css/style.css        → estilos
├── js/firebase-config.js → tus llaves de Firebase (edítalo)
├── js/app.js             → lógica de la aplicación
└── README.md
```

## Notas

- Si quieres agregar más administradores, créalos en Firebase Authentication
  → Users → Add user; no hace falta tocar el código.
- El historial nunca se borra: cada mes queda guardado y puedes consultarlo
  después desde la pestaña de Reportes.
