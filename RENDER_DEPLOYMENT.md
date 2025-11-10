# Render Deployment Guide - Student Grievance System

## Prerequisites
- GitHub repository with your code
- Render account (free tier available at https://render.com)

## Step-by-Step Deployment on Render

### 1. Push Your Code to GitHub
```bash
git add .
git commit -m "Add Docker support for deployment"
git push origin main
```

### 2. Create New Web Service on Render

1. Go to https://dashboard.render.com
2. Click **"New +"** → **"Web Service"**
3. Connect your GitHub repository
4. Select your `windsurf-project` repository

### 3. Configure Web Service Settings

#### Basic Settings
- **Name**: `student-grievance` (or your preferred name)
- **Region**: Choose closest to your users (e.g., Singapore, Oregon)
- **Branch**: `main` (or your default branch)
- **Runtime**: **Docker**

#### Build & Deploy Settings
- **Dockerfile Path**: `./Dockerfile` (default, leave as is)
- **Docker Build Context Directory**: `.` (root directory)

#### Instance Type
- **Free** (for testing)
- **Starter** or higher (for production with better performance)

### 4. Environment Variables

Click **"Advanced"** and add these environment variables:

| Key | Value | Notes |
|-----|-------|-------|
| `SECRET_KEY` | `your-random-secret-key-here` | Generate a strong random string |
| `DATABASE_URL` | `sqlite:///instance/complaints.db` | SQLite database path |
| `PORT` | `10000` | Render's default port (auto-set) |

**To generate a secure SECRET_KEY**, run in Python:
```python
import secrets
print(secrets.token_hex(32))
```

### 5. Deploy

1. Click **"Create Web Service"**
2. Render will automatically:
   - Clone your repository
   - Build the Docker image
   - Deploy the container
   - Assign a URL like: `https://student-grievance.onrender.com`

### 6. Monitor Deployment

- Watch the **Logs** tab for build progress
- Wait 3-5 minutes for first deployment
- Once you see "Your service is live 🎉", visit your URL

### 7. Verify Deployment

1. Visit your Render URL: `https://your-app-name.onrender.com`
2. Test login with default admin credentials:
   - Username: `admin`
   - Password: `admin123`

## Important Notes

### Database Persistence
- **Free tier**: Database resets on each deploy (ephemeral storage)
- **Paid tier**: Use persistent disk for database
- **Production**: Consider PostgreSQL instead of SQLite

### To Add Persistent Disk (Paid Plans)
1. Go to your service → **"Disks"** tab
2. Add disk:
   - **Mount Path**: `/app/instance`
   - **Size**: 1GB (minimum)
3. Redeploy service

### Custom Domain (Optional)
1. Go to **"Settings"** → **"Custom Domain"**
2. Add your domain
3. Update DNS records as instructed

## Troubleshooting

### 500 Internal Server Error
- Check **Logs** tab for error details
- Verify environment variables are set
- Ensure `instance` directory is writable

### Build Fails
- Verify `Dockerfile` is in repository root
- Check `requirements.txt` has all dependencies
- Review build logs for specific errors

### Database Issues
- Free tier: Database resets on restart
- Solution: Upgrade to paid plan with persistent disk
- Or: Use PostgreSQL (Render provides free PostgreSQL)

## Local Testing with Docker

Before deploying, test locally:

```bash
# Build image
docker build -t student-grievance .

# Run container
docker run -p 5000:10000 \
  -e SECRET_KEY=test-secret-key \
  -e DATABASE_URL=sqlite:///instance/complaints.db \
  student-grievance

# Or use docker-compose
docker-compose up
```

Visit: http://localhost:5000

## Updating Your Deployment

```bash
# Make changes to your code
git add .
git commit -m "Update feature"
git push origin main
```

Render will automatically detect the push and redeploy.

## Manual Deploy
If auto-deploy is disabled:
1. Go to your service dashboard
2. Click **"Manual Deploy"** → **"Deploy latest commit"**

## Cost Estimate

- **Free Tier**: $0/month (spins down after 15 min inactivity)
- **Starter**: $7/month (always on, 512MB RAM)
- **Standard**: $25/month (2GB RAM)
- **Persistent Disk**: +$0.25/GB/month

## Security Recommendations

1. **Change default admin password** after first login
2. **Use strong SECRET_KEY** (not the default)
3. **Enable HTTPS** (automatic on Render)
4. **Set up environment variables** properly
5. **Don't commit secrets** to Git

## Support

- Render Docs: https://render.com/docs
- Render Community: https://community.render.com
- Your app logs: Render Dashboard → Logs tab

---

**Your deployment URL will be**: `https://student-grievance.onrender.com` (or your chosen name)
