# 🚀 K8s Cost Optimization Dashboard.            

<div align="center">

![K8s Cost Dashboard](https://img.shields.io/badge/Kubernetes-Cost%20Optimizer-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)
![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)
![Chart.js](https://img.shields.io/badge/Chart.js-FF6384?style=for-the-badge&logo=chartdotjs&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)
![Stars](https://img.shields.io/github/stars/Pradeepkumar160/k8s-cost-dashboard?style=for-the-badge)

**A production-grade, single-file Kubernetes FinOps dashboard built with zero dependencies.**  
Visualize cluster costs, detect waste, and get right-sizing recommendations — all in your browser.

[🌐 Live Demo](#live-demo) · [📸 Screenshots](#screenshots) · [🚀 Quick Start](#quick-start) · [📖 Docs](#documentation)

</div>

---

## 📌 Overview

The **K8s Cost Optimization Dashboard** is a fully self-contained HTML dashboard designed for DevOps engineers and FinOps teams. It simulates a real-world Kubernetes cost analysis platform with interactive charts, namespace breakdowns, pod-level waste detection, and actionable savings recommendations.

> **Built as a UI prototype / frontend showcase** — no backend required. All data is rendered client-side using Chart.js with realistic mock data that mirrors a production Kubernetes environment.

---

## ✨ Features

| Feature | Description |
|---|---|
| 💰 **Cost Overview** | Monthly spend, daily burn rate, savings potential, efficiency score |
| 🏷️ **Namespace Analysis** | Cost breakdown per namespace with utilization heatmap |
| 📦 **Pod Analysis** | Per-pod CPU/Memory waste detection with severity levels |
| 🔧 **Right-sizing Recommendations** | Actionable suggestions with estimated monthly savings |
| 📈 **Savings Forecast** | 6-month projected savings trend chart |
| 🌐 **Multi-cluster Support** | Simulated cluster switcher (prod, staging, dev) |
| 🌙 **Dark Theme** | Terminal-inspired dark UI optimized for long working sessions |
| 📱 **Responsive Design** | Works on desktop, tablet, and mobile |

---

## 🖥️ Screenshots

> *Add screenshots after deploying. Place images in `/screenshots/` folder.*

```
screenshots/
├── overview.png          ← Cost overview panel
├── namespace-view.png    ← Namespace breakdown
├── recommendations.png   ← Right-sizing suggestions
└── mobile-view.png       ← Responsive mobile layout
```

---

## 🚀 Quick Start

### Option 1: Open Directly (Zero Setup)
```bash
git clone https://github.com/Pradeepkumar160/k8s-cost-dashboard.git
cd k8s-cost-dashboard
# Just open index.html in your browser
open index.html          # macOS
xdg-open index.html      # Linux
start index.html         # Windows
```

### Option 2: Serve Locally
```bash
# Python (built-in)
python3 -m http.server 8080
# Then open: http://localhost:8080

# Node.js (npx)
npx serve .
# Then open: http://localhost:3000
```

### Option 3: GitHub Pages
1. Fork this repo
2. Go to **Settings → Pages**
3. Set source to `main` branch, `/ (root)` folder
4. Your dashboard is live at `https://<your-username>.github.io/k8s-cost-dashboard`

---

## 📁 Project Structure

```
k8s-cost-dashboard/
│
├── index.html              ← Main dashboard (self-contained, single file)
│
├── docs/
│   ├── CUSTOMIZATION.md    ← How to modify cost models and data
│   ├── DEPLOYMENT.md       ← Deployment options (GitHub Pages, Nginx, Docker)
│   └── COST_MODEL.md       ← How cost calculations work
│
├── screenshots/            ← Add dashboard screenshots here
│
├── .gitignore
├── LICENSE
└── README.md
```

---

## 🏗️ Production Architecture (Full Stack Vision)

This dashboard is the **frontend layer** of a larger FinOps platform. The full production architecture looks like:

```
┌─────────────────────────────────────┐
│         React Dashboard (UI)         │
│  Cost Overview | Namespace Analysis  │
│  Pod Analysis  | Recommendations     │
└──────────────┬──────────────────────┘
               │ REST API
               ▼
┌─────────────────────────────────────┐
│           FastAPI Backend            │
│  Auth | Cluster Service | Reports    │
│  Cost Engine | Recommendation Engine │
└──────────────┬──────────────────────┘
               │
     ┌─────────┴──────────┐
     ▼                    ▼
┌──────────┐     ┌─────────────────┐
│ K8s API  │     │  Prometheus API │
└──────────┘     └─────────────────┘
               │
               ▼
    ┌─────────────────┐
    │   PostgreSQL DB  │
    └─────────────────┘
```

**Tech stack for full production build:**
- **Frontend:** React + TypeScript + Recharts
- **Backend:** Python FastAPI
- **Data Sources:** Kubernetes API + Prometheus
- **Database:** PostgreSQL
- **Auth:** JWT
- **Infra:** Docker + Helm

---

## ⚙️ Customization

### Change Cost Data
Edit the JavaScript variables inside `index.html`:
```javascript
// Find this section in index.html
const mockData = {
  totalMonthlyCost: 24850,
  wastedCost: 7420,
  // ... modify values here
};
```

### Change Color Theme
Edit CSS variables at the top of `index.html`:
```css
:root {
  --accent: #00d4ff;      /* Primary cyan color */
  --accent2: #7c3aed;     /* Purple accent */
  --accent3: #10b981;     /* Green for savings */
}
```

→ See [`docs/CUSTOMIZATION.md`](docs/CUSTOMIZATION.md) for full customization guide.

---

## 🚢 Deployment

| Platform | Command / Steps |
|---|---|
| **GitHub Pages** | Settings → Pages → Deploy from `main` |
| **Netlify** | Drag & drop `index.html` to netlify.com/drop |
| **Vercel** | `vercel --prod` |
| **Nginx** | Copy to `/var/www/html/` |
| **Docker** | See `docs/DEPLOYMENT.md` |

---

## 📊 Cost Model

The dashboard uses a **cloud-provider-agnostic cost model**:

```
Monthly Cost = (CPU_requested × $0.048/vCPU-hour + Memory_requested × $0.006/GB-hour) × 730 hours
Wasted Cost  = (CPU_unused + Memory_unused) × same rates
Efficiency % = (Actual_usage / Requested_resources) × 100
```

→ See [`docs/COST_MODEL.md`](docs/COST_MODEL.md) for full pricing model details.

---

## 🛠️ Tech Stack

- **HTML5** — Semantic markup, single-file architecture
- **CSS3** — Custom properties, Grid, Flexbox, animations
- **Vanilla JavaScript** — No framework, zero build step
- **[Chart.js v4](https://www.chartjs.org/)** — Interactive charts (CDN)
- **[Google Fonts](https://fonts.google.com/)** — Space Mono + DM Sans

---

## 🤝 Contributing

Contributions are welcome! Here's how:

1. Fork the repo
2. Create a feature branch: `git checkout -b feature/add-export-button`
3. Commit your changes: `git commit -m 'Add CSV export feature'`
4. Push and open a PR

**Ideas for contributions:**
- [ ] CSV/PDF export of recommendations
- [ ] Real Kubernetes API integration
- [ ] Dark/light theme toggle
- [ ] More chart types (treemap, sankey)
- [ ] Budget alert thresholds

---

## 📄 License

This project is licensed under the **MIT License** — see [LICENSE](LICENSE) for details.

---

## 👨‍💻 Author

**Pradeep Kumar**  
B.Tech Computer Science | Lovely Professional University

[![GitHub](https://img.shields.io/badge/GitHub-Pradeepkumar160-181717?style=flat&logo=github)](https://github.com/Pradeepkumar160)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-07pradeepk-0A66C2?style=flat&logo=linkedin)](https://linkedin.com/in/07pradeepk/)

---

<div align="center">
  <sub>⭐ Star this repo if you found it useful!</sub>
</div>
