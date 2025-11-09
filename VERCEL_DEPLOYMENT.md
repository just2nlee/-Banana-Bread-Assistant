# Deploying to Vercel

This guide will help you deploy both the frontend and backend to Vercel.

## ⚠️ Important Note About PyTorch

**PyTorch is very large (~2-3GB)** and may hit Vercel's serverless function limits:
- **Function size limit**: 50MB (uncompressed)
- **Memory limit**: 3GB (Pro plan) or 1GB (Hobby plan)
- **Timeout**: 10s (Hobby) or 60s (Pro)

If you encounter issues, consider:
1. **Using Vercel Pro** (better limits)
2. **Deploying API separately** on Railway/Render (recommended for production)
3. **Optimizing the model** (quantization, smaller architecture)

## Quick Deploy

### Option 1: Vercel CLI (Recommended)

1. **Install Vercel CLI**:
   ```bash
   npm i -g vercel
   ```

2. **Login**:
   ```bash
   vercel login
   ```

3. **Deploy** (from project root):
   ```bash
   vercel
   ```

4. **Follow prompts**:
   - Link to existing project? **No** (first time)
   - Project name: `banana-bread-assistant` (or your choice)
   - Directory: `.` (root)
   - Override settings? **No**

5. **Add environment variables** (if needed):
   ```bash
   vercel env add NEXT_PUBLIC_API_URL
   # Enter: /api (or leave blank to use relative path)
   ```

### Option 2: GitHub Integration

1. **Go to [vercel.com](https://vercel.com)** and sign in with GitHub

2. **Import Project**:
   - Click "Add New" → "Project"
   - Import your GitHub repository
   - Vercel will auto-detect the configuration

3. **Configure**:
   - **Framework Preset**: Next.js (auto-detected)
   - **Root Directory**: `.` (root)
   - **Build Command**: (auto-detected)
   - **Output Directory**: (auto-detected)

4. **Environment Variables** (optional):
   - `NEXT_PUBLIC_API_URL`: `/api` (or leave blank)

5. **Deploy**:
   - Click "Deploy"
   - Wait for build to complete

## Project Structure

Vercel will automatically:
- Deploy `frontend/` as a Next.js app
- Deploy `api/` as Python serverless functions (via `vercel.json`)

## API Routes

The API will be available at:
- Production: `https://your-project.vercel.app/api/predict`
- The frontend automatically uses `/api` (relative path)

## Model File

Make sure `banana_model.pt` is in the repository root or `api/` directory. Vercel will include it in the deployment.

## Troubleshooting

### Build Fails Due to PyTorch Size

**Solution 1**: Deploy API separately
- Deploy frontend on Vercel
- Deploy API on Railway/Render
- Set `NEXT_PUBLIC_API_URL` to your API URL

**Solution 2**: Upgrade to Vercel Pro
- Better memory and timeout limits
- More suitable for ML workloads

**Solution 3**: Optimize model
- Use model quantization
- Use a smaller model architecture
- Compress the model file

### API Returns 503 (Model Not Loaded)

- Check that `banana_model.pt` exists in the repo
- Check build logs for model loading errors
- Verify the model path in `api/main.py`

### CORS Errors

The API is configured to allow all origins. If you have issues:
- Update `allow_origins` in `api/main.py` with your Vercel domain

## Environment Variables

Set in Vercel Dashboard → Settings → Environment Variables:

- `NEXT_PUBLIC_API_URL`: API endpoint (default: `/api` for same domain)

## Custom Domain

1. Go to Project Settings → Domains
2. Add your custom domain
3. Follow DNS configuration instructions

## Monitoring

- Check deployment logs in Vercel Dashboard
- Monitor function execution in the Functions tab
- Set up error tracking (Sentry, etc.)

## Next Steps

After deployment:
1. Test the API: `https://your-project.vercel.app/api/health`
2. Test the frontend: `https://your-project.vercel.app`
3. Upload a banana image to verify end-to-end functionality

