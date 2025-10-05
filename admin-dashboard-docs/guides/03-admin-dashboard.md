# SuperClaude Frontend - Admin Dashboard Guide (Part 3)

## 📊 Admin Dashboard (Platform-wide)

> **Part 3**: Admin Dashboard Implementation
> 
> **See also**: 
> - [Part 1](./01-setup-and-layout.md) - Setup & Layout
> - [Part 2](./02-operator-dashboard.md) - Operator Admin Dashboard
> - [Part 4](./04-superadmin-dashboard.md) - Super Admin Dashboard
> - [Part 5](./05-core-dummy-data.md) - Dummy Data Structures
> - [Part 6](./06-dashboard-data.md) - Dashboard Data
> - [Part 7](./07-chart-components.md) - Charts Library
> - [Part 8](./08-testing-deployment.md) - Deployment

---

## 🎨 Admin Dashboard Overview

### 1. Home / Dashboard Landing

**Route**: `/admin/dashboard`

**Purpose**: Platform-wide monitoring untuk semua operator

**Dashboard Layout:**
```
+---------------------------+---------------------------+
|  💰 Platform Revenue      |  🏢 Active Operators      |
|  Rp 450,000,000           |  48 / 50                  |
|  ↑ 15% vs last month      |  ↑ 5 new | 3 pending      |
+---------------------------+---------------------------+
|  👥 Total Users           |  📋 Total Bookings        |
|  12,450                   |  15,234                   |
|  ↑ 20% | 12K customers    |  ↑ 12% | 507/day avg      |
+---------------------------+---------------------------+
|        Platform Revenue Trend (Last 90 Days)          |
|  Multi-line: Total | Commission | Operator Net         |
+-------------------------------------------------------+
|  Operator Growth         |  Booking Distribution     |
|  Bar Chart (6 months)    |  Pie Chart (by operator)  |
+---------------------------+---------------------------+
|  Top Performing Operators (Top 10)                    |
|  Operator | Revenue | Bookings | Buses | Growth       |
+-------------------------------------------------------+
|  Pending Approvals (3)   |  Recent Activities (10)   |
|  - PT Transport Baru     |  - Operator X approved    |
|  - CV Bus Jaya           |  - 1,234 bookings today   |
+---------------------------+---------------------------+
```

---

## 2. Platform KPI Metrics

### 2.1 Total Platform Revenue 💰

```typescript
interface PlatformRevenue {
  current: number;          // 450,000,000
  previous: number;         // 391,000,000
  change: number;           // 15%
  commission: number;       // 45,000,000 (10%)
  operatorNet: number;      // 405,000,000 (90%)
}
```

**Display:**
- Main value: Rp 450 Jt
- Change: ↑ 15% vs last month
- Subtitle: Commission Rp 45 Jt (10%)
- Color: Blue
- Icon: Currency

---

### 2.2 Active Operators 🏢

```typescript
interface OperatorMetric {
  active: number;           // 48
  total: number;            // 50
  newThisMonth: number;     // 5
  pending: number;          // 3
  suspended: number;        // 2
  totalBuses: number;       // 350
}
```

**Display:**
- Main value: 48 / 50
- Subtitle: 3 pending approval
- Badge: "5 new"
- Color: Green
- Click: Navigate to operator list

---

### 2.3 Total Users 👥

```typescript
interface UserMetric {
  current: number;          // 12,450
  previous: number;         // 10,375
  change: number;           // 20%
  breakdown: {
    customers: number;      // 12,000
    operators: number;      // 400
    admins: number;         // 50
  }
}
```

**Display:**
- Main value: 12,450
- Change: ↑ 20%
- Subtitle: 12K customers | 400 operators
- Color: Purple

---

### 2.4 Total Bookings 📋

```typescript
interface PlatformBooking {
  current: number;          // 15,234
  previous: number;         // 13,602
  change: number;           // 12%
  averagePerDay: number;    // 507
  successRate: number;      // 92%
}
```

**Display:**
- Main value: 15,234
- Change: ↑ 12%
- Subtitle: Avg 507/day | 92% success
- Color: Orange

---

## 3. Platform Charts

### 3.1 Platform Revenue Trend (Multi-line)

**Component**: `PlatformRevenueChart.tsx`

```typescript
interface RevenueChartData {
  date: string;             // '2024-01-01'
  totalRevenue: number;     // 14,500,000
  commission: number;       // 1,450,000
  operatorNet: number;      // 13,050,000
}
```

**Chart Features:**
- 3 lines: Total, Commission, Operator Net
- Last 90 days
- Interactive tooltip
- Date range selector
- Export to CSV

**Lines:**
- Total Revenue: Blue (#3b82f6)
- Commission: Green (#10b981)
- Operator Net: Orange (#f59e0b)

---

### 3.2 Operator Growth (Bar Chart)

**Component**: `OperatorGrowthChart.tsx`

```typescript
interface OperatorGrowth {
  month: string;            // 'Jan 2024'
  new: number;              // 5
  suspended: number;        // 1
  total: number;            // 48
}
```

**Chart Features:**
- Stacked bars: New (green) + Suspended (red)
- Line overlay: Total active
- Last 6 months
- Click bar to filter operators

---

### 3.3 Booking Distribution (Pie Chart)

**Component**: `BookingDistributionChart.tsx`

```typescript
interface OperatorBookingShare {
  operatorId: string;
  operatorName: string;
  bookings: number;
  percentage: number;
  revenue: number;
}
```

**Chart Features:**
- Top 10 operators
- Others grouped as "Others"
- Click to view operator details
- Donut chart style
- Legend with percentages

---

### 3.4 Popular Routes (Bar Chart)

**Component**: `PopularRoutesChart.tsx`

```typescript
interface PopularRoute {
  route: string;            // 'Jakarta - Bandung'
  bookings: number;         // 2,340
  revenue: number;          // 58,500,000
  occupancy: number;        // 78%
  operatorCount: number;    // 12 (operators serving)
}
```

**Features:**
- Top 10 routes platform-wide
- Horizontal bars
- Color by occupancy level
- Show metrics on hover

---

## 4. Top Operators Table

**Component**: `TopOperatorsTable.tsx`

```typescript
interface TopOperator {
  rank: number;
  id: string;
  name: string;
  revenue: number;
  bookings: number;
  activeBuses: number;
  occupancy: number;
  growth: number;           // % vs last period
  status: 'active' | 'suspended';
}
```

**Table Columns:**
1. Rank (1-10)
2. Operator Name (link)
3. Revenue (formatted)
4. Bookings
5. Active Buses
6. Occupancy %
7. Growth % (↑ or ↓)
8. Actions (View, Suspend)

**Features:**
- Sortable columns
- Click row → Operator details page
- Color coding for performance
- Export to Excel
- "View All" button

---

## 5. Pending Approvals Widget

**Component**: `PendingApprovalsWidget.tsx`

```typescript
interface PendingOperator {
  id: string;
  name: string;
  email: string;
  phone: string;
  appliedDate: string;
  busesPlanned: number;
  documentsStatus: 'complete' | 'incomplete';
}
```

**Display:**
```
Pending Approvals (3)

1. PT Transport Baru
   Applied: 2024-01-10
   Buses: 15 planned
   [Review] [Approve] [Reject]

2. CV Bus Jaya
   Applied: 2024-01-12
   Buses: 8 planned
   [Review] [Approve] [Reject]

[View All Pending →]
```

**Features:**
- List of pending operators
- Quick approve/reject actions
- Click to view full application
- Badge for incomplete documents
- Notifications for new applications

---

## 6. Recent Activities Feed

**Component**: `RecentActivitiesWidget.tsx`

```typescript
interface Activity {
  id: string;
  type: 'operator_approved' | 'operator_suspended' | 
        'user_registered' | 'booking_milestone';
  message: string;
  timestamp: string;
  userId?: string;
  userName?: string;
  metadata?: any;
}
```

**Activity Types:**
- Operator approved
- Operator suspended
- Large booking created
- Revenue milestone reached
- New user registered
- System alert

**Display:**
```
Recent Activities

🟢 Operator PT Bus Jaya approved
   by Admin User | 2 hours ago

🔴 Operator CV Transport suspended
   by Admin User | 5 hours ago

👤 1,234 new users registered today
   Platform milestone | Today

💰 Platform revenue reached Rp 1B
   Monthly milestone | Today

[View All Activities →]
```

---

## 7. Quick Stats Cards (Additional)

**Optional metrics to show:**

### Revenue Metrics:
- Commission Rate (avg): 10%
- Revenue per Booking: Rp 29,500
- Revenue per Operator: Rp 9.4 Jt

### User Metrics:
- Active Users (30d): 8,450
- New Users Today: 45
- Customer Retention: 78%

### Booking Metrics:
- Cancellation Rate: 8%
- Average Booking Value: Rp 185K
- Peak Booking Hour: 08:00-10:00

---

## 8. Filter & Date Range

**Component**: `AdminDashboardFilters.tsx`

**Filters Available:**
1. **Date Range**
   - Last 7 days
   - Last 30 days
   - Last 90 days (default)
   - This month
   - Last month
   - Custom range

2. **Operator Filter**
   - All operators (default)
   - Specific operator
   - Operator status (active/suspended)

3. **Metric View**
   - Revenue focus
   - Booking focus
   - User focus
   - Comprehensive (default)

---

## 9. Export & Reports

**Component**: `AdminReportGenerator.tsx`

**Report Types:**
1. **Platform Summary Report**
   - All KPIs
   - Charts & graphs
   - Top performers
   - Period: Selectable

2. **Operator Performance Report**
   - Per operator breakdown
   - Revenue & bookings
   - Growth analysis

3. **Financial Report**
   - Commission breakdown
   - Revenue by operator
   - Payment status

4. **User Analytics Report**
   - User growth
   - Registration trends
   - Demographics

**Export Formats:**
- PDF (formatted report)
- Excel (raw data)
- CSV (data export)

---

## 10. Additional Admin Pages

### 10.1 Operator Management
**Route**: `/admin/operators`

**Features:**
- List all operators
- Filter by status
- Approve/reject
- Suspend/activate
- View operator details

### 10.2 User Management
**Route**: `/admin/users`

**Features:**
- List all users
- Filter by role
- View user activity
- Suspend users
- Reset passwords

### 10.3 Platform Analytics
**Route**: `/admin/analytics`

**Features:**
- Detailed analytics
- Custom reports
- Data visualization
- Export capabilities

### 10.4 Financial Dashboard
**Route**: `/admin/financial`

**Features:**
- Revenue breakdown
- Commission tracking
- Refunds
- Transactions
- Reconciliation

---

## Continue to Part 4

**Next in Part 4:**
- Super Admin Dashboard (System-level)
- System health monitoring
- Security & audit features

**See**: [Part 4 - Super Admin Dashboard](./04-superadmin-dashboard.md)