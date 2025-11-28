# HydroSmart System Architecture

## High-Level Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                        Frontend (React + TypeScript)             │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │                      App.tsx (Router)                    │   │
│  │  ┌──────────────────────────────────────────────────┐    │   │
│  │  │              AuthContext (Global State)          │    │   │
│  │  │  - Current user                                  │    │   │
│  │  │  - Login/logout functions                        │    │   │
│  │  └──────────────────────────────────────────────────┘    │   │
│  │  ┌──────────────────────────────────────────────────┐    │   │
│  │  │  Navigation + Pages (8 Feature Modules)         │    │   │
│  │  ├─ Dashboard (Real-time monitoring)               │    │   │
│  │  ├─ Energy Management (Optimization)               │    │   │
│  │  ├─ Storage (Tank management)                      │    │   │
│  │  ├─ Alerts (Emergency control)                     │    │   │
│  │  ├─ Certificates (Green credentials)              │    │   │
│  │  ├─ Marketplace (Buy/sell)                         │    │   │
│  │  ├─ Logistics (Delivery tracking)                  │    │   │
│  │  └─ Analytics (Reporting)                          │    │   │
│  │  └──────────────────────────────────────────────────┘    │   │
│  │  ┌──────────────────────────────────────────────────┐    │   │
│  │  │  Components (Reusable Widgets)                  │    │   │
│  │  ├─ MetricCard (KPI display)                       │    │   │
│  │  ├─ GaugeChart (Radial visualization)              │    │   │
│  │  ├─ LineChart (Time series)                        │    │   │
│  │  ├─ TankCard (Tank status)                         │    │   │
│  │  └─ AlertBanner (Notification toast)               │    │   │
│  │  └──────────────────────────────────────────────────┘    │   │
│  └──────────────────────────────────────────────────────────┘   │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │              Services (Business Logic)                   │   │
│  ├─ TelemetrySimulator (Real-time data generation)          │   │
│  ├─ DataService (Mock data & analytics)                    │   │
│  └─ Types (TypeScript definitions)                         │   │
│  └──────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
```

---

## Data Flow Architecture

### 1. Real-Time Telemetry Flow

```
┌─────────────────────────────────────────┐
│  TelemetrySimulator                     │
│  ├─ generateTelemetry()                 │
│  │  ├─ generateSolarPower()             │
│  │  ├─ generateWindPower()              │
│  │  ├─ generateHydroPower()             │
│  │  └─ calculateProduction()            │
│  ├─ updateStorageTanks()                │
│  └─ checkForAlerts()                    │
└────────────┬────────────────────────────┘
             │ Every 5 seconds
             ▼
┌─────────────────────────────────────────┐
│  Component State (useState)             │
│  ├─ currentData (TelemetryData)         │
│  ├─ history (TelemetryData[])           │
│  ├─ tanks (StorageTank[])               │
│  └─ alerts (Alert[])                    │
└────────────┬────────────────────────────┘
             │ React renders
             ▼
┌─────────────────────────────────────────┐
│  DOM (Browser Display)                  │
│  ├─ Charts update                       │
│  ├─ Gauges update                       │
│  ├─ Metrics update                      │
│  └─ Alerts appear                       │
└─────────────────────────────────────────┘
```

### 2. User Interaction Flow

```
┌──────────────────────┐
│  User Action         │
│  (Click, Type, etc)  │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────────┐
│  Component Event Handler │
│  (onClick, onChange)     │
└──────────┬───────────────┘
           │
           ▼
┌──────────────────────────────┐
│  Update Component State      │
│  (setState)                  │
└──────────┬───────────────────┘
           │
           ▼
┌──────────────────────────────┐
│  Trigger Side Effects        │
│  (if needed)                 │
└──────────┬───────────────────┘
           │
           ▼
┌──────────────────────────────┐
│  Re-render Component         │
│  (with new state)            │
└──────────┬───────────────────┘
           │
           ▼
┌──────────────────────────────┐
│  DOM Updates                 │
│  (React reconciliation)      │
└──────────────────────────────┘
```

### 3. Authentication Flow

```
┌──────────────────┐
│  Login Page      │
│  User enters     │
│  email & pwd     │
└────────┬─────────┘
         │
         ▼
┌──────────────────────────────┐
│  useAuth() Hook              │
│  │ login(email, password)    │
└────────┬─────────────────────┘
         │
         ▼
┌──────────────────────────────┐
│  Find user in mockUsers      │
│  (dataService.ts)            │
└────────┬─────────────────────┘
         │
         ▼
┌──────────────────────────────┐
│  Update AuthContext          │
│  setUser(foundUser)          │
└────────┬─────────────────────┘
         │
         ▼
┌──────────────────────────────┐
│  Save to localStorage        │
│  (persist on refresh)        │
└────────┬─────────────────────┘
         │
         ▼
┌──────────────────────────────┐
│  Render Main App             │
│  (Dashboard)                 │
└──────────────────────────────┘
```

---

## Component Structure

### Folder Hierarchy

```
src/
├── components/              # Presentational components (no business logic)
│   ├── AlertBanner.tsx      # Toast notifications for alerts
│   ├── GaugeChart.tsx       # Circular progress indicator
│   ├── LineChart.tsx        # SVG-based line chart
│   ├── MetricCard.tsx       # KPI card with icon
│   └── TankCard.tsx         # Storage tank status card
│
├── contexts/                # React Context for global state
│   └── AuthContext.tsx      # User authentication state & functions
│
├── pages/                   # Full-page components (complete features)
│   ├── Login.tsx            # Authentication page
│   ├── Dashboard.tsx        # Production monitoring
│   ├── EnergyManagement.tsx # Optimization system
│   ├── Storage.tsx          # Tank management
│   ├── Alerts.tsx           # Safety & emergency
│   ├── Certificates.tsx     # Green credentials
│   ├── Marketplace.tsx      # Trading platform
│   ├── Logistics.tsx        # Delivery tracking
│   └── Analytics.tsx        # Performance metrics
│
├── services/                # Business logic & data
│   ├── telemetrySimulator.ts # Real-time data generation
│   └── dataService.ts       # Mock data & calculations
│
├── types/                   # TypeScript interfaces
│   └── index.ts             # All type definitions
│
├── App.tsx                  # Main router & layout
├── main.tsx                 # Entry point
├── index.css                # Global styles
└── vite-env.d.ts            # Vite type definitions
```

---

## State Management Strategy

### Local Component State
Used for data that only one component needs:

```typescript
// Example: Dashboard.tsx
const [currentData, setCurrentData] = useState<TelemetryData>(simulator.generateTelemetry());
const [history, setHistory] = useState<TelemetryData[]>([]);
const [alerts, setAlerts] = useState<AlertType[]>([]);
```

**When to use**: Component-specific UI state, temporary data

### Global Context (AuthContext)
Used for data needed across entire app:

```typescript
// Example: Using in any component
const { user, login, logout, isAuthenticated } = useAuth();
```

**When to use**: Authentication, user preferences, theme

### No Redux/Zustand Needed
The app is simple enough that React Context + useState is sufficient.

---

## Type System

### Core Types (src/types/index.ts)

```typescript
// User Management
type UserRole = 'admin' | 'operator' | 'energy_manager' | 'logistics' | 'buyer';
interface User { id, name, email, role, createdAt }

// Real-Time Data
interface TelemetryData {
  timestamp, plantId, solarPowerKw, windPowerKw, hydroPowerKw,
  electrolyserPowerKw, hydrogenProductionKgph, stackTemperatureC,
  hydrogenStorageKg, efficiencyPercent
}

// Storage
type TankStatus = 'normal' | 'full' | 'low' | 'critical';
interface StorageTank {
  id, tankId, hydrogenMassKg, pressureBar, temperatureC,
  capacityKg, status, lastUpdated
}

// Orders
type OrderStatus = 'pending' | 'approved' | 'in_transit' | 'delivered' | 'cancelled';
interface Order {
  id, orderId, buyerId, buyerName, quantityKg, pricePerKg,
  totalPrice, status, deliveryLocation, deliveryDate,
  currentLat, currentLng, certificateId, createdAt, updatedAt
}

// Certificates
type RenewableSource = 'solar' | 'wind' | 'hydro' | 'mixed';
interface Certificate {
  id, certId, batchId, renewableSource, energyConsumedKwh,
  hydrogenProducedKg, co2SavedTons, productionStart,
  productionEnd, createdAt
}

// Alerts
type AlertType = 'pressure' | 'temperature' | 'leak' | 'low_inventory' | 'system';
type AlertSeverity = 'low' | 'medium' | 'high' | 'critical';
interface Alert {
  id, alertId, type, severity, message, source, resolved,
  resolvedAt, resolvedBy, createdAt
}
```

---

## Data Update Patterns

### Pattern 1: Real-Time Update with setInterval

```typescript
useEffect(() => {
  const interval = setInterval(() => {
    const newData = simulator.generateTelemetry();
    setCurrentData(newData);
    setHistory(prev => [...prev.slice(-29), newData]); // Keep last 30
  }, 5000); // Every 5 seconds

  return () => clearInterval(interval); // Cleanup on unmount
}, []);
```

### Pattern 2: Immutable State Update

```typescript
// ✅ Correct: Create new array
setAlerts(prev => [newAlert, ...prev]);

// ❌ Wrong: Mutating array directly
alerts.push(newAlert);
setAlerts(alerts);
```

### Pattern 3: Filtering/Mapping State

```typescript
// Resolve an alert
setAlerts(prev => prev.map(alert =>
  alert.id === id
    ? { ...alert, resolved: true, resolvedAt: new Date() }
    : alert
));
```

### Pattern 4: Conditional Updates

```typescript
setOrders(prevOrders =>
  prevOrders.map(order => {
    if (order.status === 'in_transit') {
      // Update position
      return { ...order, currentLat: order.currentLat + offset };
    }
    return order; // No change
  })
);
```

---

## Lifecycle & Effects

### Dashboard Component Lifecycle

```
Mount
  ↓
Generate initial telemetry data ──┐
  ↓                               │
Create useEffect hook             │
  ↓                               ├─ Happens on mount
Setup 5-second interval           │
  ↓                               │
Return cleanup function ──────────┘
  ↓
Component renders
  ↓
Every 5 seconds:
  ├─ Generate new telemetry
  ├─ Update state
  ├─ Component re-renders
  └─ Charts/Gauges update
  ↓
User navigates away
  ↓
Cleanup function runs
  ↓
Interval cleared
  ↓
Unmount
```

### useEffect Dependency Array

```typescript
// Run once on mount, cleanup on unmount
useEffect(() => { ... }, [])

// Run when 'user' changes
useEffect(() => { ... }, [user])

// Run on every render (avoid!)
useEffect(() => { ... })
```

---

## Styling Architecture

### CSS Cascade

```
index.css (Global)
    ↓
Tailwind Utilities (Applied to elements)
    ↓
Component Classes (Specific styling)
    ↓
Inline Styles (Last resort)
```

### Responsive Breakpoints

| Breakpoint | Screen Size | Classes |
|-----------|-----------|---------|
| Base | Mobile | No prefix: `p-4 block` |
| sm | 640px | `sm:` prefix: `sm:p-6` |
| md | 768px | `md:` prefix: `md:grid-cols-2` |
| lg | 1024px | `lg:` prefix: `lg:block` |
| xl | 1280px | `xl:` prefix: `xl:p-8` |
| 2xl | 1536px | `2xl:` prefix: `2xl:text-2xl` |

### Color System

```
Primary:   Blue   (bg-blue-500, text-blue-600, border-blue-200)
Secondary: Cyan   (bg-cyan-500, text-cyan-600)
Success:   Green  (bg-green-500, text-green-600)
Warning:   Orange (bg-orange-500, text-orange-600)
Danger:    Red    (bg-red-500, text-red-600)
Neutral:   Gray   (bg-gray-100, text-gray-900, border-gray-200)
```

---

## Performance Optimizations

### Current Optimizations

1. **Interval Updates**: Only update every 5 seconds (not every render)
2. **History Limiting**: Keep last 30 data points, not 1000+
3. **Conditional Rendering**: Only render affected components
4. **CSS Optimization**: Tailwind PurgeCSS removes unused styles

### Potential Future Optimizations

```typescript
// Memoize expensive components
export const MetricCard = React.memo(({ title, value, icon }: Props) => {
  return /* ... */;
});

// Memoize callbacks
const handleSubmit = useCallback((e) => {
  // ...
}, [dependencies]);

// Lazy load pages
const Dashboard = lazy(() => import('./pages/Dashboard'));
```

---

## Error Handling

### Current Error Handling

- Try/catch in event handlers
- Graceful fallbacks for missing data
- Console logging for debugging

### Console Access

```typescript
// Log current data
console.log('Telemetry:', currentData);

// Log state changes
console.log('Alerts:', alerts);

// Log user info
const { user } = useAuth();
console.log('Current user:', user);
```

---

## Testing Approach

### Manual Testing Checklist

- [ ] Login with each role
- [ ] Data updates every 5 seconds
- [ ] Charts build over time
- [ ] Alerts appear randomly
- [ ] Emergency shutdown works
- [ ] Can place orders
- [ ] Deliveries track correctly
- [ ] Responsive on mobile
- [ ] localStorage persists user

### Browser DevTools Usage

```
F12 → Console: Check for errors
F12 → Network: Monitor requests (none yet)
F12 → Application → Storage: Check localStorage
F12 → Elements: Inspect component structure
Ctrl+Shift+M: Toggle mobile view
```

---

## Deployment Architecture

### Build Process

```
Source Code (TypeScript + JSX)
    ↓ npm run build
Vite bundler
    ↓
TypeScript compiler
    ↓
JSX transpiler
    ↓
CSS minimizer
    ↓
Code splitting
    ↓
dist/ folder
    ↓ Deploy
Vercel / Netlify / AWS S3
    ↓
Production Server
    ↓
User browsers
```

### Build Output

```
dist/
├── index.html         (Entry point)
├── assets/
│   ├── index-[hash].js   (Bundled JS, minified)
│   └── index-[hash].css  (Bundled CSS, minified)
└── (Static files)
```

---

## Future Integration Points

### Database Integration (Ready for implementation)

```typescript
// Current: Mock data
import { mockUsers, initialStorageTanks } from './services/dataService';

// Future: Supabase queries
const { data: users } = await supabase.from('users').select();
const { data: tanks } = await supabase.from('storage').select();
```

### API Integration Points

- Real IoT data ingestion
- Third-party energy forecasts
- Payment gateway (Marketplace)
- Email/SMS notifications
- Real map service (Google Maps)

---

## Key Principles

1. **Separation of Concerns**: Pages handle features, components handle UI
2. **Single Responsibility**: Each function does one thing
3. **Immutability**: Never modify state directly
4. **Type Safety**: All data has types
5. **Scalability**: Structure supports adding new features
6. **Maintainability**: Clean code easy to understand
7. **Responsiveness**: Works on mobile, tablet, desktop

---

This architecture supports the current scope and scales for future enhancements like database integration, real IoT connectivity, and mobile apps.
