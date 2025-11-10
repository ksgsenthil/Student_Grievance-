# Deployment Checklist ✅

## Pre-Deployment Steps

### 1. ✅ Files Created
- [x] Dockerfile
- [x] .dockerignore
- [x] docker-compose.yml
- [x] .env.example (with generated SECRET_KEY)
- [x] .gitignore (updated with Python/Flask patterns)
- [x] RENDER_DEPLOYMENT.md
- [x] README_DOCKER.md

### 2. ✅ Code Updated
- [x] app.py uses environment variables
- [x] Database path: `sqlite:///instance/complaints.db`
- [x] SECRET_KEY from environment
- [x] Production-ready configuration

### 3. 🔄 Test Locally (Optional but Recommended)

```bash
# Test with Docker Compose
docker-compose up --build

# Open browser: http://localhost:5000
# Login as admin/admin123
# Submit a test complaint
# Verify everything works

# Stop when done
docker-compose down
```

### 4. 📤 Push to GitHub

```bash
# Check status
git status

# Add all files
git add .

# Commit changes
git commit -m "Add Docker support for Render deployment"

# Push to GitHub
git push origin main
```

### 5. 🚀 Deploy on Render

#### A. Create Web Service
1. Go to https://dashboard.render.com
2. Click **"New +"** → **"Web Service"**
3. Connect your GitHub account (if not already)
4. Select your repository: `windsurf-project`
5. Click **"Connect"**

#### B. Configure Service
**Name**: `student-grievance` (or your choice)

**Region**: Choose closest to you:
- 🇸🇬 Singapore
- 🇺🇸 Oregon
- 🇪🇺 Frankfurt

**Branch**: `main`

**Runtime**: **Docker** ⚠️ IMPORTANT!

**Dockerfile Path**: `./Dockerfile` (default)

**Instance Type**:
- **Free** (for testing - spins down after 15 min inactivity)
- **Starter** ($7/month - recommended for production)

#### C. Environment Variables
Click **"Advanced"** → **"Add Environment Variable"**

Add these variables:

| Key | Value |
|-----|-------|
| `SECRET_KEY` | `1cedd6d35c99f198e7b35709f20b0ebacf15df292f408adc9dbf722d81eecba2` |
| `DATABASE_URL` | `sqlite:///instance/complaints.db` |

#### D. Deploy
1. Click **"Create Web Service"**
2. Wait 3-5 minutes for build
3. Watch the **Logs** tab for progress

### 6. ✅ Verify Deployment

Once deployed, you'll get a URL like:
```
https://student-grievance.onrender.com
```

**Test the following:**
- [ ] Homepage loads
- [ ] Can access login page
- [ ] Admin login works (admin/admin123)
- [ ] Can create a complaint
- [ ] Can view complaints
- [ ] Can update complaint status
- [ ] Can download CSV report

### 7. 🔒 Post-Deployment Security

**IMPORTANT - Do these immediately:**

1. **Change Admin Password**
   - Login as admin
   - Go to profile/settings
   - Change password from `admin123` to something secure

2. **Verify Environment Variables**
   - Check Render dashboard → Environment tab
   - Ensure SECRET_KEY is set correctly

3. **Monitor Logs**
   - Check for any errors
   - Render Dashboard → Logs tab

### 8. 📊 Monitor Your App

**Free Tier Notes:**
- App spins down after 15 minutes of inactivity
- First request after spin-down takes ~30 seconds
- Database resets on each deploy (ephemeral storage)

**To Keep Database Persistent (Paid Plans):**
1. Render Dashboard → Your Service
2. **"Disks"** tab → **"Add Disk"**
3. Mount Path: `/app/instance`
4. Size: 1GB minimum
5. Save and redeploy

### 9. 🌐 Custom Domain (Optional)

1. Render Dashboard → **"Settings"**
2. **"Custom Domain"** section
3. Add your domain
4. Update DNS records as instructed

---

## Quick Commands Reference

### Local Testing
```bash
# Docker Compose
docker-compose up --build
docker-compose down

# Docker CLI
docker build -t student-grievance .
docker run -p 5000:10000 student-grievance
```

### Git Commands
```bash
git status
git add .
git commit -m "Your message"
git push origin main
```

### Generate New Secret Key
```bash
python -c "import secrets; print(secrets.token_hex(32))"
```

---

## Troubleshooting

### Build Fails on Render
- Check **Logs** tab for specific error
- Verify Dockerfile is in repository root
- Ensure requirements.txt has all dependencies

### 500 Internal Server Error
- Check Render **Logs** for Python traceback
- Verify environment variables are set
- Check database path is correct

### App Won't Start
- Ensure Runtime is set to **Docker**
- Verify Dockerfile CMD is correct
- Check port is 10000 in Dockerfile

### Database Resets
- Free tier has ephemeral storage
- Upgrade to paid plan with persistent disk
- Or use PostgreSQL (Render offers free PostgreSQL)

---

## Support Resources

- **Render Docs**: https://render.com/docs
- **Render Community**: https://community.render.com
- **Docker Docs**: https://docs.docker.com
- **Flask Docs**: https://flask.palletsprojects.com

---

## Your Deployment Info

**Repository**: `windsurf-project`

**Render URL**: `https://student-grievance.onrender.com` (update after deployment)

**Admin Credentials**:
- Username: `admin`
- Password: `admin123` (⚠️ CHANGE IMMEDIATELY)

**SECRET_KEY**: `1cedd6d35c99f198e7b35709f20b0ebacf15df292f408adc9dbf722d81eecba2`

---

## Next Steps After Deployment

1. ✅ Test all features
2. 🔒 Change admin password
3. 📝 Document your custom URL
4. 👥 Share with users
5. 📊 Monitor usage and logs
6. 🔄 Plan for database persistence if needed

**Good luck with your deployment! 🚀**
