# Deployment Guide

This guide will help you deploy the Air Quality Monitoring System to production.

## 🚀 Quick Start

1. **Frontend**: Deploy to Vercel
2. **Backend**: Deploy to Render or Railway
3. **Database**: Set up MongoDB Atlas
4. **Configure**: Environment variables and CORS

---

## 📱 Frontend Deployment (Vercel)

### Prerequisites
- Vercel account
- GitHub repository with your code

### Steps

1. **Connect to Vercel**
   ```bash
   # Install Vercel CLI
   npm i -g vercel
   
   # Login to Vercel
   vercel login
   ```

2. **Deploy from CLI**
   ```bash
   cd frontend
   vercel --prod
   ```

3. **Or Deploy via Vercel Dashboard**
   - Go to [vercel.com](https://vercel.com)
   - Click "New Project"
   - Import your GitHub repository
   - Select the `frontend` folder as root directory
   - Configure build settings:
     - **Build Command**: `npm run build`
     - **Output Directory**: `build`
     - **Install Command**: `npm install`

4. **Set Environment Variables**
   In Vercel dashboard, go to Settings → Environment Variables:
   ```
   REACT_APP_API_URL = https://your-backend-url.onrender.com/api
   NODE_ENV = production
   ```

5. **Redeploy** after setting environment variables

---

## 🔧 Backend Deployment

### Option 1: Render (Recommended)

#### Prerequisites
- Render account
- MongoDB Atlas account

#### Steps

1. **Set up MongoDB Atlas**
   - Go to [MongoDB Atlas](https://cloud.mongodb.com)
   - Create a new cluster
   - Create a database user
   - Whitelist IP addresses (0.0.0.0/0 for all IPs)
   - Get connection string

2. **Deploy to Render**
   - Go to [render.com](https://render.com)
   - Click "New +" → "Web Service"
   - Connect your GitHub repository
   - Configure:
     - **Root Directory**: `backend`
     - **Build Command**: `npm install`
     - **Start Command**: `npm start`
     - **Environment**: `Node`

3. **Set Environment Variables**
   ```
   NODE_ENV = production
   MONGODB_URI = mongodb+srv://username:password@cluster.mongodb.net/air_quality_db?retryWrites=true&w=majority
   JWT_SECRET = your-secure-jwt-secret-here
   CORS_ORIGIN = https://your-frontend.vercel.app
   API_RATE_LIMIT = 1000
   ```

### Option 2: Railway

#### Steps

1. **Deploy to Railway**
   - Go to [railway.app](https://railway.app)
   - Click "New Project" → "Deploy from GitHub repo"
   - Select your repository
   - Railway will auto-detect the backend folder

2. **Set Environment Variables**
   In Railway dashboard:
   ```
   NODE_ENV = production
   MONGODB_URI = your-mongodb-atlas-connection-string
   JWT_SECRET = your-secure-jwt-secret-here
   CORS_ORIGIN = https://your-frontend.vercel.app
   API_RATE_LIMIT = 1000
   ```

---

## 🗄️ Database Setup (MongoDB Atlas)

### Steps

1. **Create MongoDB Atlas Account**
   - Go to [cloud.mongodb.com](https://cloud.mongodb.com)
   - Sign up for free account

2. **Create Cluster**
   - Choose "M0 Sandbox" (free tier)
   - Select region closest to your users
   - Name your cluster

3. **Configure Security**
   - **Database Access**: Create user with read/write permissions
   - **Network Access**: Add IP address 0.0.0.0/0 (allow all IPs)

4. **Get Connection String**
   - Click "Connect" on your cluster
   - Choose "Connect your application"
   - Copy the connection string
   - Replace `<password>` with your database user password

5. **Test Connection**
   ```bash
   # Test locally
   cd backend
   MONGODB_URI="your-connection-string" npm start
   ```

---

## 🔒 Security Configuration

### Environment Variables Checklist

#### Frontend (.env.production)
```env
REACT_APP_API_URL=https://your-backend-url.onrender.com/api
NODE_ENV=production
```

#### Backend
```env
NODE_ENV=production
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/air_quality_db?retryWrites=true&w=majority
JWT_SECRET=your-very-secure-jwt-secret-minimum-32-characters
CORS_ORIGIN=https://your-frontend.vercel.app
API_RATE_LIMIT=1000
```

### Security Best Practices

1. **JWT Secret**: Use a strong, random secret (32+ characters)
2. **CORS**: Only allow your frontend domain
3. **Rate Limiting**: Configure appropriate limits
4. **HTTPS**: Both frontend and backend should use HTTPS
5. **Environment Variables**: Never commit secrets to version control

---

## 🧪 Testing Deployment

### 1. Test Backend API
```bash
# Health check
curl https://your-backend-url.onrender.com/api/health

# Test smoke reports
curl https://your-backend-url.onrender.com/api/smoke-reports
```

### 2. Test Frontend
- Visit your Vercel URL
- Test all features:
  - Home page loads
  - Dashboard works
  - Admin login (admin/admin123)
  - API calls work

### 3. Test Admin Dashboard
- Go to `/admin`
- Login with admin credentials
- Verify all features work:
  - Smoke reports table
  - Charts display
  - Status updates

---

## 🔄 Updating Deployment

### Frontend Updates
```bash
cd frontend
git add .
git commit -m "Update frontend"
git push origin main
# Vercel will auto-deploy
```

### Backend Updates
```bash
cd backend
git add .
git commit -m "Update backend"
git push origin main
# Render/Railway will auto-deploy
```

---

## 🐛 Troubleshooting

### Common Issues

1. **CORS Errors**
   - Check CORS_ORIGIN matches your frontend URL exactly
   - Ensure no trailing slashes

2. **Database Connection Issues**
   - Verify MongoDB Atlas IP whitelist includes 0.0.0.0/0
   - Check connection string format
   - Ensure database user has correct permissions

3. **Build Failures**
   - Check Node.js version compatibility
   - Verify all dependencies are in package.json
   - Check build logs for specific errors

4. **Environment Variables**
   - Ensure all required variables are set
   - Check for typos in variable names
   - Redeploy after changing environment variables

### Debug Commands

```bash
# Check backend logs
# Render: Dashboard → Service → Logs
# Railway: Dashboard → Deployments → View Logs

# Test API locally
curl -X GET http://localhost:5000/api/health

# Test with CORS
curl -H "Origin: https://your-frontend.vercel.app" \
     -H "Access-Control-Request-Method: GET" \
     -H "Access-Control-Request-Headers: X-Requested-With" \
     -X OPTIONS \
     https://your-backend-url.onrender.com/api/health
```

---

## 📊 Monitoring

### Health Checks
- **Backend**: `GET /api/health`
- **Frontend**: Check Vercel deployment status

### Performance Monitoring
- **Vercel**: Built-in analytics
- **Render**: Service metrics
- **Railway**: Deployment logs

---

## 🎯 Production Checklist

- [ ] Frontend deployed to Vercel
- [ ] Backend deployed to Render/Railway
- [ ] MongoDB Atlas cluster created and configured
- [ ] Environment variables set correctly
- [ ] CORS configured for production domains
- [ ] HTTPS enabled on both frontend and backend
- [ ] Admin dashboard accessible and functional
- [ ] API endpoints responding correctly
- [ ] Database connection working
- [ ] Error handling and logging configured
- [ ] Rate limiting configured
- [ ] Security headers enabled

---

## 📞 Support

If you encounter issues:

1. Check the troubleshooting section above
2. Review deployment logs
3. Verify environment variables
4. Test API endpoints individually
5. Check MongoDB Atlas connection

For additional help, refer to:
- [Vercel Documentation](https://vercel.com/docs)
- [Render Documentation](https://render.com/docs)
- [Railway Documentation](https://docs.railway.app)
- [MongoDB Atlas Documentation](https://docs.atlas.mongodb.com)