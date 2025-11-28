# HydroSmart Development Guide

## Quick Start

### 1. Prerequisites
- Node.js 16+ installed
- npm or yarn package manager
- Text editor (VS Code recommended)
- Git (optional, for version control)

### 2. Installation & Setup

```bash
# Install dependencies
npm install

# Start development server
npm run dev

# Build for production
npm run build

# Preview production build
npm run preview
```

After running `npm run dev`, open your browser to `http://localhost:5173`

### 3. Default Login Credentials

Use any of these demo accounts (password doesn't matter):
- **admin@hydrosmart.com** - Admin access to all features
- **operator@hydrosmart.com** - Plant operator access
- **energy@hydrosmart.com** - Energy manager access
- **logistics@hydrosmart.com** - Logistics coordinator access
- **buyer@example.com** - Buyer/customer access

---

## Architecture Overview

### File Structure
```
src/
├── components/          # Reusable UI components
│   ├── AlertBanner.tsx
│   ├── GaugeChart.tsx
│   ├── LineChart.tsx
│   ├── MetricCard.tsx
│   └── TankCard.tsx
├── contexts/           # React contexts
│   └── AuthContext.tsx
├── pages/              # Page components
│   ├── Analytics.tsx
│   ├── Alerts.tsx
│   ├── Certificates.tsx
│   ├── Dashboard.tsx
│   ├── EnergyManagement.tsx
│   ├── Logistics.tsx
│   ├── Login.tsx
│   ├── Marketplace.tsx
│   └── Storage.tsx
├── services/           # Business logic
│   ├── dataService.ts  # Mock data & analytics
│   └── telemetrySimulator.ts # Real-time data generation
├── types/              # TypeScript types
│   └── index.ts
├── App.tsx             # Main app component
├── main.tsx            # Entry point
└── index.css           # Global styles
```

### Key Components

#### **TelemetrySimulator** (`services/telemetrySimulator.ts`)
- Generates realistic hydrogen production data
- Simulates solar/wind/hydro power variations
- Updates storage tank levels dynamically
- Generates random alerts

#### **AuthContext** (`contexts/AuthContext.tsx`)
- Manages user authentication state
- Persists user in localStorage
- Provides useAuth hook for components

#### **Pages**
Each page is a complete feature module:
- Dashboard: Real-time production monitoring
- Energy Management: Production optimization
- Storage: Tank inventory management
- Alerts: Safety and emergency management
- Certificates: Digital green hydrogen certificates
- Marketplace: Buy/sell hydrogen interface
- Logistics: Delivery tracking
- Analytics: Performance metrics

---

## Development Workflow

### Running the App

```bash
# Terminal 1: Start dev server
npm run dev

# Terminal 2 (optional): Type checking
npm run typecheck

# Terminal 3 (optional): Linting
npm run lint
```

Visit `http://localhost:5173` in your browser.

### Making Changes

1. **Edit a component**: Changes auto-reload in browser
2. **Add a new page**:
   - Create `src/pages/NewPage.tsx`
   - Add to navigation in `src/App.tsx`
   - Import and add route

3. **Modify data service**:
   - Edit `src/services/dataService.ts` for mock data
   - Edit `src/services/telemetrySimulator.ts` for real-time data

4. **Add a component**:
   - Create in `src/components/`
   - Export and use in pages

### Example: Adding a New Feature

```typescript
// 1. Create component (src/components/NewWidget.tsx)
export const NewWidget = () => {
  return (
    <div className="bg-white rounded-lg shadow-md p-6">
      <h2 className="text-lg font-semibold">New Feature</h2>
    </div>
  );
};

// 2. Use in page (src/pages/Dashboard.tsx)
import { NewWidget } from '../components/NewWidget';

export const Dashboard = () => {
  return (
    <div className="space-y-6">
      {/* ... existing content ... */}
      <NewWidget />
    </div>
  );
};
```

---

## Real-Time Data System

### How Telemetry Works

```typescript
// In Dashboard.tsx
const [currentData, setCurrentData] = useState<TelemetryData>(simulator.generateTelemetry());
const [history, setHistory] = useState<TelemetryData[]>([]);

useEffect(() => {
  const interval = setInterval(() => {
    const newData = simulator.generateTelemetry();
    setCurrentData(newData);
    setHistory(prev => [...prev.slice(-29), newData]); // Keep last 30 data points
  }, 5000); // Update every 5 seconds

  return () => clearInterval(interval);
}, []);
```

### Modifying Data Generation

Edit `src/services/telemetrySimulator.ts`:

```typescript
private generateSolarPower(timeInHours: number): number {
  const hourOfDay = (timeInHours % 24);

  if (hourOfDay < 6 || hourOfDay > 18) return 0;

  // Adjust these values to change solar power generation
  const solarCurve = Math.sin(((hourOfDay - 6) / 12) * Math.PI);
  const basePower = 800 * solarCurve; // Change 800 for different peak power
  const cloudVariation = (Math.sin(timeInHours * 2) * 0.2 + 0.8);

  return Math.max(0, basePower * cloudVariation + (Math.random() - 0.5) * 50);
}
```

---

## Styling with Tailwind CSS

### Colors Used

- **Primary**: Blue (`bg-blue-500`, `text-blue-600`)
- **Secondary**: Cyan (`bg-cyan-500`, `text-cyan-600`)
- **Success**: Green (`bg-green-500`, `text-green-600`)
- **Warning**: Orange (`bg-orange-500`, `text-orange-600`)
- **Danger**: Red (`bg-red-500`, `text-red-600`)

### Common Classes

```html
<!-- Cards -->
<div class="bg-white rounded-lg shadow-md p-6">

<!-- Buttons -->
<button class="px-4 py-2 bg-blue-500 hover:bg-blue-600 text-white rounded-lg transition-colors">

<!-- Grid layouts -->
<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">

<!-- Text hierarchy -->
<h1 class="text-3xl font-bold text-gray-900">
<p class="text-sm text-gray-600">
```

### Responsive Design

- Mobile-first: `block md:hidden` for mobile only
- Tablet+: `hidden md:block lg:hidden` for tablet
- Desktop+: `hidden lg:block` for desktop
- Full width: Use `w-full`

---

## State Management Pattern

### Local State (Single Component)

```typescript
const [alerts, setAlerts] = useState<AlertType[]>([]);

setAlerts(prev => [newAlert, ...prev]); // Immutable update
```

### Global State (Auth)

```typescript
// In AuthContext.tsx
const { user, login, logout, isAuthenticated } = useAuth();
```

### Real-Time Updates (Effects)

```typescript
useEffect(() => {
  const interval = setInterval(() => {
    // Update state
  }, 5000);

  return () => clearInterval(interval); // Cleanup
}, [dependencies]);
```

---

## Testing the App

### Test Different Roles

1. Login as **admin** → Full access to all features
2. Login as **operator** → Can manage production and storage
3. Login as **buyer** → Can only access marketplace and certificates
4. Test **emergency shutdown** in Alerts page

### Test Real-Time Features

1. **Dashboard**: Watch production rates update every 5 seconds
2. **Storage**: See tank levels fill and pressure increase
3. **Marketplace**: Place an order and track it
4. **Logistics**: Watch delivery vehicle move on map
5. **Alerts**: Generate alerts by waiting 10-20 seconds

### Mobile Testing

```bash
# Build and preview
npm run build
npm run preview

# Or use browser DevTools:
# Press F12 → Toggle device toolbar (Ctrl+Shift+M)
```

---

## Database Integration (Optional - Coming Soon)

Currently using mock data. To connect to Supabase:

```typescript
// Would use Supabase client
import { createClient } from '@supabase/supabase-js';

const supabase = createClient(SUPABASE_URL, SUPABASE_ANON_KEY);

// Replace mock data with real queries
const { data: telemetry } = await supabase
  .from('telemetry')
  .select('*')
  .order('timestamp', { ascending: false })
  .limit(30);
```

---

## Common Tasks

### Add a New Alert Type

1. Edit `src/types/index.ts`:
```typescript
export type AlertType = 'pressure' | 'temperature' | 'leak' | 'low_inventory' | 'system' | 'new_type';
```

2. Update simulator in `src/services/telemetrySimulator.ts`:
```typescript
const alertTypes = [
  { type: 'new_type', severity: 'high', getMessage: () => 'New alert message' },
  // ...
];
```

### Change Update Frequency

In any page with `setInterval`:
```typescript
// Change 5000ms (5 seconds) to desired interval
}, 5000); // ← Edit this number
```

### Modify Chart Data

Charts use `LineChart` component with data array:
```typescript
const chartData = history.map((d, i) => ({
  label: new Date(d.timestamp).toLocaleTimeString(),
  value: d.hydrogenProductionKgph
}));

<LineChart data={chartData} color="#10b981" />
```

### Add New Navigation Item

In `src/App.tsx`:
```typescript
const navigation = [
  { id: 'dashboard' as Page, name: 'Dashboard', icon: LayoutDashboard },
  // ... add new item:
  { id: 'new_page' as Page, name: 'New Page', icon: SomeIcon },
];
```

---

## Troubleshooting

### Port Already in Use
```bash
# Kill process on port 5173
# macOS/Linux:
lsof -ti:5173 | xargs kill -9

# Windows:
netstat -ano | findstr :5173
taskkill /PID <PID> /F
```

### Dependencies Issues
```bash
# Clear cache and reinstall
rm -rf node_modules package-lock.json
npm install
```

### Build Errors
```bash
# Check TypeScript errors
npm run typecheck

# Check linting issues
npm run lint

# Clear build cache
rm -rf dist
npm run build
```

### Data Not Updating
- Check browser console for errors (F12)
- Verify useEffect dependencies
- Ensure interval is not being cleared

---

## Performance Tips

1. **Limit Chart History**: Keep last 30-50 data points, not 1000+
2. **Throttle Updates**: Don't update faster than 5 seconds
3. **Memoize Components**: Use `React.memo()` for static components
4. **Lazy Load Pages**: Import pages on demand
5. **Monitor Bundle Size**: `npm run build` shows gzip size

---

## Next Steps for Enhancement

1. **Connect to Supabase**: Replace mock data with real database
2. **Add User Settings**: Store preferences in database
3. **Export Reports**: Add CSV/PDF export functionality
4. **Push Notifications**: Real-time alerts via email/SMS
5. **Mobile App**: React Native version
6. **API Integration**: Connect to real IoT devices
7. **Machine Learning**: Predictive analytics
8. **Multi-plant Support**: Handle multiple production facilities

---

## Resources

- **React Docs**: https://react.dev
- **Tailwind CSS**: https://tailwindcss.com/docs
- **Lucide Icons**: https://lucide.dev
- **TypeScript**: https://www.typescriptlang.org/docs
- **Vite**: https://vitejs.dev

---

## Support

For issues or questions:
1. Check browser console (F12)
2. Review relevant component code
3. Check git history for recent changes
4. Verify all dependencies are installed

Happy developing! 🚀
