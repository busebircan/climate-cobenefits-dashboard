# Climate Action Co-Benefits Dashboard

An interactive data visualization dashboard showcasing **£145.6 billion** in co-benefits from climate action across 46,426 UK small areas (2025-2050). Built for The Data Lab's 2025 Data Visualisation Competition.

## Overview

Climate action is often framed solely in terms of carbon reduction, but this dashboard reveals the massive hidden value: health improvements, economic benefits, and enhanced quality of life across every UK community. Using data from the Edinburgh Climate Change Institute's UK Co-Benefits Atlas, this visualization demonstrates that climate policy is simultaneously health policy, economic policy, and social policy.

The dashboard transforms complex academic research into an accessible, interactive experience that allows policymakers, researchers, and citizens to explore how climate action creates value in their local areas.

## Key Features

### 📊 Interactive Data Visualization

The dashboard presents three integrated views of the co-benefits data, each designed to answer different questions about the distribution and composition of climate action value.

**Overview Page** provides immediate context with the £145.6 billion headline figure, followed by four key insights that summarize the most important findings. A breakdown chart shows the relative contribution of each co-benefit type, while detailed statistics cards reveal the full picture across all 11 categories including physical activity (£129.9B), air quality (£48.3B), and noise reduction (£34.2B).

**Geographic Page** features an interactive choropleth map of the UK with 46,426 small areas color-coded by total co-benefits. Users can hover over regions to see local authority names and benefit values, then explore detailed rankings of the top 100 and bottom 100 areas with progressive loading for optimal performance.

**About Page** explains the methodology, data sources, and assumptions underlying the analysis, ensuring transparency and enabling informed interpretation of the results.

### 🗺️ Interactive UK Map

The Leaflet-based choropleth map provides spatial context for the co-benefits distribution. Each of the 46,426 small areas is color-coded using a seven-tier scale from dark green (high benefits) to orange-red (net costs), making geographic patterns immediately visible. Hovering over any area displays the local authority name and total co-benefits, while clicking opens a detailed popup with additional information.

The map automatically fits to UK bounds on load and includes a legend explaining the color scale. This geographic visualization reveals that benefits are concentrated in urban areas with high population density, where active travel and air quality improvements have the greatest impact.

### 📈 Progressive Data Loading

To ensure fast initial page load and smooth user experience, the area rankings implement progressive disclosure. The top and bottom 100 lists initially display 20 areas each, with a "See More" button that loads an additional 20 areas at a time. The button shows current progress (e.g., "See More (20 of 100)") and disappears when all areas are visible.

This approach balances completeness with performance, allowing users to quickly scan the highest-impact areas while retaining access to the full dataset for detailed analysis.

### 🎨 Climate-Themed Design

The visual design reinforces the environmental theme through a carefully crafted animated background in the hero section. Prominent green and blue radial gradients (opacity 0.4-0.5) float and drift across the blue gradient background with 15-second and 20-second animation cycles, creating noticeable movement that evokes atmospheric and oceanic themes while maintaining professional aesthetics. The orbs move up to 80px in various directions, providing dynamic visual interest without overwhelming the content.

The color palette uses accessible combinations that meet WCAG contrast standards, ensuring readability for all users. Green indicates benefits, amber represents costs, and the overall aesthetic communicates professionalism appropriate for policy and research audiences.

## Technology Stack

The dashboard is built with modern web technologies optimized for performance, maintainability, and developer experience.

| Category | Technologies |
|----------|-------------|
| **Frontend Framework** | React 18 with TypeScript for type-safe component development |
| **Build Tool** | Vite for fast development and optimized production builds |
| **UI Framework** | Tailwind CSS 4 with shadcn/ui components for consistent design |
| **Routing** | wouter for lightweight client-side routing |
| **Mapping** | Leaflet.js with react-leaflet for interactive choropleth visualization |
| **Backend** | Express 4 with tRPC 11 for type-safe API communication |
| **Database** | Drizzle ORM with TiDB for scalable data storage |
| **Authentication** | Manus OAuth integration for secure user management |

The architecture follows a modern full-stack pattern with tRPC providing end-to-end type safety between the React frontend and Express backend. Data flows from JSON files processed from the original Excel datasets, through the backend API, to the interactive frontend components.

## Data Sources

All data originates from the Edinburgh Climate Change Institute's UK Co-Benefits Atlas, a comprehensive research project quantifying the health, economic, and social benefits of climate action across the United Kingdom. The analysis covers 46,426 small areas (Lower Layer Super Output Areas in England and Wales, Data Zones in Scotland) over the period 2025-2050.

The dataset includes 11 distinct co-benefit types measured in millions of GBP (net present value, 2025 base year, discounted per UK Green Book guidance):

**Health Benefits** arise from reduced air pollution, increased physical activity through active travel, warmer homes through better insulation, healthier diets from plant-based shifts, reduced dampness and mold, and cooler homes through improved ventilation.

**Economic Benefits** include reduced road maintenance costs, decreased traffic congestion, and improved road safety outcomes.

**Quality of Life** impacts encompass noise reduction from quieter transport and changes in travel time (hassle costs).

The original data was provided in three Excel files (Level_1, Level_2, Level_3) containing area-level aggregations, temporal breakdowns, and damage pathway details respectively. For this dashboard, the Level_1 data was processed into JSON format and merged with geographic boundary data from UK government sources to enable the interactive map visualization.

## Installation & Setup

### Prerequisites

Ensure you have the following installed on your system:

- Node.js 22.13.0 or higher
- pnpm package manager
- Git for version control

### Local Development

Clone the repository and install dependencies:

```bash
git clone https://github.com/yourusername/climate-cobenefits-dashboard.git
cd climate-cobenefits-dashboard
pnpm install
```

Start the development server:

```bash
pnpm dev
```

The application will be available at `http://localhost:3000`. The development server includes hot module replacement for instant updates during development.

### Environment Variables

The application requires several environment variables for full functionality. Create a `.env` file in the project root with the following structure:

```env
DATABASE_URL=your_database_connection_string
JWT_SECRET=your_jwt_secret_key
OAUTH_SERVER_URL=your_oauth_server_url
VITE_APP_ID=your_app_id
```

For local development without authentication features, the dashboard will function with mock data using only the public JSON files in `client/public/data/`.

### Production Build

Generate an optimized production build:

```bash
pnpm build
pnpm start
```

The build process creates minified bundles with code splitting for optimal loading performance. Static assets are fingerprinted for effective caching.

## Project Structure

The codebase follows a clear separation between client-side and server-side code, with shared types and utilities accessible to both.

```
climate_cobenefits_dashboard/
├── client/
│   ├── public/
│   │   └── data/              # Processed JSON data files
│   │       ├── summary.json
│   │       ├── top_100_areas.json
│   │       ├── bottom_100_areas.json
│   │       ├── area_names.json
│   │       └── uk_areas_with_data.geojson
│   ├── src/
│   │   ├── components/        # Reusable UI components
│   │   │   ├── ui/           # shadcn/ui components
│   │   │   └── UKCoBenefitsMap.tsx
│   │   ├── pages/            # Page-level components
│   │   │   ├── Overview.tsx
│   │   │   ├── Geographic.tsx
│   │   │   └── About.tsx
│   │   ├── App.tsx           # Routes and layout
│   │   ├── main.tsx          # Application entry point
│   │   └── index.css         # Global styles and animations
├── server/
│   ├── routers.ts            # tRPC API endpoints
│   ├── db.ts                 # Database query helpers
│   └── _core/                # Framework infrastructure
├── drizzle/
│   └── schema.ts             # Database schema definitions
└── shared/                    # Shared types and constants
```

The `client/public/data/` directory contains all processed data files that power the visualizations. These JSON files were generated from the original Excel datasets through a data processing pipeline that aggregated statistics, created rankings, and merged geographic boundary information.

## Competition Criteria

This dashboard was designed to excel across all three judging criteria for The Data Lab's 2025 Data Visualisation Competition.

### Clarity and Communication

The dashboard prioritizes immediate comprehension through progressive disclosure of information. The hero section leads with the £145.6 billion headline figure, establishing the scale of impact before diving into details. Four numbered key insights provide a narrative framework that guides interpretation, explaining what the data means rather than simply displaying it.

Human-readable area names replace cryptic codes throughout the interface, making the data accessible to non-technical audiences. The color-coded map and charts use intuitive visual encoding (green for benefits, amber for costs) that requires no specialized knowledge to interpret.

### Creativity and Design

The animated background with floating orbs creates visual interest while reinforcing the climate theme through atmospheric movement. The three-page structure (Overview, Geographic, About) balances exploration with focused analysis, allowing users to choose their own path through the data.

Progressive loading of the area rankings demonstrates attention to user experience, ensuring fast initial load times while maintaining access to the complete dataset. The choropleth map transforms abstract numbers into geographic patterns, revealing spatial distribution that would be invisible in tabular format.

### Data Analysis, Understanding and Accuracy

All monetary values are properly attributed to the Edinburgh Climate Change Institute with full citation of the UK Co-Benefits Atlas. The About page provides methodological transparency, explaining the discount rate, base year, and geographic coverage.

The dashboard accurately represents the underlying data without distortion or cherry-picking. Both benefits and costs are shown with equal prominence, including the £70.8 billion hassle cost that represents the largest single negative value. The Key Insights section contextualizes this figure, explaining that it represents time valuation rather than actual economic loss and is offset 2:1 by health gains.

## Future Enhancements

Several features could extend the dashboard's analytical capabilities and user engagement:

**Temporal Visualization** using the Level_2 data would add an interactive timeline slider showing how co-benefits accumulate from 2025 to 2050, allowing users to see the trajectory of value creation over time.

**Area Comparison Tool** would enable side-by-side comparison of 2-3 selected areas across all 11 co-benefit categories, revealing which communities benefit most from different types of climate action.

**Data Export Functionality** including CSV download of rankings and PDF report generation would support offline analysis and sharing of findings with stakeholders.

**Integration with External Datasets** such as Index of Multiple Deprivation or health outcome statistics would strengthen the narrative by showing correlations between co-benefits and existing community characteristics.

**Search and Filter Capabilities** would allow users to quickly locate specific local authorities and filter areas by benefit type, making the dashboard more useful for targeted policy analysis.

## License

This project is licensed under the MIT License. See the LICENSE file for details.

## Acknowledgments

Data source: Edinburgh Climate Change Institute, UK Co-Benefits Atlas (www.ukcobenefitsatlas.net)

Built for The Data Lab's 2025 Data Visualisation Competition

Developed by Buse Bircan


## Credits

**Data Source:** Edinburgh Climate Change Institute (ECCI) - UK Co-Benefits Atlas

**Competition:** The Data Lab - Data Visualisation Competition 2025

**Framework:** React, Recharts, Tailwind CSS, shadcn/ui

**Created:** November 2025
