# VaultIQ  Inventory Management System

> A premium full-stack inventory management system built with Node.js, Express, MongoDB, and React. Designed for precision in every transaction and clarity in every decision.

---

## 👤 Author

**KAYIRANGA Jesus**  
CEO & Founder (VaultIQ)  
Built and designed from scratch.

---

## 📌 Project Overview

VaultIQ is a role-based inventory management web application that allows businesses to track products, manage stock levels, and handle orders — all through a clean, premium Black & Gold interface.

There are two types of users in the system:
- **Admin** — has full control over products, orders, and users
- **User** — can browse products and place/manage their own orders

---

## 🗂️ Project Structure
```
VaultIQ/
├── inventory-api/                  # Backend (Node.js + Express)
│   ├── config/
│   │   └── database.js             # MongoDB connection
│   ├── controllers/
│   │   ├── authController.js       # Register & login logic
│   │   ├── prodController.js       # Product CRUD logic
│   │   └── orderController.js      # Order CRUD logic
│   ├── middleware/
│   │   ├── auth.js                 # JWT authentication middleware
│   │   └── admin.js                # Admin role guard middleware
│   ├── models/
│   │   ├── user.js                 # User schema
│   │   ├── product.js              # Product schema
│   │   └── order.js                # Order schema
│   ├── routes/
│   │   ├── authRoutes.js           # Auth endpoints
│   │   ├── productRoutes.js        # Product endpoints
│   │   └── orderRoutes.js          # Order endpoints
│   ├── .env                        # Environment variables
│   └── server.js                   # Express server entry point
│
└── frontend/                       # Frontend (React)
    └── src/
        ├── api/
        │   └── index.js            # API fetch helper with JWT injection
        ├── context/
        │   └── AuthContext.jsx     # Global auth state (login/logout)
        ├── components/
        │   ├── Login.jsx           # Login page with CEO card
        │   ├── register.jsx        # Register page
        │   ├── Navbar.jsx          # Sidebar navigation
        │   ├── Dashboard.jsx       # User dashboard with stats
        │   ├── Products.jsx        # Product browsing & ordering
        │   ├── AddProduct.jsx      # Create/edit product modal
        │   ├── AdminDashboard.jsx  # Admin analytics overview
        │   ├── AdminProducts.jsx   # Admin product management
        │   ├── AdminOrders.jsx     # Admin all orders + MyOrders
        │   ├── AdminUsers.jsx      # Admin user list
        │   └── ProtectedRoute.jsx  # Route guard component
        ├── App.js                  # Main app entry & routing
        └── App.css                 # All styles (Black & Gold theme)
```

---

## ⚙️ Tech Stack

### Backend
| Technology | Purpose |
|---|---|
| Node.js | Runtime environment |
| Express.js | Web framework & API routing |
| MongoDB | NoSQL database |
| Mongoose | MongoDB object modeling |
| JWT (jsonwebtoken) | Authentication tokens |
| bcrypt | Password hashing |
| dotenv | Environment variable management |
| cors | Cross-origin request handling |

### Frontend
| Technology | Purpose |
|---|---|
| React | UI framework |
| Context API | Global state management |
| Fetch API | HTTP requests to backend |
| CSS (custom) | Black & Gold premium design |
| Google Fonts | Inter + Playfair Display fonts |

---

## 🚀 Getting Started

### Prerequisites
Make sure you have these installed:
- Node.js (v16 or higher)
- MongoDB (running on port 27018)
- npm

---

### 1. Clone the repository
```bash
git clone https://github.com/donjesuskayiranga-coder/inventory-management
cd inventory-api
```

### 2. Set up the Backend
```bash
cd inventory-api
npm install
```

Create a `.env` file in the `inventory-api` folder:
```env
PORT=7000
MONGO_URI=mongodb://127.0.0.1:27018/inventory-db
JWT_SECRET=your_secret_key_here
```

Start the backend:
```bash
npm start
```

Backend runs on: `http://localhost:7000`

---

### 3. Set up the Frontend
```bash
cd frontend
npm install
npm start
```

Frontend runs on: `http://localhost:3000`

---

## 🔐 Authentication Flow

1. User registers with username, email and password
2. Password is hashed with bcrypt before saving to MongoDB
3. On login, server verifies credentials and returns a JWT token
4. Token is stored in `localStorage` in the browser
5. Every API request includes the token in the `Authorization: Bearer <token>` header
6. Backend middleware validates the token on every protected route

---

## 👥 User Roles

### Regular User
- View all products
- Search products by name or SKU
- Place orders on available products
- View and manage their own orders
- Cancel pending orders

### Admin
Everything a regular user can do, plus:
- Create, edit and delete products
- View all orders from all users system-wide
- Update order status (pending → completed → cancelled)
- Delete any order
- View all registered users

---

## 🌐 API Endpoints

### Auth Routes — `/api/auth`
| Method | Endpoint | Access | Description |
|---|---|---|---|
| POST | `/api/auth/register` | Public | Register new account |
| POST | `/api/auth/login` | Public | Login and get JWT token |
| GET | `/api/auth/users` | Admin only | Get all users |

### Product Routes — `/api/products`
| Method | Endpoint | Access | Description |
|---|---|---|---|
| GET | `/api/products` | Any logged-in user | Get all products |
| POST | `/api/products` | Admin only | Create new product |
| PUT | `/api/products/:id` | Admin only | Update a product |
| DELETE | `/api/products/:id` | Admin only | Delete a product |

### Order Routes — `/api/orders`
| Method | Endpoint | Access | Description |
|---|---|---|---|
| GET | `/api/orders` | Logged-in user | Get orders (own orders for users, all orders for admin) |
| POST | `/api/orders` | Any logged-in user | Place a new order |
| PUT | `/api/orders/:id` | Admin only | Update order status |
| DELETE | `/api/orders/:id` | Admin only | Delete an order |

---

## 🗄️ Database Models

### User
```
username    String   required, min 3 chars
email       String   required, unique
password    String   required, hashed with bcrypt
role        String   "user" or "admin", default "user"
timestamps  Auto     createdAt, updatedAt
```

### Product
```
name        String   required
sku         String   required, unique
price       Number   required
quantity    Number   default 0
description String   optional
timestamps  Auto     createdAt, updatedAt
```

### Order
```
user        ObjectId  ref to User
products    Array     [{ product: ObjectId, quantity: Number }]
status      String    "pending" | "completed" | "cancelled"
timestamps  Auto      createdAt, updatedAt
```

---

## 🎨 Design System

| Property | Value |
|---|---|
| Primary font | Inter |
| Display font | Playfair Display |
| Background | #0a0a0a |
| Surface | #111111 |
| Gold accent | #c9a84c |
| Gold light | #e8c96d |
| Success | #52a96e |
| Danger | #e05252 |
| Warning | #d4943a |

---

## 🔧 Making a User Admin

After registering, open MongoDB shell and run:
```bash
mongosh --port 27018
use inventory-db
db.users.updateOne({ email: "youremail@example.com" }, { $set: { role: "admin" } })
```

---

## 🛡️ Security Features

- Passwords hashed with bcrypt (10 salt rounds)
- JWT tokens expire after 7 days
- Admin middleware blocks non-admin users from protected routes
- Input validation on register and login
- CORS enabled for frontend-backend communication

---

## 📋 Features Summary

- ✅ JWT Authentication (register & login)
- ✅ Role-based access control (admin & user)
- ✅ Product management (CRUD)
- ✅ Order management with status tracking
- ✅ Admin analytics dashboard
- ✅ Low stock alerts
- ✅ Order status breakdown with progress bars
- ✅ Top products by price & stock level ranking
- ✅ User management panel
- ✅ Search products by name or SKU
- ✅ Black & Gold premium UI design
- ✅ Fully responsive sidebar navigation

---

## 📄 License

This project was built and designed by **KAYIRANGA Jesus**.  
All rights reserved © 2026 VaultIQ.

