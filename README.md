# statemachine

A minimal full-stack application with Express (ES6) backend and Vue 3 (Composition API) frontend, configured for Vercel subdomain deployment.

## Tech Stack

### Backend (Server)
- Express.js with ES6 modules
- CORS enabled
- JSON encoding middleware
- Ready for Vercel serverless deployment

### Frontend (Client)
- Vue 3 with Composition API
- Vue Router for navigation
- Tailwind CSS 4 for styling
- Axios for HTTP requests
- Vite for build tooling

## Project Structure

```
├── server/           # Express backend
│   ├── index.js     # Main server file
│   ├── package.json
│   └── vercel.json  # Vercel config for API
└── client/          # Vue 3 frontend
    ├── src/
    │   ├── components/
    │   ├── views/
    │   ├── router/
    │   ├── App.vue
    │   ├── main.js
    │   └── style.css
    ├── index.html
    ├── package.json
    └── vercel.json  # Vercel config for frontend
```

## Local Development

### Server
```bash
cd server
npm install
npm run dev
# Server runs on http://localhost:3000
```

### Client
```bash
cd client
npm install
npm run dev
# Frontend runs on http://localhost:5173
```

The client is configured with a proxy to forward `/api` requests to the backend during development.

## Vercel Deployment (Subdomain Setup)

Deploy the backend and frontend as **separate Vercel projects** to run on different subdomains.

### Backend Deployment (api.yourdomain.com)

1. Create a new Vercel project for the backend:
   ```bash
   cd server
   vercel
   ```

2. Configure the project:
   - Set root directory to `server`
   - Framework: Other
   - Build command: (leave empty)
   - Output directory: (leave empty)

3. Set up custom domain:
   - Go to Project Settings → Domains
   - Add domain: `api.yourdomain.com`

### Frontend Deployment (app.yourdomain.com or yourdomain.com)

1. Create a new Vercel project for the frontend:
   ```bash
   cd client
   vercel
   ```

2. Configure the project:
   - Set root directory to `client`
   - Framework: Vite
   - Build command: `npm run build`
   - Output directory: `dist`

3. Add environment variable:
   - Name: `VITE_API_URL`
   - Value: `https://api.yourdomain.com`

4. Update `client/vite.config.js` for production:
   ```javascript
   server: {
     proxy: {
       '/api': {
         target: process.env.VITE_API_URL || 'http://localhost:3000',
         changeOrigin: true,
       }
     }
   }
   ```

5. Set up custom domain:
   - Go to Project Settings → Domains
   - Add domain: `app.yourdomain.com` or `yourdomain.com`

### Update Frontend to Use API Subdomain

Update axios calls in the frontend to use the API subdomain in production. You can create an axios instance:

```javascript
// client/src/api.js
import axios from 'axios'

const api = axios.create({
  baseURL: import.meta.env.VITE_API_URL || '/api'
})

export default api
```

## API Endpoints

- `GET /api/health` - Health check
- `GET /api/data` - Fetch sample data
- `POST /api/data` - Submit data

## Features

- ✅ Express with ES6 modules
- ✅ Vue 3 with Composition API
- ✅ Vue Router with multiple routes
- ✅ Tailwind CSS 4 styling
- ✅ Axios for HTTP requests
- ✅ CORS enabled
- ✅ JSON encoding
- ✅ Vercel deployment ready
- ✅ Subdomain configuration support

## License

ISC
