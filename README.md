# Trichromind

A self-assessment web app estimating relative patterns in Norepinephrine, Dopamine, and Serotonin domains. Informational only — not a diagnosis or medical advice.

## Quick start

```bash
npm install
npm start
```

Then open http://localhost:3000

## Deployment

### Change the port

Set the `PORT` environment variable:

```bash
PORT=8080 npm start
```

### Deploy to a VPS (Ubuntu / Debian)

```bash
# 1. Copy files to your server
scp -r trichromind-app/ user@yourserver.com:/home/user/

# 2. SSH in and install dependencies
ssh user@yourserver.com
cd trichromind-app
npm install --omit=dev

# 3. Run with PM2 (keeps it alive after logout)
npm install -g pm2
pm2 start server.js --name trichromind
pm2 save
pm2 startup    # follow the printed command to enable on boot
```

### Nginx reverse proxy (optional, recommended)

If you want to serve on port 80/443 via nginx:

```nginx
server {
    listen 80;
    server_name yourdomain.com;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

Then enable with `sudo nginx -t && sudo systemctl reload nginx`.

For HTTPS, use Certbot: `sudo certbot --nginx -d yourdomain.com`

### Deploy to Railway / Render / Fly.io

These platforms auto-detect Node.js apps. Just push the folder and they'll run `npm start`. Set the `PORT` env var if the platform requires it (Railway and Render inject it automatically).

### Deploy to Heroku

```bash
heroku create your-app-name
git init && git add . && git commit -m "init"
heroku git:remote -a your-app-name
git push heroku main
```

## Project structure

```
trichromind-app/
├── server.js        # Express server — serves static files from /public
├── package.json
├── README.md
└── public/
    └── index.html   # Entire app (HTML + CSS + JS, no build step needed)
```

## Notes

- No database required — all scoring runs client-side in the browser.
- No personal data is sent to the server or stored anywhere.
- The Tabler Icons font is loaded from cdn.jsdelivr.net — requires internet access on the client.
