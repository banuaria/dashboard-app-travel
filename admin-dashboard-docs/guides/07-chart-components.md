# SuperClaude Frontend - Admin Dashboard Guide (Part 7)

## 📊 Chart Components with Recharts

> **Part 7**: Chart Components Implementation
> 
> **See also**: 
> - [Part 1-6](./01-setup-and-layout.md) - Previous guides
> - [Part 8](./08-testing-deployment.md) - Testing & Deployment

---

## 📦 Setup Recharts

### Installation

```bash
npm install recharts
```

---

## 1. Line Chart Component

**File**: `src/components/charts/LineChartComponent.tsx`

```typescript
import React from 'react';
import {
  LineChart, Line, XAxis, YAxis, CartesianGrid, Tooltip, Legend,
  ResponsiveContainer,
} from 'recharts';

interface LineChartComponentProps {
  data: any[];
  lines: Array<{
    dataKey: string;
    stroke: string;
    name: string;
    strokeDasharray?: string;
  }>;
  xAxisKey: string;
  height?: number;
  xAxisFormatter?: (value: any) => string;
  yAxisFormatter?: (value: any) => string;
}

export const LineChartComponent: React.FC<LineChartComponentProps> = ({
  data, lines, xAxisKey, height = 300,
  xAxisFormatter, yAxisFormatter,
}) => {
  return (
    <ResponsiveContainer width="100%" height={height}>
      <LineChart data={data}>
        <CartesianGrid strokeDasharray="3 3" />
        <XAxis dataKey={xAxisKey} tickFormatter={xAxisFormatter} />
        <YAxis tickFormatter={yAxisFormatter} />
        <Tooltip />
        <Legend />
        {lines.map((line) => (
          <Line
            key={line.dataKey}
            type="monotone"
            dataKey={line.dataKey}
            stroke={line.stroke}
            name={line.name}
            strokeWidth={2}
            strokeDasharray={line.strokeDasharray}
          />
        ))}
      </LineChart>
    </ResponsiveContainer>
  );
};
```

**Usage:**
```typescript
<LineChartComponent
  data={revenueData}
  xAxisKey="date"
  xAxisFormatter={(date) => formatDate(date, 'dd MMM')}
  yAxisFormatter={(value) => formatCurrency(value)}
  lines={[
    { dataKey: 'revenue', stroke: '#3b82f6', name: 'Revenue' },
    { dataKey: 'movingAvg', stroke: '#10b981', name: 'Avg', strokeDasharray: '5 5' },
  ]}
/>
```

---

## 2. Bar Chart Component

**File**: `src/components/charts/BarChartComponent.tsx`

```typescript
import React from 'react';
import {
  BarChart, Bar, XAxis, YAxis, CartesianGrid, Tooltip, Legend,
  ResponsiveContainer, Cell,
} from 'recharts';

interface BarChartComponentProps {
  data: any[];
  bars: Array<{
    dataKey: string;
    fill: string;
    name: string;
  }>;
  xAxisKey: string;
  height?: number;
  customColors?: boolean;
}

export const BarChartComponent: React.FC<BarChartComponentProps> = ({
  data, bars, xAxisKey, height = 300, customColors = false,
}) => {
  return (
    <ResponsiveContainer width="100%" height={height}>
      <BarChart data={data}>
        <CartesianGrid strokeDasharray="3 3" />
        <XAxis dataKey={xAxisKey} angle={-45} textAnchor="end" height={100} />
        <YAxis />
        <Tooltip />
        <Legend />
        {bars.map((bar) => (
          <Bar key={bar.dataKey} dataKey={bar.dataKey} fill={bar.fill} name={bar.name}>
            {customColors && data.map((entry, idx) => (
              <Cell key={`cell-${idx}`} fill={entry.color || bar.fill} />
            ))}
          </Bar>
        ))}
      </BarChart>
    </ResponsiveContainer>
  );
};
```

---

## 3. Pie Chart Component

**File**: `src/components/charts/PieChartComponent.tsx`

```typescript
import React from 'react';
import { PieChart, Pie, Cell, Tooltip, Legend, ResponsiveContainer } from 'recharts';

interface PieChartComponentProps {
  data: Array<{ name: string; value: number; color: string }>;
  height?: number;
  innerRadius?: number;
}

export const PieChartComponent: React.FC<PieChartComponentProps> = ({
  data, height = 300, innerRadius = 0,
}) => {
  return (
    <ResponsiveContainer width="100%" height={height}>
      <PieChart>
        <Pie
          data={data}
          cx="50%"
          cy="50%"
          labelLine={false}
          label={({ name, percent }) => `${name}: ${(percent * 100).toFixed(0)}%`}
          outerRadius={80}
          innerRadius={innerRadius}
          dataKey="value"
        >
          {data.map((entry, idx) => (
            <Cell key={`cell-${idx}`} fill={entry.color} />
          ))}
        </Pie>
        <Tooltip />
        <Legend />
      </PieChart>
    </ResponsiveContainer>
  );
};
```

---

## 4. Area Chart Component

**File**: `src/components/charts/AreaChartComponent.tsx`

```typescript
import React from 'react';
import {
  AreaChart, Area, XAxis, YAxis, CartesianGrid, Tooltip, ResponsiveContainer,
} from 'recharts';

interface AreaChartComponentProps {
  data: any[];
  areas: Array<{
    dataKey: string;
    fill: string;
    stroke: string;
    name: string;
  }>;
  xAxisKey: string;
  height?: number;
}

export const AreaChartComponent: React.FC<AreaChartComponentProps> = ({
  data, areas, xAxisKey, height = 300,
}) => {
  return (
    <ResponsiveContainer width="100%" height={height}>
      <AreaChart data={data}>
        <CartesianGrid strokeDasharray="3 3" />
        <XAxis dataKey={xAxisKey} />
        <YAxis />
        <Tooltip />
        {areas.map((area) => (
          <Area
            key={area.dataKey}
            type="monotone"
            dataKey={area.dataKey}
            stroke={area.stroke}
            fill={area.fill}
            name={area.name}
          />
        ))}
      </AreaChart>
    </ResponsiveContainer>
  );
};
```

---

## 5. Chart Helper Hooks

**File**: `src/hooks/useChartData.ts`

```typescript
import { useMemo } from 'react';
import { formatCurrency, formatDate } from '@/data/helpers';

export const useChartData = (data: any[], type: 'revenue' | 'occupancy' | 'booking') => {
  return useMemo(() => {
    if (type === 'revenue') {
      return {
        xFormatter: (date: string) => formatDate(date, 'dd MMM'),
        yFormatter: (value: number) => formatCurrency(value),
      };
    }
    if (type === 'occupancy') {
      return {
        xFormatter: undefined,
        yFormatter: (value: number) => `${value}%`,
      };
    }
    return {
      xFormatter: undefined,
      yFormatter: undefined,
    };
  }, [type]);
};
```

---

## 6. Chart Usage Examples

### Revenue Trend Chart:
```typescript
import { LineChartComponent } from '@/components/charts/LineChartComponent';
import { useChartData } from '@/hooks/useChartData';

const RevenueTrendChart = ({ data }) => {
  const { xFormatter, yFormatter } = useChartData(data, 'revenue');
  
  return (
    <div className="bg-white p-6 rounded-lg shadow">
      <h3 className="text-lg font-semibold mb-4">Revenue Trend</h3>
      <LineChartComponent
        data={data}
        xAxisKey="date"
        xAxisFormatter={xFormatter}
        yAxisFormatter={yFormatter}
        lines={[
          { dataKey: 'revenue', stroke: '#3b82f6', name: 'Revenue' },
          { dataKey: 'movingAvg', stroke: '#10b981', name: '7-day Avg', strokeDasharray: '5 5' },
        ]}
        height={300}
      />
    </div>
  );
};
```

### Occupancy Bar Chart:
```typescript
const OccupancyByRouteChart = ({ data }) => {
  const dataWithColors = data.map(item => ({
    ...item,
    color: item.occupancy >= 75 ? '#10b981' : item.occupancy >= 50 ? '#f59e0b' : '#ef4444',
  }));
  
  return (
    <div className="bg-white p-6 rounded-lg shadow">
      <h3 className="text-lg font-semibold mb-4">Occupancy by Route</h3>
      <BarChartComponent
        data={dataWithColors}
        xAxisKey="route"
        bars={[{ dataKey: 'occupancy', fill: '#3b82f6', name: 'Occupancy %' }]}
        customColors={true}
        height={300}
      />
    </div>
  );
};
```

### Booking Status Pie Chart:
```typescript
const BookingStatusChart = ({ data }) => {
  const chartData = [
    { name: 'Confirmed', value: 865, color: '#10b981' },
    { name: 'Pending', value: 185, color: '#f59e0b' },
    { name: 'Cancelled', value: 123, color: '#ef4444' },
    { name: 'Completed', value: 61, color: '#3b82f6' },
  ];
  
  return (
    <div className="bg-white p-6 rounded-lg shadow">
      <h3 className="text-lg font-semibold mb-4">Booking Status</h3>
      <PieChartComponent
        data={chartData}
        innerRadius={50}
        height={300}
      />
    </div>
  );
};
```

---

## 7. Responsive Chart Container

**File**: `src/components/charts/ChartCard.tsx`

```typescript
interface ChartCardProps {
  title: string;
  children: React.ReactNode;
  action?: React.ReactNode;
}

export const ChartCard: React.FC<ChartCardProps> = ({ title, children, action }) => {
  return (
    <div className="bg-white rounded-lg shadow p-6">
      <div className="flex items-center justify-between mb-4">
        <h3 className="text-lg font-semibold text-gray-900">{title}</h3>
        {action}
      </div>
      {children}
    </div>
  );
};
```

**Usage:**
```typescript
<ChartCard 
  title="Revenue Trend"
  action={<button className="text-sm text-blue-600">Export</button>}
>
  <LineChartComponent {...props} />
</ChartCard>
```

---

## 8. Advanced Chart Features

### Multi-line Chart with Custom Tooltip:
```typescript
const CustomTooltip = ({ active, payload, label }) => {
  if (active && payload && payload.length) {
    return (
      <div className="bg-white p-3 border rounded shadow">
        <p className="font-semibold">{formatDate(label)}</p>
        {payload.map((entry, index) => (
          <p key={index} style={{ color: entry.color }}>
            {entry.name}: {formatCurrency(entry.value)}
          </p>
        ))}
      </div>
    );
  }
  return null;
};

// Use in LineChart
<LineChart data={data}>
  <Tooltip content={<CustomTooltip />} />
  {/* ... */}
</LineChart>
```

---

## 9. Chart Export Functionality

**File**: `src/utils/chartExport.ts`

```typescript
export const exportChartToCSV = (data: any[], filename: string) => {
  const csvContent = [
    Object.keys(data[0]).join(','),
    ...data.map(row => Object.values(row).join(','))
  ].join('\n');
  
  const blob = new Blob([csvContent], { type: 'text/csv' });
  const url = window.URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.href = url;
  a.download = `${filename}.csv`;
  a.click();
};

export const exportChartToPNG = (chartRef: React.RefObject<HTMLDivElement>) => {
  // Implementation using html2canvas or similar library
  // npm install html2canvas
};
```

---

## 10. Chart Color Themes

**File**: `src/utils/chartThemes.ts`

```typescript
export const chartColors = {
  primary: '#3b82f6',
  success: '#10b981',
  warning: '#f59e0b',
  error: '#ef4444',
  info: '#06b6d4',
  purple: '#8b5cf6',
};

export const getChartTheme = (darkMode: boolean) => ({
  background: darkMode ? '#1f2937' : '#ffffff',
  text: darkMode ? '#f9fafb' : '#111827',
  grid: darkMode ? '#374151' : '#e5e7eb',
});
```

---

## Continue to Part 8

**Next in Part 8:**
- Testing strategies
- Deployment configuration
- Production optimization

**See**: [Part 8 - Testing & Deployment](./08-testing-deployment.md)