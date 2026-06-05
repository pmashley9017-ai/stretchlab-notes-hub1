# StretchLab Session Notes Hub - Refactored Unified App

**Status:** ✅ **Ready for Deployment**

This is the refactored version combining the frontend notes app and backend proxy server into a single Node.js Web Service.

---

## 📋 What Changed

### Before (Two Apps)
- **Frontend:** `stretchlab-notes-hub1` (Static HTML → https://stretchlab-notes-hub1.onrender.com)
- **Backend Proxy:** `stretchlab-proxy` (Node.js → https://note-taking-sl.onrender.com)
- Frontend called external proxy at `https://note-taking-sl.onrender.com/generate`
- Required password authentication in proxy

### After (One Unified App)
- **Single Web Service:** Node.js server serves both frontend + backend
- Frontend calls local endpoint: `/api/generate`
- **No password authentication** - removed completely
- Same features: usage tracking, dashboard, admin stats
- Simpler deployment & maintenance

---

## 🚀 Deployment to Render

### Step 1: Push to GitHub

```bash
# Create repo stretchlab-notes-hub-unified (or update existing)
cd stretchlab-notes-hub
git init
git add -A
git commit -m "Refactor: unified frontend + backend, removed password auth"
git remote add origin https://github.com/YOUR_USERNAME/stretchlab-notes-hub-unified.git
git push -u origin main
```

**Files in repo:**
```
stretchlab-notes-hub-unified/
├── server.js              # Express server (new: unified)
├── package.json           # Dependencies
├── .env.example           # Environment template
├── public/
│   └── index.html         # Frontend (updated: calls /api/generate)
└── README.md
```

### Step 2: Deploy on Render

1. **Go to Render Dashboard** → https://dashboard.render.com
2. **Create New** → Web Service
3. **Connect Repository:** Select `stretchlab-notes-hub-unified`
4. **Configure:**
   - **Name:** `stretchlab-notes-hub-unified`
   - **Environment:** Node
   - **Build Command:** `npm install`
   - **Start Command:** `node server.js`
   - **Instance Type:** Free (or paid for production)
5. **Environment Variables:**
   - **ANTHROPIC_API_KEY:** `sk-ant-api03-...` (your full key)
   - **ADMIN_PASSWORD:** `YourAdminPassword2024`
6. **Create Web Service** → Deploy

**Deployment takes ~2 minutes.**

---

## 📍 Access Points

Once deployed to Render, you'll have:

### Main App
- **URL:** `https://stretchlab-notes-hub-unified.onrender.com`
- **Tabs:** 
  - 📋 Full Session Form
  - 🎤 Quick Voice Notes

### Admin Dashboard
- **URL:** `https://stretchlab-notes-hub-unified.onrender.com/api/dashboard?pw=YourAdminPassword2024`
- **Shows:** Total notes, cost estimate, flexologist usage, last 30 days

### API Stats (JSON)
- **URL:** `https://stretchlab-notes-hub-unified.onrender.com/api/stats?pw=YourAdminPassword2024`
- **Returns:** JSON with usage metrics

### Health Check
- **URL:** `https://stretchlab-notes-hub-unified.onrender.com/api/health`
- **Returns:** `{ "ok": true }`

---

## 🛠️ Local Development

### Run Locally

```bash
# 1. Install dependencies
npm install

# 2. Create .env file
cp .env.example .env
# Edit .env and add your ANTHROPIC_API_KEY

# 3. Start server
npm start
```

**Server runs on:** http://localhost:10000

---

## 🔧 What Was Removed

✅ **Password Authentication**
- Header auth field: **REMOVED** (was prompting for password)
- Password validation in API: **COMMENTED OUT** in proxy code, **REMOVED** in unified server
- Password parameter in frontend fetch calls: **REMOVED**

✅ **External Proxy Dependency**
- No longer calls `https://note-taking-sl.onrender.com/generate`
- Frontend calls local `/api/generate` endpoint instead
- Reduced latency, simpler architecture

---

## ✨ Key Endpoints

### POST `/api/generate`
Generates ClubReady notes via Claude API.

**Request:**
```javascript
{
  messages: [{ role: 'user', content: '...' }],
  max_tokens: 1024,
  flexologist: 'user@email.com'
}
```

**Response:**
```json
{
  "content": [{ "type": "text", "text": "Generated note..." }],
  "usage": { "input_tokens": ..., "output_tokens": ... }
}
```

### GET `/api/dashboard?pw=adminPassword`
Admin dashboard with usage stats.

### GET `/api/stats?pw=adminPassword`
JSON stats endpoint.

### GET `/api/health`
Health check - returns `{ "ok": true }`

---

## 📊 Usage Tracking

The app tracks:
- **Total notes** generated (cumulative)
- **Notes per day** (last 30 days)
- **Notes per flexologist** (by email)
- **Estimated API cost** ($0.004/note)

Data is stored in `/tmp/sl_usage.json` and persists within the same Render instance. Note: Render free tier instances reset weekly, so usage is not permanent.

---

## 🎯 Next Steps

1. **Update in stretchlab-notes-hub-unified GitHub repo**
   - Replace old files with these new ones
   - Update any CI/CD if applicable

2. **Decommission Old Services**
   - Delete `stretchlab-proxy` from Render (no longer needed)
   - Delete `stretchlab-notes-hub1` from Render (replaced by unified app)
   - Archive or delete the two GitHub repos

3. **Share New URL with Flexologists**
   - New main URL: `https://stretchlab-notes-hub-unified.onrender.com`
   - No password needed to use the app
   - Dashboard still uses admin password

4. **Monitor & Adjust**
   - Watch the `/api/dashboard` for usage patterns
   - Upgrade from Free to Paid tier if needed (free tier has 750 hrs/month limit)

---

## 📦 Dependencies

- **express:** Web server framework
- **cors:** Cross-origin request handling
- **Node.js 18+:** Runtime

---

## 📝 Files Summary

| File | Purpose |
|------|---------|
| `server.js` | Express server (unified frontend + backend) |
| `public/index.html` | Frontend UI (updated to call `/api/generate`) |
| `package.json` | npm dependencies |
| `.env.example` | Environment variables template |
| `README.md` | This file |

---

## ❓ FAQ

**Q: Do Flexologists still need a password?**
A: No - password auth has been completely removed. They just open the app and start using it.

**Q: What about the admin dashboard?**
A: Still requires `?pw=adminPassword` in the URL to access stats.

**Q: Can I still see usage stats?**
A: Yes - visit `/api/dashboard?pw=yourAdminPassword` to see the same stats dashboard.

**Q: What if my Render service goes down?**
A: The free tier spins down after 15 minutes of inactivity (~1 minute to wake up). For production, upgrade to a paid plan.

**Q: Can I host this elsewhere?**
A: Yes - it's a standard Node.js/Express app. Compatible with any Node host (AWS, Heroku, Netlify, etc.).

---

## 🚀 Deployment Checklist

- [ ] Code pushed to GitHub
- [ ] Render Web Service created
- [ ] ANTHROPIC_API_KEY set in Render env vars
- [ ] ADMIN_PASSWORD set in Render env vars
- [ ] Deployment successful (green status)
- [ ] App loads at https://stretchlab-notes-hub-unified.onrender.com
- [ ] Can generate a test note
- [ ] Dashboard accessible at `/api/dashboard?pw=...`
- [ ] Old proxy service decommissioned
- [ ] Old static site service decommissioned
- [ ] Flexologists notified of new URL

---

**Refactored by:** Claude (Tech Support)  
**Date:** June 4, 2026  
**Status:** ✅ Ready for Production
