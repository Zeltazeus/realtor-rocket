# RealtorRocket Deployment Guide

## Render Deployment

This guide will help you deploy the RealtorRocket application to Render.

### Prerequisites

1. A GitHub account with the repository pushed
2. A Render account (free tier available)
3. The repository URL: `https://github.com/Zeltazeus/realtor-rocket.git`

### Deployment Steps

#### 1. Database Setup

1. Go to [Render Dashboard](https://dashboard.render.com)
2. Click "New +" → "PostgreSQL"
3. Configure:
   - **Name**: `realtor-rocket-db`
   - **Plan**: Free
   - **Database**: `realtor_rocket`
   - **User**: `realtor_rocket_user`
4. Save the connection string for later use

#### 2. Backend API Deployment

1. Go to [Render Dashboard](https://dashboard.render.com)
2. Click "New +" → "Web Service"
3. Connect your GitHub repository: `https://github.com/Zeltazeus/realtor-rocket.git`
4. Configure:
   - **Name**: `realtor-rocket-api`
   - **Environment**: Node
   - **Plan**: Free
   - **Build Command**: `npm install && npx prisma generate && npx prisma migrate deploy`
   - **Start Command**: `npm run start`
   - **Root Directory**: Leave empty (uses root)

5. **Environment Variables**:
   ```
   NODE_ENV=production
   DATABASE_URL=<your-postgres-connection-string>
   SECRET_KEY=<generate-a-secure-random-string>
   REFRESH_TOKEN_SECRET=<generate-another-secure-random-string>
   ACCESS_TOKEN_EXPIRES_IN=15m
   REFRESH_TOKEN_EXPIRES_IN=7d
   PORT=10000
   ```

6. Click "Create Web Service"

#### 3. Frontend Deployment

1. Go to [Render Dashboard](https://dashboard.render.com)
2. Click "New +" → "Static Site"
3. Connect your GitHub repository: `https://github.com/Zeltazeus/realtor-rocket.git`
4. Configure:
   - **Name**: `realtor-rocket-frontend`
   - **Plan**: Free
   - **Build Command**: `npm install && npm run build`
   - **Publish Directory**: `dist`
   - **Root Directory**: Leave empty

5. **Environment Variables**:
   ```
   VITE_API_URL=https://realtor-rocket-api.onrender.com
   ```

6. Click "Create Static Site"

### Environment Variables Reference

#### Backend (.env)
```env
NODE_ENV=production
DATABASE_URL=postgresql://user:password@host:port/database
SECRET_KEY=your-secret-key-here
REFRESH_TOKEN_SECRET=your-refresh-token-secret-here
ACCESS_TOKEN_EXPIRES_IN=15m
REFRESH_TOKEN_EXPIRES_IN=7d
PORT=10000
```

#### Frontend (Build-time)
```env
VITE_API_URL=https://your-backend-url.onrender.com
```

### Post-Deployment Steps

1. **Database Migration**: The database will be automatically migrated during deployment
2. **Test the Application**: 
   - Frontend: `https://realtor-rocket-frontend.onrender.com`
   - Backend API: `https://realtor-rocket-api.onrender.com`

### Troubleshooting

#### Common Issues:

1. **Database Connection Issues**:
   - Verify DATABASE_URL is correct
   - Check if database service is running

2. **Build Failures**:
   - Check Node.js version compatibility
   - Verify all dependencies are in package.json

3. **CORS Issues**:
   - Update CORS settings in server.js
   - Ensure frontend URL is whitelisted

4. **Environment Variables**:
   - Double-check all required variables are set
   - Ensure no typos in variable names

### Monitoring

- Check Render logs for any errors
- Monitor database connections
- Set up alerts for service downtime

### Scaling

- Upgrade to paid plans for better performance
- Add Redis for session management
- Implement CDN for static assets

### Security Considerations

- Use strong, unique secrets for JWT tokens
- Enable HTTPS (automatic on Render)
- Regularly update dependencies
- Monitor for security vulnerabilities

### Support

For issues with deployment:
1. Check Render service logs
2. Verify environment variables
3. Test locally with production settings
4. Contact Render support if needed
