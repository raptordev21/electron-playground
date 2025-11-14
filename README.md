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
