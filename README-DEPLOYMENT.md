# 🚀 Air Quality Monitoring System - Deployment Guide

This guide provides step-by-step instructions for deploying the Air Quality Monitoring System to production.

## 📋 Prerequisites

- GitHub account with your code repository
- Vercel account (for frontend)
- Render or Railway account (for backend)
- MongoDB Atlas account (for database)

## 🏗️ Architecture Overview

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Frontend      │    │   Backend       │    │   Database      │
│   (Vercel)      │◄──►│   (Render/      │◄──►│   (MongoDB      │
│   React App     │    │    Railway)     │    │    Atlas)       │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

## 🎯 Quick Deployment (5 minutes)

### 1. Deploy Frontend to Vercel

```bash
# Option 1: Using Vercel CLI
cd frontend
npm i -g vercel
vercel --prod

# Option 2: Using Vercel Dashboard
# 1. Go to vercel.com
# 2. Import GitHub repository
# 3. Select frontend folder
# 4. Deploy
```

### 2. Deploy Backend to Render

```bash
# 1. Go to render.com
# 2. Create new Web Service
# 3. Connect GitHub repository
# 4. Select backend folder
# 5. Deploy
```

### 3. Set up MongoDB Atlas

```bash
# 1. Go to cloud.mongodb.com
# 2. Create free cluster
# 3. Create database user
# 4. Whitelist IP addresses (0.0.0.0/0)
# 5. Get connection string
```

## 🔧 Detailed Setup Instructions

### Frontend Deployment (Vercel)

#### Method 1: Vercel CLI

```bash
# Install Vercel CLI
npm install -g vercel

# Login to Vercel
vercel login

# Navigate to frontend directory
cd frontend

# Deploy to production
vercel --prod

# Set environment variables
vercel env add REACT_APP_API_URL
# Enter: https://your-backend-url.onrender.com/api

vercel env add NODE_ENV
# Enter: production
```

#### Method 2: Vercel Dashboard

1. **Connect Repository**
   - Go to [vercel.com](https://vercel.com)
   - Click "New Project"
   - Import your GitHub repository

2. **Configure Build Settings**
   - **Root Directory**: `frontend`
   - **Build Command**: `npm run build`
   - **Output Directory**: `build`
   - **Install Command**: `npm install`

3. **Set Environment Variables**
   - Go to Settings → Environment Variables
   - Add the following:
     ```
     REACT_APP_API_URL = https://your-backend-url.onrender.com/api
     NODE_ENV = production
     ```

4. **Deploy**
   - Click "Deploy"
   - Wait for deployment to complete
   - Note your frontend URL

### Backend Deployment

#### Option 1: Render (Recommended)

1. **Create Account**
   - Go to [render.com](https://render.com)
   - Sign up with GitHub

2. **Deploy Service**
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

#### Option 2: Railway

1. **Create Account**
   - Go to [railway.app](https://railway.app)
   - Sign up with GitHub

2. **Deploy Service**
   - Click "New Project" → "Deploy from GitHub repo"
   - Select your repository
   - Railway auto-detects the backend

3. **Set Environment Variables**
   - Go to Variables tab
   - Add the same variables as Render

### Database Setup (MongoDB Atlas)

1. **Create Account**
   - Go to [cloud.mongodb.com](https://cloud.mongodb.com)
   - Sign up for free account

2. **Create Cluster**
   - Choose "M0 Sandbox" (free tier)
   - Select region closest to your users
   - Name your cluster (e.g., "air-quality-cluster")

3. **Configure Security**
   - **Database Access**:
     - Click "Add New Database User"
     - Username: `air-quality-user`
     - Password: Generate secure password
     - Database User Privileges: "Read and write to any database"
   
   - **Network Access**:
     - Click "Add IP Address"
     - Choose "Allow Access from Anywhere" (0.0.0.0/0)

4. **Get Connection String**
   - Click "Connect" on your cluster
   - Choose "Connect your application"
   - Copy the connection string
   - Replace `<password>` with your database user password

## 🔒 Security Configuration

### Environment Variables Checklist

#### Frontend (Vercel)
```env
REACT_APP_API_URL=https://your-backend-url.onrender.com/api
NODE_ENV=production
```

#### Backend (Render/Railway)
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
4. **HTTPS**: Both frontend and backend use HTTPS
5. **Environment Variables**: Never commit secrets to version control

## 🧪 Testing Your Deployment

### 1. Test Backend API

```bash
# Health check
curl https://your-backend-url.onrender.com/api/health

# Expected response:
# {"success":true,"message":"API is healthy","timestamp":"2024-01-01T00:00:00.000Z"}
```

### 2. Test Frontend

1. Visit your Vercel URL
2. Test all features:
   - Home page loads
   - Dashboard works
   - Admin login (admin/admin123)
   - API calls work

### 3. Test Admin Dashboard

1. Go to `/admin`
2. Login with admin credentials
3. Verify all features work:
   - Smoke reports table
   - Charts display
   - Status updates

## 🔄 Updating Your Deployment

### Frontend Updates
```bash
cd frontend
git add .
git commit -m "Update frontend"
git push origin main
# Vercel auto-deploys
```

### Backend Updates
```bash
cd backend
git add .
git commit -m "Update backend"
git push origin main
# Render/Railway auto-deploys
```

## 🐛 Troubleshooting

### Common Issues

1. **CORS Errors**
   ```
   Error: Not allowed by CORS
   ```
   - **Solution**: Check CORS_ORIGIN matches your frontend URL exactly
   - Ensure no trailing slashes

2. **Database Connection Issues**
   ```
   MongoDB connection error
   ```
   - **Solution**: Verify MongoDB Atlas IP whitelist includes 0.0.0.0/0
   - Check connection string format
   - Ensure database user has correct permissions

3. **Build Failures**
   ```
   Build failed
   ```
   - **Solution**: Check Node.js version compatibility
   - Verify all dependencies are in package.json
   - Check build logs for specific errors

4. **Environment Variables Not Working**
   ```
   undefined environment variable
   ```
   - **Solution**: Ensure all required variables are set
   - Check for typos in variable names
   - Redeploy after changing environment variables

### Debug Commands

```bash
# Test API locally
curl -X GET http://localhost:5000/api/health

# Test with CORS
curl -H "Origin: https://your-frontend.vercel.app" \
     -H "Access-Control-Request-Method: GET" \
     -H "Access-Control-Request-Headers: X-Requested-With" \
     -X OPTIONS \
     https://your-backend-url.onrender.com/api/health

# Check deployment logs
# Render: Dashboard → Service → Logs
# Railway: Dashboard → Deployments → View Logs
```

## 📊 Monitoring and Maintenance

### Health Checks
- **Backend**: `GET /api/health`
- **Frontend**: Check Vercel deployment status

### Performance Monitoring
- **Vercel**: Built-in analytics and performance metrics
- **Render**: Service metrics and logs
- **Railway**: Deployment logs and metrics

### Regular Maintenance
1. **Monitor logs** for errors
2. **Update dependencies** regularly
3. **Check database** performance
4. **Review security** settings

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

## 📞 Support and Resources

### Documentation
- [Vercel Documentation](https://vercel.com/docs)
- [Render Documentation](https://render.com/docs)
- [Railway Documentation](https://docs.railway.app)
- [MongoDB Atlas Documentation](https://docs.atlas.mongodb.com)

### Getting Help
1. Check the troubleshooting section above
2. Review deployment logs
3. Verify environment variables
4. Test API endpoints individually
5. Check MongoDB Atlas connection

## 🎉 Congratulations!

Your Air Quality Monitoring System is now deployed and ready for production use! 

### Next Steps
1. **Test all features** thoroughly
2. **Set up monitoring** and alerts
3. **Configure backups** for your database
4. **Plan for scaling** as your user base grows
5. **Consider adding** additional features like email notifications

---

**Happy Deploying! 🚀**
