# MERN E-Commerce Application

A full-stack e-commerce application built with MongoDB, Express.js, React, and Node.js. Features include user authentication, product management, shopping cart, Stripe payments, and an admin dashboard.

## Tech Stack

### Backend
- **Node.js** with Express.js
- **MongoDB** with Mongoose
- **JWT** for authentication
- **Stripe** for payments
- **Jest + Supertest** for testing

### Frontend
- **React** with Vite
- **React Router** for navigation
- **Tailwind CSS** for styling
- **Axios** for API calls
- **Stripe.js** for payment processing

## Features

- ✅ User authentication (register, login, JWT)
- ✅ Product catalog with filtering and search
- ✅ Shopping cart with localStorage persistence
- ✅ Stripe payment integration
- ✅ Order management
- ✅ Product reviews and ratings
- ✅ Admin dashboard
- ✅ Responsive design
- ✅ Docker support
- ✅ RESTful API

## Project Structure

```
├── backend/
│   ├── config/          # Database configuration
│   ├── middleware/       # Auth and error handlers
│   ├── models/          # Mongoose models
│   ├── routes/          # API routes
│   ├── scripts/         # Seed script
│   ├── tests/           # Test files
│   └── server.js        # Express server
├── frontend/
│   ├── src/
│   │   ├── components/  # React components
│   │   ├── contexts/    # React contexts
│   │   ├── pages/       # Page components
│   │   └── utils/       # Utilities
│   └── package.json
├── docker-compose.yml   # Docker Compose config
└── README.md
```

## Getting Started

### Prerequisites

- Node.js 18+ and npm
- MongoDB (local or MongoDB Atlas)
- Stripe account (for payments)

### Local Development

#### 1. Clone the repository

```bash
git clone <repository-url>
cd e-commerce
```

#### 2. Backend Setup

```bash
cd backend
npm install
cp env.example .env
```

Edit `.env` and add your configuration:
```env
# Database
MONGODB_URI=your_mongodb_atlas_uri

# JWT / Auth
JWT_SECRET=your_jwt_secret_here
JWT_EXPIRE=7d

# Stripe
STRIPE_SECRET_KEY=sk_test_xxx
STRIPE_WEBHOOK_SECRET=whsec_xxx

# App
NODE_ENV=development
PORT=5000
FRONTEND_URL=http://localhost:5173

# Email (optional)
EMAIL_HOST=smtp.example.com
EMAIL_USER=your_email@example.com
EMAIL_PASS=your_email_password
```

#### 3. Frontend Setup

```bash
cd ../frontend
npm install
cp env.example .env
```

Edit `.env`:
```env
# API
VITE_API_URL=http://localhost:5000

# Stripe
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=pk_test_xxx

# App
NEXT_PUBLIC_APP_URL=http://localhost:5173
```

#### 4. Seed Database

```bash
cd ../backend
npm run seed
```

This creates:
- Admin user: `admin@example.com` / `admin123`
- Regular user: `user@example.com` / `user123`
- 10 sample products

#### 5. Run Development Servers

**Terminal 1 - Backend:**
```bash
cd backend
npm run dev
```

**Terminal 2 - Frontend:**
```bash
cd frontend
npm run dev
```

- Backend: http://localhost:5000
- Frontend: http://localhost:5173

## Docker Deployment

### Using Docker Compose

1. **Create environment files:**

   `backend/.env`:
   ```env
   # Database
   MONGODB_URI=mongodb://mongodb:27017/ecommerce
   
   # JWT / Auth
   JWT_SECRET=your-super-secret-jwt-key
   JWT_EXPIRE=7d
   
   # Stripe
   STRIPE_SECRET_KEY=sk_test_xxx
   STRIPE_WEBHOOK_SECRET=whsec_xxx
   
   # App
   NODE_ENV=production
   PORT=5000
   FRONTEND_URL=http://localhost:80
   ```

2. **Build and run:**
   ```bash
   docker-compose up -d
   ```

3. **Seed database:**
   ```bash
   docker-compose exec backend npm run seed
   ```

4. **Access application:**
   - Frontend: http://localhost:80
   - Backend API: http://localhost:5000

### Individual Docker Builds

**Backend:**
```bash
cd backend
docker build -t ecommerce-backend .
docker run -p 5000:5000 --env-file .env ecommerce-backend
```

**Frontend:**
```bash
cd frontend
docker build -t ecommerce-frontend .
docker run -p 80:80 ecommerce-frontend
```

## Deployment to Render

### Backend Deployment

1. Create a new **Web Service** on Render
2. Connect your GitHub repository
3. Set build command: `cd backend && npm install`
4. Set start command: `cd backend && npm start`
5. Add environment variables:
   - `MONGODB_URI`
   - `JWT_SECRET`
   - `STRIPE_SECRET_KEY`
   - `STRIPE_WEBHOOK_SECRET`
   - `FRONTEND_URL` (your frontend URL)

### Frontend Deployment

1. Create a new **Static Site** on Render
2. Connect your GitHub repository
3. Set build command: `cd frontend && npm install && npm run build`
4. Set publish directory: `frontend/dist`
5. Add environment variables:
   - `VITE_API_URL` (your backend URL)
   - `VITE_STRIPE_PUBLISHABLE_KEY`

### MongoDB Setup

1. Create a MongoDB database on MongoDB Atlas
2. Get connection string
3. Add to backend environment variables as `MONGODB_URI`

### Stripe Webhook Setup

1. In Stripe Dashboard, go to Webhooks
2. Add endpoint: `https://your-backend-url.onrender.com/api/stripe/webhook`
3. Select event: `checkout.session.completed`
4. Copy webhook secret to `STRIPE_WEBHOOK_SECRET`

## API Endpoints

### Authentication
- `POST /api/auth/register` - Register user
- `POST /api/auth/login` - Login user
- `POST /api/auth/logout` - Logout user
- `GET /api/auth/me` - Get current user

### Products
- `GET /api/products` - Get all products (with filters)
- `GET /api/products/:id` - Get single product
- `POST /api/products/:id/reviews` - Add product review

### Orders
- `POST /api/orders` - Create order
- `GET /api/orders` - Get user orders
- `GET /api/orders/:id` - Get single order

### Stripe
- `POST /api/stripe/create-checkout` - Create checkout session
- `POST /api/stripe/webhook` - Stripe webhook handler

### Admin
- `GET /api/admin/products` - Get all products (admin)
- `POST /api/admin/products` - Create product
- `PUT /api/admin/products/:id` - Update product
- `DELETE /api/admin/products/:id` - Delete product
- `GET /api/admin/orders` - Get all orders (admin)
- `PUT /api/admin/orders/:id` - Update order
- `GET /api/admin/stats` - Get sales statistics

## Testing

### Backend Tests

```bash
cd backend
npm test
```

Tests are located in `backend/tests/`:
- `auth.test.js` - Authentication tests
- `products.test.js` - Product API tests

## Scripts

### Backend
- `npm start` - Start production server
- `npm run dev` - Start development server with nodemon
- `npm run seed` - Seed database
- `npm test` - Run tests

### Frontend
- `npm run dev` - Start development server
- `npm run build` - Build for production
- `npm run preview` - Preview production build

## Environment Variables

### Backend (.env)

**Database:**
- `MONGODB_URI` - MongoDB connection string (MongoDB Atlas URI)

**JWT / Auth:**
- `JWT_SECRET` - JWT signing secret (use a strong random string)
- `JWT_EXPIRE` - JWT expiration time (default: 7d)

**Stripe:**
- `STRIPE_SECRET_KEY` - Stripe API secret key (starts with `sk_test_` or `sk_live_`)
- `STRIPE_WEBHOOK_SECRET` - Stripe webhook signing secret (starts with `whsec_`)

**App:**
- `NODE_ENV` - Environment (development/production)
- `PORT` - Server port (default: 5000)
- `FRONTEND_URL` - Frontend URL for CORS

**Email (optional):**
- `EMAIL_HOST` - SMTP server hostname
- `EMAIL_USER` - SMTP username
- `EMAIL_PASS` - SMTP password

### Frontend (.env)

**API:**
- `VITE_API_URL` - Backend API URL

**Stripe:**
- `VITE_STRIPE_PUBLISHABLE_KEY` - Stripe publishable key (starts with `pk_test_` or `pk_live_`)

**App:**
- `VITE_APP_URL` - Frontend application URL (optional, for redirects)

## Demo Credentials

After running `npm run seed`:
- **Admin**: `admin@example.com` / `admin123`
- **User**: `user@example.com` / `user123`

## Security Features

- JWT authentication with HTTP-only cookies
- Password hashing with bcrypt
- Input validation with express-validator
- Rate limiting on auth endpoints
- CORS configuration
- Environment variables for secrets

## License

MIT

## Support

For issues and questions, please open an issue on GitHub.
