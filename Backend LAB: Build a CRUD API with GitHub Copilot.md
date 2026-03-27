# 🚀 Backend LAB: Build a CRUD API with GitHub Copilot

## 📋 Lab Overview


### What You'll Build
A production-ready REST API with:
- ✅ Full CRUD operations (Create, Read, Update, Delete)
- ✅ MongoDB integration with Mongoose
- ✅ Request validation
- ✅ Winston logger with file rotation
- ✅ Global error handling
- ✅ Comprehensive unit tests
- ✅ Test coverage reporting

---

## 🎯 Learning Objectives

By the end of this lab, you will:
1. Build a RESTful API using Express.js
2. Implement MongoDB schemas with Mongoose
3. Use GitHub Copilot to accelerate development
4. Write unit tests with Jest and Supertest
5. Implement logging and error handling middleware
6. Follow REST API best practices

---

## 📦 Prerequisites

### Required Software
- **Node.js** (v18 or higher)
- **VS Code** with GitHub Copilot extension
- **Git** for version control
- **MongoDB** (Atlas account or local installation)

### Required Knowledge
- Basic JavaScript/Node.js
- Understanding of REST APIs
- Basic Git commands
- Familiarity with VS Code

---

## 🛠️ Lab Setup

### Step 1: Create Project Directory

```powershell
# Create and navigate to project directory
mkdir backend-lab
cd backend-lab
```

### Step 2: Initialize Node.js Project

```powershell
# Initialize package.json
npm init -y
```

### Step 3: Install Dependencies

```powershell
# Install all required packages
npm install express mongoose winston jest supertest
```

**Packages installed:**
- `express` - Web framework
- `mongoose` - MongoDB ODM
- `winston` - Logging library
- `jest` - Testing framework
- `supertest` - HTTP assertions for testing

---

## 📁 Project Structure

Use GitHub Copilot to create the folder structure:

### Copilot Prompt:
```
Create folders:
/src
/src/routes
/src/models
/tests
```

**Expected Structure:**
```
backend-lab/
├── src/
│   ├── routes/
│   ├── models/
│   ├── server.js
│   ├── logger.js
│   ├── requestLogger.js
│   └── errorHandler.js
├── tests/
│   ├── users.test.js
│   └── setup.js
├── logs/
├── package.json
├── jest.config.js
└── babel.config.js
```

---

## 🔨 Building the API - Step by Step

### Step 1: Create Express Server

**Copilot Prompt:**
```
Create an Express server with JSON middleware and a health check route
```

**File:** `src/server.js`

**What to expect:**
- Express app initialization
- JSON middleware
- Health check endpoint at `/health`
- Server listening on port 3000

**Test it:**
```powershell
node src/server.js
```
Visit: `http://localhost:3000/health`

---

### Step 2: Create User Routes

**Copilot Prompt:**
```
Generate CRUD routes for users using Express Router. Only structure, no DB logic
```

**File:** `src/routes/userRoutes.js`

**What to expect:**
- GET `/users` - Get all users
- GET `/users/:id` - Get user by ID
- POST `/users` - Create new user
- PUT `/users/:id` - Update user
- DELETE `/users/:id` - Delete user

**Copilot Prompt:**
```
Add the userRoutes and mount them at /api/users
```

**What happens:** Routes are imported and mounted in `server.js`

---

### Step 3: Create Mongoose User Model

**Copilot Prompt:**
```
Create a mongoose User schema with name, email, password, timestamps
```

**File:** `src/models/User.js`

**What to expect:**
- Schema with validation rules
- Required fields with error messages
- Unique email constraint
- Automatic timestamps (createdAt, updatedAt)
- Password minimum length validation

---

### Step 4: Integrate Mongoose with Routes

**Copilot Prompt:**
```
Convert these routes to use the Mongoose User model for CRUD operations.
```

**What to expect:**
- Async route handlers
- MongoDB operations (find, findById, create, etc.)
- Error handling in try-catch blocks
- Password field excluded from responses
- 404 handling for missing resources

---

### Step 5: Add Error Handling Middleware

**Copilot Prompt:**
```
Create an Express error-handling middleware returning JSON with message and stack in dev mode
```

**File:** `src/errorHandler.js`

**What to expect:**
- Global error handler
- JSON error responses
- Stack trace in development mode only
- Proper HTTP status codes

---

### Step 6: Add Request Logging

**Copilot Prompt:**
```
Create a Winston logger with levels: info, warn, error, timestamp formatting, and log file rotation.
```

**File:** `src/logger.js`

**What to expect:**
- Multiple log levels
- File rotation (5MB max, 5 files)
- Separate files for errors, warnings, and combined logs
- Console output in development
- Timestamp formatting

**Copilot Prompt:**
```
Add logger middleware to log all incoming requests with method, URL, and response time
```

**File:** `src/requestLogger.js`

**What to expect:**
- Request logging before processing
- Response time calculation
- Status code logging

---

## 🧪 Writing Tests

### Step 1: Configure Jest

**Copilot Prompt:**
```
Generate jest.config.js for Node environment and use Babel or ESM support
```

**Files:**
- `jest.config.js` - Test configuration
- `babel.config.js` - Babel configuration
- `tests/setup.js` - Global test setup

**What to expect:**
- Coverage thresholds (70%)
- Test file patterns
- Coverage reporters

---

### Step 2: Write Unit Tests

**Copilot Prompt:**
```
Write unit tests for GET /api/users using Jest and Supertest. Mock the User model.
```

**File:** `tests/users.test.js`

**What to expect:**
- Mocked User model
- Test cases for success scenarios
- Test cases for error scenarios
- AAA pattern (Arrange, Act, Assert)

**Copilot Prompt:**
```
Generate tests for POST, PUT, and DELETE /api/users following best practices
```

**What to expect:**
- Comprehensive test coverage
- Validation tests
- Error handling tests
- Edge case coverage

---

### Step 3: Run Tests

```powershell
# Run all tests
npm test

# Run tests in watch mode
npm run test:watch

# Run tests with coverage
npm run test:coverage
```

**Expected Results:**
- All tests passing ✓
- Coverage report generated
- Coverage meets thresholds

---

## 🔌 Database Connection (Bonus/optional)

### Option 1: MongoDB Atlas (Cloud - Free)

1. **Create MongoDB Atlas Account:** https://www.mongodb.com/atlas
2. **Create a Free Cluster**
3. **Get Connection String**
4. **Create `.env` file:**

```env
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/backend-lab?retryWrites=true&w=majority
NODE_ENV=development
PORT=3000
```

5. **Install dotenv:**
```powershell
npm install dotenv
```

6. **Add DB connection in `server.js`:**
```javascript
const mongoose = require('mongoose');
require('dotenv').config();

mongoose.connect(process.env.MONGODB_URI)
  .then(() => console.log('MongoDB connected'))
  .catch(err => console.error('MongoDB connection error:', err));
```

### Option 2: Local MongoDB

```powershell
# Install MongoDB Community Edition
# Then use connection string:
# mongodb://localhost:27017/backend-lab
```

---

## 🧪 Testing Your API

### Using Thunder Client (VS Code Extension)

1. **Install Extension:** Thunder Client
2. **Create New Request**
3. **Test Endpoints:**

#### Create User (POST)
```
POST http://localhost:3000/api/users
Content-Type: application/json

{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "password123"
}
```

#### Get All Users (GET)
```
GET http://localhost:3000/api/users
```

#### Get User by ID (GET)
```
GET http://localhost:3000/api/users/USER_ID_HERE
```

#### Update User (PUT)
```
PUT http://localhost:3000/api/users/USER_ID_HERE
Content-Type: application/json

{
  "name": "John Updated"
}
```

#### Delete User (DELETE)
```
DELETE http://localhost:3000/api/users/USER_ID_HERE
```

---

## 📊 Understanding Test Coverage

After running `npm run test:coverage`, you'll see:

```
--------------------------|---------|----------|---------|---------|
File                      | % Stmts | % Branch | % Funcs | % Lines |
--------------------------|---------|----------|---------|---------|
All files                 |   85.5  |   75.3   |  82.1   |  85.5   |
 routes/userRoutes.js     |   100   |   100    |  100    |  100    |
 models/User.js           |   100   |   100    |  100    |  100    |
 errorHandler.js          |   80    |   50     |  75     |  80     |
--------------------------|---------|----------|---------|---------|
```

**Coverage Metrics:**
- **Statements:** Individual instructions executed
- **Branches:** IF/ELSE paths covered
- **Functions:** Functions called
- **Lines:** Lines of code executed

---


---

## 📚 Additional Resources

### Documentation
- [Express.js](https://expressjs.com/)
- [Mongoose](https://mongoosejs.com/)
- [Jest](https://jestjs.io/)
- [Winston](https://github.com/winstonjs/winston)

### Best Practices
- [REST API Design](https://restfulapi.net/)
- [Node.js Best Practices](https://github.com/goldbergyoni/nodebestpractices)
- [Testing Best Practices](https://github.com/goldbergyoni/javascript-testing-best-practices)

### GitHub Copilot
- [Copilot Documentation](https://docs.github.com/en/copilot)
- [Copilot Best Practices](https://github.blog/2023-06-20-how-to-write-better-prompts-for-github-copilot/)

---

## 🐛 Troubleshooting

### Common Issues

**Issue:** Tests failing with "Cannot find module"
```powershell
# Solution: Ensure all dependencies are installed
npm install
```

**Issue:** MongoDB connection error
```
# Solution: Check connection string, network access in Atlas
# Verify .env file exists and MONGODB_URI is correct
```

**Issue:** Port 3000 already in use
```powershell
# Solution: Use different port or kill existing process
# In .env: PORT=3001
```

**Issue:** Coverage below threshold
```
# Solution: Add more test cases or reduce threshold in jest.config.js
```

---

## 🎉 Completion

Congratulations! You've built a production-ready REST API using GitHub Copilot!
