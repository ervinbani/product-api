# Product API

A RESTful API built with Node.js, Express, and MongoDB for managing products with advanced querying capabilities.

## Features

- Full CRUD operations for products
- Advanced filtering by category and price range
- Sorting functionality
- Pagination support
- MongoDB data validation
- Error handling with appropriate HTTP status codes

## Tech Stack

- **Node.js** - Runtime environment
- **Express.js** - Web framework
- **MongoDB** - NoSQL database
- **Mongoose** - ODM for MongoDB
- **dotenv** - Environment variable management

## Installation

1. Clone the repository:

```bash
git clone https://github.com/ervinbani/product-api.git
cd product-api
```

2. Install dependencies:

```bash
npm install
```

3. Create a `.env` file in the root directory:

```env
MONGO_URI=your_mongodb_connection_string
PORT=3000
```

4. Start the server:

```bash
node server.js
```

The server will run on `http://localhost:3000`

## Project Structure

```
product-api/
├── config/
│   └── connection.js    # MongoDB connection logic
├── models/
│   └── Product.js       # Product schema and model
├── routes/
│   └── productRoutes.js # API route handlers
├── .env                 # Environment variables
├── .gitignore          # Git ignore file
├── server.js           # Main application entry point
└── package.json        # Project dependencies
```

## Product Schema

```javascript
{
  name: String (required),
  description: String (required),
  price: Number (required, must be > 0),
  category: String (required),
  inStock: Boolean (default: true),
  tags: Array of Strings,
  createdAt: Date (default: current date)
}
```

## API Endpoints

### Base URL

```
http://localhost:3000/api/products
```

### 1. Get All Products (with Advanced Querying)

**GET** `/api/products`

**Query Parameters:**

- `category` - Filter by product category
- `minPrice` - Minimum price filter
- `maxPrice` - Maximum price filter
- `sortBy` - Sort results (`price_asc` or `price_desc`)
- `page` - Page number (default: 1)
- `limit` - Items per page (default: 10)

**Examples:**

```bash
# Get all products
GET /api/products

# Filter by category
GET /api/products?category=Electronics

# Filter by price range
GET /api/products?minPrice=50&maxPrice=500

# Sort by price ascending
GET /api/products?sortBy=price_asc

# Combine filters with pagination
GET /api/products?category=Electronics&minPrice=100&sortBy=price_desc&page=1&limit=5
```

**Response:** `200 OK`

```json
[
  {
    "_id": "507f1f77bcf86cd799439011",
    "name": "Laptop",
    "description": "High performance laptop",
    "price": 999.99,
    "category": "Electronics",
    "inStock": true,
    "tags": ["tech", "computers"],
    "createdAt": "2026-01-18T00:00:00.000Z"
  }
]
```

### 2. Get Single Product

**GET** `/api/products/:id`

**Response:** `200 OK`

```json
{
  "_id": "507f1f77bcf86cd799439011",
  "name": "Laptop",
  "description": "High performance laptop",
  "price": 999.99,
  "category": "Electronics",
  "inStock": true,
  "tags": ["tech", "computers"],
  "createdAt": "2026-01-18T00:00:00.000Z"
}
```

**Error Response:** `404 Not Found`

```json
{
  "message": "Product not found"
}
```

### 3. Create Product

**POST** `/api/products`

**Request Body:**

```json
{
  "name": "Laptop",
  "description": "High performance laptop",
  "price": 999.99,
  "category": "Electronics",
  "inStock": true,
  "tags": ["tech", "computers"]
}
```

**Response:** `201 Created`

```json
{
  "_id": "507f1f77bcf86cd799439011",
  "name": "Laptop",
  "description": "High performance laptop",
  "price": 999.99,
  "category": "Electronics",
  "inStock": true,
  "tags": ["tech", "computers"],
  "createdAt": "2026-01-18T00:00:00.000Z"
}
```

**Error Response:** `400 Bad Request`

```json
{
  "message": "Product validation failed: price: Price must be greater than 0"
}
```

### 4. Update Product

**PUT** `/api/products/:id`

**Request Body:**

```json
{
  "price": 899.99,
  "inStock": false
}
```

**Response:** `200 OK`

```json
{
  "_id": "507f1f77bcf86cd799439011",
  "name": "Laptop",
  "description": "High performance laptop",
  "price": 899.99,
  "category": "Electronics",
  "inStock": false,
  "tags": ["tech", "computers"],
  "createdAt": "2026-01-18T00:00:00.000Z"
}
```

**Error Response:** `404 Not Found`

```json
{
  "message": "Product not found"
}
```

### 5. Delete Product

**DELETE** `/api/products/:id`

**Response:** `200 OK`

```json
{
  "message": "Product deleted successfully"
}
```

**Error Response:** `404 Not Found`

```json
{
  "message": "Product not found"
}
```

## Testing with Postman/Insomnia

### Sample Test Flow

1. **Create a product:**
   - Method: POST
   - URL: `http://localhost:3000/api/products`
   - Body (JSON):
     ```json
     {
       "name": "Wireless Mouse",
       "description": "Ergonomic wireless mouse",
       "price": 29.99,
       "category": "Electronics",
       "inStock": true,
       "tags": ["accessories", "wireless"]
     }
     ```

2. **Get all products:**
   - Method: GET
   - URL: `http://localhost:3000/api/products`

3. **Filter and sort:**
   - Method: GET
   - URL: `http://localhost:3000/api/products?category=Electronics&sortBy=price_asc&limit=5`

4. **Update a product:**
   - Method: PUT
   - URL: `http://localhost:3000/api/products/{product_id}`
   - Body (JSON):
     ```json
     {
       "price": 24.99
     }
     ```

5. **Delete a product:**
   - Method: DELETE
   - URL: `http://localhost:3000/api/products/{product_id}`

## Error Handling

All endpoints include try-catch blocks and return appropriate HTTP status codes:

- `200 OK` - Successful GET, PUT, DELETE
- `201 Created` - Successful POST
- `400 Bad Request` - Validation errors
- `404 Not Found` - Resource not found
- `500 Internal Server Error` - Server errors

## Environment Variables

| Variable  | Description               | Example                                                  |
| --------- | ------------------------- | -------------------------------------------------------- |
| MONGO_URI | MongoDB connection string | `mongodb+srv://user:password@cluster.mongodb.net/dbname` |
| PORT      | Server port number        | `3000`                                                   |

## Project Reflection

### Challenges Faced

**1. Schema Design and Validation**

- Initially struggled with implementing proper Mongoose schema validation, particularly ensuring the price field was greater than 0 rather than just non-negative
- Had to carefully consider which fields should be required vs optional, and what default values made sense for the business logic

**2. Advanced Query Building**

- The most complex part was implementing the dynamic query builder for the GET /api/products endpoint
- Learning to conditionally build MongoDB query objects based on optional query parameters required understanding how to properly structure `$gte` and `$lte` operators
- Ensuring pagination logic worked correctly with skip/limit calculations

**3. Error Handling Consistency**

- Maintaining consistent error responses across all endpoints while handling different types of errors (validation, not found, server errors)
- Understanding when to use different HTTP status codes (400 vs 404 vs 500)

**4. MongoDB Connection Management**

- Setting up proper connection error handling and ensuring the database connection was established before the server started accepting requests
- Securing the MongoDB connection string in environment variables

### What I Learned

**1. RESTful API Design Principles**

- Proper use of HTTP methods (GET, POST, PUT, DELETE) and status codes
- Structuring endpoints logically with consistent naming conventions
- Importance of returning appropriate response data for different operations

**2. MongoDB and Mongoose**

- How to design NoSQL schemas with Mongoose
- Using Mongoose query builders and methods like `findByIdAndUpdate` with options (`new: true`, `runValidators: true`)
- Understanding the difference between schema definitions and model instances
- Working with MongoDB operators for filtering and sorting

**3. Express.js Best Practices**

- Organizing routes in separate files for better code maintainability
- Using middleware for parsing JSON and URL-encoded data
- Proper project structure separation (models, routes, config)

**4. Environment Configuration**

- Using dotenv for managing sensitive configuration data
- Importance of .gitignore to prevent exposing credentials

**5. Asynchronous JavaScript**

- Extensive use of async/await for handling database operations
- Proper error handling with try-catch blocks in asynchronous code
