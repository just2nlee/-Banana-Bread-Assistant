# Troubleshooting API Connection Issues

## Error: "Unexpected end of JSON input"

This means the API returned an empty or invalid response. Here's how to debug:

### Step 1: Check Browser Console

1. Open your Vercel site
2. Press **F12** to open Developer Tools
3. Go to **Console** tab
4. Try uploading an image
5. Look for logs showing:
   - `Calling API at: [URL]`
   - `Response status: [status code]`
   - `Response text: [actual response]`

### Step 2: Test Railway API Directly

1. **Test health endpoint**:
   ```
   https://your-api.railway.app/health
   ```
   Should return: `{"status": "healthy", "model_loaded": true}`

2. **Test root endpoint**:
   ```
   https://your-api.railway.app/
   ```
   Should return: `{"message": "BananaPredictor API is running!", "model_loaded": true}`

### Step 3: Check Railway Logs

1. Go to Railway Dashboard
2. Click on your API service
3. Go to **Logs** tab
4. Look for errors when you try to upload an image

### Common Issues & Fixes

#### Issue 1: Model Not Loaded
**Symptoms**: API returns `"model_loaded": false`

**Fix**:
- Make sure `banana_model.pt` is in the `api/` directory
- Check Railway logs for model loading errors
- Verify the model file was committed to git

#### Issue 2: Empty Response
**Symptoms**: Response text is empty or HTML

**Possible causes**:
- API crashed during request
- Wrong endpoint URL
- CORS blocking the response

**Fix**:
- Check Railway logs for errors
- Verify the API URL in Vercel environment variables
- Test the API endpoint directly in browser/Postman

#### Issue 3: CORS Error
**Symptoms**: Browser console shows CORS errors

**Fix**:
- The API already has CORS enabled (`allow_origins=["*"]`)
- Check Railway logs to see if requests are reaching the API
- Verify your Vercel domain is accessible

#### Issue 4: Wrong API URL
**Symptoms**: "Failed to fetch" or connection errors

**Fix**:
1. Get Railway API URL: Railway Dashboard → Service → Settings → Networking
2. Set in Vercel: Settings → Environment Variables → `NEXT_PUBLIC_API_URL`
3. Redeploy Vercel

### Step 4: Test with curl/Postman

Test the API directly:

```bash
curl -X POST https://your-api.railway.app/predict \
  -F "file=@path/to/banana.jpg"
```

This will show you the exact response from the API.

### Step 5: Check Response Format

The API should return:
```json
{
  "prediction": 5,
  "days_until_bake_ready": 5,
  "message": "Predicted: 5 days until bake-ready 🍞",
  "raw_prediction": 11.0
}
```

If you see a different format, the API code might need updating.

## Quick Debug Checklist

- [ ] Railway API is deployed and running
- [ ] Railway API `/health` endpoint works
- [ ] `NEXT_PUBLIC_API_URL` is set in Vercel
- [ ] Vercel has been redeployed after setting env var
- [ ] `banana_model.pt` is in the repository
- [ ] Railway logs show no errors
- [ ] Browser console shows the API URL being called

## Still Not Working?

1. **Check Railway logs** - Look for Python errors
2. **Check browser console** - Look for the actual response text
3. **Test API directly** - Use curl or Postman to isolate the issue
4. **Verify model file** - Make sure it's in the right location

