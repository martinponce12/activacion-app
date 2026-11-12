# Activación – pasos para ponerla a funcionar

Archivos: `index.html` (la app), `fb.js` (librería de Firebase incluida, no se edita), `sw.js` (modo sin conexión), `manifest.webmanifest` e `icon-*.png` (instalable en el celular). Tienen que quedar juntos en la misma carpeta.

## 1. Crear el proyecto en Firebase (gratis, plan Spark)
1. https://console.firebase.google.com → **Agregar proyecto** → "activacion-app". Desactivá Google Analytics.
2. **Build → Authentication → Comenzar → Método de acceso**: habilitá **Correo electrónico/contraseña** y **Google** (en Google elegí el email de soporte).
3. **Build → Firestore Database → Crear base de datos** → modo producción → región `southamerica-east1` (São Paulo).
4. **Configuración del proyecto (⚙️) → Tus apps → ícono Web `</>`** → registrá la app (sin Hosting) y copiá el objeto `firebaseConfig`.

## 2. Pegar la configuración
En `index.html` reemplazá los `"PEGAR"` de la constante `FB` con `apiKey`, `authDomain`, `projectId` y `appId`.

## 3. Reglas de seguridad (importante)
Cada cuenta tiene su propio espacio de datos, y nadie ve el de otro. En **Firestore → Reglas** pegá esto y **Publicar**:
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{uid}/{document=**} {
      allow read, write: if request.auth != null && request.auth.uid == uid;
    }
  }
}
```

## 4. Probar en tu PC
Abrir el archivo con doble clic no funciona (usa módulos). En VSCode: extensión **Live Server** → clic derecho en `index.html` → *Open with Live Server*. En `localhost` el ingreso con Google funciona sin más configuración.

## 5. Publicar en Netlify (gratis)
- **Rápido:** https://app.netlify.com/drop → arrastrá la carpeta `activacion-app`.
- **Con GitHub:** subí la carpeta a un repositorio → Netlify → *Add new site → Import from Git* → sin comando de build, directorio de publicación `/`.

Después, en Firebase → **Authentication → Configuración → Dominios autorizados**, agregá el dominio `xxxx.netlify.app`. **Sin este paso el ingreso con Google falla.**

## 6. Uso y sesión
- Al abrir la app: **Ingresar**, **Crear cuenta** (email y contraseña, mínimo 6 caracteres) o **Continuar con Google**.
- La sesión queda abierta en ese dispositivo hasta que toque **Salir**.
- Los datos pertenecen a la cuenta: si la encargada entra en otro dispositivo, tiene que usar la misma cuenta (mismo email o mismo Google).
- Instalar en el celular: abrí la URL en Chrome (Android) o Safari (iPhone) → **Agregar a pantalla de inicio / Instalar app**. Conviene ingresar una vez desde el navegador antes de instalarla; después funciona sin internet y sincroniza al volver la señal.

## 7. Primer uso (en Config)
1. Creá el **período**. 2. Cargá **activadores** y **categorías** (con % mínimo de aprobación si corresponde). 3. Cargá los **objetivos** grupales e individuales. 4. Cargá **clientes** (o agregalos desde Cargar). 5. Registrá desde **Cargar** y corregí desde **Historial**. 6. Mes siguiente: **Crear período** con "Copiar objetivos" tildado.

## Notas
- El CSV usa `;` y abre directo en Excel en español.
- Si después de republicar ves la versión vieja, subí el número de `CACHE` en `sw.js` (`activacion-v3`).
