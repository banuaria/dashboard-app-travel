# SuperClaude Frontend - Admin Dashboard Guide

## 🎯 Overview
Dashboard admin untuk bus travel booking platform dengan 3 role:
- **Operator Admin**: Kelola bus & schedule operator sendiri
- **Admin**: Kelola semua operator & monitoring platform
- **Super Admin**: System configuration & platform management

**Note**: Menggunakan dummy data dulu, belum konek API backend.

---

## 🎨 Phase 1: Dashboard Setup

### 1. Project Architecture Design
```
/sc:design --persona-frontend
```

Design dashboard admin architecture dengan focus pada:

**Role-Based Dashboard:**
- 3-tier role system (Operator Admin, Admin, Super Admin)
- Role-based routing & navigation
- Permission-based component visibility
- Dummy data structure yang mirip dengan API response nantinya

**Tech Stack:**
- React 18+ dengan TypeScript
- Vite untuk build tool
- React Router untuk routing
- Tailwind CSS untuk styling
- Zustand/Redux Toolkit untuk state management (simple)
- React Hook Form untuk forms
- Recharts untuk analytics charts

**Dashboard Features per Role:**

**Operator Admin:**
- Fleet management (bus list, add/edit bus)
- Schedule management
- Booking list (operator scope)
- Analytics dashboard (operator metrics)

**Admin:**
- All operators overview
- User management
- Platform analytics
- Operator approval

**Super Admin:**
- System configuration
- Role management
- Audit trail
- Platform health monitoring

---

### 2. Project Setup & Structure
```
/sc:build --persona-frontend
```

Generate project structure:

```
admin-dashboard/
├── src/
│   ├── components/
│   │   ├── common/           # Shared components
│   │   │   ├── Sidebar.tsx
│   │   │   ├── Header.tsx
│   │   │   ├── Card.tsx
│   │   │   └── Table.tsx
│   │   ├── operator/         # Operator admin components
│   │   │   ├── BusManagement.tsx
│   │   │   ├── ScheduleManager.tsx
│   │   │   └── OperatorAnalytics.tsx
│   │   ├── admin/            # Admin components
│   │   │   ├── OperatorList.tsx
│   │   │   ├── UserManagement.tsx
│   │   │   └── PlatformAnalytics.tsx
│   │   └── super-admin/      # Super admin components
│   │       ├── SystemConfig.tsx
│   │       ├── RoleManagement.tsx
│   │       └── AuditTrail.tsx
│   ├── pages/
│   │   ├── Login.tsx
│   │   ├── operator/
│   │   ├── admin/
│   │   └── super-admin/
│   ├── data/                 # Dummy data
│   │   ├── buses.ts
│   │   ├── schedules.ts
│   │   ├── bookings.ts
│   │   ├── operators.ts
│   │   └── users.ts
│   ├── hooks/                # Custom hooks
│   │   ├── useAuth.ts
│   │   └── useRole.ts
│   ├── types/                # TypeScript interfaces
│   │   ├── bus.ts
│   │   ├── booking.ts
│   │   ├── user.ts
│   │   └── auth.ts
│   ├── utils/                # Utilities
│   │   └── rbac.ts
│   └── App.tsx
```

**Dependencies:**
```json
{
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
    "typescript": "^5.3.0",
    "vite": "^5.0.0",
    "tailwindcss": "^3.4.0"
  }
}
```

---

### 3. Authentication & Role Setup (Simple)
```
/sc:build --persona-frontend
```

Implement simple authentication dengan role switching:

**Login Component:**
- Username/password form
- Role selector (untuk testing)
- Dummy authentication (hardcoded users)

**Dummy Users:**
```typescript
const DUMMY_USERS = [
  {
    id: '1',
    email: 'operator@example.com',
    password: 'password',
    role: 'operator_admin',
    name: 'Operator Admin',
    operatorId: 'op-001'
  },
  {
    id: '2',
    email: 'admin@example.com',
    password: 'password',
    role: 'admin',
    name: 'Platform Admin'
  },
  {
    id: '3',
    email: 'superadmin@example.com',
    password: 'password',
    role: 'super_admin',
    name: 'Super Admin'
  }
];
```

**Auth Store (Zustand):**
```typescript
interface AuthState {
  user: User | null;
  role: Role | null;
  login: (email: string, password: string) => void;
  logout: () => void;
  switchRole: (role: Role) => void; // For testing
}
```

**Protected Routes:**
- Route guards based on role
- Redirect to login if not authenticated
- Redirect to appropriate dashboard based on role

---

## 🎨 Phase 2: Dashboard Development

### 4. Operator Admin Dashboard
```
/sc:build --persona-frontend
```

Build operator admin interface dengan dummy data:

#### 4.1 Bus Management
**Components:**
- `BusList.tsx` - Table of buses
- `BusForm.tsx` - Add/Edit bus modal
- `BusDetails.tsx` - View bus details

**Dummy Data Structure:**
```typescript
interface Bus {
  id: string;
  operatorId: string;
  plateNumber: string;
  busType: 'economy' | 'business' | 'executive';
  totalSeats: number;
  seatLayout: {
    rows: number;
    columns: number;
    layout: string; // e.g., "2-2" or "2-3"
  };
  facilities: string[];
  status: 'active' | 'maintenance' | 'inactive';
  createdAt: string;
}
```

**Features:**
- View bus list (filtered by operator)
- Add new bus
- Edit bus details
- Change bus status
- View seat configuration

#### 4.2 Schedule Management
**Components:**
- `ScheduleList.tsx`
- `ScheduleForm.tsx`
- `ScheduleCalendar.tsx`

**Dummy Data Structure:**
```typescript
interface Schedule {
  id: string;
  operatorId: string;
  busId: string;
  routeId: string;
  departureTime: string;
  arrivalTime: string;
  price: number;
  availableSeats: number;
  status: 'scheduled' | 'departed' | 'arrived' | 'cancelled';
  date: string;
}
```

**Features:**
- View schedule calendar
- Create new schedule
- Edit schedule
- Cancel schedule
- View seat availability

#### 4.3 Operator Bookings
**Components:**
- `BookingList.tsx`
- `BookingDetails.tsx`

**Dummy Data Structure:**
```typescript
interface Booking {
  id: string;
  scheduleId: string;
  customerId: string;
  customerName: string;
  seatNumbers: string[];
  totalPrice: number;
  status: 'pending' | 'confirmed' | 'cancelled';
  paymentStatus: 'pending' | 'paid' | 'refunded';
  bookingDate: string;
}
```

**Features:**
- View bookings (operator scope)
- Filter by date, status
- View booking details
- Cancel booking

#### 4.4 Operator Analytics
**Components:**
- `AnalyticsDashboard.tsx`
- `RevenueChart.tsx`
- `OccupancyChart.tsx`

**Metrics:**
- Total revenue (this month)
- Total bookings
- Average occupancy rate
- Popular routes
- Revenue trend (chart)

---

### 5. Admin Dashboard
```
/sc:build --persona-frontend
```

Build admin interface untuk platform management:

#### 5.1 Operator Management
**Components:**
- `OperatorList.tsx`
- `OperatorDetails.tsx`
- `OperatorApproval.tsx`

**Dummy Data Structure:**
```typescript
interface Operator {
  id: string;
  name: string;
  email: string;
  phone: string;
  totalBuses: number;
  status: 'active' | 'pending' | 'suspended';
  joinDate: string;
  commission: number;
}
```

**Features:**
- View all operators
- Approve/reject operator
- Suspend operator
- View operator details & statistics

#### 5.2 User Management
**Components:**
- `UserList.tsx`
- `UserDetails.tsx`

**Features:**
- View all users (customers, operators, admins)
- Filter by role
- View user activity
- Suspend/activate user

#### 5.3 Platform Analytics
**Components:**
- `PlatformDashboard.tsx`
- Analytics charts untuk:
  - Total revenue (all operators)
  - Total bookings
  - Active operators
  - Customer growth
  - Popular routes across platform

---

### 6. Super Admin Dashboard
```
/sc:build --persona-frontend
```

Build super admin interface:

#### 6.1 System Configuration
**Components:**
- `SystemSettings.tsx`
- `PaymentConfig.tsx`
- `NotificationSettings.tsx`

**Features:**
- Platform settings
- Payment gateway config
- Email templates
- Notification preferences

#### 6.2 Role Management
**Components:**
- `RoleList.tsx`
- `RoleForm.tsx`
- `PermissionMatrix.tsx`

**Features:**
- View all roles
- Create/edit roles
- Assign permissions
- View permission matrix

#### 6.3 Audit Trail
**Components:**
- `AuditLog.tsx`
- `ActivityTimeline.tsx`

**Dummy Data Structure:**
```typescript
interface AuditLog {
  id: string;
  userId: string;
  userName: string;
  action: string;
  resource: string;
  timestamp: string;
  ipAddress: string;
  details: any;
}
```

---

## 🎨 Phase 3: UI/UX Components & Layout

### 7. Dashboard Layout System
```
/sc:build --persona-frontend
```

Build comprehensive dashboard layout dengan sidebar, header, dan footer:

#### 7.1 Main Layout Structure

**DashboardLayout.tsx** - Main wrapper component:
```typescript
interface DashboardLayoutProps {
  children: React.ReactNode;
}

// Layout structure:
// +------------------+
// |     Header       |
// +------+-----------+
// | Side |  Content  |
// | bar  |           |
// |      |           |
// +------+-----------+
// |     Footer       |
// +------------------+
```

**Features:**
- Responsive layout (mobile-first)
- Collapsible sidebar
- Sticky header
- Fixed footer
- Content area dengan proper spacing
- Dark mode support

---

#### 7.2 Sidebar Component

**Sidebar.tsx** - Navigation sidebar dengan role-based menu:

**Design Specifications:**
- Width: 256px (desktop), full width (mobile)
- Background: Gradient atau solid color
- Logo placement di top
- Scrollable menu items
- Active state indicators
- Collapse/expand functionality
- User info di bottom sidebar

**Sidebar Structure:**
```
+------------------+
|      LOGO        |
|   (App Name)     |
+------------------+
|                  |
|  📊 Dashboard    |  <- Active
|  🚌 Buses        |
|  📅 Schedules    |
|  📋 Bookings     |
|  📈 Analytics    |
|                  |
|  ... (role menu) |
|                  |
+------------------+
|   👤 User Info   |
|   Logout Button  |
+------------------+
```

**Navigation Menu by Role:**

**Operator Admin:**
- 🏠 **Home / Dashboard** ← Landing page dengan summary
- 🚌 Bus Management
  - All Buses
  - Add New Bus
  - Maintenance Schedule
- 📅 Schedule Management
  - All Schedules
  - Create Schedule
  - Calendar View
- 📋 Bookings
  - All Bookings
  - Pending Bookings
  - Cancelled Bookings
- 📈 Analytics
  - Revenue Reports
  - Occupancy Stats
  - Route Performance
- ⚙️ Settings

**Admin:**
- 🏠 **Home / Dashboard** ← Landing page dengan summary
- 🏢 Operators
  - All Operators
  - Pending Approvals
  - Suspended
- 👥 Users
  - All Users
  - Customers
  - Operators
  - Admins
- 📈 Platform Analytics
  - Revenue Overview
  - Growth Metrics
  - Popular Routes
- 💰 Financial
  - Transactions
  - Commissions
  - Refunds
- 📊 Reports
- ⚙️ Settings

**Super Admin:**
- 🏠 **Home / Dashboard** ← Landing page dengan summary
- 🏢 Operators (same as Admin)
- 👥 Users (same as Admin)
- 📈 Analytics (same as Admin)
- 🔐 Role Management
  - All Roles
  - Permissions
  - Create Role
- ⚙️ System Config
  - Payment Gateway
  - Email Settings
  - Notifications
  - Feature Flags
- 📜 Audit Trail
  - System Logs
  - User Activity
  - Security Events
- 🛠️ Advanced Settings

**Sidebar Features:**
- Icons menggunakan Heroicons
- Nested menu (expandable)
- Active route highlighting
- Hover effects
- Badge untuk notifications (e.g., "5 pending")
- Search menu (optional)
- Keyboard navigation support

---

#### 7.3 Header Component

**Header.tsx** - Top navigation bar:

**Design Specifications:**
- Height: 64px
- Sticky position
- Box shadow untuk depth
- Backdrop blur effect (modern look)

**Header Structure:**
```
+----------------------------------------------------------+
| ☰ Toggle | Breadcrumb > Path    |  🔔 🌙 👤 | Logout  |
+----------------------------------------------------------+
```

**Header Elements:**

1. **Menu Toggle (Mobile)**
   - Hamburger icon
   - Toggles sidebar on mobile
   - Hidden on desktop

2. **Breadcrumb Navigation**
   - Dynamic path (e.g., Dashboard > Buses > Edit)
   - Clickable links
   - Current page bold/different color

3. **Search Bar (Optional)**
   - Global search functionality
   - Keyboard shortcut (Cmd/Ctrl + K)
   - Search across buses, schedules, bookings

4. **Right Section:**
   - **Notifications Bell** 🔔
     - Dropdown with notifications list
     - Unread count badge
     - Mark as read functionality
     - "View all" link
   
   - **Dark Mode Toggle** 🌙
     - Sun/Moon icon
     - Smooth transition
     - Persistent preference
   
   - **User Profile Dropdown** 👤
     - User avatar (or initials)
     - User name & role
     - Dropdown menu:
       - View Profile
       - Settings
       - Help & Support
       - Logout

**Notification Types:**
- New bookings
- Payment confirmations
- Schedule changes
- System alerts
- Role-specific notifications

---

#### 7.4 Footer Component

**Footer.tsx** - Bottom information bar:

**Design Specifications:**
- Height: auto (min 60px)
- Background: subtle gray/dark
- Border top untuk separation

**Footer Structure:**
```
+----------------------------------------------------------+
|  © 2024 BusApp  |  v1.0.0  |  Privacy  |  Terms         |
|  Contact: support@busapp.com  |  Status: ✓ All Systems  |
+----------------------------------------------------------+
```

**Footer Elements:**

1. **Left Section:**
   - Copyright text
   - App version
   - Quick links (Privacy Policy, Terms of Service)

2. **Center Section (Optional):**
   - Last sync time
   - Server status indicator
   - Quick stats

3. **Right Section:**
   - Contact email/support link
   - Social media icons (optional)
   - System status (🟢 Online, 🔴 Issues)

**Footer Variants:**

**Simple Footer:**
```typescript
<footer className="border-t bg-gray-50 dark:bg-gray-900">
  <div className="px-6 py-4 flex justify-between items-center">
    <p className="text-sm text-gray-600">
      © 2024 BusApp. All rights reserved.
    </p>
    <div className="flex gap-4 text-sm">
      <a href="#">Privacy</a>
      <a href="#">Terms</a>
      <a href="#">Support</a>
    </div>
  </div>
</footer>
```

**Detailed Footer:**
- Multiple columns
- More links
- Newsletter signup (optional)
- Social media

---

#### 7.5 Content Area

**ContentArea.tsx** - Main content wrapper:

**Features:**
- Proper padding & spacing
- Max-width constraints
- Responsive grid system
- Smooth page transitions
- Loading skeletons

**Content Layout Patterns:**

1. **Dashboard Grid:**
```
+---------------------------+---------------------------+
|     KPI Card 1            |       KPI Card 2          |
+---------------------------+---------------------------+
|     KPI Card 3            |       KPI Card 4          |
+---------------------------+---------------------------+
|              Chart / Graph                            |
+-------------------------------------------------------+
|              Recent Activity Table                    |
+-------------------------------------------------------+
```

2. **List Page:**
```
+-------------------------------------------------------+
|  Page Title  |  Search  |  Filters  |  Add Button    |
+-------------------------------------------------------+
|                   Data Table                          |
|  Columns: Name | Status | Date | Actions              |
+-------------------------------------------------------+
|                   Pagination                          |
+-------------------------------------------------------+
```

3. **Detail Page:**
```
+-------------------------------------------------------+
|  < Back  |  Title                        | Edit       |
+-------------------------------------------------------+
|  Info Section 1           |  Info Section 2          |
+-------------------------------------------------------+
|                Related Data / Activity                |
+-------------------------------------------------------+
```

---

#### 7.6 Additional Layout Components

**PageHeader.tsx:**
- Page title
- Action buttons
- Tabs navigation
- Filters/search

**Breadcrumb.tsx:**
- Dynamic breadcrumb trail
- Clickable navigation
- Icons for each level

**Card.tsx:**
- Consistent card styling
- Header, body, footer sections
- Variants (default, bordered, shadow)

**EmptyState.tsx:**
- No data illustrations
- CTA buttons
- Helpful messages

---

### 8. Common UI Components
```
/sc:build --persona-frontend
```

Build reusable UI components:

#### 8.1 Data Display Components

**Table.tsx** - Advanced data table:
- Sortable columns
- Pagination
- Row selection
- Action buttons per row
- Empty state
- Loading skeleton
- Responsive (horizontal scroll on mobile)
- Export functionality (CSV, PDF)

**Card.tsx** - Metric/Info cards:
- Title, value, icon
- Trend indicators (↑↓)
- Different variants (primary, success, warning, danger)
- Loading state
- Click action support

**Badge.tsx** - Status badges:
- Color variants (success, warning, danger, info)
- Sizes (sm, md, lg)
- Dot indicator option
- Pill/rounded styles

**Stats.tsx** - Statistics display:
- Large number display
- Label
- Change percentage
- Icon
- Comparison (vs last period)

#### 8.2 Form Components

**Input.tsx:**
- Text, number, email, password
- Label & helper text
- Error states
- Icons (prefix/suffix)
- Disabled state

**Select.tsx:**
- Dropdown select
- Multi-select
- Search/filter
- Custom option rendering
- Async options

**DatePicker.tsx:**
- Single date
- Date range
- Time picker
- Calendar view
- Preset ranges (Today, This Week, etc.)

**FileUpload.tsx:**
- Drag & drop
- Multiple files
- File type validation
- Size limit
- Preview thumbnails
- Progress bar

**SearchBar.tsx:**
- Debounced search
- Clear button
- Search icon
- Loading indicator
- Suggestions dropdown

**Toggle.tsx:**
- On/off switch
- Label support
- Disabled state

**Radio/Checkbox:**
- Single/multiple selection
- Label support
- Helper text
- Error states

#### 8.3 Feedback Components

**Modal.tsx:**
- Overlay backdrop
- Close button
- Header, body, footer
- Sizes (sm, md, lg, xl, full)
- Centered/top positioning
- Scrollable content
- Keyboard ESC to close

**Toast/Notification.tsx:**
- Success, error, warning, info
- Auto-dismiss
- Manual close
- Position (top-right, bottom-right, etc.)
- Action buttons
- Progress bar

**ConfirmDialog.tsx:**
- Title & message
- Confirm/cancel buttons
- Destructive actions (red confirm button)
- Loading state during action

**Loading.tsx:**
- Spinner
- Skeleton screens
- Progress bars
- Full page loader
- Inline loaders

**Tooltip.tsx:**
- Hover tooltip
- Position (top, bottom, left, right)
- Delay options

**Dropdown.tsx:**
- Menu items
- Dividers
- Icons
- Keyboard navigation
- Click outside to close

#### 8.4 Chart Components

**LineChart.tsx:**
- Time series data
- Multiple lines
- Tooltip on hover
- Legend
- Responsive

**BarChart.tsx:**
- Vertical/horizontal
- Grouped/stacked
- Color customization

**PieChart.tsx:**
- Donut variant
- Labels
- Percentage display

**AreaChart.tsx:**
- Gradient fill
- Stacked areas
- Comparison charts

---

### 9. Design System & Styling
```
/sc:build --persona-frontend
```

**Color Palette:**
```css
/* Primary Colors */
--primary-50: #eff6ff;
--primary-500: #3b82f6;
--primary-600: #2563eb;
--primary-700: #1d4ed8;

/* Success, Warning, Error */
--success-500: #10b981;
--warning-500: #f59e0b;
--error-500: #ef4444;

/* Neutral/Gray */
--gray-50: #f9fafb;
--gray-100: #f3f4f6;
--gray-500: #6b7280;
--gray-900: #111827;
```

**Typography:**
- Font: Inter or System UI
- Headings: Bold, larger sizes
- Body: Regular, 14-16px
- Small text: 12-14px

**Spacing System:**
- 4px base unit
- Padding: 16px, 24px, 32px
- Margins: 8px, 16px, 24px, 32px

**Border Radius:**
- Small: 4px
- Medium: 8px
- Large: 12px
- Full: 9999px (pills)

**Shadows:**
- sm: subtle
- md: standard
- lg: prominent
- xl: dramatic

**Animations:**
- Transitions: 150-300ms
- Hover effects
- Loading states
- Page transitions

---

## 🎨 Phase 4: Dummy Data Management

### 8. Create Comprehensive Dummy Data
```
/sc:build --persona-frontend
```

Generate realistic dummy data:

**data/buses.ts:**
```typescript
export const DUMMY_BUSES: Bus[] = [
  {
    id: 'bus-001',
    operatorId: 'op-001',
    plateNumber: 'B 1234 ABC',
    busType: 'executive',
    totalSeats: 40,
    seatLayout: { rows: 10, columns: 4, layout: '2-2' },
    facilities: ['AC', 'WiFi', 'USB Charger', 'Toilet'],
    status: 'active',
    createdAt: '2024-01-15'
  },
  // ... more buses
];
```

**data/schedules.ts:**
- 50+ schedules untuk berbagai routes
- Different dates & times
- Various operators

**data/bookings.ts:**
- 100+ bookings
- Different statuses
- Spread across operators

**data/operators.ts:**
- 10+ operators
- Various statuses

**data/users.ts:**
- Customers, operators, admins
- Different roles & statuses

---

## 📊 Phase 5: Charts & Analytics

### 9. Analytics Implementation
```
/sc:build --persona-frontend
```

Implement charts dengan Recharts:

**Chart Components:**
- `LineChart` - Revenue trends
- `BarChart` - Bookings per route
- `PieChart` - Booking status distribution
- `AreaChart` - Occupancy rates

**Dashboard Metrics:**
- KPI cards (total revenue, bookings, etc.)
- Trend indicators (↑ 12% from last month)
- Comparative analytics
- Time-range filters (today, week, month, year)

---

## 🎨 Phase 6: Polish & Refinement

### 10. Responsive Design
```
/sc:build --persona-frontend
```

Ensure mobile responsiveness:
- Mobile-friendly sidebar (drawer)
- Responsive tables
- Touch-friendly buttons
- Mobile charts

### 11. Dark Mode (Optional)
```
/sc:build --persona-frontend
```

Implement dark mode toggle:
- Dark color scheme
- Persistent preference
- Smooth transitions

---

## 🚀 Deployment

### 12. Build & Deploy
```
/sc:deploy --persona-frontend
```

**Build Configuration:**
```bash
npm run build
```

**Deployment Options:**
- Vercel (recommended for quick deployment)
- Netlify
- GitHub Pages

**Environment Variables:**
```
VITE_APP_NAME=Bus Admin Dashboard
VITE_APP_VERSION=1.0.0
```

---

## 📋 Development Checklist

### Phase 1: Setup
- [ ] Project initialization
- [ ] Tailwind configuration
- [ ] Router setup
- [ ] Auth system (simple)

### Phase 2: Operator Dashboard
- [ ] Bus management
- [ ] Schedule management
- [ ] Booking list
- [ ] Analytics dashboard

### Phase 3: Admin Dashboard
- [ ] Operator management
- [ ] User management
- [ ] Platform analytics

### Phase 4: Super Admin Dashboard
- [ ] System configuration
- [ ] Role management
- [ ] Audit trail

### Phase 5: Common Components
- [ ] Layout components
- [ ] Form components
- [ ] Data display components
- [ ] Charts

### Phase 6: Dummy Data
- [ ] Generate comprehensive dummy data
- [ ] Test all CRUD operations
- [ ] Verify role-based filtering

### Phase 7: Polish
- [ ] Responsive design
- [ ] Loading states
- [ ] Error handling
- [ ] Dark mode (optional)

### Phase 8: Deployment
- [ ] Build optimization
- [ ] Deploy to Vercel/Netlify

---

## 🔄 Future: API Integration

Ketika backend sudah siap, tinggal:
1. Replace dummy data dengan API calls
2. Add axios/fetch untuk HTTP requests
3. Implement loading & error states
4. Add real-time updates (WebSocket)

**API Service Structure (nanti):**
```typescript
// services/api.ts
export const busService = {
  getAll: () => api.get('/buses'),
  create: (data) => api.post('/buses', data),
  update: (id, data) => api.put(`/buses/${id}`, data),
  delete: (id) => api.delete(`/buses/${id}`)
};
```

---

## 📝 Quick Start Commands

```bash
# 1. Create project
npm create vite@latest admin-dashboard -- --template react-ts

# 2. Install dependencies
cd admin-dashboard
npm install react-router-dom zustand react-hook-form recharts
npm install @headlessui/react @heroicons/react date-fns clsx
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p

# 3. Run development server
npm run dev

# 4. Build for production
npm run build
```

---

## 🎯 Summary

**What we're building:**
- Admin dashboard dengan 3 roles
- Dummy data untuk testing
- Full CRUD operations
- Analytics & charts
- Responsive design

**What we're NOT including (yet):**
- Customer-facing UI
- Real API integration
- Payment processing
- Real-time features (WebSocket)

**Next steps after dummy data:**
- Integrate dengan backend API
- Add real-time updates
- Implement payment flow
- Build customer UI (separate project)