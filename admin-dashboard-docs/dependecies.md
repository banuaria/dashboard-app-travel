# 📦 Dependencies Reference

## Quick Reference untuk semua package yang dibutuhkan

---

## 🎯 Production Dependencies

### Core Framework
```bash
npm install react@^18.2.0 react-dom@^18.2.0
```
- **react**: Library utama untuk UI components
- **react-dom**: DOM rendering untuk React

---

### Routing
```bash
npm install react-router-dom@^6.20.0
```
- **react-router-dom**: Client-side routing, protected routes, navigation

---

### State Management
```bash
npm install zustand@^4.4.7
```
- **zustand**: Simple state management (alternative: `@reduxjs/toolkit` + `react-redux`)

---

### Form Handling
```bash
npm install react-hook-form@^7.49.0
```
- **react-hook-form**: Form validation, error handling, form state

---

### Charts & Data Visualization
```bash
npm install recharts@^2.10.0
```
- **recharts**: Line, Bar, Pie, Area charts - semua chart components

---

### UI Components & Icons
```bash
npm install @headlessui/react@^1.7.17 @heroicons/react@^2.1.1
```
- **@headlessui/react**: Unstyled accessible UI components (Modal, Dropdown, Dialog)
- **@heroicons/react**: Icon library dari Tailwind (solid & outline icons)

---

### Date & Utilities
```bash
npm install date-fns@^3.0.0 clsx@^2.0.0
```
- **date-fns**: Date formatting, manipulation, parsing
- **clsx**: Conditional className utility

---

## 🛠️ Development Dependencies

### TypeScript
```bash
npm install --save-dev typescript@^5.3.0 @types/react@^18.2.0 @types/react-dom@^18.2.0
```
- **typescript**: TypeScript compiler
- **@types/react**: Type definitions untuk React
- **@types/react-dom**: Type definitions untuk React DOM

---

### Build Tool
```bash
npm install --save-dev vite@^5.0.0 @vitejs/plugin-react@^4.2.0
```
- **vite**: Fast build tool & dev server
- **@vitejs/plugin-react**: React plugin untuk Vite

---

### CSS Framework
```bash
npm install --save-dev tailwindcss@^3.4.0 postcss@^8.4.32 autoprefixer@^10.4.16
```
- **tailwindcss**: Utility-first CSS framework
- **postcss**: CSS transformer
- **autoprefixer**: Add vendor prefixes automatically

---

### Testing
```bash
npm install --save-dev vitest@^1.0.0 jsdom@^23.0.0
npm install --save-dev @testing-library/react@^14.1.0
npm install --save-dev @testing-library/jest-dom@^6.1.5
npm install --save-dev @testing-library/user-event@^14.5.1
```
- **vitest**: Fast unit test framework (Vite-native)
- **jsdom**: DOM implementation untuk testing
- **@testing-library/react**: React component testing utilities
- **@testing-library/jest-dom**: Custom matchers untuk DOM
- **@testing-library/user-event**: User interaction simulation

---

### Code Quality
```bash
npm install --save-dev eslint@^8.55.0 @typescript-eslint/eslint-plugin@^6.15.0
npm install --save-dev @typescript-eslint/parser@^6.15.0
npm install --save-dev prettier@^3.1.1
```
- **eslint**: JavaScript/TypeScript linter
- **@typescript-eslint/eslint-plugin**: ESLint rules untuk TypeScript
- **@typescript-eslint/parser**: TypeScript parser untuk ESLint
- **prettier**: Code formatter

---

## 📋 Complete Installation Command

### All at Once (Recommended)

```bash
# Production dependencies
npm install \
  react@^18.2.0 \
  react-dom@^18.2.0 \
  react-router-dom@^6.20.0 \
  zustand@^4.4.7 \
  react-hook-form@^7.49.0 \
  recharts@^2.10.0 \
  @headlessui/react@^1.7.17 \
  @heroicons/react@^2.1.1 \
  date-fns@^3.0.0 \
  clsx@^2.0.0

# Development dependencies
npm install --save-dev \
  typescript@^5.3.0 \
  @types/react@^18.2.0 \
  @types/react-dom@^18.2.0 \
  vite@^5.0.0 \
  @vitejs/plugin-react@^4.2.0 \
  tailwindcss@^3.4.0 \
  postcss@^8.4.32 \
  autoprefixer@^10.4.16 \
  vitest@^1.0.0 \
  jsdom@^23.0.0 \
  @testing-library/react@^14.1.0 \
  @testing-library/jest-dom@^6.1.5 \
  @testing-library/user-event@^14.5.1
```

---

## 🔧 Optional Dependencies

### Alternative State Management (jika tidak pakai Zustand)
```bash
npm install @reduxjs/toolkit@^2.0.1 react-redux@^9.0.4
```

### API Client (untuk future integration)
```bash
npm install axios@^1.6.2
# atau
npm install @tanstack/react-query@^5.14.2
```

### Form Validation
```bash
npm install zod@^3.22.4 @hookform/resolvers@^3.3.3
```
- **zod**: Schema validation library
- **@hookform/resolvers**: Integrates Zod with react-hook-form

### Advanced Charts (Optional)
```bash
npm install chart.js@^4.4.1 react-chartjs-2@^5.2.0
```

### WebSocket (untuk real-time features nanti)
```bash
npm install socket.io-client@^4.6.0
```

---

## 📦 Package.json Reference

**File**: `package.json`

```json
{
  "name": "admin-dashboard",
  "private": true,
  "version": "1.0.0",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "tsc && vite build",
    "build:dev": "vite build --mode development",
    "build:prod": "vite build --mode production",
    "preview": "vite preview",
    "test": "vitest",
    "test:ui": "vitest --ui",
    "lint": "eslint . --ext ts,tsx",
    "format": "prettier --write \"src/**/*.{ts,tsx,css}\""
  },
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-router-dom": "^6.20.0",
    "zustand": "^4.4.7",
    "react-hook-form": "^7.49.0",
    "recharts": "^2.10.0",
    "@headlessui/react": "^1.7.17",
    "@heroicons/react": "^2.1.1",
    "date-fns": "^3.0.0",
    "clsx": "^2.0.0"
  },
  "devDependencies": {
    "@types/react": "^18.2.0",
    "@types/react-dom": "^18.2.0",
    "@typescript-eslint/eslint-plugin": "^6.15.0",
    "@typescript-eslint/parser": "^6.15.0",
    "@vitejs/plugin-react": "^4.2.0",
    "autoprefixer": "^10.4.16",
    "eslint": "^8.55.0",
    "jsdom": "^23.0.0",
    "postcss": "^8.4.32",
    "tailwindcss": "^3.4.0",
    "typescript": "^5.3.0",
    "vite": "^5.0.0",
    "vitest": "^1.0.0",
    "@testing-library/react": "^14.1.0",
    "@testing-library/jest-dom": "^6.1.5",
    "@testing-library/user-event": "^14.5.1"
  }
}
```

---

## 🎯 Dependency Categories

### Must Have (Core)
- ✅ react, react-dom
- ✅ react-router-dom
- ✅ zustand
- ✅ tailwindcss
- ✅ vite
- ✅ typescript

### Dashboard Essentials
- ✅ recharts (charts)
- ✅ @heroicons/react (icons)
- ✅ @headlessui/react (UI components)
- ✅ react-hook-form (forms)

### Development Tools
- ✅ vitest (testing)
- ✅ @testing-library/react
- ✅ eslint (code quality)

### Nice to Have (Optional)
- 🔹 @tanstack/react-query (API state)
- 🔹 axios (HTTP client)
- 🔹 zod (validation)
- 🔹 prettier (formatting)

---

## 🚨 Common Issues & Solutions

### Issue: Peer dependency warnings
```bash
npm install --legacy-peer-deps
```

### Issue: TypeScript errors with libraries
```bash
npm install --save-dev @types/[library-name]
```

### Issue: Vite not found
```bash
npm install -g vite
# or
npx vite
```

---

## 📊 Bundle Size Considerations

**Approximate sizes:**
- react + react-dom: ~140 KB
- react-router-dom: ~10 KB
- recharts: ~450 KB (largest!)
- zustand: ~1 KB (tiny!)
- @headlessui/react: ~25 KB
- @heroicons/react: ~20 KB per import
- tailwindcss: ~10 KB (after purge)

**Total estimated bundle:** ~650-700 KB (gzipped: ~200 KB)

---

## 🔄 Update Dependencies

```bash
# Check outdated packages
npm outdated

# Update all to latest
npm update

# Update specific package
npm update react

# Update to latest major version
npm install react@latest
```

---

## 📝 Notes

- **Version pinning**: Use `^` untuk minor updates, `~` untuk patch updates
- **Lock file**: Commit `package-lock.json` ke Git
- **Clean install**: `npm ci` untuk production (faster & reliable)
- **Audit**: Run `npm audit` regularly untuk security issues

---
