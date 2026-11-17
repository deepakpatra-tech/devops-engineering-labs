# CloudX Microservices Platform — API Contracts

**Version:** 1.0  
**API Style:** REST  
**API Version:** v1  
**Data Format:** JSON  
**Authentication:** JWT Bearer Token

---

## 1. API Standards

### Base URL

Local development:

- User Service: `http://localhost:5001`
- Product Service: `http://localhost:5002`
- Order Service: `http://localhost:5003`

Production:

`https://api.cloudx.example.com/api/v1`

The production domain is a placeholder.

### HTTP Methods

| Method | Purpose |
|---|---|
| GET | Retrieve resources |
| POST | Create resources |
| PUT | Replace resources |
| PATCH | Partially update resources |
| DELETE | Delete or deactivate resources |

### Standard Response Format

Successful response:

```json
{
  "success": true,
  "message": "Operation completed successfully",
  "data": {}
}
```

Error response:

```json
{
  "success": false,
  "message": "Validation failed",
  "error": {
    "code": "VALIDATION_ERROR",
    "details": []
  }
}
```

## 2. User Service API

Base path: `/api/v1/users`

| Method | Endpoint | Description | Auth |
|---|---|---|---|
| POST | `/register` | Register user | No |
| POST | `/login` | Authenticate user | No |
| GET | `/profile` | Get current profile | Yes |
| PUT | `/profile` | Update profile | Yes |
| POST | `/logout` | Logout / revoke session | Yes |

### Register User

`POST /api/v1/users/register`

Request:

```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "SecurePassword123"
}
```

Expected response: `201 Created`

```json
{
  "success": true,
  "message": "User registered successfully",
  "data": {
    "id": "user-uuid",
    "name": "John Doe",
    "email": "john@example.com"
  }
}
```

### Login

`POST /api/v1/users/login`

Request:

```json
{
  "email": "john@example.com",
  "password": "SecurePassword123"
}
```

Expected response: `200 OK`

```json
{
  "success": true,
  "message": "Login successful",
  "data": {
    "access_token": "jwt-token",
    "token_type": "Bearer",
    "expires_in": 900
  }
}
```

## 3. Product Service API

Base path: `/api/v1/products`

| Method | Endpoint | Description | Auth |
|---|---|---|---|
| GET | `/` | List products | No |
| GET | `/{id}` | Get product details | No |
| POST | `/` | Create product | Admin |
| PUT | `/{id}` | Update product | Admin |
| DELETE | `/{id}` | Deactivate product | Admin |
| PATCH | `/{id}/inventory` | Update inventory | Admin |

### Create Product

`POST /api/v1/products`

Request:

```json
{
  "name": "Laptop",
  "description": "Business laptop",
  "price": 55000,
  "sku": "LAP-001",
  "quantity": 20
}
```

Expected response: `201 Created`

```json
{
  "success": true,
  "message": "Product created successfully",
  "data": {
    "id": "product-uuid",
    "name": "Laptop",
    "price": 55000,
    "sku": "LAP-001",
    "quantity": 20
  }
}
```

## 4. Order Service API

Base path: `/api/v1/orders`

| Method | Endpoint | Description | Auth |
|---|---|---|---|
| POST | `/` | Create order | Yes |
| GET | `/` | List current user's orders | Yes |
| GET | `/{id}` | Get order details | Yes |
| PATCH | `/{id}/status` | Update order status | Admin |

### Create Order

`POST /api/v1/orders`

Request:

```json
{
  "items": [
    {
      "product_id": "product-uuid",
      "quantity": 2
    }
  ]
}
```

Expected response: `201 Created`

```json
{
  "success": true,
  "message": "Order created successfully",
  "data": {
    "id": "order-uuid",
    "status": "PENDING",
    "total_amount": 110000,
    "items": [
      {
        "product_id": "product-uuid",
        "quantity": 2,
        "unit_price": 55000
      }
    ]
  }
}
```

## 5. HTTP Status Codes

| Status | Meaning |
|---|---|
| 200 | OK |
| 201 | Created |
| 204 | No Content |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Resource Not Found |
| 409 | Conflict |
| 422 | Validation Error |
| 429 | Too Many Requests |
| 500 | Internal Server Error |
| 503 | Service Unavailable |

## 6. Authentication

Protected requests must include:

```http
Authorization: Bearer <access_token>
Content-Type: application/json
```

The token must be validated before accessing protected resources.

## 7. API Security Requirements

- Validate all request payloads.
- Enforce authorization at the service level.
- Never expose passwords or password hashes.
- Apply request size limits.
- Use HTTPS in production.
- Implement rate limiting.
- Avoid returning internal stack traces.
- Store secrets outside source control.

## 8. API Reliability Requirements

- Consistent response structures.
- Request timeouts.
- Controlled retry policies.
- Pagination for collection endpoints.
- Idempotency protection for order creation.
- Structured error responses.
- Request correlation IDs.

## 9. Contract Evolution

- Maintain `/api/v1` as the initial API version.
- Avoid breaking existing contracts.
- Introduce a new API version for incompatible changes.
- Document all contract changes.
- Validate API contracts through automated tests.

---

**Document:** API Contracts  
**Project:** CloudX Microservices Platform