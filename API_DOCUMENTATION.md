# 🔌 API Documentation

## 📋 Overview

The ERP CRM API is a RESTful service built with Node.js and Express.js. It provides endpoints for managing customers, invoices, payments, quotes, and other business operations.

**Base URL**: `http://localhost:5000/api/v1`

## 🔐 Authentication

### JWT Token Authentication

All API endpoints require authentication using JWT tokens.

#### Login to get token:
```http
POST /api/v1/auth/login
Content-Type: application/json

{
  "email": "admin@example.com",
  "password": "password123"
}
```

#### Response:
```json
{
  "success": true,
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": "507f1f77bcf86cd799439011",
    "email": "admin@example.com",
    "role": "admin"
  }
}
```

#### Using the token:
```http
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

## 📊 API Endpoints

### 🔐 Authentication Endpoints

#### POST `/auth/login`
Login with email and password.

**Request Body:**
```json
{
  "email": "admin@example.com",
  "password": "password123"
}
```

**Response:**
```json
{
  "success": true,
  "token": "jwt_token_here",
  "user": {
    "id": "507f1f77bcf86cd799439011",
    "email": "admin@example.com",
    "role": "admin",
    "name": "Admin User"
  }
}
```

#### POST `/auth/register`
Register a new user.

**Request Body:**
```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "password123",
  "role": "user"
}
```

#### POST `/auth/logout`
Logout and invalidate token.

### 👥 Customer Management

#### GET `/customers`
Get all customers with pagination and filtering.

**Query Parameters:**
- `page` (number): Page number (default: 1)
- `limit` (number): Items per page (default: 10)
- `search` (string): Search by name or email
- `status` (string): Filter by status (active, inactive)

**Response:**
```json
{
  "success": true,
  "data": {
    "customers": [
      {
        "id": "507f1f77bcf86cd799439011",
        "name": "John Doe",
        "email": "john@example.com",
        "phone": "+1234567890",
        "address": "123 Main St",
        "status": "active",
        "createdAt": "2024-01-01T00:00:00.000Z"
      }
    ],
    "pagination": {
      "page": 1,
      "limit": 10,
      "total": 100,
      "pages": 10
    }
  }
}
```

#### POST `/customers`
Create a new customer.

**Request Body:**
```json
{
  "name": "Jane Smith",
  "email": "jane@example.com",
  "phone": "+1234567890",
  "address": "456 Oak Ave",
  "company": "ABC Corp",
  "notes": "VIP customer"
}
```

#### GET `/customers/:id`
Get customer by ID.

#### PUT `/customers/:id`
Update customer information.

#### DELETE `/customers/:id`
Delete customer (soft delete).

### 📄 Invoice Management

#### GET `/invoices`
Get all invoices with filtering.

**Query Parameters:**
- `page` (number): Page number
- `limit` (number): Items per page
- `status` (string): Filter by status (draft, sent, paid, overdue)
- `customer` (string): Filter by customer ID
- `dateFrom` (string): Filter from date (YYYY-MM-DD)
- `dateTo` (string): Filter to date (YYYY-MM-DD)

**Response:**
```json
{
  "success": true,
  "data": {
    "invoices": [
      {
        "id": "507f1f77bcf86cd799439011",
        "invoiceNumber": "INV-2024-001",
        "customer": {
          "id": "507f1f77bcf86cd799439012",
          "name": "John Doe"
        },
        "amount": 1500.00,
        "status": "paid",
        "dueDate": "2024-02-01T00:00:00.000Z",
        "createdAt": "2024-01-01T00:00:00.000Z"
      }
    ],
    "pagination": {
      "page": 1,
      "limit": 10,
      "total": 50,
      "pages": 5
    }
  }
}
```

#### POST `/invoices`
Create a new invoice.

**Request Body:**
```json
{
  "customerId": "507f1f77bcf86cd799439012",
  "items": [
    {
      "description": "Web Development",
      "quantity": 1,
      "unitPrice": 1000.00,
      "tax": 0.10
    },
    {
      "description": "Hosting Setup",
      "quantity": 1,
      "unitPrice": 500.00,
      "tax": 0.10
    }
  ],
  "dueDate": "2024-02-01T00:00:00.000Z",
  "notes": "Payment due within 30 days"
}
```

#### GET `/invoices/:id`
Get invoice by ID.

#### PUT `/invoices/:id`
Update invoice.

#### DELETE `/invoices/:id`
Delete invoice.

#### POST `/invoices/:id/send`
Send invoice to customer.

#### POST `/invoices/:id/mark-paid`
Mark invoice as paid.

### 💰 Payment Management

#### GET `/payments`
Get all payments.

**Query Parameters:**
- `page` (number): Page number
- `limit` (number): Items per page
- `method` (string): Filter by payment method
- `dateFrom` (string): Filter from date
- `dateTo` (string): Filter to date

**Response:**
```json
{
  "success": true,
  "data": {
    "payments": [
      {
        "id": "507f1f77bcf86cd799439011",
        "invoiceId": "507f1f77bcf86cd799439012",
        "amount": 1500.00,
        "method": "credit_card",
        "status": "completed",
        "transactionId": "txn_123456789",
        "createdAt": "2024-01-01T00:00:00.000Z"
      }
    ],
    "pagination": {
      "page": 1,
      "limit": 10,
      "total": 25,
      "pages": 3
    }
  }
}
```

#### POST `/payments`
Record a new payment.

**Request Body:**
```json
{
  "invoiceId": "507f1f77bcf86cd799439012",
  "amount": 1500.00,
  "method": "credit_card",
  "transactionId": "txn_123456789",
  "notes": "Payment received"
}
```

#### GET `/payments/:id`
Get payment by ID.

#### PUT `/payments/:id`
Update payment.

#### DELETE `/payments/:id`
Delete payment.

### 📋 Quote Management

#### GET `/quotes`
Get all quotes.

**Query Parameters:**
- `page` (number): Page number
- `limit` (number): Items per page
- `status` (string): Filter by status (draft, sent, accepted, rejected)
- `customer` (string): Filter by customer ID

#### POST `/quotes`
Create a new quote.

**Request Body:**
```json
{
  "customerId": "507f1f77bcf86cd799439012",
  "items": [
    {
      "description": "Consultation Services",
      "quantity": 10,
      "unitPrice": 100.00
    }
  ],
  "validUntil": "2024-02-01T00:00:00.000Z",
  "notes": "Valid for 30 days"
}
```

#### GET `/quotes/:id`
Get quote by ID.

#### PUT `/quotes/:id`
Update quote.

#### DELETE `/quotes/:id`
Delete quote.

#### POST `/quotes/:id/send`
Send quote to customer.

#### POST `/quotes/:id/accept`
Accept quote (converts to invoice).

#### POST `/quotes/:id/reject`
Reject quote.

### 📊 Dashboard & Analytics

#### GET `/dashboard/stats`
Get dashboard statistics.

**Response:**
```json
{
  "success": true,
  "data": {
    "totalRevenue": 50000.00,
    "totalInvoices": 150,
    "totalCustomers": 75,
    "pendingInvoices": 25,
    "overdueInvoices": 5,
    "monthlyRevenue": [
      {"month": "Jan", "revenue": 10000},
      {"month": "Feb", "revenue": 12000},
      {"month": "Mar", "revenue": 15000}
    ],
    "topCustomers": [
      {
        "id": "507f1f77bcf86cd799439012",
        "name": "ABC Corp",
        "totalSpent": 15000.00
      }
    ]
  }
}
```

#### GET `/dashboard/revenue-chart`
Get revenue chart data.

#### GET `/dashboard/customer-growth`
Get customer growth data.

### 👤 User Management

#### GET `/users`
Get all users (admin only).

#### POST `/users`
Create new user (admin only).

#### GET `/users/:id`
Get user by ID.

#### PUT `/users/:id`
Update user.

#### DELETE `/users/:id`
Delete user.

#### PUT `/users/profile`
Update current user profile.

#### PUT `/users/password`
Change password.

## 📝 Error Handling

### Error Response Format
```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Validation failed",
    "details": [
      {
        "field": "email",
        "message": "Email is required"
      }
    ]
  }
}
```

### Common Error Codes
- `AUTHENTICATION_ERROR`: Invalid or missing token
- `AUTHORIZATION_ERROR`: Insufficient permissions
- `VALIDATION_ERROR`: Invalid request data
- `NOT_FOUND`: Resource not found
- `DUPLICATE_ERROR`: Resource already exists
- `SERVER_ERROR`: Internal server error

## 🔧 Rate Limiting

API requests are rate-limited to prevent abuse:
- **Authenticated users**: 1000 requests per hour
- **Unauthenticated users**: 100 requests per hour

## 📋 Request/Response Headers

### Required Headers
```http
Content-Type: application/json
Authorization: Bearer <token>
```

### Optional Headers
```http
Accept: application/json
Accept-Language: en-US
```

## 🧪 Testing the API

### Using cURL
```bash
# Login
curl -X POST http://localhost:5000/api/v1/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email": "admin@example.com", "password": "password123"}'

# Get customers (with token)
curl -X GET http://localhost:5000/api/v1/customers \
  -H "Authorization: Bearer YOUR_TOKEN_HERE"
```

### Using Postman
1. Import the API collection
2. Set the base URL: `http://localhost:5000/api/v1`
3. Use the login endpoint to get a token
4. Set the token in the Authorization header for other requests

## 📚 SDKs and Libraries

### JavaScript/Node.js
```javascript
import axios from 'axios';

const api = axios.create({
  baseURL: 'http://localhost:5000/api/v1',
  headers: {
    'Authorization': `Bearer ${token}`
  }
});

// Get customers
const customers = await api.get('/customers');

// Create invoice
const invoice = await api.post('/invoices', {
  customerId: '507f1f77bcf86cd799439012',
  items: [...]
});
```

### Python
```python
import requests

headers = {
    'Authorization': f'Bearer {token}',
    'Content-Type': 'application/json'
}

# Get customers
response = requests.get('http://localhost:5000/api/v1/customers', headers=headers)
customers = response.json()
```

---

*For more detailed information about specific endpoints, please refer to the source code or contact the development team.*

---

<div align="center">
  <p><strong>Built upon and refined with ❤️ by Prem Mehta</strong></p>
</div> 