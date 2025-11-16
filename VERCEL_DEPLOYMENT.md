# Vercel Subdomain Deployment Guide

This guide explains how to deploy the backend and frontend on separate Vercel projects to enable subdomain deployment (e.g., `api.yourdomain.com` and `app.yourdomain.com`).

## Prerequisites

- A Vercel account
- Vercel CLI installed: `npm install -g vercel`
- A custom domain added to your Vercel account

## Step 1: Deploy Backend API

1. Navigate to the server directory:
   ```bash
   cd server
   ```

2. Deploy to Vercel:
   ```bash
   vercel
   ```

3. Follow the prompts:
   - Set up and deploy? **Y**
   - Which scope? Select your account
   - Link to existing project? **N**
   - Project name: `statemachine-api` (or your preferred name)
   - Directory: `./` (current directory)
   - Override settings? **N**

4. After deployment, set up the custom domain:
   - Go to your Vercel dashboard
   - Select the `statemachine-api` project
   - Go to Settings → Domains
   - Add domain: `api.yourdomain.com`
   - Follow DNS instructions to add the required records

## Step 2: Deploy Frontend

1. Navigate to the client directory:
   ```bash
   cd ../client
   ```

2. Deploy to Vercel:
   ```bash
   vercel
   ```

3. Follow the prompts:
   - Set up and deploy? **Y**
   - Which scope? Select your account
   - Link to existing project? **N**
   - Project name: `statemachine-app` (or your preferred name)
   - Directory: `./` (current directory)
   - Override settings? **Y**
   - Build Command: `npm run build`
   - Output Directory: `dist`
   - Development Command: `npm run dev`

4. Add environment variable for API URL:
   - Go to your Vercel dashboard
   - Select the `statemachine-app` project
   - Go to Settings → Environment Variables
   - Add variable:
     - Name: `VITE_API_URL`
     - Value: `https://api.yourdomain.com`
   - Select all environments (Production, Preview, Development)
   - Save

5. Set up the custom domain:
   - Go to Settings → Domains
   - Add domain: `app.yourdomain.com` or `yourdomain.com`
   - Follow DNS instructions

## Step 3: Update Frontend Configuration (Optional)

If you want the frontend to use the API subdomain in production, create an axios instance:

```javascript
// client/src/api.js
import axios from 'axios'

const api = axios.create({
  baseURL: import.meta.env.VITE_API_URL || '/api'
})

export default api
```

Then update your components to use this instance instead of axios directly:

```javascript
import api from '../api'

// Instead of: axios.get('/api/data')
// Use: api.get('/data')
```

## Step 4: Redeploy

After setting up the environment variable, redeploy the frontend:

```bash
cd client
vercel --prod
```

## Alternative: Monorepo Setup

If you prefer to keep everything in one repository but deploy separately:

1. Create `vercel.json` in the root:

```json
{
  "version": 2,
  "builds": [
    {
      "src": "server/index.js",
      "use": "@vercel/node"
    },
    {
      "src": "client/package.json",
      "use": "@vercel/static-build",
      "config": {
        "distDir": "client/dist"
      }
    }
  ]
}
```

2. Deploy different parts to different projects by specifying the directory during deployment.

## Verifying Deployment

After deployment, verify:

1. API is accessible at `https://api.yourdomain.com/api/health`
2. Frontend is accessible at `https://app.yourdomain.com` or `https://yourdomain.com`
3. Frontend can communicate with the API

## Troubleshooting

### CORS Issues
Make sure your backend CORS configuration allows requests from your frontend domain:

```javascript
app.use(cors({
  origin: ['https://app.yourdomain.com', 'https://yourdomain.com'],
  credentials: true
}));
```

### API Not Found
- Check that the `VITE_API_URL` environment variable is set correctly
- Verify the API is deployed and accessible
- Check browser console for errors

### Build Failures
- Ensure all dependencies are listed in `package.json`
- Check Vercel build logs for specific error messages
- Verify build command and output directory are correct
