# 🤖 Prompt Templates untuk Claude

## Copy-paste templates untuk development yang efisien

---

## 📋 Template Format

```
Context: [Situasi saat ini]
File: [File guide yang relevan]
Task: [Apa yang diminta]
Requirements: [Detail spesifik]
```

---

## 🚀 Phase 1: Project Setup

### 1.1 Initial Project Setup
```
Context: Saya ingin membuat admin dashboard untuk bus booking platform dengan 3 role (Operator Admin, Admin, Super Admin).

File: guides/01-setup-and-layout.md

Task: Setup project React + TypeScript + Vite sesuai panduan.

Requirements:
- Buat project dengan Vite template react-ts
- Install semua dependencies dari file dependencies.md
- Setup Tailwind CSS
- Buat folder structure sesuai guides/01-setup-and-layout.md section 2
```

### 1.2 Tailwind Configuration
```
Context: Project sudah di-setup, butuh konfigurasi Tailwind.

File: guides/01-setup-and-layout.md section 9 (Design System)

Task: Setup Tailwind dengan design system yang sudah ditentukan.

Requirements:
- Configure tailwind.config.js dengan color palette
- Setup PostCSS
- Tambahkan design tokens (spacing, border radius, shadows)
- Configure content paths
```

---

## 🎨 Phase 2: Layout Components

### 2.1 Sidebar Component
```
Context: Butuh sidebar navigation dengan role-based menu.

File: guides/01-setup-and-layout.md section 5

Task: Buatkan component Sidebar.tsx dengan navigation menu untuk 3 role.

Requirements:
- Width 256px (desktop), full width (mobile)
- Logo di top
- Role-based menu (Operator Admin, Admin, Super Admin)
- Active state highlighting
- Collapsible untuk mobile
- User info di bottom dengan logout button
- Gunakan @heroicons/react untuk icons
- TypeScript interfaces
```

### 2.2 Header Component
```
Context: Dashboard butuh header dengan breadcrumb dan user actions.

File: guides/01-setup-and-layout.md section 6

Task: Buatkan component Header.tsx.

Requirements:
- Height 64px, sticky position
- Menu toggle (mobile only)
- Breadcrumb navigation
- Search bar (optional)
- Notifications bell dengan dropdown
- Dark mode toggle
- User profile dropdown
- Responsive design
```

### 2.3 Footer Component
```
Context: Butuh footer untuk dashboard layout.

File: guides/01-setup-and-layout.md section 7

Task: Buatkan component Footer.tsx.

Requirements:
- Copyright text
- App version
- Quick links (Privacy, Terms, Support)
- System status indicator
- Simple & clean design
```

### 2.4 Dashboard Layout
```
Context: Butuh main layout yang combine Sidebar, Header, Footer.

File: guides/01-setup-and-layout.md section 4

Task: Buatkan DashboardLayout.tsx component.

Requirements:
- Sidebar (left, 256px)
- Header (top, 64px, sticky)
- Content area (center, scrollable)
- Footer (bottom)
- Responsive (sidebar collapse di mobile)
- Props untuk children components
```

---

## 📊 Phase 3: Operator Dashboard

### 3.1 Operator Dashboard Page
```
Context: Butuh halaman dashboard untuk Operator Admin dengan KPI dan charts.

File: 
- guides/02-operator-dashboard.md section 1
- guides/06-dashboard-data.md

Task: Buatkan OperatorDashboard.tsx page.

Requirements:
- Layout grid 2x2 untuk KPI cards
- Revenue trend chart (line)
- Occupancy by route chart (bar)
- Booking status chart (pie)
- Recent bookings table
- Quick actions buttons
- Time period filter
- Gunakan dummy data dari OPERATOR_DASHBOARD_DATA
```

### 3.2 StatCard Component
```
Context: Dashboard butuh KPI metric cards yang reusable.

File: guides/02-operator-dashboard.md section 1.1

Task: Buatkan StatCard.tsx component.

Requirements:
- Props: title, value, change, trend, icon, color, subtitle
- Show trend indicator dengan arrow (↑ up, ↓ down)
- Color variants: blue, green, purple, orange, red
- Responsive card design
- TypeScript interface StatCardProps
- Tailwind CSS styling
```

### 3.3 Recent Bookings Table
```
Context: Dashboard perlu tabel untuk menampilkan booking terbaru.

File: guides/02-operator-dashboard.md section 1.3

Task: Buatkan RecentBookingsTable.tsx component.

Requirements:
- Columns: Booking ID, Customer, Route, Date & Time, Seats, Amount, Status
- Status badge dengan warna (pending: yellow, confirmed: green, cancelled: red)
- Last 10 bookings
- Click row untuk view details
- "View All" button
- Empty state
- Responsive table (horizontal scroll di mobile)
```

### 3.4 Quick Actions
```
Context: Dashboard butuh tombol untuk aksi cepat.

File: guides/02-operator-dashboard.md section 1.4

Task: Buatkan QuickActions.tsx component.

Requirements:
- 4 action buttons: Add Bus, Create Schedule, View Bookings, Generate Report
- Icon untuk setiap button (Heroicons)
- Color coding per action
- Grid layout 2x2
- Click handlers dengan navigation
```

---

## 📈 Phase 4: Chart Components

### 4.1 Line Chart Component
```
Context: Butuh reusable line chart untuk revenue trend.

File: guides/07-chart-components.md section 1

Task: Buatkan LineChartComponent.tsx dengan Recharts.

Requirements:
- Props: data, lines, xAxisKey, height, formatters
- Support multiple lines
- Custom xAxis & yAxis formatters
- Responsive container
- Interactive tooltip & legend
- TypeScript interfaces
- Default height 300px
```

### 4.2 Bar Chart Component
```
Context: Butuh bar chart untuk occupancy by route.

File: guides/07-chart-components.md section 2

Task: Buatkan BarChartComponent.tsx.

Requirements:
- Props: data, bars, xAxisKey, height, customColors
- Support custom color per bar (untuk occupancy level)
- Responsive container
- Rotated xAxis labels (-45 degree)
- TypeScript interfaces
```

### 4.3 Pie Chart Component
```
Context: Butuh pie/donut chart untuk booking status.

File: guides/07-chart-components.md section 3

Task: Buatkan PieChartComponent.tsx.

Requirements:
- Props: data, height, innerRadius
- Support donut chart (innerRadius > 0)
- Show percentage labels
- Custom colors per segment
- Interactive tooltip & legend
- TypeScript interfaces
```

### 4.4 Chart Card Wrapper
```
Context: Chart components butuh wrapper card untuk consistency.

File: guides/07-chart-components.md section 7

Task: Buatkan ChartCard.tsx component.

Requirements:
- Props: title, children, action
- Card styling (white bg, shadow, rounded)
- Title di header
- Optional action button (e.g., Export)
- Padding untuk content
```

---

## 💾 Phase 5: Dummy Data

### 5.1 Core Dummy Data
```
Context: Butuh dummy data untuk testing semua fitur dashboard.

File: guides/05-core-dummy-data.md

Task: Generate semua dummy data files.

Requirements:
- users.ts: DUMMY_USERS + generateCustomers(100)
- operators.ts: DUMMY_OPERATORS (10 operators)
- buses.ts: DUMMY_BUSES + generateBusesForOperator()
- routes.ts: DUMMY_ROUTES (popular routes Indonesia)
- schedules.ts: generateSchedules(100)
- bookings.ts: generateBookings(500)
- helpers.ts: formatCurrency, formatDate, calculateChange, getTrend
- Semua dengan TypeScript interfaces
```

### 5.2 Dashboard Data
```
Context: Butuh dashboard-specific data untuk metrics dan charts.

File: guides/06-dashboard-data.md

Task: Generate dashboard data files.

Requirements:
- dashboards/operator.ts: OPERATOR_DASHBOARD_DATA
- dashboards/admin.ts: ADMIN_DASHBOARD_DATA
- dashboards/superadmin.ts: SUPER_ADMIN_DASHBOARD_DATA
- Helper functions: generateRevenueChartData(), generatePlatformRevenueData()
- Export getDashboardData(role) function
```

---

## 🎨 Phase 6: Admin Dashboard

### 6.1 Admin Dashboard Page
```
Context: Butuh dashboard untuk Platform Admin dengan platform-wide metrics.

File: 
- guides/03-admin-dashboard.md section 1
- guides/06-dashboard-data.md

Task: Buatkan AdminDashboard.tsx page.

Requirements:
- Platform KPI cards (Revenue, Operators, Users, Bookings)
- Platform revenue trend chart (3 lines: total, commission, operator net)
- Operator growth chart (bar)
- Booking distribution chart (pie)
- Top operators table
- Pending approvals widget
- Recent activities feed
- Gunakan ADMIN_DASHBOARD_DATA
```

### 6.2 Top Operators Table
```
Context: Admin dashboard butuh table untuk top performing operators.

File: guides/03-admin-dashboard.md section 4

Task: Buatkan TopOperatorsTable.tsx component.

Requirements:
- Columns: Rank, Name, Revenue, Bookings, Buses, Occupancy, Growth, Actions
- Top 10 operators
- Sortable columns
- Click row untuk operator details
- Color coding untuk performance
- "View All" button
- Export to Excel button
```

### 6.3 Pending Approvals Widget
```
Context: Admin butuh widget untuk approve operator baru.

File: guides/03-admin-dashboard.md section 5

Task: Buatkan PendingApprovalsWidget.tsx.

Requirements:
- List pending operators
- Show: Name, Applied date, Buses planned
- Quick actions: Review, Approve, Reject
- Badge untuk incomplete documents
- "View All Pending" link
- Empty state jika tidak ada pending
```

---

## 🔐 Phase 7: Super Admin Dashboard

### 7.1 Super Admin Dashboard Page
```
Context: Butuh dashboard untuk Super Admin dengan system monitoring.

File:
- guides/04-superadmin-dashboard.md section 1
- guides/06-dashboard-data.md

Task: Buatkan SuperAdminDashboard.tsx page.

Requirements:
- System health metrics
- Security score card
- System performance chart (4 lines: CPU, Memory, API, Disk)
- Storage usage donut chart
- Audit logs table (critical actions only)
- Active sessions list
- System alerts
- Gunakan SUPER_ADMIN_DASHBOARD_DATA
```

### 7.2 Audit Logs Table
```
Context: Super Admin butuh audit trail untuk critical actions.

File: guides/04-superadmin-dashboard.md section 4.1

Task: Buatkan AuditLogsTable.tsx component.

Requirements:
- Columns: Timestamp, User, Action, Resource, IP, Status, Details
- Last 20 critical actions
- Filters: Time range, User/Role, Action type, Status
- Action icons (Login, Created, Updated, Deleted, etc)
- Expandable details
- Status indicators (✅ success, ❌ failed)
- Export logs functionality
```

### 7.3 System Health Widget
```
Context: Super Admin butuh monitoring untuk system health.

File: guides/04-superadmin-dashboard.md section 5.1

Task: Buatkan ServiceHealthWidget.tsx.

Requirements:
- List all services (Database, API, Payment, Email, Storage, WebSocket)
- Show status untuk each service (✅ operational, ⚠️ degraded, ❌ down)
- Uptime percentage
- Response time
- Last check timestamp
- Refresh button
```

---

## 🧪 Phase 8: Testing

### 8.1 Component Test
```
Context: Butuh unit test untuk StatCard component.

File: guides/08-testing-deployment.md section 2

Task: Buatkan StatCard.test.tsx dengan Vitest.

Requirements:
- Test rendering title & value
- Test trend indicator display
- Test color variants
- Test change percentage calculation
- Gunakan @testing-library/react
- Setup sesuai guides/08-testing-deployment.md
```

### 8.2 Helper Function Test
```
Context: Butuh test untuk data helper functions.

File: guides/08-testing-deployment.md section 2

Task: Buatkan helpers.test.ts untuk data helpers.

Requirements:
- Test formatCurrency() dengan berbagai input
- Test formatDate() dengan format options
- Test calculateChange()
- Test getTrend()
- Test getOccupancyColor()
- Gunakan Vitest
```

---

## 🚀 Phase 9: Integration & Polish

### 9.1 Routing Setup
```
Context: Butuh routing untuk semua pages dengan role-based access.

File: guides/01-setup-and-layout.md section 3

Task: Setup react-router-dom dengan protected routes.

Requirements:
- Public route: /login
- Operator routes: /operator/*
- Admin routes: /admin/*
- Super admin routes: /superadmin/*
- Protected routes dengan role check
- Redirect based on user role after login
- 404 page
```

### 9.2 Authentication Integration
```
Context: Integrate dummy authentication dengan routing.

File: 
- guides/01-setup-and-layout.md section 3
- guides/05-core-dummy-data.md (DUMMY_USERS)

Task: Setup authentication dengan Zustand store.

Requirements:
- authStore.ts dengan login, logout, switchRole
- Login page dengan form
- Role-based redirect after login
- Protected route component
- Logout functionality
- Persist auth state (localStorage)
```

---

## 🐛 Debugging Prompts

### Fix Error
```
Context: Saya mendapat error saat [describe action].

Error:
[paste error message]

File: [component/file name]

Code:
[paste relevant code snippet]

Task: Debug dan fix error ini.
```

### Optimize Performance
```
Context: Component [name] berjalan lambat saat [action].

File: [component file]

Current Code:
[paste code]

Task: Optimize performance sesuai guides/08-testing-deployment.md section "Performance Optimization".
```

### Style Fix
```
Context: Component [name] styling tidak sesuai design system.

File: 
- [component file]
- guides/01-setup-and-layout.md section 9

Task: Perbaiki styling sesuai design system (colors, spacing, shadows).
```

---

## 💡 Quick Prompts

### Quick Component
```
Buatkan component [ComponentName] sesuai guides/[file].md section [X].
Gunakan TypeScript, Tailwind CSS, dan format sesuai design system.
```

### Quick Integration
```
Integrate [ComponentA] dengan data dari guides/06-dashboard-data.md.
Format data menggunakan helpers dari guides/05-core-dummy-data.md.
```

### Quick Test
```
Buatkan unit test untuk [ComponentName] sesuai guides/08-testing-deployment.md.
Test: rendering, props, user interaction, edge cases.
```

---

## 📋 Checklist Sebelum Prompt

- [ ] Sudah baca file guide yang relevan?
- [ ] File reference jelas? (guides/XX-xxx.md section X)
- [ ] Requirements spesifik dan detail?
- [ ] Context dijelaskan (apa yang sudah ada)?
- [ ] Task fokus (tidak terlalu banyak sekaligus)?

---

**Pro Tip**: Copy template → Fill in the blanks → Paste ke Claude! 🚀