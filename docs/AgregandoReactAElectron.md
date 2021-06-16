# Agregar React a Electron

Para facilitar la configuración de React JS, utilizaremos el CLI Create React App para crear nuestra aplicación de React.

### 1) Crear la nueva app de React

```bash
npx create-react-app nombredelproyecto
```

### 2) Instalar Electron de manera global

```
yarn global add electron@latest
```

### 3) Añadir Electron a nuestra aplicación de React
```
yarn add electron electron-builder --dev
```

### 4) Añadir herramientas de desarrollo adicionales
```
yarn add wait-on concurrently --dev
yarn add electron-is-dev
```

### 5) Crear un nuevo archivo en, /public/electron.js, con el siguiente contenido
```javascript
const electron = require('electron');
const app = electron.app;
const BrowserWindow = electron.BrowserWindow;

const path = require('path');
const isDev = require('electron-is-dev');

let mainWindow;

function createWindow() {
  mainWindow = new BrowserWindow({width: 900, height: 680});
  mainWindow.loadURL(isDev ? 'http://localhost:3000' : `file://${path.join(__dirname, '../build/index.html')}`);
  if (isDev) {
    mainWindow.webContents.openDevTools();
  }
  mainWindow.on('closed', () => mainWindow = null);
}

app.on('ready', createWindow);

app.on('window-all-closed', () => {
  if (process.platform !== 'darwin') {
    app.quit();
  }
});

app.on('activate', () => {
  if (mainWindow === null) {
    createWindow();
  }
});
```

### 6) Agregar el siguiente comando a la etiqueta de scripts package.json.

```
"electron-dev": "concurrently \"BROWSER=none yarn start\" \"wait-on http://localhost:3000 && electron .\""
```

### 7) Agregar el "main" correcto en package.json
```
"main": "public/electron.js",
```

### 8) Ejecutar app en modo develop
```
yarn electron-dev
```

### 9) Compilar
```
yarn electron-pack --win
```


32 bit (modificamos el package.json)
```
"electron-pack": "electron-builder -c.extraMetadata.main=build/electron.js --ia32 --x64 -w"
```
y luego
```
npm run electron-pack
```
---
## Corregir warning Content-Security-Policy
En el header dentro de public/index.html agregamos:
```html
<meta http-equiv="Content-Security-Policy" content="script-src 'self'" />
```
---


[Volver al README](../README.md)
