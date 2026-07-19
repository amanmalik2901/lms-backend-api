# 📚 LMS Backend API

A production-grade **RESTful API** for a Learning Management System built with **Node.js**, **Express 5**, and **MongoDB**. Supports full course lifecycle management, dual payment gateways (Stripe + Razorpay), role-based access control, and a multi-layered security stack.

---

## 🚀 Features

- 🔐 **JWT Authentication** — Secure cookie-based auth with bcrypt password hashing (12 salt rounds)
- 👥 **Role-Based Access Control** — Student, Instructor, and Admin roles with middleware enforcement
- 🎓 **Course Management** — Full CRUD for courses and lectures with Cloudinary media uploads
- 💳 **Dual Payment Gateways** — Stripe Checkout Sessions + Razorpay Orders with HMAC signature verification
- 📈 **Course Progress Tracking** — Lecture-level completion state with percentage calculation
- 🛡️ **Security Hardened** — Helmet, HPP, rate limiting (100 req/15 min), mongo-sanitize, XSS protection
- 🩺 **Health Check Endpoint** — DB connection status monitoring at `/health`
- ✅ **Input Validation** — Schema-level validation via express-validator with reusable validation chains

---

## 🛠️ Tech Stack

| Layer              | Technology                                                         |
| ------------------ | ------------------------------------------------------------------ |
| **Runtime**        | Node.js (ES Modules)                                               |
| **Framework**      | Express 5                                                          |
| **Database**       | MongoDB + Mongoose 8                                               |
| **Authentication** | JWT + bcryptjs                                                     |
| **Payments**       | Stripe, Razorpay                                                   |
| **File Uploads**   | Multer + Cloudinary                                                |
| **Security**       | Helmet, HPP, express-rate-limit, express-mongo-sanitize, xss-clean |
| **Validation**     | express-validator                                                  |
| **Logging**        | Morgan                                                             |

---

## 📁 Project Structure

```
server-solution/
├── controllers/         # Business logic handlers
│   ├── user.controller.js
│   ├── course.controller.js
│   ├── coursePurchase.controller.js
│   ├── courseProgress.controller.js
│   ├── razorpay.controller.js
│   └── health.controller.js
├── models/              # Mongoose schemas
│   ├── user.model.js
│   ├── course.model.js
│   ├── lecture.model.js
│   ├── coursePurchase.model.js
│   └── courseProgress.js
├── routes/              # Express routers (7 route modules)
├── middleware/          # Auth, error handling, validation
├── utils/               # Cloudinary, JWT token, Multer
├── database/            # MongoDB connection with retry logic
└── index.js             # App entry point with security middleware stack
```

---

## 🔌 API Endpoints

| Module    | Base Path          | Key Endpoints                                                            |
| --------- | ------------------ | ------------------------------------------------------------------------ |
| Users     | `/api/v1/user`     | signup, signin, signout, profile, password change, forgot/reset password |
| Courses   | `/api/v1/course`   | create, update, delete, search, publish, lectures                        |
| Purchases | `/api/v1/purchase` | Stripe checkout, webhook, purchase status                                |
| Razorpay  | `/api/v1/razorpay` | create order, verify payment                                             |
| Progress  | `/api/v1/progress` | get progress, update lecture, mark complete, reset                       |
| Media     | `/api/v1/media`    | upload media to Cloudinary                                               |
| Health    | `/health`          | server and DB status                                                     |

---

## ⚙️ Getting Started

### 1. Clone the repo

```bash
git clone https://github.com/amanmalik2901/lms-backend-api.git
cd lms-backend-api
```

### 2. Install dependencies

```bash
npm install
```

### 3. Set up environment variables

```bash
cp env.example .env
```

Fill in your values:

```env
PORT=8000
NODE_ENV=development
MONGO_URI=your_mongodb_connection_string
JWT_SECRET=your_jwt_secret
JWT_EXPIRES_IN=7d
CLIENT_URL=http://localhost:3000
CLOUDINARY_CLOUD_NAME=your_cloud_name
CLOUDINARY_API_KEY=your_api_key
CLOUDINARY_API_SECRET=your_api_secret
STRIPE_SECRET_KEY=sk_test_...
STRIPE_WEBHOOK_SECRET=whsec_...
RAZORPAY_KEY_ID=rzp_test_...
RAZORPAY_KEY_SECRET=your_razorpay_secret
```

### 4. Run the development server

```bash
npm run dev
```

Server starts at `http://localhost:8000`

---

## 🔐 Security Architecture

```
Request → Rate Limiter (100/15min) → Helmet → HPP → CORS → Auth Middleware → Controller
```

- Passwords hashed with **bcrypt** (12 salt rounds)
- Password reset via **SHA-256 hashed token** with 10-minute expiry
- All routes protected by **JWT cookie verification**
- Role enforcement via `restrictTo('admin', 'instructor')` middleware

---
