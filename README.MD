EasyShop E-Commerce Backend API
A full-stack e-commerce REST API built with Spring Boot, featuring JWT authentication, shopping cart functionality, and comprehensive product and category management.
Project Overview
EasyShop is a backend API for an e-commerce platform that supports multiple store types (clothing, electronics, groceries, records, and video games). The application provides secure user authentication, product browsing, category management, and shopping cart operations.
Features

JWT Authentication - Secure token-based authentication and authorization
Role-Based Access Control - Admin and User roles with different permissions
Product Management - Full CRUD operations for products with filtering capabilities
Category Management - Organize products into categories with admin controls
Shopping Cart - Add, update, remove items and view cart totals
User Profiles - User registration and profile management
Multiple Store Databases - Support for different store types (clothing, electronics, etc.)

Technologies Used

Spring Boot 2.7.3 - Application framework
Spring Security - Authentication and authorization
JWT (JSON Web Tokens) - Secure token-based authentication
MySQL - Database management
Maven - Dependency management
JDBC - Database connectivity

Database Schema
The application uses the following main tables:

users - User authentication and roles
profiles - User profile information
categories - Product categories
products - Product catalog
shopping_cart - User shopping cart items
orders - Order history
order_line_items - Order details

API Endpoints
Authentication

POST /login - User login (returns JWT token)
POST /register - User registration

Categories

GET /categories - Get all categories
GET /categories/{id} - Get category by ID
GET /categories/{id}/products - Get products in a category
POST /categories - Create category (Admin only)
PUT /categories/{id} - Update category (Admin only)
DELETE /categories/{id} - Delete category (Admin only)

Products

GET /products - Search products (supports filtering by category, price range, subcategory)
GET /products/{id} - Get product by ID
POST /products - Create product (Admin only)
PUT /products/{id} - Update product (Admin only)
DELETE /products/{id} - Delete product (Admin only)

Shopping Cart

GET /cart - Get user's shopping cart
POST /cart/products/{productId} - Add product to cart
PUT /cart/products/{productId} - Update product quantity
DELETE /cart - Clear cart

Setup Instructions
Prerequisites

Java 17 or higher
MySQL 8.0 or higher
Maven 3.6 or higher

Database Setup

Create a MySQL database:

sqlCREATE DATABASE easyshop;

Run one of the provided SQL scripts from the database/ folder:

create_database_easyshop.sql - Electronics/Fashion/Home & Kitchen store
create_database_clothingstore.sql - Clothing store
create_database_groceryapp.sql - Grocery store
create_database_recordshop.sql - Music store
create_database_videogamestore.sql - Video game store



Application Configuration
Update src/main/resources/application.properties:
propertiesdatasource.url=jdbc:mysql://localhost:3306/easyshop
datasource.username=root
datasource.password=yourpassword

jwt.secret=your-secret-key
jwt.token-timeout-seconds=108000
Running the Application

Clone the repository
Navigate to the project directory
Run with Maven:

bash./mvnw spring-boot:run
Or on Windows:
bashmvnw.cmd spring-boot:run
The application will start on http://localhost:8080
Default Users
The application comes with pre-configured test users:

Admin User

Username: admin
Password: password
Role: ROLE_ADMIN


Regular User

Username: user
Password: password
Role: ROLE_USER



Testing
Run tests with Maven:
bash./mvnw test
```

Test configuration is in `src/test/resources/application.properties`

## Security

- All endpoints except `/login` and `/register` require authentication
- Admin-only endpoints are protected with `@PreAuthorize("hasRole('ADMIN')")`
- Passwords are encrypted using BCrypt
- JWT tokens expire after 30 hours (configurable)

## Key Implementation Details

### Shopping Cart
- Uses composite primary key (user_id, product_id)
- Automatically handles quantity increments for duplicate additions
- Returns complete cart with product details after operations

### Product Search
- Supports multiple filter parameters: category, min/max price, subcategory
- Uses prepared statements to prevent SQL injection
- Returns empty list when no matches found

### Error Handling
- Consistent error responses with appropriate HTTP status codes
- `404 NOT_FOUND` for missing resources
- `500 INTERNAL_SERVER_ERROR` for unexpected errors
- Custom "Oops... our bad." messages for user-facing errors

## Project Structure
```
src/
├── main/
│   ├── java/org/yearup/
│   │   ├── controllers/      # REST controllers
│   │   ├── data/             # DAO interfaces
│   │   │   └── mysql/        # MySQL implementations
│   │   ├── models/           # Domain models
│   │   ├── security/         # Security configuration
│   │   └── configurations/   # Application configuration
│   └── resources/
│       └── application.properties
└── test/                     # Unit tests
Contributing
This project was developed as a capstone project. Key features implemented:

Category CRUD operations with authorization
Shopping cart functionality with full CRUD
Product search and filtering
JWT authentication integration
Database design for multiple store types