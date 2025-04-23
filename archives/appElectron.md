# Configuracion app de electron

[Volver a Readme](../README.md)

Para crear una aplicación **Vite + Electron** que se pueda distribuir en **Windows** como un archivo ejecutable (`.exe`), debes empaquetarla usando herramientas como **Electron Forge**, **Electron Builder** o **Electron Packager**. Aquí tienes una guía paso a paso para lograrlo:

---

### **1. Configurar tu proyecto con Vite y Electron**
Si aún no tienes el proyecto configurado, puedes seguir estos pasos básicos:
1. **Inicia el proyecto Vite**:
   ```bash
   npm create vite@latest my-electron-app
   cd my-electron-app
   npm install
   ```

2. **Instala Electron**:
   ```bash
   npm install electron --save-dev
   ```

3. **Crea un archivo principal para Electron**:
   - En la raíz del proyecto, crea un archivo `main.js` o `main.ts` (si usas TypeScript) con este contenido básico:

     ```javascript
     const { app, BrowserWindow } = require('electron');

     let mainWindow;

     app.on('ready', () => {
         mainWindow = new BrowserWindow({
             width: 800,
             height: 600,
             webPreferences: {
                 nodeIntegration: true,
             },
         });

         mainWindow.loadURL('http://localhost:5173'); // Reemplaza con tu build en producción
     });
     ```

4. **Modifica los scripts de Vite para el modo Electron**:
   - En tu `package.json`, agrega:
     ```json
     "scripts": {
       "dev": "vite", 
       "start": "electron .",
       "build": "vite build"
     }
     ```

---

### **2. Configurar el empaquetado con Electron Builder**
**Electron Builder** es una herramienta popular para empaquetar tu aplicación en un `.exe`. Sigue estos pasos:

1. **Instala Electron Builder**:
   ```bash
   npm install electron-builder --save-dev
   ```

2. **Configura Electron Builder en el `package.json`**:
   - Agrega una sección `build`:
     ```json
     "build": {
       "appId": "com.miempresa.miaplicacion",
       "productName": "MiAplicacion",
       "directories": {
         "output": "dist-electron"
       },
       "files": [
         "dist/**",
         "main.js"
       ],
       "win": {
         "target": "nsis",
         "icon": "build/icon.ico"
       }
     }
     ```
   - Asegúrate de que `dist` contenga los archivos estáticos de tu app (se generan con `vite build`).

3. **Crea un ícono para Windows**:
   - Genera un archivo `.ico` para el icono de tu app y guárdalo en `build/icon.ico`.

4. **Modifica el `main.js` para cargar los archivos de producción**:
   - Cambia el `loadURL` para que cargue la carpeta `dist` después de construir la aplicación:
     ```javascript
     mainWindow.loadFile('dist/index.html');
     ```

---

### **3. Empaquetar la app**
1. Ejecuta el comando de build:
   ```bash
   npm run build
   ```

2. Genera el ejecutable:
   ```bash
   npx electron-builder
   ```

Esto generará un archivo instalador `.exe` dentro de la carpeta `dist-electron`.

---

### **4. Probar y distribuir**
- **Prueba el instalador**: Abre el `.exe` en un entorno de prueba para verificar que la aplicación funcione correctamente.
- **Firma digitalmente tu aplicación**: Si planeas distribuir tu app de forma pública, considera firmarla digitalmente para evitar advertencias de seguridad de Windows.
- **Distribución**: Puedes compartir tu aplicación empaquetada a través de un sitio web, plataformas de distribución como GitHub Releases, o mediante un archivo `.zip` con el instalador.

---

¿Necesitas ayuda configurando algo específico en el proyecto? 😊