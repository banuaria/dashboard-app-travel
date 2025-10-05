# ⚡ Quick Commands Reference

## Command cheatsheet untuk development

---

## 🚀 Project Setup

### Create New Project
```bash
# Create Vite + React + TypeScript
npm create vite@latest admin-dashboard -- --template react-ts

# Navigate to project
cd admin-dashboard

# Install dependencies
npm install
```

### Install All Dependencies at Once
```bash
# Production deps
npm install react react-dom react-router-dom zustand react-hook-form recharts @headlessui/react @heroicons/react date-fns clsx

# Dev deps
npm install -D typescript @types/react @types/react-dom vite @vitejs/plugin-react tailwindcss postcss autoprefixer vitest jsdom @testing-library/react @testing-library/jest-dom @testing-library/user-event
```

### Initialize Tailwind
```bash
npx tailwindcss init -p
```

---

## 💻 Development

### Start Dev Server
```bash
npm run dev
# Access: http://localhost:5173
```

### Run with Different Port
```bash
npm run dev -- --port 3000
```

### Open in Browser Automatically
```bash
npm run dev -- --open
```

---

## 🧪 Testing

### Run All Tests
```bash
npm run test
```

### Run Tests in Watch Mode
```bash
npm run test -- --watch
```

### Run Tests with UI
```bash
npm run test:ui
```

### Run Tests with Coverage
```bash
npm run test -- --coverage
```

### Run Specific Test File
```bash
npm run test src/components/StatCard.test.tsx
```

---

## 🏗️ Building

### Build for Production
```bash
npm run build
```

### Build for Development (with source maps)
```bash
npm run build:dev
```

### Preview Production Build
```bash
npm run preview
# Access: http://localhost:4173
```

### Build and Preview
```bash
npm run build && npm run preview
```

---

## 🎨 Code Quality

### Run Linter
```bash
npm run lint
```

### Fix Linting Issues
```bash
npm run lint -- --fix
```

### Format Code with Prettier
```bash
npm run format
```

### Check TypeScript Errors
```bash
npx tsc --noEmit
```

---

## 📦 Package Management

### Install Single Package
```bash
npm install package-name
```

### Install Dev Dependency
```bash
npm install -D package-name
```

### Uninstall Package
```bash
npm uninstall package-name
```

### Update All Packages
```bash
npm update
```

### Check Outdated Packages
```bash
npm outdated
```

### Clean Install (CI/CD)
```bash
npm ci
```

### Audit Security Issues
```bash
npm audit

# Auto fix
npm audit fix
```

---

## 🚢 Deployment

### Vercel
```bash
# Install Vercel CLI
npm install -g vercel

# Login
vercel login

# Deploy to preview
vercel

# Deploy to production
vercel --prod
```

### Netlify
```bash
# Install Netlify CLI
npm install -g netlify-cli

# Login
netlify login

# Deploy
netlify deploy

# Deploy to production
netlify deploy --prod
```

### Docker
```bash
# Build image
docker build -t admin-dashboard .

# Run container
docker run -p 3000:80 admin-dashboard

# Build and run
docker build -t admin-dashboard . && docker run -p 3000:80 admin-dashboard
```

---

## 🧹 Cleanup

### Clear Node Modules
```bash
rm -rf node_modules
npm install
```

### Clear Build Files
```bash
rm -rf dist
```

### Clear All Cache
```bash
rm -rf node_modules dist .vite package-lock.json
npm install
```

### Clear Vite Cache
```bash
rm -rf node_modules/.vite
```

---

## 🔍 Debugging

### Check Vite Config
```bash
npx vite --config
```

### Analyze Bundle Size
```bash
npm run build -- --mode analyze
```

### Check Environment Variables
```bash
# In your component
console.log(import.meta.env)
```

---

## 📊 Git Commands (Quick Reference)

### Initial Setup
```bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin <repo-url>
git push -u origin main
```

### Daily Workflow
```bash
# Pull latest changes
git pull

# Create feature branch
git checkout -b feature/dashboard

# Stage and commit
git add .
git commit -m "Add operator dashboard"

# Push to remote
git push origin feature/dashboard
```

---

## 🎯 Common Task Shortcuts

### Full Reset & Reinstall
```bash
rm -rf node_modules package-lock.json dist && npm install && npm run dev
```

### Quick Test & Build
```bash
npm run test && npm run build
```

### Format, Lint, Test, Build
```bash
npm run format && npm run lint && npm run test && npm run build
```

### Deploy to Production (Vercel)
```bash
npm run build && vercel --prod
```

---

## 📝 File & Folder Operations

### Create Component Structure
```bash
mkdir -p src/components/operator
touch src/components/operator/Dashboard.tsx
```

### Create Multiple Files
```bash
touch src/data/{users,operators,buses,routes,schedules,bookings}.ts
```

### Create Test File
```bash
touch src/components/__tests__/StatCard.test.tsx
```

---

## 🔧 Tailwind Commands

### Generate Tailwind Config
```bash
npx tailwindcss init
```

### Generate with PostCSS
```bash
npx tailwindcss init -p
```

### Full Config (uncommented)
```bash
npx tailwindcss init --full
```

---

## 🎨 Component Generation (Manual)

### Create Component with Test
```bash
# Create component
touch src/components/MyComponent.tsx

# Create test
touch src/components/__tests__/MyComponent.test.tsx

# Create styles (if needed)
touch src/components/MyComponent.module.css
```

---

## 🚀 Performance Commands

### Measure Build Time
```bash
time npm run build
```

### Check Bundle Size
```bash
npm run build && du -sh dist
```

### Lighthouse CI
```bash
npx lighthouse http://localhost:4173 --view
```

---

## 🐛 Troubleshooting Commands

### Clear npm Cache
```bash
npm cache clean --force
```

### Verify npm Cache
```bash
npm cache verify
```

### Reinstall Dependencies
```bash
rm -rf node_modules package-lock.json
npm install
```

### Fix Permissions Issues (Mac/Linux)
```bash
sudo chown -R $USER ~/.npm
```

### Check Node & npm Versions
```bash
node -v
npm -v
```

---

## 📋 Quick Copy-Paste Commands

### Setup New Feature Branch
```bash
git checkout -b feature/new-feature && git push -u origin feature/new-feature
```

### Update Main Branch
```bash
git checkout main && git pull origin main
```

### Clean & Rebuild
```bash
rm -rf node_modules dist && npm install && npm run build
```

### Test & Preview
```bash
npm run test && npm run build && npm run preview
```

---

## 🎯 VSCode Integrated Terminal Tips

### Split Terminal
```
Ctrl + Shift + 5 (or Cmd + Shift + 5 on Mac)
```

### Run Multiple Commands
```bash
# Terminal 1: Dev server
npm run dev

# Terminal 2: Tests
npm run test -- --watch

# Terminal 3: Type checking
npx tsc --noEmit --watch
```

---

## 💡 Pro Tips

1. **Use npm scripts** instead of remembering long commands
2. **Create aliases** in your shell config:
   ```bash
   alias nrd="npm run dev"
   alias nrb="npm run build"
   alias nrt="npm run test"
   ```

3. **Use npx** for one-time commands instead of global installs

4. **Check package.json scripts** untuk custom commands

---