# Electron Playground

## Setup Steps:

### 1. Create Vite Project

```bash
npm create vite@latest .
npm i
npm run dev
```

### 2. Move src files to ui directory

Update index.html

```html
<script type="module" src="/src/ui/main.tsx"></script>
```

Delete public folder

Clean React generated files like App.tsx

### 3. Update vite.config.ts

```ts
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";

export default defineConfig({
  plugins: [react()],
  base: "./",
  build: {
    outDir: "dist-react",
  },
  server: {
    port: 5123,
    strictPort: true,
  },
});
```

### 4. Update .gitignore

```text
# Logs
logs
*.log
npm-debug.log*
yarn-debug.log*
yarn-error.log*
pnpm-debug.log*
lerna-debug.log*

node_modules
dist
dist-react
dist-electron
*.local

# Editor directories and files
.vscode/*
!.vscode/extensions.json
.idea
.DS_Store
*.suo
*.ntvs*
*.njsproj
*.sln
*.sw?
/test-results/
/playwright-report/
/blob-report/
/playwright/.cache/
```

### 5. Install electron

```bash
npm install --save-dev electron
```

### 6. Create electron directory in src directory

Create relevant files in electron directory

### 7. Update package.json

```json
{
  "name": "electron-playground",
  "private": true,
  "version": "0.0.0",
  "type": "module",
  "main": "dist-electron/main.js",
  "scripts": {
    "dev": "npm-run-all --parallel dev:react dev:electron",
    "dev:react": "vite",
    "dev:electron": "npm run transpile:electron && cross-env NODE_ENV=development electron .",
    "build": "tsc -b && vite build",
    "lint": "eslint .",
    "preview": "vite preview",
    "transpile:electron": "tsc --project src/electron/tsconfig.json",
    "dist:mac": "npm run transpile:electron && npm run build && electron-builder --mac --arm64",
    "dist:win": "npm run transpile:electron && npm run build && electron-builder --win --x64",
    "dist:linux": "npm run transpile:electron && npm run build && electron-builder --linux --x64"
  },
  "dependencies": {
    "os-utils": "^0.0.14",
    "react": "^19.1.0",
    "react-dom": "^19.1.0",
    "react-router": "^7.7.0",
    "recharts": "^3.1.0"
  },
  "devDependencies": {
    "@eslint/js": "^9.29.0",
    "@playwright/test": "^1.54.1",
    "@types/node": "^24.0.14",
    "@types/os-utils": "^0.0.4",
    "@types/react": "^19.1.8",
    "@types/react-dom": "^19.1.6",
    "@vitejs/plugin-react": "^4.5.2",
    "cross-env": "^7.0.3",
    "electron": "^37.2.2",
    "electron-builder": "^26.0.12",
    "eslint": "^9.29.0",
    "eslint-plugin-react-hooks": "^5.2.0",
    "eslint-plugin-react-refresh": "^0.4.20",
    "globals": "^16.2.0",
    "npm-run-all": "^4.1.5",
    "typescript": "~5.8.3",
    "typescript-eslint": "^8.34.1",
    "vite": "^7.0.0",
    "vitest": "^3.2.4"
  }
}
```

### 8. Exclude electron typescript files from react typescript configuration

> tsconfig.app.json

```json
{
  "exclude": ["src/electron"]
}
```

### 9. Create tsconfig.json for electron

```json
{
  "compilerOptions": {
    // require strict types (null-save)
    "strict": true,
    // tell TypeScript to generate ESM Syntax
    "target": "ESNext",
    // tell TypeScript to require ESM Syntax as input (including .js file imports)
    "module": "NodeNext",
    // define where to put generated JS
    "outDir": "../../dist-electron",
    // ignore errors from dependencies
    "skipLibCheck": true,
    // add global types
    "types": ["../../types"]
  }
}
```

### 10. Setup electron builder

```bash
npm i --save-dev electron-builder
```

> electron-builder.json

```json
{
  "appId": "com.raptordev21.electron-playground",
  "files": ["dist-electron", "dist-react"],
  "extraResources": ["dist-electron/preload.cjs", "src/assets/**"],
  "icon": "./desktopIcon.png",
  "mac": {
    "target": "dmg"
  },
  "linux": {
    "target": "AppImage",
    "category": "Utility"
  },
  "win": {
    "target": ["portable", "msi"]
  }
}
```
