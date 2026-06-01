# Customization Guide

This document explains how to customize the K8s Cost Optimization Dashboard to match your environment or preferences.

---

## Changing Mock Data

All dashboard data lives in the `<script>` section near the bottom of `index.html`. Look for the data initialization block and modify the values:

### KPI Cards (Top Row)
```javascript
// Modify these values to reflect your cluster's actual numbers
const kpiData = {
  totalMonthlyCost: 24850,   // in USD
  dailyBurnRate: 828,
  savingsPotential: 7420,
  efficiencyScore: 68,
};
```

### Namespace Breakdown
```javascript
const namespaceData = [
  { name: 'production', cost: 12400, waste: 2100, pods: 48, utilization: 82 },
  { name: 'staging',    cost: 5200,  waste: 2800, pods: 22, utilization: 45 },
  // Add or modify namespaces here
];
```

### Right-sizing Recommendations
```javascript
const recommendations = [
  {
    name: 'payment-service',
    namespace: 'production',
    currentCPU: '2000m',
    recommendedCPU: '800m',
    saving: 142,
    priority: 'high'
  },
  // Add more recommendations
];
```

---

## Changing Colors / Theme

Edit the CSS custom properties at the top of `index.html` inside the `:root` block:

```css
:root {
  /* Backgrounds */
  --bg: #0a0e1a;          /* Main background */
  --surface: #111827;     /* Card background */
  --surface2: #1a2235;    /* Input/nested background */
  --border: #1f2d45;      /* Border color */

  /* Accent Colors */
  --accent: #00d4ff;      /* Cyan – primary highlight */
  --accent2: #7c3aed;     /* Purple – secondary highlight */
  --accent3: #10b981;     /* Green – savings/positive */
  --danger: #ef4444;      /* Red – warnings/high waste */
  --warning: #f59e0b;     /* Amber – medium priority */

  /* Text */
  --text: #e2e8f0;        /* Primary text */
  --text-muted: #64748b;  /* Muted/secondary text */
}
```

---

## Adding Your Own Charts

The dashboard uses Chart.js v4. To add a new chart:

```html
<!-- 1. Add a canvas element in the HTML -->
<canvas id="myNewChart"></canvas>
```

```javascript
// 2. Initialize it in the script section
const ctx = document.getElementById('myNewChart').getContext('2d');
new Chart(ctx, {
  type: 'bar',   // line, bar, doughnut, radar, etc.
  data: {
    labels: ['Jan', 'Feb', 'Mar'],
    datasets: [{
      label: 'My Dataset',
      data: [100, 200, 150],
      backgroundColor: 'rgba(0, 212, 255, 0.2)',
      borderColor: '#00d4ff',
    }]
  },
  options: {
    responsive: true,
    // ... Chart.js options
  }
});
```

---

## Connecting to a Real Backend

To replace mock data with live Kubernetes/Prometheus data:

1. Set up a FastAPI or Express backend that queries your K8s cluster.
2. Replace the mock data with `fetch()` calls:

```javascript
// Replace static data with API call
async function loadDashboardData() {
  const response = await fetch('https://your-api.com/api/v1/cost-summary');
  const data = await response.json();
  updateKPICards(data.kpis);
  updateCharts(data.namespaces);
}

document.addEventListener('DOMContentLoaded', loadDashboardData);
```

---

## Changing Fonts

The dashboard uses Google Fonts. To switch fonts, edit the `<link>` tag in `<head>`:

```html
<!-- Current fonts -->
<link href="https://fonts.googleapis.com/css2?family=Space+Mono:wght@400;700&family=DM+Sans:opsz,wght@9..40,300;400;500;600;700&display=swap" rel="stylesheet">
```

Then update the CSS variables:
```css
--font-mono: 'Your Mono Font', monospace;
--font-sans: 'Your Sans Font', sans-serif;
```
