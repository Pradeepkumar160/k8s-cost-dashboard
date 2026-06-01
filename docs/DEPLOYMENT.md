# Deployment Guide

This project is a **single HTML file** — deployment is extremely simple. Choose any option below.

---

## Option 1: GitHub Pages (Recommended — Free)

1. Push this repo to GitHub
2. Go to your repo → **Settings** → **Pages**
3. Under "Source", select **Deploy from a branch**
4. Choose **main** branch, **/ (root)** folder
5. Click **Save**
6. Your dashboard goes live at:  
   `https://Pradeepkumar160.github.io/k8s-cost-dashboard`

> Takes 1–2 minutes to deploy after saving.

---

## Option 2: Netlify (Free, Instant)

### Drag & Drop
1. Go to [netlify.com/drop](https://app.netlify.com/drop)
2. Drag your `index.html` file onto the page
3. You get a live URL instantly (e.g., `random-name.netlify.app`)
4. Optionally connect your GitHub repo for auto-deploy on every push

### CLI
```bash
npm install -g netlify-cli
netlify deploy --dir . --prod
```

---

## Option 3: Vercel (Free)

```bash
npm install -g vercel
cd k8s-cost-dashboard
vercel --prod
```

---

## Option 4: Local Nginx Server

```nginx
server {
    listen 80;
    server_name your-domain.com;
    root /var/www/k8s-cost-dashboard;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

```bash
# Copy files
sudo cp -r k8s-cost-dashboard /var/www/
sudo nginx -s reload
```

---

## Option 5: Docker

```dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80
```

```bash
docker build -t k8s-cost-dashboard .
docker run -p 8080:80 k8s-cost-dashboard
# Open: http://localhost:8080
```

---

## Option 6: Python Quick Server (Dev Only)

```bash
cd k8s-cost-dashboard
python3 -m http.server 8080
# Open: http://localhost:8080
```

---

## Environment Notes

Since this is a static HTML file with no backend, there is no:
- Server configuration needed
- Database setup
- Environment variables
- Build step

Just host the `index.html` file anywhere that serves static files.
