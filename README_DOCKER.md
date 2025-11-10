# Docker Setup - Student Grievance System

## Quick Start

### Using Docker Compose (Recommended for Local Development)

```bash
# Build and start the application
docker-compose up --build

# Access the application
# Open browser: http://localhost:5000
```

### Using Docker CLI

```bash
# Build the image
docker build -t student-grievance .

# Run the container
docker run -d \
  -p 5000:10000 \
  -e SECRET_KEY=your-secret-key \
  -e DATABASE_URL=sqlite:///instance/complaints.db \
  --name grievance-app \
  student-grievance

# View logs
docker logs -f grievance-app

# Stop container
docker stop grievance-app

# Remove container
docker rm grievance-app
```

## Files Created

- **Dockerfile**: Container configuration
- **.dockerignore**: Files to exclude from Docker build
- **docker-compose.yml**: Multi-container orchestration (optional)
- **.env.example**: Environment variables template

## Default Credentials

After deployment, login with:
- **Admin**:
  - Username: `admin`
  - Password: `admin123`

**⚠️ Change the admin password immediately after first login!**

## Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `SECRET_KEY` | Flask secret key for sessions | `your-secret-key-here` |
| `DATABASE_URL` | Database connection string | `sqlite:///instance/complaints.db` |
| `DEBUG` | Enable debug mode | `False` |
| `PORT` | Application port | `10000` (Docker), `5000` (local) |

## Production Deployment

See **RENDER_DEPLOYMENT.md** for detailed Render deployment instructions.

## Troubleshooting

### Container won't start
```bash
# Check logs
docker logs student-grievance

# Check if port is already in use
netstat -ano | findstr :5000  # Windows
lsof -i :5000                 # Linux/Mac
```

### Database issues
```bash
# Remove old container and volume
docker-compose down -v

# Rebuild and restart
docker-compose up --build
```

### Permission errors
```bash
# Ensure instance directory exists and is writable
mkdir -p instance
chmod 755 instance
```

## Development Workflow

1. **Make code changes**
2. **Rebuild container**:
   ```bash
   docker-compose up --build
   ```
3. **Test locally**
4. **Commit and push to GitHub**
5. **Render auto-deploys**

## Stopping the Application

```bash
# Using docker-compose
docker-compose down

# Using docker CLI
docker stop grievance-app
```

## Cleaning Up

```bash
# Remove containers and images
docker-compose down --rmi all

# Remove volumes (deletes database)
docker-compose down -v
```
