# UK Climate Action Co-Benefits Dashboard - Deployment Guide

## Overview

This dashboard is a static web application built with React, Recharts, and Tailwind CSS. It visualizes climate action co-benefits data for policy makers.

## Deployment Options

### Option 1: GitHub Pages (Recommended - Free)

**Steps:**

1. Create a GitHub repository
2. Push the contents of this deployment package to the repository
3. Go to repository Settings → Pages
4. Select "Deploy from a branch" and choose your main branch
5. Your site will be live at `https://yourusername.github.io/repository-name`

**Custom Domain:** You can add a custom domain in the GitHub Pages settings.

### Option 2: Netlify (Free)

**Steps:**

1. Sign up at [netlify.com](https://www.netlify.com)
2. Click "Add new site" → "Deploy manually"
3. Drag and drop the deployment-package folder
4. Your site will be live instantly with a Netlify subdomain
5. Optional: Add a custom domain in site settings

**Features:**
- Automatic HTTPS
- Continuous deployment from Git
- Form handling
- Free subdomain: `yoursite.netlify.app`

### Option 3: Vercel (Free)

**Steps:**

1. Sign up at [vercel.com](https://vercel.com)
2. Click "Add New Project"
3. Import from Git or upload the deployment package
4. Deploy with one click
5. Your site will be live at `yoursite.vercel.app`

**Features:**
- Automatic HTTPS
- Edge network (fast global delivery)
- Analytics available
- Free custom domains

### Option 4: Cloudflare Pages (Free)

**Steps:**

1. Sign up at [pages.cloudflare.com](https://pages.cloudflare.com)
2. Create a new project
3. Connect your Git repository or upload files
4. Deploy automatically
5. Site live at `yoursite.pages.dev`

**Features:**
- Cloudflare CDN
- Unlimited bandwidth
- Automatic HTTPS
- Fast global performance

### Option 5: Tiiny.host (Quick & Simple - Free)

**Steps:**

1. Go to [tiiny.host](https://tiiny.host)
2. Create a ZIP file of the deployment-package folder
3. Upload the ZIP file
4. Get instant hosting with a unique URL
5. Free for 7 days, then requires renewal

**Best for:** Quick demos and temporary hosting

## File Structure

```
deployment-package/
├── index.html          # Main HTML file
├── assets/             # CSS and JavaScript bundles
│   ├── index-*.css    # Compiled styles
│   └── index-*.js     # Compiled React application
├── data/               # JSON data files
│   ├── summary_stats.json
│   ├── temporal_data.json
│   ├── top_areas.json
│   └── metadata.json
└── DEPLOYMENT_GUIDE.md # This file
```

## Requirements

- **Server:** Any static file server (no backend required)
- **HTTPS:** Recommended for production (all platforms provide this)
- **Browser Support:** Modern browsers (Chrome, Firefox, Safari, Edge)

## Configuration

No configuration needed! The dashboard is ready to deploy as-is.

## Data Files

The dashboard loads data from the `/data/` directory. All data files are included:

- `summary_stats.json` - Aggregated co-benefit statistics
- `temporal_data.json` - Annual breakdown (2025-2050)
- `top_areas.json` - Top 100 areas by benefit
- `metadata.json` - Co-benefit metadata and descriptions

## Performance

- **Bundle Size:** ~950KB JavaScript, ~118KB CSS
- **Data Size:** ~52KB total JSON files
- **Load Time:** < 2 seconds on average connection
- **Optimization:** Code is minified and optimized for production

## Customization

To modify the dashboard:

1. Edit the source files in the original project
2. Run `pnpm build` to regenerate the production files
3. Copy the new `dist/public/*` files to your deployment

## Support

For issues or questions about the dashboard, refer to the original project documentation.

## License

Created for The Data Lab - Data Visualisation Competition 2025
Data Source: Edinburgh Climate Change Institute (ECCI) - UK Co-Benefits Atlas

---

**Quick Start:** Upload the entire `deployment-package` folder to any of the platforms above for instant deployment!
