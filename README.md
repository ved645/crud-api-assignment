# Product Management REST API (CRUD)

A production-ready RESTful CRUD API built with **Node.js**, **Express.js**, and **MongoDB (Mongoose)**, ready for deployment on **Render** and testing with **Postman**.

---

## 📋 Features

- **Full CRUD Operations**:
  - `POST /api/products`: Create a product
  - `GET /api/products`: Read all products (with optional search, filter by category/inStock)
  - `GET /api/products/:id`: Read a single product by ObjectId
  - `PUT /api/products/:id`: Update an existing product
  - `DELETE /api/products/:id`: Delete a product
- **Health Check Endpoint**: `GET /api/health`
- **Root Info Endpoint**: `GET /`
- **Robust Error Handling**: Handles 404s, malformed JSON, invalid MongoDB ObjectIds, and schema validation errors.
- **Ready-to-Use Postman Collection**: `postman_collection.json` with automated test assertions and dynamic variable storage.
- **Production-Ready**: CORS enabled, environment-based configuration, clean modular architecture.

---

## 📁 Project Structure

```text
ass_6/
├── config/
│   └── db.js                 # MongoDB connection using Mongoose
├── controllers/
│   └── productController.js  # CRUD controller functions
├── middleware/
│   └── errorHandler.js       # Centralized 404 & error handlers
├── models/
│   └── Product.js            # Mongoose schema for Product entity
├── routes/
│   └── productRoutes.js      # API routes definitions
├── .env                      # Local environment variables
├── .env.example              # Template environment variables
├── .gitignore                # Git ignore rules
├── package.json              # Project dependencies & scripts
├── postman_collection.json   # Exportable Postman collection
├── README.md                 # Full documentation
└── server.js                 # Express server entry point
```

---

## 🛠️ Prerequisites

- **Node.js** (v18+ or v20+)
- **npm** (v9+)
- A **MongoDB** database (MongoDB Atlas cloud cluster or local MongoDB instance)
- **Git**
- **Postman** (Desktop app or web version)

---

## 🚀 Step 1: Local Setup

1. **Install dependencies**:
   ```bash
   npm install
   ```

2. **Configure Environment Variables**:
   Open `.env` and set your MongoDB connection string and port:
   ```env
   PORT=5000
   MONGODB_URI=mongodb+srv://<username>:<password>@cluster0.xxxxx.mongodb.net/product_db?retryWrites=true&w=majority
   NODE_ENV=development
   ```

3. **Run the server**:
   - For standard start:
     ```bash
     npm start
     ```
   - For auto-reloading dev mode:
     ```bash
     npm run dev
     ```

4. **Verify the server is running**:
   Open your browser or terminal:
   ```bash
   curl http://localhost:5000/api/health
   ```
   Output:
   ```json
   { "status": "UP", "timestamp": "...", "uptime": 1.23 }
   ```

---

## ☁️ Step 2: Setting up MongoDB Atlas (Free Cloud Database)

1. Go to [MongoDB Atlas](https://www.mongodb.com/cloud/atlas) and sign in / sign up (free).
2. Click **Create a Deployment** -> Select **M0 Free Cluster**.
3. Under **Security Quickstart**:
   - Create a **Database User** (e.g. username: `admin`, password: create a strong password).
   - Under **Network Access** / **IP Access List**: Select **Allow Access from Anywhere (`0.0.0.0/0`)** so both your local machine and Render can connect.
4. Click **Database** -> Click **Connect** on your cluster -> Select **Drivers** (Node.js).
5. Copy the connection string. It will look like:
   ```text
   mongodb+srv://<username>:<password>@cluster0.abcde.mongodb.net/product_db?retryWrites=true&w=majority
   ```
6. Replace `<username>` and `<password>` with your database user credentials.

---

## 🌐 Step 3: Deploying to Render (Free Hosting)

1. **Push your code to GitHub**:
   ```bash
   git init
   git add .
   git commit -m "Initial commit of CRUD API"
   git branch -M main
   git remote add origin https://github.com/<your-username>/<your-repo-name>.git
   git push -u origin main
   ```

2. **Create Web Service on Render**:
   - Go to [Render.com](https://render.com/) and sign in.
   - Click **New +** -> Select **Web Service**.
   - Connect your GitHub repository.

3. **Configure Service Settings**:
   - **Name**: `product-crud-api` (or your preferred name)
   - **Region**: Choose closest (e.g., Oregon, Frankfurt, Singapore)
   - **Branch**: `main`
   - **Root Directory**: (Leave blank)
   - **Runtime**: `Node`
   - **Build Command**: `npm install`
   - **Start Command**: `npm start`
   - **Instance Type**: `Free`

4. **Set Environment Variables in Render**:
   Scroll down to **Environment Variables** and add:
   - `MONGODB_URI`: `<Your MongoDB Atlas connection string>`
   - `NODE_ENV`: `production`

5. **Deploy**:
   - Click **Create Web Service**.
   - Wait 1–2 minutes for the build to complete.
   - Render will provide your public URL, e.g.:
     `https://product-crud-api-xxxx.onrender.com`

---

## 📬 Step 4: Testing the API Using Postman

### Option A: Import the Pre-Built Postman Collection (Recommended)
1. Open the Postman application.
2. Click the **Import** button (top-left).
3. Drag and drop the `postman_collection.json` file from this project folder.
4. You will see a collection called **"Product CRUD API Collection"** containing all test requests.

#### How to Switch Between Localhost and Deployed Render URL:
- Click on the collection name in Postman -> Click the **Variables** tab.
- Set `baseUrl` to:
  - For local: `http://localhost:5000`
  - For deployed Render: `https://product-crud-api-xxxx.onrender.com`
- Click **Save**.

---

### Option B: Manual Testing Reference

#### 1. Create a Product
- **Method**: `POST`
- **URL**: `{{baseUrl}}/api/products`
- **Headers**: `Content-Type: application/json`
- **Body (raw JSON)**:
  ```json
  {
    "name": "Wireless Mechanical Keyboard",
    "description": "RGB backlit mechanical keyboard with hot-swappable switches",
    "price": 89.99,
    "category": "Electronics",
    "inStock": true
  }
  ```
- **Response**: `201 Created`

#### 2. Get All Products
- **Method**: `GET`
- **URL**: `{{baseUrl}}/api/products`
- **Response**: `200 OK` (Returns an array of products)

#### 3. Get Single Product by ID
- **Method**: `GET`
- **URL**: `{{baseUrl}}/api/products/<product_id>`
- **Response**: `200 OK`

#### 4. Update Product
- **Method**: `PUT`
- **URL**: `{{baseUrl}}/api/products/<product_id>`
- **Headers**: `Content-Type: application/json`
- **Body (raw JSON)**:
  ```json
  {
    "price": 79.99,
    "inStock": true
  }
  ```
- **Response**: `200 OK`

#### 5. Delete Product
- **Method**: `DELETE`
- **URL**: `{{baseUrl}}/api/products/<product_id>`
- **Response**: `200 OK`
  ```json
  {
    "success": true,
    "message": "Product deleted successfully"
  }
  ```
