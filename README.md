# Session Cookie Authentication

A Node.js authentication system built with Express, MongoDB, and session management. This application provides user registration, login, logout, and protected route functionality.

![Node.js](https://img.shields.io/badge/Node.js-22%2B-green?logo=node.js)
![Express](https://img.shields.io/badge/Express-5.x-blue?logo=express)
![MongoDB](https://img.shields.io/badge/MongoDB-8.x-brightgreen?logo=mongodb)
![License: ISC](https://img.shields.io/badge/License-ISC-yellow.svg)

---

## 📑 Table of Contents

- [✨ Features](#features)
- [🖥️ System Requirements](#system-requirements)
- [🛠️ Technologies Used](#technologies-used)
- [📁 Project Structure](#project-structure)
- [📡 API Endpoints](#api-endpoints)
- [⚡ Installation](#installation)
- [⚙️ Configuration](#configuration)
- [🚀 Usage](#usage)
- [🔐 Authentication Flow](#authentication-flow)
- [📋 API Examples](#api-examples)

---

## ✨ Features

- 🔐 **User Registration**: Create new user accounts with encrypted passwords
- 🔑 **User Login**: Authenticate users with username/password
- 🚪 **User Logout**: Secure session termination
- 👤 **Protected Routes**: Access control for authenticated users
- 🔒 **Password Hashing**: Secure password storage using bcrypt
- 🍪 **Session Management**: Express sessions with MongoDB store
- 🛡️ **Input Validation**: Server-side validation for user inputs
- 📊 **User Profile**: Access user information for authenticated users

---

## 🖥️ System Requirements

- **Node.js** (v22 or higher)
- **MongoDB** (running locally or on cloud)
- **npm** or **yarn** package manager

---

## 🛠️ Technologies Used

- **Backend**: Node.js, Express.js 5.x
- **Database**: MongoDB with Mongoose ODM
- **Authentication**: bcryptjs for password hashing
- **Session Management**: express-session with connect-mongo
- **Middleware**: cookie-parser for cookie handling
- **Development**: nodemon (recommended)

---

## 📁 Project Structure

```
Lab_05/
├── models/
│   └── User.js              # User model with bcrypt integration
├── routes/
│   └── auth.js              # Authentication routes (register, login, logout, profile)
├── .gitignore               # Git ignore file
├── .env                     # Environment variables (create this file)
├── app.js                   # Main application file
├── package.json             # Project dependencies
└── README.md                # Project documentation
```

---

## 📡 API Endpoints

### Authentication Routes

| Method | Endpoint         | Description         | Authentication Required |
| ------ | ---------------- | ------------------- | ----------------------- |
| `POST` | `/auth/register` | Register a new user | ❌                      |
| `POST` | `/auth/login`    | Login user          | ❌                      |
| `POST` | `/auth/logout`   | Logout user         | ✅                      |
| `GET`  | `/auth/profile`  | Get user profile    | ✅                      |

---

## ⚡ Installation


 **Install dependencies**
   ```bash
   npm install
   ```

---

## ⚙️ Configuration

Create a `.env` file in the root directory and add the following environment variables:

```env
# MongoDB Connection
MONGODB_URI=mongodb://localhost:27017/session_auth

# Session Configuration
SESSION_SECRET=your_super_secret_key_here

# Application Port
PORT=3000

# Environment
NODE_ENV=development
```

**Important**: Replace `your_super_secret_key_here` with a secure, random string for production use.

---

## 🚀 Usage

**Run the application:**
   ```bash
   node app.js
   ```

4. **Access the application**:
   - Server will be running at: `http://localhost:3000`
   - Test the API endpoints using Postman, curl, or any HTTP client

---

## 🔐 Authentication Flow

1. **User Registration**:

   - User provides username and password
   - Password is automatically hashed using bcrypt (10 salt rounds)
   - User data is saved to MongoDB

2. **User Login**:

   - User provides credentials
   - Password is compared with hashed version
   - Session is created with user ID
   - Session cookie is set in browser

3. **Protected Routes**:

   - Middleware checks for valid session
   - User ID is retrieved from session
   - Access granted or denied based on authentication status

4. **User Logout**:
   - Session is destroyed
   - Session cookies are cleared

---

## 📋 API Examples

### Register a New User

```bash
curl -X POST http://localhost:3000/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "username": "johndoe",
    "password": "securepassword123"
  }'
```

**Response:**

```json
{
  "message": "User registered successfully!"
}
```

### Login User

```bash
curl -X POST http://localhost:3000/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "username": "johndoe",
    "password": "securepassword123"
  }'
```

**Response:**

```json
{
  "message": "Login successful!"
}
```

### Get User Profile (Protected Route)

```bash
curl -X GET http://localhost:3000/auth/profile \
  -H "Cookie: connect.sid=your_session_cookie"
```

**Response:**

```json
{
  "_id": "user_id_here",
  "username": "johndoe",
  "__v": 0
}
```

### Logout User

```bash
curl -X POST http://localhost:3000/auth/logout \
  -H "Cookie: connect.sid=your_session_cookie"
```

**Response:**

```json
{
  "message": "Logout successful!"
}
```

---

## 🔍 Key Features Explained

### Password Security

- Passwords are hashed using bcryptjs with 10 salt rounds
- Original passwords are never stored in the database
- Password comparison is done securely using bcrypt.compare()

### Session Management

- Sessions are stored in MongoDB using connect-mongo
- Session cookies are configured for security:
  - `httpOnly: true` - Prevents XSS attacks
  - `secure: false` - Set to true in production with HTTPS
  - `maxAge: 1 hour` - Session expires after 1 hour

### User Model Features

- **Pre-save middleware**: Automatically hashes passwords before saving
- **Instance methods**: `comparePassword()` method for authentication
- **Validation**: Built-in MongoDB validation for required fields
- **Unique usernames**: Prevents duplicate user registrations

---

## 🛡️ Security Considerations

- **Password Hashing**: Uses bcrypt with salt rounds for secure password storage
- **Session Security**: Configured with secure cookie options
- **Input Validation**: Server-side validation for all user inputs
- **Error Handling**: Proper error responses without exposing sensitive information
- **Environment Variables**: Sensitive data stored in environment variables

---


## 📝 Dependencies

### Production Dependencies

- **express**: ^5.1.0 - Web application framework
- **mongoose**: ^8.18.1 - MongoDB object modeling
- **bcryptjs**: ^3.0.2 - Password hashing library
- **express-session**: ^1.18.2 - Session middleware
- **connect-mongo**: ^5.1.0 - MongoDB session store
- **cookie-parser**: ^1.4.7 - Cookie parsing middleware

---

## 🐛 Troubleshooting

### Common Issues

1. **MongoDB Connection Error**:

   - Ensure MongoDB is running
   - Check the connection string in `.env`
   - Verify database permissions

2. **Session Issues**:

   - Clear browser cookies
   - Check session secret configuration
   - Verify MongoDB session store connection

3. **Password Issues**:
   - Ensure password meets minimum requirements
   - Check bcrypt installation

---

## 📞 Support

If you encounter any issues or have questions:

- Create an issue in the project repository
- Check the MongoDB and Express.js documentation
- Review the error logs for detailed information

---

⭐ **If you find this project helpful for learning authentication concepts, please give it a star!**

---

## 📄 License

This project is licensed under the ISC License.
