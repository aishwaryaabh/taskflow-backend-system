# TaskFlow API with Authentication & Role-Based Access

## 📌 Project Overview
This is a full-stack task management application built with Node.js and Express. It includes secure authentication, role-based access control, and CRUD operations for managing tasks.

## 🚀 Features
- User Registration & Login (JWT Authentication)
- Password hashing using bcrypt
- Role-based access (user/admin)
- CRUD operations for Tasks
- Protected routes using middleware
- Simple frontend UI to interact with APIs

## 🛠 Tech Stack
- Backend: Node.js, Express
- Database: MongoDB (Mongoose)
- Frontend: HTML, CSS, JavaScript

## ⚙️ Setup Instructions
1. Clone the repository
2. Run: npm install
3. Create a `.env` file and add:
   - MONGO_URI=your_mongodb_uri
   - JWT_SECRET=your_secret
4. Run: npm start

## 📡 API Endpoints
### Auth
- POST /api/v1/auth/register
- POST /api/v1/auth/login

### Tasks
- GET /api/v1/tasks
- POST /api/v1/tasks
- PUT /api/v1/tasks/:id
- DELETE /api/v1/tasks/:id

## 🔐 Security
- JWT-based authentication
- Password hashing with bcrypt
- Protected routes with middleware

## 📈 Scalability Notes
- Can be extended into microservices architecture
- Redis can be used for caching
- Load balancer can improve performance
- Docker can be used for containerization

## 🌐 Live Demo
https://replit.com/@aishwaryab155/Node-Express-Build

## 👨‍💻 Author
Aishwarya
