# climate-cobenefits-dashboard
climate-cobenefits-dashboard
# UK Climate Action Co-Benefits Dashboard - Code Documentation

## Project Overview

This dashboard visualizes climate action co-benefits data from the Edinburgh Climate Change Institute (ECCI) UK Co-Benefits Atlas, designed specifically for policy makers participating in The Data Lab's Data Visualisation Competition 2025.

## Architecture

### Technology Stack

- **Frontend Framework:** React 19
- **Build Tool:** Vite 7
- **Styling:** Tailwind CSS 4
- **UI Components:** shadcn/ui
- **Charts:** Recharts
- **Routing:** Wouter
- **Language:** TypeScript

### Project Structure

```
climate-cobenefits-dashboard/
├── client/                    # Frontend application
│   ├── public/               # Static assets
│   │   └── data/            # JSON data files
│   │       ├── summary_stats.json
│   │       ├── temporal_data.json
│   │       ├── top_areas.json
│   │       └── metadata.json
│   └── src/
│       ├── components/       # React components
│       │   ├── ui/          # shadcn/ui components
│       │   ├── CoBenefitsChart.tsx
│       │   ├── TemporalChart.tsx
│       │   ├── CostBenefitChart.tsx
│       │   └── KeyInsights.tsx
│       ├── pages/
│       │   └── Dashboard.tsx # Main dashboard page
│       ├── App.tsx          # Application root
│       ├── main.tsx         # Entry point
│       └── index.css        # Global styles
├── process_data.py          # Data processing script
├── package.json             # Dependencies
└── vite.config.ts          # Vite configuration
```

## Key Components

### 1. Dashboard.tsx

**Purpose:** Main dashboard page with tabbed interface

**Features:**
- Hero section with total net benefit (£145.6B)
- Three tabs: Overview, Temporal Trends, Key Insights
- Responsive layout with gradient backgrounds
- Footer with data attribution

**State Management:**
```typescript
const [summaryStats, setSummaryStats] = useState<SummaryStats | null>(null);
const [loading, setLoading] = useState(true);
const [error, setError] = useState<string | null>(null);
```

**Data Loading:**
```typescript
useEffect(() => {
  fetch('/data/summary_stats.json')
    .then(res => res.json())
    .then(data => setSummaryStats(data))
    .catch(err => setError(err.message));
}, []);
```

### 2. CoBenefitsChart.tsx

**Purpose:** Horizontal bar chart comparing 11 co-benefit types

**Features:**
- Color-coded bars (green for benefits, red for costs)
- Sorted by absolute value
- Custom tooltips with formatted values
- Responsive design

**Data Transformation:**
```typescript
const chartData = Object.entries(data)
  .filter(([key]) => key !== 'total_net_benefit' && COBENEFIT_NAMES[key])
  .map(([key, value]) => ({
    name: COBENEFIT_NAMES[key],
    value: stats.total / 1000, // Convert to billions
    isBenefit: stats.total > 0
  }))
  .sort((a, b) => Math.abs(b.rawValue) - Math.abs(a.rawValue));
```

### 3. TemporalChart.tsx

**Purpose:** Line chart showing annual co-benefits from 2025-2050

**Features:**
- Multi-line chart with selectable co-benefits
- Checkbox filters for each co-benefit type
- Custom color scheme for each line
- Interactive tooltips

**State Management:**
```typescript
const [temporalData, setTemporalData] = useState<TemporalData[]>([]);
const [selectedCobenefits, setSelectedCobenefits] = useState<string[]>([
  'physical_activity', 'air_quality', 'noise', 'excess_cold'
]);
```

### 4. CostBenefitChart.tsx

**Purpose:** Stacked bar chart showing benefits vs costs

**Features:**
- Two bars: total benefits and total costs
- Summary metrics (ROI, net benefit)
- Detailed tooltip with breakdown
- Key message highlighting ROI

**Calculations:**
```typescript
const totalBenefits = Object.entries(data)
  .filter(([key, value]) => typeof value === 'object' && value.total > 0)
  .reduce((sum, [_, value]) => sum + (value as CoBenefitStats).total, 0) / 1000;

const roi = ((totalBenefits / totalCosts) - 1) * 100;
```

### 5. KeyInsights.tsx

**Purpose:** Policy insights and recommendations

**Features:**
- Five key insight cards with icons
- Calculated metrics (health percentage, ROI)
- Policy recommendations section
- Color-coded cards by theme

**Insights Calculation:**
```typescript
const healthBenefits = (
  (data.physical_activity as CoBenefitStats).total +
  (data.air_quality as CoBenefitStats).total +
  (data.excess_cold as CoBenefitStats).total +
  (data.diet_change as CoBenefitStats).total +
  (data.dampness as CoBenefitStats).total +
  (data.excess_heat as CoBenefitStats).total
) / 1000;
```

## Data Processing

### process_data.py

**Purpose:** Convert Excel files to JSON format

**Process:**
1. Read Level 1, 2, and 3 Excel files
2. Calculate summary statistics for each co-benefit
3. Aggregate temporal data by year and co-benefit type
4. Extract top 100 areas by total benefit
5. Create metadata file with co-benefit descriptions

**Output Files:**
- `summary_stats.json` - Total statistics for all co-benefits
- `temporal_data.json` - Annual breakdown (2025-2050)
- `top_areas.json` - Top 100 areas ranked by benefit
- `metadata.json` - Co-benefit metadata and units

## Styling

### Color Scheme

**Primary Colors:**
- Emerald: `#10b981` (benefits, positive metrics)
- Red: `#ef4444` (costs, negative metrics)
- Teal: `#14b8a6` (accents)

**Gradients:**
- Hero: `from-emerald-600 to-teal-600`
- Background: `from-emerald-50 via-teal-50 to-cyan-50`
- Footer: `from-emerald-800 to-teal-800`

### Responsive Design

**Breakpoints:**
- Mobile: < 768px
- Tablet: 768px - 1024px
- Desktop: > 1024px

**Grid Layouts:**
```css
grid-cols-1 md:grid-cols-3  /* Hero stats */
grid-cols-2 md:grid-cols-3 lg:grid-cols-4  /* Filters */
```

## Data Structure

### SummaryStats Interface

```typescript
interface CoBenefitStats {
  total: number;
  mean: number;
  median: number;
  min: number;
  max: number;
}

interface SummaryStats {
  [key: string]: CoBenefitStats | number;
  total_net_benefit: number;
}
```

### Temporal Data Interface

```typescript
interface TemporalData {
  cobenefit: string;
  data: { [year: string]: number };
}
```

## Build Process

### Development

```bash
pnpm install
pnpm dev
```

### Production Build

```bash
pnpm build
```

**Output:**
- `dist/public/` - Static files ready for deployment
- `dist/public/index.html` - Entry point
- `dist/public/assets/` - Bundled CSS and JS
- `dist/public/data/` - JSON data files

### Build Optimization

- Code splitting
- Minification
- Tree shaking
- Asset optimization

**Bundle Sizes:**
- JavaScript: ~950KB (minified)
- CSS: ~118KB (minified)
- Total: ~1.1MB

## Deployment

### Static Hosting Requirements

- No backend server required
- Supports client-side routing
- HTTPS recommended
- Modern browser support

### Recommended Platforms

1. **GitHub Pages** - Free, simple setup
2. **Netlify** - Free tier with CI/CD
3. **Vercel** - Free with edge network
4. **Cloudflare Pages** - Free with CDN

## Performance Considerations

### Optimization Strategies

1. **Data Loading:** JSON files loaded asynchronously
2. **Chart Rendering:** Recharts uses canvas for performance
3. **Code Splitting:** Dynamic imports for routes
4. **Caching:** Static assets cached by CDN

### Load Time Targets

- First Contentful Paint: < 1.5s
- Time to Interactive: < 3s
- Total Load Time: < 5s

## Accessibility

### WCAG AA Compliance

- Color contrast ratios meet AA standards
- Keyboard navigation supported
- Screen reader compatible
- Focus indicators visible
- Semantic HTML structure

### Color Contrast

- Text on emerald background: 4.5:1
- Text on white background: 21:1
- Interactive elements: Clear focus states

## Browser Compatibility

**Supported Browsers:**
- Chrome 90+
- Firefox 88+
- Safari 14+
- Edge 90+

**Polyfills:** Not required for modern browsers

## Future Enhancements

### Potential Features

1. Geographic map visualization
2. Data export functionality (CSV/PDF)
3. Custom date range filtering
4. Comparison between regions
5. Print-optimized layouts

### Performance Improvements

1. Lazy loading for charts
2. Virtual scrolling for large datasets
3. Service worker for offline support
4. Progressive Web App (PWA) features

## Troubleshooting

### Common Issues

**Issue:** Charts not displaying
**Solution:** Check data files are in `/public/data/` directory

**Issue:** Build fails
**Solution:** Clear node_modules and reinstall: `rm -rf node_modules && pnpm install`

**Issue:** TypeScript errors
**Solution:** Check interface definitions match data structure

## Credits

**Data Source:** Edinburgh Climate Change Institute (ECCI) - UK Co-Benefits Atlas

**Competition:** The Data Lab - Data Visualisation Competition 2025

**Framework:** React, Recharts, Tailwind CSS, shadcn/ui

**Created:** November 2025
