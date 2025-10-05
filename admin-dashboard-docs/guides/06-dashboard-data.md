# SuperClaude Frontend - Admin Dashboard Guide (Part 6)

## 📊 Dashboard-Specific Dummy Data

> **Part 6**: Dashboard Data for Operator, Admin & Super Admin
> 
> **See also**: 
> - [Part 1-5](./01-setup-and-layout.md) - Previous guides
> - [Part 7](./07-chart-components.md) - Chart Components
> - [Part 8](./08-testing-deployment.md) - Testing & Deployment

---

## 1. Operator Dashboard Data

**File**: `src/data/dashboards/operator.ts`

```typescript
import { DUMMY_BOOKINGS, DUMMY_BUSES, DUMMY_SCHEDULES } from '../index';

export interface OperatorDashboardData {
  metrics: {
    totalRevenue: {
      current: number;
      previous: number;
      change: number;
      trend: 'up' | 'down';
    };
    activeBuses: {
      active: number;
      total: number;
      newThisMonth: number;
    };
    totalBookings: {
      current: number;
      previous: number;
      change: number;
      trend: 'up' | 'down';
    };
    occupancyRate: {
      current: number;
      previous: number;
      change: number;
      trend: 'up' | 'down';
    };
  };
  revenueChart: Array<{
    date: string;
    revenue: number;
    movingAvg?: number;
  }>;
  occupancyByRoute: Array<{
    route: string;
    occupancy: number;
    bookings: number;
  }>;
  bookingStatus: Array<{
    status: string;
    count: number;
    percentage: number;
  }>;
  topRoutes: Array<{
    route: string;
    revenue: number;
    bookings: number;
    occupancy: number;
  }>;
  recentBookings: typeof DUMMY_BOOKINGS;
  upcomingSchedules: {
    today: number;
    tomorrow: number;
    thisWeek: number;
  };
}

// Generate operator dashboard data
export const generateOperatorDashboard = (operatorId: string): OperatorDashboardData => {
  const totalRevenue = 45000000;
  const previousRevenue = 40000000;
  
  return {
    metrics: {
      totalRevenue: {
        current: totalRevenue,
        previous: previousRevenue,
        change: ((totalRevenue - previousRevenue) / previousRevenue) * 100,
        trend: 'up',
      },
      activeBuses: {
        active: 12,
        total: 15,
        newThisMonth: 2,
      },
      totalBookings: {
        current: 1234,
        previous: 1143,
        change: 8,
        trend: 'up',
      },
      occupancyRate: {
        current: 78.5,
        previous: 74.8,
        change: 5,
        trend: 'up',
      },
    },
    revenueChart: generateRevenueChartData(30),
    occupancyByRoute: [
      { route: 'Jakarta - Bandung', occupancy: 85, bookings: 45 },
      { route: 'Surabaya - Malang', occupancy: 78, bookings: 38 },
      { route: 'Jakarta - Yogyakarta', occupancy: 72, bookings: 32 },
      { route: 'Bandung - Surabaya', occupancy: 65, bookings: 28 },
      { route: 'Semarang - Solo', occupancy: 58, bookings: 22 },
    ],
    bookingStatus: [
      { status: 'Confirmed', count: 865, percentage: 70 },
      { status: 'Pending', count: 185, percentage: 15 },
      { status: 'Cancelled', count: 123, percentage: 10 },
      { status: 'Completed', count: 61, percentage: 5 },
    ],
    topRoutes: [
      { route: 'Jakarta - Bandung', revenue: 12500000, bookings: 45, occupancy: 85 },
      { route: 'Surabaya - Malang', revenue: 9800000, bookings: 38, occupancy: 78 },
      { route: 'Jakarta - Yogyakarta', revenue: 8500000, bookings: 32, occupancy: 72 },
    ],
    recentBookings: DUMMY_BOOKINGS.slice(0, 10),
    upcomingSchedules: {
      today: 5,
      tomorrow: 8,
      thisWeek: 35,
    },
  };
};

// Helper to generate revenue chart data
const generateRevenueChartData = (days: number) => {
  const data = [];
  for (let i = days; i >= 0; i--) {
    const date = new Date();
    date.setDate(date.getDate() - i);
    data.push({
      date: date.toISOString().split('T')[0],
      revenue: 1000000 + Math.floor(Math.random() * 500000),
      movingAvg: 1200000,
    });
  }
  return data;
};

// Default operator dashboard data
export const OPERATOR_DASHBOARD_DATA = generateOperatorDashboard('op-001');
```

---

## 2. Admin Dashboard Data

**File**: `src/data/dashboards/admin.ts`

```typescript
export interface AdminDashboardData {
  platformMetrics: {
    totalRevenue: {
      current: number;
      previous: number;
      change: number;
      commission: number;
      operatorNet: number;
    };
    activeOperators: {
      active: number;
      total: number;
      newThisMonth: number;
      pending: number;
      suspended: number;
    };
    totalUsers: {
      current: number;
      previous: number;
      change: number;
      breakdown: {
        customers: number;
        operators: number;
        admins: number;
      };
    };
    totalBookings: {
      current: number;
      previous: number;
      change: number;
      averagePerDay: number;
    };
  };
  revenueChart: Array<{
    date: string;
    totalRevenue: number;
    commission: number;
    operatorNet: number;
  }>;
  operatorGrowth: Array<{
    month: string;
    new: number;
    suspended: number;
    total: number;
  }>;
  topOperators: Array<{
    rank: number;
    id: string;
    name: string;
    revenue: number;
    bookings: number;
    buses: number;
    occupancy: number;
    growth: number;
  }>;
  popularRoutes: Array<{
    route: string;
    bookings: number;
    revenue: number;
    occupancy: number;
    operators: number;
  }>;
  pendingApprovals: Array<{
    id: string;
    name: string;
    appliedDate: string;
    email: string;
    busesPlanned: number;
  }>;
  recentActivities: Array<{
    id: string;
    type: string;
    message: string;
    timestamp: string;
    user: string;
  }>;
}

// Generate Admin Dashboard Data
export const ADMIN_DASHBOARD_DATA: AdminDashboardData = {
  platformMetrics: {
    totalRevenue: {
      current: 450000000,
      previous: 391000000,
      change: 15,
      commission: 45000000,   // 10%
      operatorNet: 405000000,
    },
    activeOperators: {
      active: 48,
      total: 50,
      newThisMonth: 5,
      pending: 3,
      suspended: 2,
    },
    totalUsers: {
      current: 12450,
      previous: 10375,
      change: 20,
      breakdown: {
        customers: 12000,
        operators: 400,
        admins: 50,
      },
    },
    totalBookings: {
      current: 15234,
      previous: 13602,
      change: 12,
      averagePerDay: 507,
    },
  },

  revenueChart: generatePlatformRevenueData(90),

  operatorGrowth: [
    { month: 'Jul 2024', new: 5, suspended: 1, total: 38 },
    { month: 'Aug 2024', new: 7, suspended: 0, total: 45 },
    { month: 'Sep 2024', new: 4, suspended: 2, total: 47 },
    { month: 'Oct 2024', new: 3, suspended: 0, total: 50 },
    { month: 'Nov 2024', new: 5, suspended: 1, total: 54 },
    { month: 'Dec 2024', new: 6, suspended: 0, total: 60 },
  ],

  topOperators: [
    {
      rank: 1,
      id: 'op-001',
      name: 'PT. Bus Jaya Makmur',
      revenue: 85000000,
      bookings: 1234,
      buses: 25,
      occupancy: 82,
      growth: 15,
    },
    {
      rank: 2,
      id: 'op-002',
      name: 'CV. Transport Sejahtera',
      revenue: 62000000,
      bookings: 890,
      buses: 18,
      occupancy: 78,
      growth: 12,
    },
    {
      rank: 3,
      id: 'op-004',
      name: 'PT. Antar Nusa',
      revenue: 54000000,
      bookings: 745,
      buses: 15,
      occupancy: 75,
      growth: 8,
    },
    {
      rank: 4,
      id: 'op-005',
      name: 'CV. Maju Jaya',
      revenue: 48000000,
      bookings: 680,
      buses: 12,
      occupancy: 72,
      growth: 10,
    },
    {
      rank: 5,
      id: 'op-006',
      name: 'PT. Transportasi Prima',
      revenue: 42000000,
      bookings: 620,
      buses: 14,
      occupancy: 70,
      growth: 6,
    },
  ],

  popularRoutes: [
    {
      route: 'Jakarta - Bandung',
      bookings: 2340,
      revenue: 58500000,
      occupancy: 78,
      operators: 12,
    },
    {
      route: 'Surabaya - Malang',
      bookings: 1890,
      revenue: 47250000,
      occupancy: 75,
      operators: 8,
    },
    {
      route: 'Jakarta - Yogyakarta',
      bookings: 1560,
      revenue: 39000000,
      occupancy: 72,
      operators: 10,
    },
    {
      route: 'Bandung - Surabaya',
      bookings: 1230,
      revenue: 30750000,
      occupancy: 68,
      operators: 6,
    },
    {
      route: 'Semarang - Solo',
      bookings: 980,
      revenue: 24500000,
      occupancy: 65,
      operators: 5,
    },
  ],

  pendingApprovals: [
    {
      id: 'op-pending-1',
      name: 'PT. Transport Baru',
      appliedDate: '2024-01-10',
      email: 'admin@transportbaru.com',
      busesPlanned: 15,
    },
    {
      id: 'op-pending-2',
      name: 'CV. Bus Jaya',
      appliedDate: '2024-01-12',
      email: 'info@busjaya.com',
      busesPlanned: 8,
    },
    {
      id: 'op-pending-3',
      name: 'PT. Nusantara Express',
      appliedDate: '2024-01-13',
      email: 'contact@nusantaraexpress.com',
      busesPlanned: 20,
    },
  ],

  recentActivities: [
    {
      id: 'act-001',
      type: 'operator_approved',
      message: 'Operator PT. Bus Jaya approved',
      timestamp: '2024-01-15 10:30',
      user: 'Admin User',
    },
    {
      id: 'act-002',
      type: 'operator_suspended',
      message: 'Operator CV. Transport suspended due to policy violation',
      timestamp: '2024-01-15 09:15',
      user: 'Admin User',
    },
    {
      id: 'act-003',
      type: 'milestone',
      message: '1,234 new users registered today',
      timestamp: '2024-01-15 08:00',
      user: 'System',
    },
    {
      id: 'act-004',
      type: 'milestone',
      message: 'Platform revenue reached Rp 1 Billion',
      timestamp: '2024-01-14 23:45',
      user: 'System',
    },
    {
      id: 'act-005',
      type: 'user_registered',
      message: 'New operator admin registered',
      timestamp: '2024-01-14 16:20',
      user: 'System',
    },
  ],
};

// Helper function to generate platform revenue data
function generatePlatformRevenueData(days: number) {
  const data = [];
  for (let i = days; i >= 0; i--) {
    const date = new Date();
    date.setDate(date.getDate() - i);
    
    const totalRevenue = 14000000 + Math.floor(Math.random() * 2000000);
    const commission = totalRevenue * 0.1;
    const operatorNet = totalRevenue - commission;
    
    data.push({
      date: date.toISOString().split('T')[0],
      totalRevenue,
      commission,
      operatorNet,
    });
  }
  return data;
}
```

---

## 3. Super Admin Dashboard Data

**File**: `src/data/dashboards/superadmin.ts`

```typescript
export interface SuperAdminDashboardData {
  systemHealth: {
    status: 'operational' | 'degraded' | 'down';
    uptime: number;
    lastDeployment: string;
    responseTime: number;
    services: {
      database: 'operational' | 'degraded' | 'down';
      api: 'operational' | 'degraded' | 'down';
      payment: 'operational' | 'degraded' | 'down';
      email: 'operational' | 'degraded' | 'down';
      storage: 'operational' | 'degraded' | 'down';
      websocket: 'operational' | 'degraded' | 'down';
    };
  };
  securityScore: {
    score: number;
    grade: 'A+' | 'A' | 'B' | 'C' | 'D' | 'F';
    criticalIssues: number;
    warnings: number;
    lastAudit: string;
    failedLogins24h: number;
    blockedIPs: number;
  };
  systemUsers: {
    total: number;
    active30d: number;
    newToday: number;
    growth: number;
  };
  apiMetrics: {
    total24h: number;
    averagePerMin: number;
    peakPerMin: number;
    peakTime: string;
    errorRate: number;
  };
  systemPerformance: Array<{
    timestamp: string;
    cpuUsage: number;
    memoryUsage: number;
    apiResponseTime: number;
    diskIO: number;
  }>;
  storageUsage: Array<{
    category: string;
    size: number;
    percentage: number;
    color: string;
  }>;
  auditLogs: Array<{
    id: string;
    timestamp: string;
    userId: string;
    userName: string;
    userRole: string;
    action: string;
    resource: string;
    ipAddress: string;
    status: 'success' | 'failed';
  }>;
  activeSessions: Array<{
    sessionId: string;
    userId: string;
    userName: string;
    userRole: string;
    loginTime: string;
    lastActivity: string;
    ipAddress: string;
    device: string;
  }>;
  systemAlerts: Array<{
    id: string;
    type: 'critical' | 'warning' | 'info';
    title: string;
    message: string;
    timestamp: string;
  }>;
}

export const SUPER_ADMIN_DASHBOARD_DATA: SuperAdminDashboardData = {
  systemHealth: {
    status: 'operational',
    uptime: 99.99,
    lastDeployment: '2024-01-10 14:30',
    responseTime: 145,
    services: {
      database: 'operational',
      api: 'operational',
      payment: 'operational',
      email: 'operational',
      storage: 'operational',
      websocket: 'operational',
    },
  },

  securityScore: {
    score: 98.5,
    grade: 'A+',
    criticalIssues: 0,
    warnings: 2,
    lastAudit: '2024-01-01',
    failedLogins24h: 12,
    blockedIPs: 2,
  },

  systemUsers: {
    total: 12500,
    active30d: 8450,
    newToday: 45,
    growth: 20,
  },

  apiMetrics: {
    total24h: 1200000,
    averagePerMin: 850,
    peakPerMin: 1250,
    peakTime: '14:30',
    errorRate: 0.5,
  },

  systemPerformance: generateSystemPerformanceData(24),

  storageUsage: [
    { category: 'Database', size: 120, percentage: 40, color: '#3b82f6' },
    { category: 'File Uploads', size: 90, percentage: 30, color: '#10b981' },
    { category: 'Logs', size: 45, percentage: 15, color: '#f59e0b' },
    { category: 'Backups', size: 30, percentage: 10, color: '#8b5cf6' },
    { category: 'Other', size: 15, percentage: 5, color: '#6b7280' },
  ],

  auditLogs: [
    {
      id: 'audit-001',
      timestamp: '2024-01-15 14:30:25',
      userId: 'user-003',
      userName: 'Siti Rahayu',
      userRole: 'super_admin',
      action: 'updated',
      resource: 'operator',
      ipAddress: '192.168.1.100',
      status: 'success',
    },
    {
      id: 'audit-002',
      timestamp: '2024-01-15 14:25:10',
      userId: 'user-002',
      userName: 'Ahmad Hidayat',
      userRole: 'admin',
      action: 'created',
      resource: 'user',
      ipAddress: '192.168.1.105',
      status: 'success',
    },
    {
      id: 'audit-003',
      timestamp: '2024-01-15 14:20:45',
      userId: 'unknown',
      userName: 'Unknown',
      userRole: 'unknown',
      action: 'login',
      resource: 'auth',
      ipAddress: '45.123.45.67',
      status: 'failed',
    },
  ],

  activeSessions: [
    {
      sessionId: 'sess-001',
      userId: 'user-003',
      userName: 'Siti Rahayu',
      userRole: 'super_admin',
      loginTime: '2024-01-15 12:30',
      lastActivity: '2024-01-15 14:30',
      ipAddress: '192.168.1.100',
      device: 'Chrome on Windows',
    },
    {
      sessionId: 'sess-002',
      userId: 'user-002',
      userName: 'Ahmad Hidayat',
      userRole: 'admin',
      loginTime: '2024-01-15 13:30',
      lastActivity: '2024-01-15 14:28',
      ipAddress: '192.168.1.105',
      device: 'Firefox on macOS',
    },
  ],

  systemAlerts: [
    {
      id: 'alert-001',
      type: 'warning',
      title: 'SSL Certificate Expiring',
      message: 'Your SSL certificate expires in 30 days',
      timestamp: '2024-01-15 10:00',
    },
    {
      id: 'alert-002',
      type: 'warning',
      title: 'Backup Delayed',
      message: 'Last backup was delayed by 2 hours',
      timestamp: '2024-01-15 08:00',
    },
    {
      id: 'alert-003',
      type: 'info',
      title: 'New Version Available',
      message: 'System update 2.1.0 is ready to install',
      timestamp: '2024-01-14 16:00',
    },
  ],
};

// Helper function to generate system performance data
function generateSystemPerformanceData(hours: number) {
  const data = [];
  for (let i = hours; i >= 0; i--) {
    const timestamp = new Date();
    timestamp.setHours(timestamp.getHours() - i);
    
    data.push({
      timestamp: timestamp.toISOString(),
      cpuUsage: 30 + Math.floor(Math.random() * 30),
      memoryUsage: 50 + Math.floor(Math.random() * 25),
      apiResponseTime: 100 + Math.floor(Math.random() * 100),
      diskIO: 20 + Math.floor(Math.random() * 30),
    });
  }
  return data;
}
```

---

## 4. Data Export Utilities

**File**: `src/data/index.ts`

```typescript
// Re-export all dummy data
export * from './users';
export * from './operators';
export * from './buses';
export * from './routes';
export * from './schedules';
export * from './bookings';
export * from './helpers';

// Dashboard data
export * from './dashboards/operator';
export * from './dashboards/admin';
export * from './dashboards/superadmin';

// Central data access
export const getDashboardData = (role: string, userId?: string) => {
  switch (role) {
    case 'operator_admin':
      return OPERATOR_DASHBOARD_DATA;
    case 'admin':
      return ADMIN_DASHBOARD_DATA;
    case 'super_admin':
      return SUPER_ADMIN_DASHBOARD_DATA;
    default:
      return null;
  }
};
```

---

## Continue to Part 7

**Next in Part 7:**
- Chart components implementation
- Recharts configuration
- Custom chart hooks

**See**: [Part 7 - Chart Components](./07-chart-components.md)