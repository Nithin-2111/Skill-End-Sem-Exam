# Deployment Guide

This guide covers deploying the Student Learning Outcomes Tracking System to various platforms.

## Prerequisites

1. Git repository initialized
2. All dependencies installed
3. Build tested locally

## Deployment Options

### Option 1: Vercel (Recommended for Frontend)

1. **Install Vercel CLI:**
   ```bash
   npm i -g vercel
   ```

2. **Deploy:**
   ```bash
   cd client
   vercel
   ```

3. **Or connect via GitHub:**
   - Push code to GitHub
   - Go to [vercel.com](https://vercel.com)
   - Import your repository
   - Set root directory to `client`
   - Deploy

**Configuration:**
- Framework: Vite
- Build Command: `npm run build`
- Output Directory: `dist`
- Install Command: `npm install`

### Option 2: Netlify

1. **Install Netlify CLI:**
   ```bash
   npm i -g netlify-cli
   ```

2. **Deploy:**
   ```bash
   cd client
   netlify deploy
   ```

3. **Or connect via GitHub:**
   - Push code to GitHub
   - Go to [netlify.com](https://netlify.com)
   - Import your repository
   - Set build command: `npm run build`
   - Set publish directory: `dist`

### Option 3: GitHub Pages

1. **Install gh-pages:**
   ```bash
   cd client
   npm install --save-dev gh-pages
   ```

2. **Update package.json:**
   ```json
   "scripts": {
     "predeploy": "npm run build",
     "deploy": "gh-pages -d dist"
   },
   "homepage": "https://yourusername.github.io/task-tracker-crud"
   ```

3. **Deploy:**
   ```bash
   npm run deploy
   ```

### Backend Deployment (Railway, Render, Heroku)

#### Railway

1. Go to [railway.app](https://railway.app)
2. Create new project
3. Connect GitHub repository
4. Set root directory to `server`
5. Add environment variables:
   - `DB_HOST`
   - `DB_USER`
   - `DB_PASSWORD`
   - `DB_NAME`
   - `PORT`

#### Render

1. Go to [render.com](https://render.com)
2. Create new Web Service
3. Connect GitHub repository
4. Set:
   - Root Directory: `server`
   - Build Command: `npm install`
   - Start Command: `npm start`
5. Add environment variables

#### Heroku

1. **Install Heroku CLI:**
   ```bash
   npm i -g heroku
   ```

2. **Login:**
   ```bash
   heroku login
   ```

3. **Create app:**
   ```bash
   cd server
   heroku create your-app-name
   ```

4. **Set environment variables:**
   ```bash
   heroku config:set DB_HOST=your_host
   heroku config:set DB_USER=your_user
   heroku config:set DB_PASSWORD=your_password
   heroku config:set DB_NAME=your_database
   ```

5. **Deploy:**
   ```bash
   git push heroku main
   ```

## Environment Variables

### Frontend (.env)
```env
VITE_API_BASE=https://your-backend-url.com/api
```

### Backend (.env)
```env
DB_HOST=your_database_host
DB_USER=your_database_user
DB_PASSWORD=your_database_password
DB_NAME=your_database_name
PORT=4000
```

## Database Setup

For production, use a managed database service:

- **MySQL:** PlanetScale, AWS RDS, Google Cloud SQL
- **PostgreSQL:** Supabase, Neon, Railway

## Post-Deployment Checklist

- [ ] Test all API endpoints
- [ ] Verify authentication works
- [ ] Check CORS settings
- [ ] Test on mobile devices
- [ ] Verify database connections
- [ ] Check error logging
- [ ] Enable HTTPS
- [ ] Set up monitoring

## Git Commit Best Practices

```bash
# Feature commits
git commit -m "feat: add dark mode support"

# Bug fixes
git commit -m "fix: resolve authentication issue"

# Documentation
git commit -m "docs: update README with deployment guide"

# Refactoring
git commit -m "refactor: improve component structure"

# Style changes
git commit -m "style: update button colors"

# Performance
git commit -m "perf: optimize API calls"
```

## Troubleshooting

### Build Failures
- Check Node.js version (should be 14+)
- Clear node_modules and reinstall
- Check for TypeScript errors

### CORS Issues
- Update backend CORS settings
- Verify API URL in frontend .env

### Database Connection
- Verify database credentials
- Check firewall settings
- Ensure database is accessible from deployment platform

