# HydroSmart - Quick Start Guide

## 30-Second Setup

```bash
# 1. Install dependencies
npm install

# 2. Start development server
npm run dev

# 3. Open browser
# Visit: http://localhost:5173

# 4. Login with any demo account
# Email: admin@hydrosmart.com
# Password: (any password)
```

That's it! You're running HydroSmart.

---

## What You'll See

### Login Page
- Clean, modern design with HydroSmart branding
- Demo account options listed below the login form
- Use any email listed to login (password not required)

### Main Dashboard
After login, you'll see:

```
┌─────────────────────────────────────────────────────────────┐
│  HydroSmart  [User Name] [Logout]                           │
├──────────────┬──────────────────────────────────────────────┤
│ Dashboard    │  Real-Time Production Dashboard              │
│ Energy       │                                               │
│ Storage      │  [Metric Card] [Metric Card] [Metric Card]   │
│ Alerts       │  [Gauge] [Gauge] [Gauge]                     │
│ Certificates │  [Chart] [Chart]                             │
│ Marketplace  │  [Status Info]                               │
│ Logistics    │                                               │
│ Analytics    │                                               │
└──────────────┴──────────────────────────────────────────────┘
```

### Navigation

**Sidebar (Desktop)**: Click items to switch between pages
**Mobile Menu**: Hamburger icon (☰) in top right

---

## Core Features at a Glance

### 1. Dashboard - Real-Time Monitoring
- **What**: See live hydrogen production data
- **Data Updates**: Every 5 seconds
- **Shows**: Solar power, wind power, production rate, efficiency
- **Charts**: Production trends over last 2.5 minutes

### 2. Energy Management - Optimization
- **What**: AI-powered production scheduling
- **Modes**: Max Production or Min Cost
- **Data**: 24-hour energy forecast
- **Output**: Recommended production schedule with cost analysis

### 3. Storage - Tank Management
- **What**: Real-time hydrogen storage monitoring
- **Shows**: 4 storage tanks with pressure, temperature, fill level
- **Alerts**: Low inventory warnings
- **Status**: Normal, Low, Full, or Critical

### 4. Safety & Alerts - Emergency Management
- **What**: System monitoring and emergency control
- **Button**: Emergency Shutdown (red button, top right)
- **Alerts**: Real-time system alerts appearing in top-right corner
- **Features**: Resolve alerts, view history

### 5. Certificates - Green Credentials
- **What**: Digital certificates for hydrogen batches
- **Shows**: Renewable energy source, CO₂ saved, production details
- **Download**: Get certificate PDFs
- **Purpose**: Prove hydrogen is 100% green

### 6. Marketplace - Buy/Sell Hydrogen
- **What**: Hydrogen trading platform
- **Action**: Click "Place Order" button
- **Form**: Enter quantity, location, delivery date
- **Result**: Order appears in list with status tracking

### 7. Logistics - Delivery Tracking
- **What**: Real-time truck tracking system
- **Map**: Animated vehicles moving to destinations
- **Tabs**: Active, Pending, and Completed deliveries
- **Status**: In-transit updates with progress bar

### 8. Analytics - Performance Reports
- **What**: Business intelligence dashboard
- **Metrics**: Total production, efficiency, costs, carbon savings
- **Charts**: Weekly and monthly trends
- **KPIs**: Key performance indicators

---

## Interactive Features

### Try These Actions

#### Place an Order (Marketplace)
1. Go to **Marketplace** → Click **Place Order**
2. Enter: 150 kg, "Mumbai Industrial Zone", Today+5 days
3. Click **Confirm Order**
4. Watch it appear in orders list

#### Track a Delivery (Logistics)
1. Go to **Logistics**
2. See animated truck moving on map (updates every 5 seconds)
3. Click on delivery in "Active Deliveries" to see details
4. Watch progress bar update in real-time

#### Trigger Emergency Shutdown (Alerts)
1. Go to **Safety & Alerts**
2. Click **Emergency Shutdown** button (red, top right)
3. All production stops, emergency alert appears
4. Click **Resume Operations** to restart

#### Generate Alerts (Any Page)
1. Wait 10-20 seconds
2. Alert notification appears in top-right corner
3. Can be pressure, temperature, or low inventory alert
4. Click X to dismiss or go to Alerts page to resolve

#### View Real-Time Data (Dashboard)
1. Go to **Dashboard**
2. Watch numbers update every 5 seconds
3. Production rate changes based on solar/wind power
4. Charts build up as data is collected

---

## File Organization Quick Reference

### Where Things Are

| What | Where | File |
|------|-------|------|
| Real-time data generation | Services | `services/telemetrySimulator.ts` |
| Mock data | Services | `services/dataService.ts` |
| User login | Context | `contexts/AuthContext.tsx` |
| Main app layout | Root | `App.tsx` |
| Reusable widgets | Components | `components/*.tsx` |
| Page content | Pages | `pages/*.tsx` |
| Type definitions | Types | `types/index.ts` |

---

## Common Questions

### Q: How do I change the update frequency?
**A:** Edit the interval in any page component. Look for `setInterval(..., 5000)` and change 5000 to milliseconds desired (e.g., 2000 = 2 seconds).

### Q: How do I modify the generated data?
**A:** Edit `services/telemetrySimulator.ts`. Change formulas in methods like `generateSolarPower()`, `generateWindPower()`, etc.

### Q: Can I add a new page?
**A:** Yes!
1. Create `src/pages/MyPage.tsx`
2. Add to navigation array in `App.tsx`
3. Add case in renderPage() function

### Q: Where's the database?
**A:** Currently using mock data in memory. To add real database, install Supabase client and connect in `services/dataService.ts`.

### Q: Can I modify the styling?
**A:** Yes! Styling uses Tailwind CSS. Edit class names directly in components or modify `src/index.css` for global styles.

### Q: How do I handle multiple users?
**A:** Auth system is ready. Just connect database to store user roles and permissions in Supabase `users` table.

---

## Development Mode Tips

### Hot Reload
All changes automatically reload in browser. Just edit and save!

### Browser DevTools
Press `F12` to open:
- **Console**: See error messages and logs
- **Network**: Check API calls (none yet, only mock data)
- **Application**: View localStorage for stored user

### Toggle Mobile View
Press `Ctrl+Shift+M` (Windows/Linux) or `Cmd+Shift+M` (Mac) to see mobile layout.

### Simulate Slow Network
In DevTools → Network tab → Change "No throttling" to "Slow 3G" to test performance.

---

## Next Steps

### After Running the App

1. **Explore All Pages**: Click through each navigation item
2. **Test Interactions**: Place orders, trigger alerts, track deliveries
3. **Check Real-Time**: Watch data update in real-time
4. **Read Code**: Understand component structure
5. **Make Changes**: Try editing a component

### For Development

1. **Read DEVELOPMENT_GUIDE.md**: Detailed architecture and patterns
2. **Connect Database**: Replace mock data with Supabase
3. **Add Features**: New pages, components, or functionality
4. **Deploy**: Build for production and host

### For Production

1. **Connect Real Database**: Set up Supabase schema
2. **Add Authentication**: Real login system
3. **Deploy**: Push to Vercel, Netlify, or your host
4. **Monitor**: Set up error tracking and analytics
5. **Scale**: Add more features as needed

---

## Key Hotkeys

| Action | Shortcut |
|--------|----------|
| Open DevTools | F12 |
| Mobile View | Ctrl+Shift+M |
| Reload Page | Ctrl+R or F5 |
| Hard Reload | Ctrl+Shift+R |
| Inspect Element | Ctrl+Shift+C |

---

## Build for Production

```bash
# Create optimized build
npm run build

# Creates 'dist/' folder with production files

# Test production build locally
npm run preview

# Deploy 'dist/' folder to your hosting
# (Vercel, Netlify, AWS S3, etc.)
```

---

## Troubleshooting

### Nothing showing after login?
- Check browser console (F12) for errors
- Try hard refresh: `Ctrl+Shift+R`
- Clear localStorage: DevTools → Application → Clear Storage

### Data not updating?
- Check if 5 second interval is running
- Open console to see any errors
- Try navigating away and back to page

### Styling looks broken?
- Hard refresh the page
- Check if Tailwind CSS loaded (look for page background color)
- Rebuild: `npm run build`

### Port 5173 already in use?
```bash
# Kill the process
# macOS/Linux:
lsof -ti:5173 | xargs kill -9

# Windows:
netstat -ano | findstr :5173
taskkill /PID <PID> /F
```

---

## Getting Help

1. **Check Errors**: Browser console (F12) shows what went wrong
2. **Read Code Comments**: Components have inline explanations
3. **Review Examples**: Similar components show the pattern
4. **Check Dependencies**: Ensure npm install completed successfully

---

**Ready to go?** Run `npm run dev` and start exploring! 🚀
