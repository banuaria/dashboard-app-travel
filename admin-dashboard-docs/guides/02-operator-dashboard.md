# SuperClaude Frontend - Admin Dashboard Guide (Part 2)

## 📊 Operator Admin Dashboard Details

> **Part 2**: Operator Admin Dashboard Implementation
> 
> **See also**: 
> - [Part 1](./01-setup-and-layout.md) - Setup & Layout
> - [Part 3](./03-admin-dashboard.md) - Admin & Super Admin Dashboard
> - [Part 4](./04-superadmin-dashboard.md) - Super Admin Dashboard
> - [Part 5](./05-core-dummy-data.md) - Dummy Data
> - [Part 6](./06-dashboard-data.md) - Dashboard Data
> - [Part 7](./07-chart-components.md) - Charts
> - [Part 8](./08-testing-deployment.md) - Testing & Deployment

---

## 🎨 Operator Admin Dashboard

### 1. Home / Dashboard Overview (Landing Page)

**Route**: `/operator/dashboard` atau `/operator/home`

**Components:**
- `OperatorDashboard.tsx` - Main dashboard page
- `StatCard.tsx` - KPI metric cards
- `RevenueChart.tsx` - Revenue trend chart
- `OccupancyChart.tsx` - Bus occupancy chart
- `RecentBookings.tsx` - Latest bookings table
- `QuickActions.tsx` - Quick action buttons

**Dashboard Layout:**
```
+---------------------------+---------------------------+
|  📊 Total Revenue         |  🚌 Active Buses         |
|  Rp 45,000,000            |  12 / 15                 |
|  ↑ 12% vs last month      |  ↑ 2 new this month      |
+---------------------------+---------------------------+
|  📋 Total Bookings        |  📈 Occupancy Rate       |
|  1,234                    |  78.5%                   |
|  ↑ 8% vs last month       |  ↑ 5% vs last month      |
+---------------------------+---------------------------+
|              Revenue Trend (Last 30 Days)             |
|                  Line Chart                           |
+-------------------------------------------------------+
|  Bus Occupancy by Route  |  Booking Status Breakdown |
|     Bar Chart             |     Pie Chart            |
+---------------------------+---------------------------+
|              Recent Bookings (Last 10)                |
|  Table: ID | Customer | Route | Date | Status         |
+-------------------------------------------------------+
|  Top Routes              |  Upcoming Schedules       |
|  1. Jakarta-Bandung      |  Today: 5 departures      |
|  2. Surabaya-Malang      |  Tomorrow: 8 departures   |
+---------------------------+---------------------------+
```

---

### 1.1 KPI Metrics (StatCard Components)

**StatCard Component Structure:**
```typescript
interface StatCardProps {
  title: string;
  value: string | number;
  change?: number;
  trend?: 'up' | 'down' | 'neutral';
  icon: React.ReactNode;
  color: 'blue' | 'green' | 'purple' | 'orange' | 'red';
  subtitle?: string;
}
```

#### Metric 1: Total Revenue 💰
```typescript
interface RevenueMetric {
  current: number;        // Rp 45,000,000
  previous: number;       // Rp 40,000,000
  change: number;         // 12.5%
  trend: 'up' | 'down';
}

// Example data
const revenueData = {
  title: 'Total Revenue',
  value: formatCurrency(45000000), // 'Rp 45 Jt'
  change: 12.5,
  trend: 'up',
  icon: <CurrencyDollarIcon />,
  color: 'blue',
  subtitle: 'vs last month'
}
```

**Card Design:**
```
+-------------------------+
| 💰                   ↗️ |
| Total Revenue           |
| Rp 45,000,000           |
| +12.5% vs last month    |
+-------------------------+
```

#### Metric 2: Active Buses 🚌
```typescript
interface BusMetric {
  active: number;         // 12
  total: number;          // 15
  newThisMonth: number;   // 2
}

const busData = {
  title: 'Active Buses',
  value: '12 / 15',
  change: 2,
  icon: <TruckIcon />,
  color: 'green',
  subtitle: '+2 new this month'
}
```

#### Metric 3: Total Bookings 📋
```typescript
interface BookingMetric {
  current: number;        // 1,234
  previous: number;       // 1,143
  change: number;         // 8%
  trend: 'up' | 'down';
}

const bookingData = {
  title: 'Total Bookings',
  value: '1,234',
  change: 8,
  trend: 'up',
  icon: <DocumentTextIcon />,
  color: 'purple',
  subtitle: 'vs last month'
}
```

#### Metric 4: Occupancy Rate 📈
```typescript
interface OccupancyMetric {
  current: number;        // 78.5%
  previous: number;       // 74.8%
  change: number;         // 5%
  trend: 'up' | 'down';
}

const occupancyData = {
  title: 'Occupancy Rate',
  value: '78.5%',
  change: 5,
  trend: 'up',
  icon: <ChartBarIcon />,
  color: 'orange',
  subtitle: 'Average across all buses'
}
```

---

### 1.2 Charts & Visualizations

#### Chart 1: Revenue Trend (Line Chart)

**Component**: `RevenueChart.tsx`

**Data Structure:**
```typescript
interface RevenueChartData {
  date: string;           // '2024-01-01'
  revenue: number;        // 1,200,000
  movingAvg?: number;     // 1,100,000 (7-day MA)
  target?: number;        // 1,000,000 (optional)
}

// Example data
const revenueChartData: RevenueChartData[] = [
  { date: '2024-01-01', revenue: 1200000, movingAvg: 1100000 },
  { date: '2024-01-02', revenue: 1500000, movingAvg: 1150000 },
  // ... 30 days
];
```

**Chart Configuration:**
```typescript
<ResponsiveContainer width="100%" height={300}>
  <LineChart data={revenueChartData}>
    <CartesianGrid strokeDasharray="3 3" />
    <XAxis 
      dataKey="date" 
      tickFormatter={(date) => formatDate(date, 'dd MMM')}
    />
    <YAxis 
      tickFormatter={(value) => formatCurrency(value)}
    />
    <Tooltip 
      formatter={(value) => formatCurrency(value)}
      labelFormatter={(date) => formatDate(date, 'dd MMMM yyyy')}
    />
    <Legend />
    <Line 
      type="monotone" 
      dataKey="revenue" 
      stroke="#3b82f6" 
      strokeWidth={2}
      name="Daily Revenue"
    />
    <Line 
      type="monotone" 
      dataKey="movingAvg" 
      stroke="#10b981" 
      strokeWidth={2}
      strokeDasharray="5 5"
      name="7-day Average"
    />
  </LineChart>
</ResponsiveContainer>
```

**Features:**
- X-axis: Last 30 days (formatted: "01 Jan")
- Y-axis: Revenue in Rupiah (formatted: "Rp 1.2 Jt")
- Tooltip shows: Date, exact amount, percentage change
- Interactive: Hover for details
- Responsive design

---

#### Chart 2: Bus Occupancy by Route (Bar Chart)

**Component**: `OccupancyChart.tsx`

**Data Structure:**
```typescript
interface OccupancyByRoute {
  route: string;          // 'Jakarta - Bandung'
  occupancy: number;      // 85 (percentage)
  bookings: number;       // 45
  color: string;          // Color based on occupancy level
}

// Example with color logic
const getOccupancyColor = (occupancy: number) => {
  if (occupancy >= 75) return '#10b981'; // Green
  if (occupancy >= 50) return '#f59e0b'; // Yellow
  return '#ef4444'; // Red
};

const occupancyData: OccupancyByRoute[] = [
  { 
    route: 'Jakarta - Bandung', 
    occupancy: 85, 
    bookings: 45,
    color: getOccupancyColor(85)
  },
  { 
    route: 'Surabaya - Malang', 
    occupancy: 78, 
    bookings: 38,
    color: getOccupancyColor(78)
  },
  // ... top 5-10 routes
].sort((a, b) => b.occupancy - a.occupancy); // Sort by occupancy
```

**Chart Configuration:**
```typescript
<ResponsiveContainer width="100%" height={300}>
  <BarChart data={occupancyData}>
    <CartesianGrid strokeDasharray="3 3" />
    <XAxis 
      dataKey="route" 
      angle={-45}
      textAnchor="end"
      height={100}
    />
    <YAxis 
      domain={[0, 100]}
      tickFormatter={(value) => `${value}%`}
    />
    <Tooltip 
      formatter={(value, name) => {
        if (name === 'occupancy') return `${value}%`;
        return value;
      }}
    />
    <Legend />
    <Bar dataKey="occupancy" name="Occupancy Rate">
      {occupancyData.map((entry, index) => (
        <Cell key={`cell-${index}`} fill={entry.color} />
      ))}
    </Bar>
  </BarChart>
</ResponsiveContainer>
```

**Features:**
- Shows top 5-10 routes
- Color gradient based on occupancy:
  - Red (<50%): Need improvement
  - Yellow (50-75%): Average
  - Green (>75%): Good
- Bar labels show exact percentage
- Sorted by occupancy (highest first)
- Click bar to filter bookings by route

---

#### Chart 3: Booking Status Breakdown (Pie/Donut Chart)

**Component**: `BookingStatusChart.tsx`

**Data Structure:**
```typescript
interface BookingStatus {
  status: 'confirmed' | 'pending' | 'cancelled' | 'completed';
  count: number;
  percentage: number;
  color: string;
}

const statusColors = {
  confirmed: '#10b981',   // Green
  pending: '#f59e0b',     // Yellow
  cancelled: '#ef4444',   // Red
  completed: '#3b82f6'    // Blue
};

const bookingStatusData: BookingStatus[] = [
  { status: 'confirmed', count: 865, percentage: 70, color: statusColors.confirmed },
  { status: 'pending', count: 185, percentage: 15, color: statusColors.pending },
  { status: 'cancelled', count: 123, percentage: 10, color: statusColors.cancelled },
  { status: 'completed', count: 61, percentage: 5, color: statusColors.completed },
];
```

**Chart Configuration:**
```typescript
<ResponsiveContainer width="100%" height={300}>
  <PieChart>
    <Pie
      data={bookingStatusData}
      cx="50%"
      cy="50%"
      labelLine={false}
      label={({ status, percentage }) => `${status}: ${percentage}%`}
      outerRadius={80}
      innerRadius={50} // Makes it a donut chart
      fill="#8884d8"
      dataKey="count"
    >
      {bookingStatusData.map((entry, index) => (
        <Cell key={`cell-${index}`} fill={entry.color} />
      ))}
    </Pie>
    <Tooltip 
      formatter={(value, name, props) => [
        `${value} bookings (${props.payload.percentage}%)`,
        props.payload.status
      ]}
    />
    <Legend />
  </PieChart>
</ResponsiveContainer>
```

**Features:**
- Donut chart (inner radius for better look)
- Center text: Total bookings count
- Legend with status labels
- Interactive: Click segment to filter bookings
- Percentage labels on each segment
- Tooltip shows count + percentage

---

#### Chart 4: Revenue by Route (Horizontal Bar Chart)

**Component**: `RevenueByRouteChart.tsx`

**Data Structure:**
```typescript
interface RevenueByRoute {
  route: string;
  revenue: number;
  bookings: number;
}

const revenueByRoute: RevenueByRoute[] = [
  { route: 'Jakarta - Bandung', revenue: 12500000, bookings: 45 },
  { route: 'Surabaya - Malang', revenue: 9800000, bookings: 38 },
  { route: 'Jakarta - Yogyakarta', revenue: 8500000, bookings: 32 },
  { route: 'Bandung - Surabaya', revenue: 7200000, bookings: 28 },
  { route: 'Semarang - Solo', revenue: 5800000, bookings: 22 },
];
```

**Features:**
- Top 5 revenue-generating routes
- Horizontal bars (better for long route names)
- Show amount in Rupiah
- Gradient color
- Click to view route details

---

### 1.3 Recent Activity Section

#### Recent Bookings Table

**Component**: `RecentBookingsTable.tsx`

**Data Structure:**
```typescript
interface RecentBooking {
  id: string;             // 'BK-001'
  customerName: string;   // 'John Doe'
  customerPhone: string;  // '+62812345678'
  route: string;          // 'Jakarta - Bandung'
  date: string;           // '2024-01-15'
  time: string;           // '08:00'
  seats: string[];        // ['A1', 'A2']
  amount: number;         // 250,000
  status: 'pending' | 'confirmed' | 'cancelled';
  createdAt: string;      // '2024-01-10 14:30'
}
```

**Table Columns:**
- Booking ID (link to details)
- Customer Name
- Route
- Date & Time
- Seats (badge count)
- Amount (formatted)
- Status (colored badge)
- Actions (View/Cancel)

**Table Features:**
- Last 10 bookings
- Sortable columns
- Click row → View booking details modal
- Status badge colors:
  - Pending: Yellow
  - Confirmed: Green
  - Cancelled: Red
- "View All" button → Redirect to Bookings page
- Empty state if no bookings

**Status Badge Component:**
```typescript
const StatusBadge = ({ status }: { status: string }) => {
  const colors = {
    pending: 'bg-yellow-100 text-yellow-800',
    confirmed: 'bg-green-100 text-green-800',
    cancelled: 'bg-red-100 text-red-800',
  };
  
  return (
    <span className={`px-2 py-1 rounded-full text-xs font-medium ${colors[status]}`}>
      {status.charAt(0).toUpperCase() + status.slice(1)}
    </span>
  );
};
```

---

#### Top Routes Section

**Component**: `TopRoutes.tsx`

**Data Structure:**
```typescript
interface TopRoute {
  route: string;
  revenue: number;
  bookings: number;
  occupancy: number;
  growth: number; // percentage vs last period
}

const topRoutes: TopRoute[] = [
  { 
    route: 'Jakarta - Bandung', 
    revenue: 12500000, 
    bookings: 45, 
    occupancy: 85,
    growth: 12
  },
  // ... top 5
];
```

**Display Format:**
```
Top Routes This Month
1. 🥇 Jakarta - Bandung
   Revenue: Rp 12.5 Jt | Bookings: 45 | Occupancy: 85%
   
2. 🥈 Surabaya - Malang
   Revenue: Rp 9.8 Jt | Bookings: 38 | Occupancy: 78%
   
3. 🥉 Jakarta - Yogyakarta
   Revenue: Rp 8.5 Jt | Bookings: 32 | Occupancy: 72%
```

**Features:**
- Medal icons for top 3 (🥇🥈🥉)
- Show all key metrics
- Click to view route analytics
- Growth indicator (↑ 12%)

---

#### Upcoming Schedules

**Component**: `UpcomingSchedules.tsx`

**Data Structure:**
```typescript
interface UpcomingSchedule {
  today: number;          // 5
  tomorrow: number;       // 8
  thisWeek: number;       // 35
  todaySchedules?: Schedule[]; // Detailed list (optional)
}
```

**Display Format:**
```
Upcoming Departures
📅 Today: 5 departures
📅 Tomorrow: 8 departures
📅 This Week: 35 departures

[View Schedule Calendar →]
```

**Features:**
- Quick summary counts
- Link to Schedule Calendar view
- Today's schedule list (expandable)
- Color indicators:
  - Green: On time
  - Yellow: Filling up
  - Red: Almost full

---

### 1.4 Quick Actions

**Component**: `QuickActions.tsx`

**Action Buttons:**
```typescript
const quickActions = [
  {
    label: 'Add New Bus',
    icon: <PlusIcon />,
    color: 'blue',
    onClick: () => navigate('/operator/buses/new')
  },
  {
    label: 'Create Schedule',
    icon: <CalendarIcon />,
    color: 'green',
    onClick: () => navigate('/operator/schedules/new')
  },
  {
    label: 'View Bookings',
    icon: <DocumentTextIcon />,
    color: 'purple',
    onClick: () => navigate('/operator/bookings')
  },
  {
    label: 'Generate Report',
    icon: <DocumentDownloadIcon />,
    color: 'orange',
    onClick: () => setShowReportModal(true)
  },
];
```

**Button Design:**
```
+--------------------+  +--------------------+
| ➕                  |  | 📅                  |
| Add New Bus        |  | Create Schedule    |
+--------------------+  +--------------------+
| 📋                  |  | 📊                  |
| View Bookings      |  | Generate Report    |
+--------------------+  +--------------------+
```

---

### 1.5 Time Period Filters

**Component**: `TimePeriodFilter.tsx`

**Filter Options:**
```typescript
interface TimePeriod {
  label: string;
  value: 'today' | 'week' | 'month' | 'last_month' | 'custom';
  days: number;
}

const timePeriods: TimePeriod[] = [
  { label: 'Today', value: 'today', days: 1 },
  { label: 'Last 7 Days', value: 'week', days: 7 },
  { label: 'Last 30 Days', value: 'month', days: 30 }, // Default
  { label: 'This Month', value: 'month', days: 30 },
  { label: 'Last Month', value: 'last_month', days: 30 },
  { label: 'Custom Range', value: 'custom', days: 0 },
];
```

**UI Design:**
- Dropdown select atau button group
- Default: "Last 30 Days"
- Custom range opens date picker (from - to)
- Applies filter to all charts & metrics
- Loading state during filter change

---

## Continue to Part 3

**Next in Part 3:**
- Admin Dashboard (Platform-wide)
- Super Admin Dashboard (System-level)
- Additional page details

**See**: [Part 3 - Admin & Super Admin Dashboards](./03-admin-dashboard.md)