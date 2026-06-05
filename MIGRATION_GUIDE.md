# StretchLab Notes Hub - Migration Guide

## 🎯 What You Need to Do

### Phase 1: Prepare the New Repo (5 minutes)

**On Your Computer:**

```bash
# Clone or create the new unified repo
git clone https://github.com/pmashley9017-ai/stretchlab-notes-hub-unified.git
cd stretchlab-notes-hub-unified

# Copy these files into the repo:
# - server.js
# - package.json
# - .env.example
# - public/index.html
# - README.md

# Commit and push
git add -A
git commit -m "Refactor: unified app - backend + frontend combined, no password auth"
git push origin main
```

---

### Phase 2: Deploy to Render (5 minutes)

**In Render Dashboard:**

1. Go to https://dashboard.render.com
2. Click **New +** → **Web Service**
3. Select the **stretchlab-notes-hub-unified** repo
4. **Settings:**
   - **Name:** `stretchlab-notes-hub-unified`
   - **Environment:** `Node`
   - **Build Command:** `npm install`
   - **Start Command:** `node server.js`
   - **Instance:** Free (for testing) or Paid (for production)

5. **Add Environment Variables:**
   - Click **Advanced** → **Add Environment Variable**
   - **Key:** `ANTHROPIC_API_KEY`
   - **Value:** Paste your full Anthropic API key (`sk-ant-api03-...`)
   - Click **Add Environment Variable** again
   - **Key:** `ADMIN_PASSWORD`
   - **Value:** Your admin password (e.g., `20210ates`)
   - **Key:** `PORT`
   - **Value:** `10000`

6. Click **Create Web Service**

**Wait 2–3 minutes for deployment. ✅ You'll see a green "Live" status.**

---

### Phase 3: Test the New App (2 minutes)

1. **Open:** `https://stretchlab-notes-hub-unified.onrender.com`
2. **Try Full Session Form tab** → Fill out a test session → Click "Generate Note"
3. **Try Quick Voice Notes tab** → Record a test note
4. **Check Dashboard:** `https://stretchlab-notes-hub-unified.onrender.com/api/dashboard?pw=20210ates`
   - Should show usage stats

✅ **If all works, proceed to Phase 4.**

---

### Phase 4: Decommission Old Services (5 minutes)

**⚠️ ONLY DO THIS AFTER TESTING NEW APP WORKS**

**In Render Dashboard:**

1. **Find `stretchlab-notes-hub1` service** (old static site)
   - Click **Settings**
   - Scroll to bottom → **Delete Service**
   - Confirm

2. **Find `Note-Taking-SL` service** (old proxy server)
   - Click **Settings**
   - Scroll to bottom → **Delete Service**
   - Confirm

**In GitHub:**

1. Archive or delete `stretchlab-notes-hub1` repo
2. Archive or delete `stretchlab-proxy` repo

---

### Phase 5: Update Links & Notify Team (5 minutes)

**Update anywhere these old URLs appear:**
- Slack messages
- Documentation
- Bookmarks
- Email templates
- Internal wikis

**Old URLs (retired):**
- ❌ `https://stretchlab-notes-hub1.onrender.com`
- ❌ `https://note-taking-sl.onrender.com`

**New URL (use this):**
- ✅ `https://stretchlab-notes-hub-unified.onrender.com`

**Admin Dashboard:**
- ✅ `https://stretchlab-notes-hub-unified.onrender.com/api/dashboard?pw=20210ates`

**Send this to Flexologists:**
```
🚀 UPDATE: New StretchLab Notes Hub

Hi team,

The session notes app has been updated! 📝

📍 New URL: https://stretchlab-notes-hub-unified.onrender.com

✨ What's new:
- Faster (no more external proxy)
- Simpler (one app instead of two)
- Same features (Full Form + Quick Voice)
- Same passwords? NO! Just open and use it - no auth needed

📋 Bookmark the new link and let me know if you have questions.

Thanks!
```

---

## 🔄 What Actually Changed (Technical)

### Frontend
- Removed password input field from header
- Changed API call from `https://note-taking-sl.onrender.com/generate` to `/api/generate`
- Removed `password` parameter from fetch calls

### Backend
- Merged proxy server into same service
- Express now serves static HTML from `/public` folder
- API endpoint moved from `/generate` to `/api/generate`
- Password authentication removed
- Usage tracking, dashboard, stats all preserved

### Deployment
- 2 services → 1 service
- 2 GitHub repos → 1 GitHub repo
- Auto-deployed from single GitHub repo

---

## 📊 Expected Behavior

✅ **What Should Work:**
- Opening the app (no login)
- Generating notes in Full Form
- Generating notes in Quick Voice
- Copying notes to clipboard
- Accessing admin dashboard with password
- Usage tracking and stats

❌ **What Won't Work:**
- Password field in header (removed)
- Old URLs from the two separate services
- Proxy server (decommissioned)

---

## 🆘 Troubleshooting

### "App doesn't load"
→ Wait 2 minutes after deployment (Render takes time)
→ Check that Render service status is **Live** (green)

### "Error calling API"
→ Verify ANTHROPIC_API_KEY is set correctly in Render env vars
→ Check server logs in Render dashboard

### "Dashboard won't load"
→ Check the admin password is correct: `?pw=20210ates`
→ Make sure you're using the new URL

### "Old app still works"
→ You're accessing the old services (they're still running until Phase 4)
→ Bookmark and use the new URL instead

---

## 📝 Timeline

| Phase | Task | Time | Status |
|-------|------|------|--------|
| 1 | Prepare GitHub repo | 5 min | ⏳ |
| 2 | Deploy to Render | 5 min | ⏳ |
| 3 | Test new app | 2 min | ⏳ |
| 4 | Delete old services | 5 min | ⏳ |
| 5 | Update links & notify | 5 min | ⏳ |
| **TOTAL** | **Full migration** | **~22 min** | ⏳ |

---

## ✅ Verification Checklist

- [ ] New repo has all files (server.js, package.json, public/index.html, etc.)
- [ ] GitHub repo is `stretchlab-notes-hub-unified`
- [ ] Render service is created and deploying
- [ ] ANTHROPIC_API_KEY is set in Render env vars
- [ ] ADMIN_PASSWORD is set in Render env vars
- [ ] Service status shows "Live" (green)
- [ ] Can access the app at new URL
- [ ] Can generate a test note
- [ ] Can access dashboard with password
- [ ] Old services are deleted from Render
- [ ] Old repos are archived on GitHub
- [ ] Team has been notified of new URL

---

## 🎯 Success Criteria

✅ **You'll know it worked when:**

1. Flexologists can access the app at the new URL
2. No password prompt appears
3. Session notes generate correctly
4. Voice notes work
5. Dashboard shows usage stats
6. Old services are no longer running
7. No one is confused about which URL to use

---

**Questions?**
📧 Reach out to tech support
📱 Check the main README.md for more details

**Good luck! 🚀**
