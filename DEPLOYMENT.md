# Deployment Guide

This project consists of two parts:
1. **Frontend**: Next.js app (can be deployed to GitHub Pages)
2. **Backend**: FastAPI Python API (needs to be deployed separately)

## Option 1: GitHub Pages (Frontend) + Railway/Render (Backend) - Recommended

### Frontend Deployment (GitHub Pages)

1. **Enable GitHub Pages**:
   - Go to your repository Settings → Pages
   - Source: GitHub Actions
   - The workflow will automatically deploy on push to `main`

2. **Set Environment Variable** (optional):
   - Go to Settings → Secrets and variables → Actions
   - Add `NEXT_PUBLIC_API_URL` with your API URL (e.g., `https://your-api.railway.app`)

3. **Deploy**:
   - Push to `main` branch
   - The GitHub Actions workflow will build and deploy automatically

### Backend Deployment (Railway/Render)

#### Railway:
1. Create account at [railway.app](https://railway.app)
2. New Project → Deploy from GitHub
3. Select your repository
4. Set root directory to `api/`
5. Add environment variables if needed
6. Deploy!

#### Render:
1. Create account at [render.com](https://render.com)
2. New → Web Service
3. Connect GitHub repository
4. Settings:
   - Build Command: `pip install -r requirements.txt`
   - Start Command: `uvicorn main:app --host 0.0.0.0 --port $PORT`
   - Root Directory: `api`
5. Deploy!

## Option 2: Vercel (Full Stack)

Vercel can host Next.js natively, but the Python API needs to be:
- Converted to serverless functions, OR
- Deployed separately (Railway/Render) and connected

### Steps:
1. Install Vercel CLI: `npm i -g vercel`
2. In `frontend/` directory: `vercel`
3. Follow prompts
4. Deploy API separately and set `NEXT_PUBLIC_API_URL`

## Option 3: Netlify (Frontend) + Backend elsewhere

Similar to GitHub Pages but with Netlify's build system.

## Notes

- The frontend is configured for static export
- Make sure your API has CORS enabled for your frontend domain
- Update `NEXT_PUBLIC_API_URL` in the GitHub Actions workflow or as a secret

