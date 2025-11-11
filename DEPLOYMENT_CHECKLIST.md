# Deployment Checklist

Follow these steps in order to deploy your chat app.

## ✅ Step 1: MongoDB Atlas Setup
- [ ] Sign up at https://www.mongodb.com/cloud/atlas
- [ ] Create a free cluster
- [ ] Create database user (save username & password!)
- [ ] Whitelist IP (Allow from anywhere: 0.0.0.0/0)
- [ ] Get connection string
- [ ] Test connection string format

**Connection String Format:**
```
mongodb+srv://USERNAME:PASSWORD@CLUSTER.mongodb.net/chat-app?retryWrites=true&w=majority
```

---

## ✅ Step 2: Cloudinary Setup (for image uploads)
- [ ] Sign up at https://cloudinary.com (free account)
- [ ] Go to Dashboard
- [ ] Copy:
  - Cloud Name
  - API Key
  - API Secret

---

## ✅ Step 3: Deploy Backend (Railway)
- [ ] Go to https://railway.app
- [ ] Sign up with GitHub
- [ ] New Project → Deploy from GitHub
- [ ] Select ChatApp repository
- [ ] Set Root Directory: `server`
- [ ] Add Environment Variables:
  - [ ] PORT=5000
  - [ ] MONGODB_URI=(your MongoDB connection string)
  - [ ] JWT_SECRET=(generate random string)
  - [ ] CLOUDINARY_CLOUD_NAME=(from Cloudinary)
  - [ ] CLOUDINARY_API_KEY=(from Cloudinary)
  - [ ] CLOUDINARY_API_SECRET=(from Cloudinary)
  - [ ] FRONTEND_URL=(will add after frontend deploys)
- [ ] Wait for deployment
- [ ] Copy backend URL (e.g., https://your-app.railway.app)

---

## ✅ Step 4: Deploy Frontend (Vercel)
- [ ] Go to https://vercel.com
- [ ] Sign up with GitHub
- [ ] Add New Project
- [ ] Import ChatApp repository
- [ ] Configure:
  - Framework: Vite
  - Root Directory: `client`
  - Build Command: `npm run build`
  - Output Directory: `dist`
- [ ] Add Environment Variable:
  - [ ] VITE_BACKEND_URL=(your Railway backend URL)
- [ ] Deploy
- [ ] Copy frontend URL (e.g., https://your-app.vercel.app)

---

## ✅ Step 5: Update Backend CORS
- [ ] Go back to Railway
- [ ] Update FRONTEND_URL environment variable with your Vercel URL
- [ ] Redeploy backend (Railway auto-redeploys on env change)

---

## ✅ Step 6: Test Your App
- [ ] Visit your Vercel frontend URL
- [ ] Create a new account
- [ ] Test login
- [ ] Test messaging
- [ ] Test profile picture upload

---

## 🎉 Done!
Your app should now be live and working!

