# Deploy Frontend to Vercel - Step by Step Guide

## Method 1: Using Vercel CLI (Recommended)

### Step 1: Install Vercel CLI

```bash
npm install -g vercel
```

Or if you prefer using npx (no installation needed):
```bash
npx vercel
```

### Step 2: Navigate to Client Directory

```bash
cd client
```

### Step 3: Login to Vercel

```bash
vercel login
```

This will open your browser to authenticate with Vercel (GitHub, GitLab, or Bitbucket).

### Step 4: Deploy

```bash
vercel
```

Follow the prompts:
- **Set up and deploy?** → Type `Y` and press Enter
- **Which scope?** → Select your account/team
- **Link to existing project?** → Type `N` (first time) or `Y` (if updating)
- **What's your project's name?** → Press Enter for default or type custom name
- **In which directory is your code located?** → Press Enter (current directory is correct)

### Step 5: Production Deployment

For production deployment:
```bash
vercel --prod
```

---

## Method 2: Using Vercel Dashboard (GitHub Integration)

### Step 1: Push Code to GitHub

```bash
# Initialize git if not already done
git init

# Add all files
git add .

# Commit
git commit -m "Initial commit - ready for Vercel deployment"

# Add your GitHub repository (replace with your repo URL)
git remote add origin https://github.com/yourusername/your-repo-name.git

# Push to GitHub
git push -u origin main
```

### Step 2: Connect to Vercel

1. Go to [vercel.com](https://vercel.com)
2. Click **"Sign Up"** or **"Log In"**
3. Click **"Import Project"** or **"Add New" → "Project"**
4. Import your GitHub repository
5. Select the repository containing your project

### Step 3: Configure Project Settings

In Vercel project settings:

- **Framework Preset:** Vite
- **Root Directory:** `client` (IMPORTANT: Set this!)
- **Build Command:** `npm run build` (auto-detected)
- **Output Directory:** `dist` (auto-detected)
- **Install Command:** `npm install` (auto-detected)

### Step 4: Environment Variables

Add environment variables if needed:

1. Go to **Settings** → **Environment Variables**
2. Add:
   ```
   VITE_API_BASE = https://your-backend-url.com/api
   ```
   (Replace with your actual backend URL)

### Step 5: Deploy

Click **"Deploy"** button. Vercel will:
- Install dependencies
- Build your project
- Deploy to production

---

## Method 3: Quick Deploy (One Command)

If you're already in the `client` directory:

```bash
npx vercel --prod
```

This will:
- Prompt for login if not logged in
- Deploy directly to production
- Give you a URL immediately

---

## Configuration Already Set Up

Your `vercel.json` is already configured with:
- ✅ Build command: `npm run build`
- ✅ Output directory: `dist`
- ✅ Framework: Vite
- ✅ SPA routing (rewrites)

---

## After Deployment

### Get Your Deployment URL

After deployment, Vercel will provide:
- **Production URL:** `https://your-project.vercel.app`
- **Preview URLs:** For each commit/pull request

### Update Backend CORS

Make sure your backend allows requests from your Vercel domain:

```javascript
// In server/index.js
app.use(cors({
  origin: [
    'http://localhost:5173',  // Local dev
    'https://your-project.vercel.app'  // Vercel production
  ]
}));
```

### Environment Variables

If your backend URL changes, update the environment variable in Vercel:
1. Go to **Project Settings** → **Environment Variables**
2. Update `VITE_API_BASE`

---

## Troubleshooting

### Build Fails

**Error: "Cannot find module"**
```bash
# Make sure dependencies are installed
cd client
npm install
```

**Error: "Build command failed"**
- Check that `npm run build` works locally
- Review build logs in Vercel dashboard

### 404 Errors on Routes

Your `vercel.json` already has rewrites configured. If you still get 404s:
- Ensure `vercel.json` is in the `client` directory
- Check that rewrites are correct

### Environment Variables Not Working

- Make sure variables start with `VITE_` prefix
- Redeploy after adding variables
- Check variable names match exactly

---

## Quick Reference Commands

```bash
# Deploy to preview
vercel

# Deploy to production
vercel --prod

# View deployment logs
vercel logs

# List all deployments
vercel ls

# Remove deployment
vercel remove

# Open project dashboard
vercel
```

---

## Example Workflow

```bash
# 1. Make changes to your code
# 2. Test locally
cd client
npm run dev

# 3. Build to test
npm run build

# 4. Deploy to Vercel
vercel --prod

# 5. Done! Your site is live
```

---

## Need Help?

- **Vercel Docs:** https://vercel.com/docs
- **Vercel Discord:** https://vercel.com/discord
- **Check deployment logs** in Vercel dashboard for errors

---

## Important Notes

1. **Root Directory:** If your project is in a monorepo, make sure to set **Root Directory** to `client` in Vercel settings
2. **Build Output:** Vercel automatically detects `dist` folder from Vite
3. **Environment Variables:** Must start with `VITE_` to be accessible in React
4. **Free Tier:** Vercel free tier is generous and perfect for projects

