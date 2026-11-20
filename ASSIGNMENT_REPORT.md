# Assignment Report: CRUD API Development, Deployment & Testing

- **Subject / Assignment**: Assignment 6 - CRUD API Development & Deployment
- **Developer / Student**: Ved Upadhyay
- **GitHub Repository**: https://github.com/ved645/crud-api-assignment
- **Live Deployed URL**: https://product-crud-api-13j6.onrender.com
- **Database**: MongoDB Atlas Cloud Database (`cluster0.dveapgj.mongodb.net`)
- **Hosting Provider**: Render (Web Service)
- **API Testing Tool**: Postman

---

## 1. Project Overview

This project implements a production-grade RESTful API performing complete Create, Read, Update, and Delete (CRUD) operations on a `Product` entity. The backend is built using Node.js and Express.js, connected to a cloud-hosted MongoDB Atlas database via Mongoose ODM, and deployed to Render for 24/7 web hosting.

### Tech Stack
- **Runtime Environment**: Node.js (v24.x)
- **Web Framework**: Express.js (v4.x)
- **Database & ODM**: MongoDB Atlas & Mongoose (v8.x)
- **Deployment Platform**: Render
- **Version Control**: Git & GitHub
- **API Testing**: Postman

---

## 2. Data Model (Product Schema)

The database schema is defined in `models/Product.js`:

| Field | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| `_id` | ObjectId | Auto | Unique MongoDB Identifier |
| `name` | String | Yes | Name of the product (max 120 chars) |
| `description` | String | Yes | Detailed description of the product |
| `price` | Number | Yes | Price (must be a positive number) |
| `category` | String | Yes | Product category (e.g., Electronics) |
| `inStock` | Boolean | Optional | Stock availability (Default: `true`) |
| `createdAt` | Date | Auto | Timestamp of creation |
| `updatedAt` | Date | Auto | Timestamp of last modification |

---

## 3. API Endpoints Specification

Base URL: `https://product-crud-api-13j6.onrender.com`

| Operation | HTTP Method | Endpoint | Description | Status Code |
| :--- | :--- | :--- | :--- | :--- |
| **Welcome** | `GET` | `/` | API status and endpoint directory | `200 OK` |
| **Health Check** | `GET` | `/api/health` | Server uptime & status check | `200 OK` |
| **CREATE** | `POST` | `/api/products` | Create a new product | `201 Created` |
| **READ ALL** | `GET` | `/api/products` | Retrieve all products (supports query filters) | `200 OK` |
| **READ ONE** | `GET` | `/api/products/:id` | Retrieve single product by ObjectId | `200 OK` / `404` |
| **UPDATE** | `PUT` | `/api/products/:id` | Update product fields by ObjectId | `200 OK` / `404` |
| **DELETE** | `DELETE` | `/api/products/:id` | Delete product by ObjectId | `200 OK` / `404` |

---

## 4. Sample Requests and Responses

### 4.1 CREATE Product
- **Request**: `POST /api/products`
- **Headers**: `Content-Type: application/json`
- **Body**:
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
```json
{
  "success": true,
  "message": "Product created successfully",
  "data": {
    "name": "Wireless Mechanical Keyboard",
    "description": "RGB backlit mechanical keyboard with hot-swappable switches",
    "price": 89.99,
    "category": "Electronics",
    "inStock": true,
    "_id": "6b00477d72de0dc28c18249f",
    "createdAt": "2026-11-20T13:13:33.872Z",
    "updatedAt": "2026-11-20T13:13:33.872Z",
    "__v": 0
  }
}
```

### 4.2 READ All Products
- **Request**: `GET /api/products`
- **Response**: `200 OK`
```json
{
  "success": true,
  "count": 1,
  "data": [
    {
      "_id": "6b00477d72de0dc28c18249f",
      "name": "Wireless Mechanical Keyboard",
      "description": "RGB backlit mechanical keyboard with hot-swappable switches",
      "price": 89.99,
      "category": "Electronics",
      "inStock": true,
      "createdAt": "2026-11-20T13:13:33.872Z",
      "updatedAt": "2026-11-20T13:13:33.872Z"
    }
  ]
}
```

### 4.3 READ Product by ID
- **Request**: `GET /api/products/6b00477d72de0dc28c18249f`
- **Response**: `200 OK`

### 4.4 UPDATE Product
- **Request**: `PUT /api/products/6b00477d72de0dc28c18249f`
- **Body**:
```json
{
  "price": 79.99,
  "inStock": false
}
```
- **Response**: `200 OK`
```json
{
  "success": true,
  "message": "Product updated successfully",
  "data": {
    "_id": "6b00477d72de0dc28c18249f",
    "name": "Wireless Mechanical Keyboard",
    "price": 79.99,
    "inStock": false
  }
}
```

### 4.5 DELETE Product
- **Request**: `DELETE /api/products/6b00477d72de0dc28c18249f`
- **Response**: `200 OK`
```json
{
  "success": true,
  "message": "Product deleted successfully",
  "data": {
    "id": "6b00477d72de0dc28c18249f",
    "name": "Wireless Mechanical Keyboard"
  }
}
```

---

## 5. Deployment Architecture

1. **Database Tier**: Hosted on **MongoDB Atlas** (M0 Free Tier). Configured with Network Access rule `0.0.0.0/0` to allow inbound connections from Render's cloud servers.
2. **Application Tier**: Hosted on **Render** as a Node.js Web Service. Render monitors the GitHub `main` branch and triggers automatic zero-downtime builds upon any push.
3. **Environment Security**: Sensitive keys (`MONGODB_URI`) are securely stored in Render Environment Variables and never checked into source control.

---

## 6. Testing Verification with Postman

A pre-configured Postman Collection (`postman_collection.json`) was used to execute tests. 
The collection includes automated JavaScript test scripts verifying:
- Response status codes (`200 OK`, `201 Created`).
- Response body JSON schemas and properties.
- Dynamic chaining: Automatically extracting `_id` from the `POST` response and passing it to subsequent `GET`, `PUT`, and `DELETE` requests.

All test assertions passed successfully.
