# SuperClaude Frontend - Admin Dashboard Guide (Part 5)

## 📦 Core Dummy Data Structures

> **Part 5**: Complete Dummy Data untuk Testing Dashboard
> 
> **See also**: 
> - [Part 1-4](./01-setup-and-layout.md) - Previous guides
> - [Part 6](./06-dashboard-data.md) - Dashboard Data
> - [Part 7](./07-chart-components.md) - Charts
> - [Part 8](./08-testing-deployment.md) - Deployment

---

## 🎯 Data Structure Overview

**File Organization:**
```
src/data/
├── users.ts              # User accounts (all roles)
├── operators.ts          # Operator companies
├── buses.ts              # Bus fleet data
├── routes.ts             # Travel routes
├── schedules.ts          # Bus schedules
├── bookings.ts           # Booking records
├── dashboards/
│   ├── operator.ts       # Operator dashboard data
│   ├── admin.ts          # Admin dashboard data
│   └── superadmin.ts     # Super admin dashboard data
└── helpers.ts            # Data generation utilities
```

---

## 1. Core Data Types

### 1.1 Users Data

**File**: `src/data/users.ts`

```typescript
export interface User {
  id: string;
  email: string;
  password: string;           // For dummy auth
  name: string;
  phone: string;
  role: 'customer' | 'operator_admin' | 'admin' | 'super_admin';
  operatorId?: string;        // For operator_admin
  status: 'active' | 'suspended' | 'inactive';
  emailVerified: boolean;
  createdAt: string;
  lastLogin?: string;
  avatar?: string;
}

// Dummy users for login testing
export const DUMMY_USERS: User[] = [
  {
    id: 'user-001',
    email: 'operator@example.com',
    password: 'password',
    name: 'Budi Santoso',
    phone: '+628123456789',
    role: 'operator_admin',
    operatorId: 'op-001',
    status: 'active',
    emailVerified: true,
    createdAt: '2024-01-01T00:00:00Z',
    lastLogin: '2024-01-15T08:30:00Z',
  },
  {
    id: 'user-002',
    email: 'admin@example.com',
    password: 'password',
    name: 'Ahmad Hidayat',
    phone: '+628123456790',
    role: 'admin',
    status: 'active',
    emailVerified: true,
    createdAt: '2024-01-01T00:00:00Z',
    lastLogin: '2024-01-15T09:00:00Z',
  },
  {
    id: 'user-003',
    email: 'superadmin@example.com',
    password: 'password',
    name: 'Siti Rahayu',
    phone: '+628123456791',
    role: 'super_admin',
    status: 'active',
    emailVerified: true,
    createdAt: '2024-01-01T00:00:00Z',
    lastLogin: '2024-01-15T07:45:00Z',
  },
];

// Generate random customers
export const generateCustomers = (count: number): User[] => {
  const names = ['John Doe', 'Jane Smith', 'Ali Rahman', 'Sari Wulan', 'Budi Hartono'];
  return Array.from({ length: count }, (_, i) => ({
    id: `customer-${i + 1}`,
    email: `customer${i + 1}@example.com`,
    password: 'password',
    name: names[i % names.length] + ` ${i + 1}`,
    phone: `+62812345${String(6000 + i).padStart(4, '0')}`,
    role: 'customer' as const,
    status: 'active' as const,
    emailVerified: true,
    createdAt: new Date(Date.now() - Math.random() * 90 * 24 * 60 * 60 * 1000).toISOString(),
  }));
};

export const DUMMY_CUSTOMERS = generateCustomers(100);
```

---

### 1.2 Operators Data

**File**: `src/data/operators.ts`

```typescript
export interface Operator {
  id: string;
  name: string;
  email: string;
  phone: string;
  address: string;
  city: string;
  province: string;
  status: 'active' | 'pending' | 'suspended';
  totalBuses: number;
  activeBuses: number;
  joinDate: string;
  approvedDate?: string;
  commission: number;         // Percentage (e.g., 10)
  documents: {
    businessLicense: boolean;
    taxId: boolean;
    insurance: boolean;
  };
  rating?: number;            // 0-5
  totalRevenue?: number;
  totalBookings?: number;
}

export const DUMMY_OPERATORS: Operator[] = [
  {
    id: 'op-001',
    name: 'PT. Bus Jaya Makmur',
    email: 'admin@busjaya.com',
    phone: '+622112345678',
    address: 'Jl. Sudirman No. 123',
    city: 'Jakarta',
    province: 'DKI Jakarta',
    status: 'active',
    totalBuses: 25,
    activeBuses: 23,
    joinDate: '2023-06-15T00:00:00Z',
    approvedDate: '2023-06-20T00:00:00Z',
    commission: 10,
    documents: {
      businessLicense: true,
      taxId: true,
      insurance: true,
    },
    rating: 4.5,
    totalRevenue: 85000000,
    totalBookings: 1234,
  },
  {
    id: 'op-002',
    name: 'CV. Transport Sejahtera',
    email: 'admin@transejahtera.com',
    phone: '+622212345679',
    address: 'Jl. Asia Afrika No. 45',
    city: 'Bandung',
    province: 'Jawa Barat',
    status: 'active',
    totalBuses: 18,
    activeBuses: 18,
    joinDate: '2023-08-10T00:00:00Z',
    approvedDate: '2023-08-15T00:00:00Z',
    commission: 10,
    documents: {
      businessLicense: true,
      taxId: true,
      insurance: true,
    },
    rating: 4.2,
    totalRevenue: 62000000,
    totalBookings: 890,
  },
  {
    id: 'op-003',
    name: 'PT. Trans Nusantara',
    email: 'info@transnusantara.com',
    phone: '+623112345680',
    address: 'Jl. Pahlawan No. 67',
    city: 'Surabaya',
    province: 'Jawa Timur',
    status: 'pending',
    totalBuses: 15,
    activeBuses: 0,
    joinDate: '2024-01-10T00:00:00Z',
    commission: 10,
    documents: {
      businessLicense: true,
      taxId: true,
      insurance: false,
    },
  },
];
```

---

### 1.3 Buses Data

**File**: `src/data/buses.ts`

```typescript
export interface Bus {
  id: string;
  operatorId: string;
  plateNumber: string;
  busType: 'economy' | 'business' | 'executive' | 'sleeper';
  brand: string;              // e.g., 'Mercedes', 'Hino'
  model: string;
  year: number;
  totalSeats: number;
  seatLayout: {
    rows: number;
    columns: number;
    layout: string;           // e.g., '2-2', '2-3'
  };
  facilities: string[];
  status: 'active' | 'maintenance' | 'inactive';
  createdAt: string;
  lastMaintenance?: string;
}

export const DUMMY_BUSES: Bus[] = [
  {
    id: 'bus-001',
    operatorId: 'op-001',
    plateNumber: 'B 1234 ABC',
    busType: 'executive',
    brand: 'Mercedes-Benz',
    model: 'OH 1526',
    year: 2022,
    totalSeats: 40,
    seatLayout: {
      rows: 10,
      columns: 4,
      layout: '2-2',
    },
    facilities: ['AC', 'WiFi', 'USB Charger', 'Toilet', 'Reclining Seat'],
    status: 'active',
    createdAt: '2023-06-20T00:00:00Z',
    lastMaintenance: '2024-01-01T00:00:00Z',
  },
  {
    id: 'bus-002',
    operatorId: 'op-001',
    plateNumber: 'B 5678 DEF',
    busType: 'business',
    brand: 'Hino',
    model: 'RK8',
    year: 2021,
    totalSeats: 45,
    seatLayout: {
      rows: 15,
      columns: 3,
      layout: '2-1',
    },
    facilities: ['AC', 'USB Charger', 'Reclining Seat'],
    status: 'active',
    createdAt: '2023-07-15T00:00:00Z',
    lastMaintenance: '2023-12-15T00:00:00Z',
  },
];

// Helper to generate buses for an operator
export const generateBusesForOperator = (
  operatorId: string, 
  count: number
): Bus[] => {
  const types: Bus['busType'][] = ['economy', 'business', 'executive'];
  const brands = ['Mercedes-Benz', 'Hino', 'Scania', 'Volvo'];
  
  return Array.from({ length: count }, (_, i) => ({
    id: `bus-${operatorId}-${i + 1}`,
    operatorId,
    plateNumber: `B ${Math.floor(1000 + Math.random() * 9000)} ${String.fromCharCode(65 + Math.floor(Math.random() * 26))}${String.fromCharCode(65 + Math.floor(Math.random() * 26))}${String.fromCharCode(65 + Math.floor(Math.random() * 26))}`,
    busType: types[Math.floor(Math.random() * types.length)],
    brand: brands[Math.floor(Math.random() * brands.length)],
    model: 'Model X',
    year: 2020 + Math.floor(Math.random() * 4),
    totalSeats: 40 + Math.floor(Math.random() * 10) * 5,
    seatLayout: {
      rows: 10,
      columns: 4,
      layout: '2-2',
    },
    facilities: ['AC', 'USB Charger'],
    status: Math.random() > 0.1 ? 'active' : 'maintenance',
    createdAt: new Date(Date.now() - Math.random() * 365 * 24 * 60 * 60 * 1000).toISOString(),
  }));
};
```

---

### 1.4 Routes Data

**File**: `src/data/routes.ts`

```typescript
export interface Route {
  id: string;
  origin: string;
  destination: string;
  distance: number;           // km
  duration: number;           // minutes
  basePrice: number;
  cities: string[];           // Intermediate cities
  popular: boolean;
}

export const DUMMY_ROUTES: Route[] = [
  {
    id: 'route-001',
    origin: 'Jakarta',
    destination: 'Bandung',
    distance: 150,
    duration: 180,
    basePrice: 100000,
    cities: ['Jakarta', 'Bekasi', 'Bandung'],
    popular: true,
  },
  {
    id: 'route-002',
    origin: 'Jakarta',
    destination: 'Yogyakarta',
    distance: 520,
    duration: 600,
    basePrice: 250000,
    cities: ['Jakarta', 'Cirebon', 'Tegal', 'Semarang', 'Yogyakarta'],
    popular: true,
  },
  {
    id: 'route-003',
    origin: 'Surabaya',
    destination: 'Malang',
    distance: 90,
    duration: 120,
    basePrice: 75000,
    cities: ['Surabaya', 'Malang'],
    popular: true,
  },
  {
    id: 'route-004',
    origin: 'Bandung',
    destination: 'Surabaya',
    distance: 680,
    duration: 780,
    basePrice: 350000,
    cities: ['Bandung', 'Cirebon', 'Semarang', 'Surabaya'],
    popular: false,
  },
  {
    id: 'route-005',
    origin: 'Semarang',
    destination: 'Solo',
    distance: 110,
    duration: 150,
    basePrice: 80000,
    cities: ['Semarang', 'Solo'],
    popular: false,
  },
];
```

---

### 1.5 Schedules Data

**File**: `src/data/schedules.ts`

```typescript
export interface Schedule {
  id: string;
  operatorId: string;
  busId: string;
  routeId: string;
  date: string;               // '2024-01-15'
  departureTime: string;      // '08:00'
  arrivalTime: string;        // '11:00'
  price: number;
  totalSeats: number;
  availableSeats: number;
  status: 'scheduled' | 'departed' | 'arrived' | 'cancelled';
  boardingPoints: {
    name: string;
    time: string;
  }[];
}

// Helper to generate schedules
export const generateSchedules = (count: number): Schedule[] => {
  const times = ['06:00', '08:00', '10:00', '12:00', '14:00', '16:00', '18:00', '20:00'];
  
  return Array.from({ length: count }, (_, i) => {
    const totalSeats = 40;
    const bookedSeats = Math.floor(Math.random() * totalSeats);
    const date = new Date();
    date.setDate(date.getDate() + Math.floor(Math.random() * 30));
    
    return {
      id: `schedule-${i + 1}`,
      operatorId: `op-${String(Math.floor(Math.random() * 3) + 1).padStart(3, '0')}`,
      busId: `bus-${i + 1}`,
      routeId: `route-${String(Math.floor(Math.random() * 5) + 1).padStart(3, '0')}`,
      date: date.toISOString().split('T')[0],
      departureTime: times[Math.floor(Math.random() * times.length)],
      arrivalTime: times[(Math.floor(Math.random() * times.length) + 2) % times.length],
      price: 100000 + Math.floor(Math.random() * 200000),
      totalSeats,
      availableSeats: totalSeats - bookedSeats,
      status: date > new Date() ? 'scheduled' : 'departed',
      boardingPoints: [
        { name: 'Terminal Pusat', time: '08:00' },
        { name: 'Rest Area KM 50', time: '09:30' },
      ],
    };
  });
};

export const DUMMY_SCHEDULES = generateSchedules(100);
```

---

### 1.6 Bookings Data

**File**: `src/data/bookings.ts`

```typescript
export interface Booking {
  id: string;
  scheduleId: string;
  customerId: string;
  customerName: string;
  customerPhone: string;
  customerEmail: string;
  seatNumbers: string[];
  boardingPoint: string;
  totalPrice: number;
  status: 'pending' | 'confirmed' | 'cancelled' | 'completed';
  paymentStatus: 'pending' | 'paid' | 'refunded' | 'failed';
  paymentMethod?: string;
  bookingDate: string;
  createdAt: string;
}

export const generateBookings = (count: number): Booking[] => {
  const statuses: Booking['status'][] = ['pending', 'confirmed', 'cancelled', 'completed'];
  const paymentStatuses: Booking['paymentStatus'][] = ['pending', 'paid', 'refunded'];
  const names = ['John Doe', 'Jane Smith', 'Ahmad Rahman', 'Siti Nurhaliza'];
  
  return Array.from({ length: count }, (_, i) => ({
    id: `BK-${String(i + 1).padStart(6, '0')}`,
    scheduleId: `schedule-${Math.floor(Math.random() * 100) + 1}`,
    customerId: `customer-${Math.floor(Math.random() * 100) + 1}`,
    customerName: names[Math.floor(Math.random() * names.length)],
    customerPhone: `+62812345${String(6000 + i).padStart(4, '0')}`,
    customerEmail: `customer${i + 1}@example.com`,
    seatNumbers: [`A${Math.floor(Math.random() * 20) + 1}`],
    boardingPoint: 'Terminal Pusat',
    totalPrice: 100000 + Math.floor(Math.random() * 200000),
    status: statuses[Math.floor(Math.random() * statuses.length)],
    paymentStatus: paymentStatuses[Math.floor(Math.random() * paymentStatuses.length)],
    paymentMethod: 'midtrans',
    bookingDate: new Date(Date.now() - Math.random() * 30 * 24 * 60 * 60 * 1000).toISOString(),
    createdAt: new Date(Date.now() - Math.random() * 30 * 24 * 60 * 60 * 1000).toISOString(),
  }));
};

export const DUMMY_BOOKINGS = generateBookings(500);
```

---

## 2. Helper Functions

**File**: `src/data/helpers.ts`

```typescript
// Format currency
export const formatCurrency = (amount: number): string => {
  if (amount >= 1000000) {
    return `Rp ${(amount / 1000000).toFixed(1)} Jt`;
  }
  if (amount >= 1000) {
    return `Rp ${(amount / 1000).toFixed(0)} Rb`;
  }
  return `Rp ${amount.toLocaleString('id-ID')}`;
};

// Format date
export const formatDate = (date: string, format: string = 'dd MMM yyyy'): string => {
  const d = new Date(date);
  const months = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'];
  
  if (format === 'dd MMM') {
    return `${d.getDate()} ${months[d.getMonth()]}`;
  }
  if (format === 'dd MMM yyyy') {
    return `${d.getDate()} ${months[d.getMonth()]} ${d.getFullYear()}`;
  }
  return date;
};

// Calculate percentage change
export const calculateChange = (current: number, previous: number): number => {
  if (previous === 0) return 0;
  return ((current - previous) / previous) * 100;
};

// Get trend direction
export const getTrend = (change: number): 'up' | 'down' | 'neutral' => {
  if (change > 0) return 'up';
  if (change < 0) return 'down';
  return 'neutral';
};

// Get color by value
export const getOccupancyColor = (occupancy: number): string => {
  if (occupancy >= 75) return '#10b981'; // Green
  if (occupancy >= 50) return '#f59e0b'; // Yellow
  return '#ef4444'; // Red
};

// Generate random data for testing
export const generateRandomData = (
  count: number,
  min: number,
  max: number
): number[] => {
  return Array.from({ length: count }, () => 
    Math.floor(Math.random() * (max - min + 1)) + min
  );
};
```

---

## Continue to Part 6

**Next in Part 6:**
- Dashboard-specific dummy data
- Operator, Admin, Super Admin dashboard data
- Chart data structures

**See**: [Part 6 - Dashboard Data](./06-dashboard-data.md)