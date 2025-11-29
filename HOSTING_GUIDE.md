# Hosting Guide for Chat App

This guide will help you deploy your chat application to production.

## Hosting Architecture

Your app needs to be hosted in 3 parts:
1. **Frontend (React)** - Vercel or Netlify (Recommended: Vercel)
2. **Backend (Node.js + Socket.io)** - Railway, Render, or Fly.io (Recommended: Railway)
3. **Database (MongoDB)** - MongoDB Atlas (Free tier available)

---

## Step 1: Set Up MongoDB Atlas (Database)

1. Go to [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
2. Sign up for a free account
3. Create a new cluster (choose the FREE tier)
4. Create a database user:
   - Go to "Database Access" → "Add New Database User"
   - Create username and password (save these!)
5. Whitelist your IP:
   - Go to "Network Access" → "Add IP Address"
   - Click "Allow Access from Anywhere" (0.0.0.0/0) for development
6. Get your connection string:
   - Go to "Clusters" → "Connect" → "Connect your application"
   - Copy the connection string
   - Replace `<password>` with your database user password
   - Example: `mongodb+srv://username:password@cluster0.xxxxx.mongodb.net/chat-app?retryWrites=true&w=majority`

---

## Step 2: Deploy Backend (Railway - Recommended)

Railway supports WebSockets and is easy to use.

### Option A: Railway (Recommended)

1. Go to [Railway.app](https://railway.app)
2. Sign up with GitHub
3. Click "New Project" → "Deploy from GitHub repo"
4. Select your `ChatApp` repository
5. Configure the project:
   - **Root Directory**: Set to `server`
   - **Build Command**: Leave default (Railway auto-detects)
   - **Start Command**: `npm start`
6. Add Environment Variables:
   - Go to your project → "Variables"
   - Add these variables:
     ```
     PORT=5000
     MONGODB_URI=your_mongodb_atlas_connection_string
     JWT_SECRET=your_random_secret_key_here
     CLOUDINARY_CLOUD_NAME=your_cloudinary_cloud_name
     CLOUDINARY_API_KEY=your_cloudinary_api_key
     CLOUDINARY_API_SECRET=your_cloudinary_api_secret
     FRONTEND_URL=https://your-frontend.vercel.app
     ```
   - **Important**: Replace `your-frontend.vercel.app` with your actual frontend URL after deploying the frontend
7. Deploy:
   - Railway will automatically deploy
   - Once deployed, copy your backend URL (e.g., `https://your-app.railway.app`)
   - This is your `VITE_BACKEND_URL` for the frontend

### Option B: Render

1. Go to [Render.com](https://render.com)
2. Sign up with GitHub
3. Click "New" → "Web Service"
4. Connect your GitHub repository
5. Configure:
   - **Name**: `chat-app-backend`
   - **Root Directory**: `server`
   - **Environment**: Node
   - **Build Command**: `npm install`
   - **Start Command**: `npm start`
6. Add Environment Variables (same as Railway, including FRONTEND_URL)
7. Deploy and copy the backend URL

---

## Step 3: Deploy Frontend (Vercel - Recommended)

### Option A: Vercel (Recommended)

1. Go to [Vercel.com](https://vercel.com)
2. Sign up with GitHub
3. Click "Add New Project"
4. Import your `ChatApp` repository
5. Configure:
   - **Framework Preset**: Vite
   - **Root Directory**: `client`
   - **Build Command**: `npm run build`
   - **Output Directory**: `dist`
6. Add Environment Variable:
   - Go to "Settings" → "Environment Variables"
   - Add: `VITE_BACKEND_URL` = `https://your-backend-url.railway.app` (your backend URL from Step 2)
7. Deploy:
   - Click "Deploy"
   - Vercel will build and deploy your app
   - You'll get a URL like `https://your-app.vercel.app`

### Option B: Netlify

1. Go to [Netlify.com](https://netlify.com)
2. Sign up with GitHub
3. Click "Add new site" → "Import an existing project"
4. Select your repository
5. Configure:
   - **Base directory**: `client`
   - **Build command**: `npm run build`
   - **Publish directory**: `client/dist`
6. Add Environment Variable:
   - Go to "Site settings" → "Environment variables"
   - Add: `VITE_BACKEND_URL` = your backend URL
7. Deploy

---

## Step 4: Update CORS Configuration

After deploying, update your backend CORS to allow your frontend domain:

In `server/server.js`, update the CORS configuration:

```javascript
const corsOptions = {
  origin: [
    'http://localhost:5173',
    'https://your-frontend.vercel.app',
    // Add your production frontend URL here
  ],
  credentials: true
};
app.use(cors(corsOptions));
```

And update Socket.io CORS:

```javascript
export const io = new Server(server, {
    cors: {
        origin: [
            'http://localhost:5173',
            'https://your-frontend.vercel.app',
            // Add your production frontend URL here
        ],
        credentials: true
    }
})
```

---

## Step 5: Update Frontend Socket Connection

In `client/context/AuthContext.jsx`, the Socket.io connection should automatically use `VITE_BACKEND_URL`, which is good.

---

## Quick Checklist

- [ ] MongoDB Atlas cluster created and connection string obtained
- [ ] Backend deployed (Railway/Render)
- [ ] Backend URL copied
- [ ] Frontend deployed (Vercel/Netlify)
- [ ] Environment variables set in both frontend and backend
- [ ] CORS updated with production URLs
- [ ] Test the application end-to-end

---

## Troubleshooting

### Socket.io not connecting
- Make sure your hosting provider supports WebSockets (Railway and Render do)
- Check CORS settings include your frontend URL
- Verify `VITE_BACKEND_URL` is set correctly

### CORS errors
- Update CORS origin in `server/server.js` with your frontend URL
- Make sure credentials are enabled if using cookies

### Database connection issues
- Verify MongoDB Atlas IP whitelist includes 0.0.0.0/0
- Check connection string has correct password
- Ensure database name in connection string matches

### Environment variables not working
- Restart/redeploy after adding environment variables
- Check variable names match exactly (case-sensitive)
- For Vite, variables must start with `VITE_`

---

## Cost Estimate

- **MongoDB Atlas**: Free tier (512MB storage)
- **Railway**: Free tier (500 hours/month) or $5/month
- **Vercel**: Free tier (unlimited for personal projects)
- **Cloudinary**: Free tier (25 credits/month)

**Total: FREE** (with free tiers) or **~$5/month** (if you exceed free limits)

---

## Need Help?

If you encounter issues:
1. Check the hosting provider's logs
2. Verify all environment variables are set
3. Test API endpoints with Postman/Thunder Client
4. Check browser console for errors

