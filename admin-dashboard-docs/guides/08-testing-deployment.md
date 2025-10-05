# SuperClaude Frontend - Admin Dashboard Guide (Part 8)

## 🧪 Testing & Deployment

> **Part 8**: Testing, Build & Deployment (Final)
> 
> **See also**: [Part 1-7](./01-setup-and-layout.md) - Previous guides

---

## 🧪 Testing Strategy

### 1. Component Testing Setup

**Install Testing Libraries:**
```bash
npm install --save-dev @testing-library/react @testing-library/jest-dom @testing-library/user-event vitest jsdom
```

**Configure Vitest:**

**File**: `vite.config.ts`
```typescript
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

export default defineConfig({
  plugins: [react()],
  test: {
    globals: true,
    environment: 'jsdom',
    setupFiles: './src/test/setup.ts',
  },
});
```

**File**: `src/test/setup.ts`
```typescript
import '@testing-library/jest-dom';
```

---

### 2. Example Tests

#### StatCard Component Test:
**File**: `src/components/common/__tests__/StatCard.test.tsx`

```typescript
import { render, screen } from '@testing-library/react';
import { describe, it, expect } from 'vitest';
import { StatCard } from '../StatCard';

describe('StatCard', () => {
  it('renders title and value correctly', () => {
    render(
      <StatCard
        title="Total Revenue"
        value="Rp 45,000,000"
        change={12.5}
        trend="up"
        color="blue"
      />
    );
    
    expect(screen.getByText('Total Revenue')).toBeInTheDocument();
    expect(screen.getByText('Rp 45,000,000')).toBeInTheDocument();
  });

  it('displays trend indicator', () => {
    const { container } = render(
      <StatCard
        title="Test"
        value="100"
        change={12.5}
        trend="up"
        color="blue"
      />
    );
    
    expect(screen.getByText(/12.5%/)).toBeInTheDocument();
  });
});
```

#### Dashboard Data Test:
**File**: `src/data/__tests__/helpers.test.ts`

```typescript
import { describe, it, expect } from 'vitest';
import { formatCurrency, calculateChange, getTrend } from '../helpers';

describe('Data Helpers', () => {
  describe('formatCurrency', () => {
    it('formats millions correctly', () => {
      expect(formatCurrency(45000000)).toBe('Rp 45.0 Jt');
    });

    it('formats thousands correctly', () => {
      expect(formatCurrency(500000)).toBe('Rp 500 Rb');
    });
  });

  describe('calculateChange', () => {
    it('calculates positive change', () => {
      expect(calculateChange(120, 100)).toBe(20);
    });

    it('calculates negative change', () => {
      expect(calculateChange(80, 100)).toBe(-20);
    });
  });

  describe('getTrend', () => {
    it('returns up for positive change', () => {
      expect(getTrend(10)).toBe('up');
    });

    it('returns down for negative change', () => {
      expect(getTrend(-10)).toBe('down');
    });
  });
});
```

**Run Tests:**
```bash
npm run test
```

---

## 🏗️ Build Configuration

### 1. Environment Variables

**File**: `.env.example`
```env
VITE_APP_NAME=Bus Admin Dashboard
VITE_APP_VERSION=1.0.0
VITE_API_URL=http://localhost:8080/api/v1
VITE_WS_URL=ws://localhost:8080/ws
```

**File**: `.env.development`
```env
VITE_APP_NAME=Bus Admin Dashboard (Dev)
VITE_API_URL=http://localhost:8080/api/v1
```

**File**: `.env.production`
```env
VITE_APP_NAME=Bus Admin Dashboard
VITE_API_URL=https://api.busapp.com/api/v1
VITE_WS_URL=wss://api.busapp.com/ws
```

**Usage:**
```typescript
const API_URL = import.meta.env.VITE_API_URL;
```

---

### 2. Build Scripts

**File**: `package.json`
```json
{
  "scripts": {
    "dev": "vite",
    "build": "tsc && vite build",
    "build:dev": "vite build --mode development",
    "build:prod": "vite build --mode production",
    "preview": "vite preview",
    "test": "vitest",
    "test:ui": "vitest --ui",
    "lint": "eslint . --ext ts,tsx --report-unused-disable-directives --max-warnings 0"
  }
}
```

---

### 3. Tailwind Production Config

**File**: `tailwind.config.js`
```javascript
/** @type {import('tailwindcss').Config} */
export default {
  content: [
    "./index.html",
    "./src/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {
      colors: {
        primary: {
          50: '#eff6ff',
          500: '#3b82f6',
          600: '#2563eb',
          700: '#1d4ed8',
        },
      },
    },
  },
  plugins: [],
}
```

---

## 🚀 Deployment

### 1. Vercel Deployment (Recommended)

**Step 1**: Install Vercel CLI
```bash
npm install -g vercel
```

**Step 2**: Login
```bash
vercel login
```

**Step 3**: Deploy
```bash
vercel --prod
```

**File**: `vercel.json`
```json
{
  "buildCommand": "npm run build",
  "outputDirectory": "dist",
  "devCommand": "npm run dev",
  "installCommand": "npm install",
  "framework": "vite",
  "rewrites": [
    { "source": "/(.*)", "destination": "/" }
  ]
}
```

---

### 2. Netlify Deployment

**File**: `netlify.toml`
```toml
[build]
  command = "npm run build"
  publish = "dist"

[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200
```

**Deploy via CLI:**
```bash
npm install -g netlify-cli
netlify login
netlify deploy --prod
```

---

### 3. Docker Deployment

**File**: `Dockerfile`
```dockerfile
# Build stage
FROM node:18-alpine AS builder

WORKDIR /app

COPY package*.json ./
RUN npm ci

COPY . .
RUN npm run build

# Production stage
FROM nginx:alpine

COPY --from=builder /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

**File**: `nginx.conf`
```nginx
server {
    listen 80;
    server_name localhost;
    root /usr/share/nginx/html;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }

    # Cache static assets
    location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg)$ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }
}
```

**Build & Run:**
```bash
docker build -t admin-dashboard .
docker run -p 3000:80 admin-dashboard
```

---

## ⚡ Performance Optimization

### 1. Code Splitting

```typescript
// Lazy load pages
import { lazy, Suspense } from 'react';

const OperatorDashboard = lazy(() => import('./pages/operator/Dashboard'));
const AdminDashboard = lazy(() => import('./pages/admin/Dashboard'));
const SuperAdminDashboard = lazy(() => import('./pages/super-admin/Dashboard'));

// Usage with loading fallback
<Suspense fallback={<LoadingSpinner />}>
  <OperatorDashboard />
</Suspense>
```

---

### 2. Asset Optimization

**File**: `vite.config.ts`
```typescript
export default defineConfig({
  build: {
    rollupOptions: {
      output: {
        manualChunks: {
          'vendor': ['react', 'react-dom', 'react-router-dom'],
          'charts': ['recharts'],
          'ui': ['@headlessui/react', '@heroicons/react'],
        },
      },
    },
    minify: 'terser',
    terserOptions: {
      compress: {
        drop_console: true,
      },
    },
  },
});
```

---

### 3. Image Optimization

```typescript
// Use webp format
<img 
  src="/images/logo.webp" 
  alt="Logo"
  loading="lazy"
  width={200}
  height={50}
/>
```

---

## 📋 Production Checklist

### Pre-Deployment:
- [ ] Remove console.logs
- [ ] Test all roles (Operator, Admin, Super Admin)
- [ ] Verify all charts render correctly
- [ ] Test responsive design (mobile, tablet, desktop)
- [ ] Check loading states
- [ ] Verify error handling
- [ ] Test with dummy data
- [ ] Check accessibility (keyboard navigation)
- [ ] Optimize images
- [ ] Enable compression

### Post-Deployment:
- [ ] Test production build locally
- [ ] Verify environment variables
- [ ] Check SSL certificate
- [ ] Test all routes
- [ ] Monitor performance (Lighthouse)
- [ ] Set up error tracking (Sentry)
- [ ] Configure analytics
- [ ] Test API integration (when ready)

---

## 📊 Development vs Production

### Development:
```bash
npm run dev
# Access: http://localhost:5173
# Features: Hot reload, source maps, detailed errors
```

### Production Build:
```bash
npm run build
npm run preview
# Access: http://localhost:4173
# Features: Minified, optimized, production-ready
```

---

## 🔄 Future API Integration

When backend is ready, replace dummy data:

**Current (Dummy):**
```typescript
import { OPERATOR_DASHBOARD_DATA } from '@/data/dashboards/operator';
const data = OPERATOR_DASHBOARD_DATA;
```

**Future (API):**
```typescript
import { useQuery } from '@tanstack/react-query';
import { fetchOperatorDashboard } from '@/services/api';

const { data, isLoading } = useQuery({
  queryKey: ['operatorDashboard'],
  queryFn: fetchOperatorDashboard,
});
```

---

## 🎉 Summary

Congratulations! You now have:

✅ **Complete Dashboard** with 3 roles
✅ **Dummy Data** for testing
✅ **Chart Components** (Line, Bar, Pie, Area)
✅ **Responsive Design** (Mobile & Desktop)
✅ **Testing Setup** (Vitest + React Testing Library)
✅ **Deployment Ready** (Vercel, Netlify, Docker)
✅ **Performance Optimized** (Code splitting, lazy loading)

### Next Steps:
1. Customize styling to match brand
2. Add more features as needed
3. Integrate with backend API
4. Add real-time updates
5. Implement notifications
6. Add export/print features

---

## 📚 Resources

- [React Documentation](https://react.dev)
- [Vite Guide](https://vitejs.dev/guide/)
- [Tailwind CSS](https://tailwindcss.com/docs)
- [Recharts Documentation](https://recharts.org/)
- [React Router](https://reactrouter.com/)
- [Zustand](https://github.com/pmndrs/zustand)

---

## 🎯 Quick Commands Reference

```bash
# Development
npm run dev                 # Start dev server
npm run test               # Run tests
npm run test:ui            # Run tests with UI

# Build
npm run build              # Production build
npm run build:dev          # Development build
npm run preview            # Preview production build

# Deployment
vercel --prod              # Deploy to Vercel
netlify deploy --prod      # Deploy to Netlify
docker build -t app .      # Build Docker image
```

---

**Happy Coding! 🚀**

> You've completed all 8 parts of the Admin Dashboard Guide!