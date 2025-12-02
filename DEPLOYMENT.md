# Deployment Guide

This guide covers deploying the MERN e-commerce application to various platforms.

## Quick Start

1. **Set up environment variables** (see `.env.example` files in `backend/` and `frontend/`)
2. **Seed the database**: `cd backend && npm run seed`
3. **Start services**: Use Docker Compose or run services separately

## Docker Compose Deployment

### Prerequisites
- Docker and Docker Compose installed

### Steps

1. **Configure environment variables:**
   - Copy `backend/.env.example` to `backend/.env`
   - Add your MongoDB URI, JWT secret, and Stripe keys

2. **Build and start:**
   ```bash
   docker-compose up -d
   ```

3. **Seed database:**
   ```bash
   docker-compose exec backend npm run seed
   ```

4. **Access:**
   - Frontend: http://localhost:80
   - Backend API: http://localhost:5000

## Render Deployment

### Backend (Web Service)

1. **Create new Web Service** on Render
2. **Connect repository** from GitHub
3. **Configure:**
   - **Name**: ecommerce-backend
   - **Environment**: Node
   - **Build Command**: `cd backend && npm install`
   - **Start Command**: `cd backend && npm start`
   - **Root Directory**: `backend`

4. **Environment Variables:**
   ```
   # Database
   MONGODB_URI=<your-mongodb-atlas-uri>
   
   # JWT / Auth
   JWT_SECRET=<strong-random-string>
   JWT_EXPIRE=7d
   
   # Stripe
   STRIPE_SECRET_KEY=sk_test_xxx
   STRIPE_WEBHOOK_SECRET=whsec_xxx
   
   # App
   NODE_ENV=production
   PORT=5000
   FRONTEND_URL=<your-frontend-url>
   ```

5. **Deploy**

### Frontend (Static Site)

1. **Create new Static Site** on Render
2. **Connect repository** from GitHub
3. **Configure:**
   - **Build Command**: `cd frontend && npm install && npm run build`
   - **Publish Directory**: `frontend/dist`

4. **Environment Variables:**
   ```
   # API
   VITE_API_URL=<your-backend-url>
   
   # Stripe
   VITE_STRIPE_PUBLISHABLE_KEY=pk_test_xxx
   
   # App
   VITE_APP_URL=<your-frontend-url>
   ```

5. **Deploy**

### MongoDB Atlas Setup

1. Create account at [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
2. Create a free cluster
3. Get connection string
4. Add to backend environment variables

### Stripe Webhook Configuration

1. Go to Stripe Dashboard → Webhooks
2. Add endpoint: `https://your-backend-url.onrender.com/api/stripe/webhook`
3. Select event: `checkout.session.completed`
4. Copy webhook signing secret
5. Add to backend environment variables as `STRIPE_WEBHOOK_SECRET`

## Heroku Deployment

### Backend

1. **Install Heroku CLI**
2. **Login**: `heroku login`
3. **Create app**: `heroku create your-app-name`
4. **Set environment variables**:
   ```bash
   heroku config:set MONGODB_URI=<your-uri>
   heroku config:set JWT_SECRET=<your-secret>
   heroku config:set STRIPE_SECRET_KEY=<your-key>
   # ... add all variables
   ```
5. **Deploy**: `git push heroku main`

### Frontend

1. **Build locally**: `cd frontend && npm run build`
2. **Use static hosting** (Netlify, Vercel, or similar)
3. **Configure environment variables** in hosting platform

## Environment Variables Reference

### Backend Required Variables

**Database:**
- `MONGODB_URI` - MongoDB connection string (e.g., `mongodb+srv://...`)

**JWT / Auth:**
- `JWT_SECRET` - Secret for JWT signing (use a strong random string)
- `JWT_EXPIRE` - JWT expiration (default: `7d`)

**Stripe:**
- `STRIPE_SECRET_KEY` - Stripe API secret key (e.g., `sk_test_...`)
- `STRIPE_WEBHOOK_SECRET` - Stripe webhook secret (e.g., `whsec_...`)

**App:**
- `NODE_ENV` - Environment (`development` or `production`)
- `PORT` - Server port (default: `5000`)
- `FRONTEND_URL` - Frontend URL for CORS (e.g., `https://your-frontend.com`)

### Frontend Required Variables

**API:**
- `VITE_API_URL` - Backend API URL (e.g., `https://your-backend.com`)

**Stripe:**
- `VITE_STRIPE_PUBLISHABLE_KEY` - Stripe publishable key (e.g., `pk_test_...`)

**App:**
- `VITE_APP_URL` - Frontend application URL (optional)

## Troubleshooting

### Backend won't start
- Check MongoDB connection string
- Verify all environment variables are set
- Check logs: `docker-compose logs backend`

### Frontend can't connect to backend
- Verify `VITE_API_URL` is correct
- Check CORS settings in backend
- Ensure backend is running and accessible

### Stripe payments not working
- Verify Stripe keys are correct
- Check webhook endpoint is configured
- Ensure webhook secret matches

### Database connection issues
- Verify MongoDB URI format
- Check network access (IP whitelist for Atlas)
- Ensure database user has proper permissions

