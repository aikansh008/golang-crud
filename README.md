# Golang Gin JWT Auth CRUD API

A comprehensive RESTful API built with Go, Gin framework, GORM, and JWT authentication. This project implements a complete blog system with user authentication, categories, posts, and comments featuring full CRUD operations and soft delete functionality.

## 📋 Table of Contents

- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Database Migration](#database-migration)
- [Running the Application](#running-the-application)
- [API Endpoints](#api-endpoints)
- [Authentication](#authentication)
- [Models](#models)
- [Testing](#testing)
- [Contributing](#contributing)
- [License](#license)

## ✨ Features

- **JWT Authentication**: Secure user authentication with JSON Web Tokens
- **User Management**: Complete CRUD operations for users
- **Blog System**: Create, read, update, and delete blog posts
- **Categories**: Organize posts with categories
- **Comments**: Comment system for posts
- **Soft Delete**: Trash and restore functionality for all entities
- **Pagination**: Built-in pagination support for list endpoints
- **Validation**: Comprehensive input validation with custom error messages
- **Password Hashing**: Secure password storage using bcrypt
- **Middleware Authentication**: Protected routes with JWT middleware
- **PostgreSQL Database**: Robust data persistence with GORM ORM
- **Slug Generation**: Automatic URL-friendly slug generation for posts

## 🛠 Tech Stack

- **Language**: Go 1.20
- **Web Framework**: [Gin](https://github.com/gin-gonic/gin) v1.9.1
- **ORM**: [GORM](https://gorm.io/) v1.25.2
- **Database**: PostgreSQL (via [gorm.io/driver/postgres](https://github.com/go-gorm/postgres))
- **Authentication**: [JWT](https://github.com/golang-jwt/jwt) v5.0.0
- **Password Hashing**: [bcrypt](https://pkg.go.dev/golang.org/x/crypto/bcrypt)
- **Validation**: [go-playground/validator](https://github.com/go-playground/validator) v10.14.1
- **Environment Variables**: [godotenv](https://github.com/joho/godotenv) v1.5.1
- **Slug Generation**: [gosimple/slug](https://github.com/gosimple/slug) v1.13.1

## 📁 Project Structure

```
golang-gin-jwt-auth-crud/
├── api/
│   ├── controllers/          # Request handlers
│   │   ├── categoryController.go
│   │   ├── commentController.go
│   │   ├── postController.go
│   │   └── userController.go
│   ├── middleware/           # Middleware functions
│   │   └── requireAuth.go
│   └── router/               # Route definitions
│       └── routes.go
├── config/                   # Configuration files
│   └── loadEnvVariables.go
├── db/
│   ├── initializers/         # Database initialization
│   │   └── database.go
│   └── migrate/              # Database migrations
│       └── migrate.go
├── internal/
│   ├── format-errors/        # Error formatting utilities
│   │   └── errors.go
│   ├── helpers/              # Helper functions
│   │   └── auth.go
│   ├── models/               # Data models
│   │   ├── categoryModel.go
│   │   ├── commentModel.go
│   │   ├── postModel.go
│   │   └── userModel.go
│   ├── pagination/           # Pagination utilities
│   │   └── pagination.go
│   └── validations/          # Custom validations
│       └── validations.go
├── tests/                    # Test files
│   ├── db.go
│   └── user_test.go
├── go.mod                    # Go module definition
├── go.sum                    # Go dependencies checksum
├── main.go                   # Application entry point
└── README.md                 # Project documentation
```

## 📋 Prerequisites

Before you begin, ensure you have the following installed:

- **Go**: Version 1.20 or higher ([Download](https://golang.org/dl/))
- **PostgreSQL**: Version 12 or higher ([Download](https://www.postgresql.org/download/))
- **Git**: For cloning the repository ([Download](https://git-scm.com/downloads))

## 🚀 Installation

1. **Clone the repository**

```bash
git clone https://github.com/aikansh008/golang-crud.git
cd golang-crud
```

2. **Install dependencies**

```bash
go mod download
```

3. **Verify installation**

```bash
go mod verify
```

## ⚙️ Configuration

1. **Create a `.env` file** in the root directory:

```env
# Database Configuration
DB_HOST=localhost
DB_PORT=5432
DB_USER=your_db_user
DB_PASSWORD=your_db_password
DB_NAME=your_db_name

# JWT Secret
SECRET=your_jwt_secret_key_here

# Server Configuration (Optional)
PORT=8080
```

2. **Environment Variables Explained**:

- `DB_HOST`: PostgreSQL host (default: localhost)
- `DB_PORT`: PostgreSQL port (default: 5432)
- `DB_USER`: Database username
- `DB_PASSWORD`: Database password
- `DB_NAME`: Database name
- `SECRET`: Secret key for JWT token signing (use a strong, random string)
- `PORT`: Server port (default: 8080)

## 🗄️ Database Migration

Run the migration script to create database tables:

```bash
go run db/migrate/migrate.go
```

This will create the following tables:
- `users`
- `categories`
- `posts`
- `comments`

**Note**: The migration script drops existing tables before creating new ones. Use with caution in production.

## 🏃 Running the Application

1. **Development Mode**:

```bash
go run main.go
```

2. **Build and Run**:

```bash
go build -o app
./app
```

3. **With Hot Reload** (using [Air](https://github.com/cosmtrek/air)):

```bash
# Install Air
go install github.com/cosmtrek/air@latest

# Run with hot reload
air
```

The server will start on `http://localhost:8080` (or the port specified in your `.env` file).

## 📡 API Endpoints

### Authentication

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| POST | `/api/signup` | Register a new user | No |
| POST | `/api/login` | Login and get JWT token | No |
| POST | `/api/logout` | Logout user | Yes |

### Users

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/api/users/` | Get all users (paginated) | Yes |
| GET | `/api/users/:id/edit` | Get user by ID | Yes |
| PUT | `/api/users/:id/update` | Update user | Yes |
| DELETE | `/api/users/:id/delete` | Soft delete user | Yes |
| GET | `/api/users/all-trash` | Get all trashed users | Yes |
| DELETE | `/api/users/delete-permanent/:id` | Permanently delete user | Yes |

### Categories

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/api/categories/` | Get all categories | Yes |
| POST | `/api/categories/create` | Create new category | Yes |
| GET | `/api/categories/:id/edit` | Get category by ID | Yes |
| PUT | `/api/categories/:id/update` | Update category | Yes |
| DELETE | `/api/categories/:id/delete` | Soft delete category | Yes |
| GET | `/api/categories/all-trash` | Get all trashed categories | Yes |
| DELETE | `/api/categories/delete-permanent/:id` | Permanently delete category | Yes |

### Posts

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/api/posts/` | Get all posts (paginated) | Yes |
| POST | `/api/posts/create` | Create new post | Yes |
| GET | `/api/posts/:id/show` | Get post by ID | Yes |
| GET | `/api/posts/:id/edit` | Get post for editing | Yes |
| PUT | `/api/posts/:id/update` | Update post | Yes |
| DELETE | `/api/posts/:id/delete` | Soft delete post | Yes |
| GET | `/api/posts/all-trash` | Get all trashed posts | Yes |
| DELETE | `/api/posts/delete-permanent/:id` | Permanently delete post | Yes |

### Comments

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| POST | `/api/posts/:id/comment/store` | Add comment to post | Yes |
| GET | `/api/posts/:id/comment/:comment_id/edit` | Get comment by ID | Yes |
| PUT | `/api/posts/:id/comment/:comment_id/update` | Update comment | Yes |
| DELETE | `/api/posts/:id/comment/:comment_id/delete` | Delete comment | Yes |

### Query Parameters

**Pagination** (available on list endpoints):
- `page`: Page number (default: 1)
- `perPage`: Items per page (default: 5)

Example: `/api/users?page=2&perPage=10`

## 🔐 Authentication

This API uses JWT (JSON Web Tokens) for authentication.

### How It Works

1. **Signup**: Create a new account at `/api/signup`
2. **Login**: Authenticate at `/api/login` to receive a JWT token
3. **Use Token**: The token is automatically stored as an HTTP-only cookie
4. **Protected Routes**: All routes except `/api/signup` and `/api/login` require authentication

### Example: Signup

**Request**:
```json
POST /api/signup
Content-Type: application/json

{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "securePassword123"
}
```

**Response**:
```json
{
  "user": {
    "ID": 1,
    "name": "John Doe",
    "email": "john@example.com",
    "CreatedAt": "2024-01-01T10:00:00Z",
    "UpdatedAt": "2024-01-01T10:00:00Z"
  }
}
```

### Example: Login

**Request**:
```json
POST /api/login
Content-Type: application/json

{
  "email": "john@example.com",
  "password": "securePassword123"
}
```

**Response**:
```json
{} 
```
*Note: JWT token is set as an HTTP-only cookie*

### Example: Create Category

**Request**:
```json
POST /api/categories/create
Content-Type: application/json
Cookie: Authorization=<jwt_token>

{
  "name": "Technology"
}
```

**Response**:
```json
{
  "category": {
    "ID": 1,
    "name": "Technology",
    "slug": "technology",
    "CreatedAt": "2024-01-01T10:00:00Z",
    "UpdatedAt": "2024-01-01T10:00:00Z"
  }
}
```

### Example: Create Post

**Request**:
```json
POST /api/posts/create
Content-Type: application/json
Cookie: Authorization=<jwt_token>

{
  "title": "My First Post",
  "body": "This is the content of my first post.",
  "categoryID": 1
}
```

**Response**:
```json
{
  "post": {
    "ID": 1,
    "categoryID": 1,
    "title": "My First Post",
    "body": "This is the content of my first post.",
    "userID": 1,
    "CreatedAt": "2024-01-01T10:00:00Z",
    "UpdatedAt": "2024-01-01T10:00:00Z"
  }
}
```

### Example: Create Comment

**Request**:
```json
POST /api/posts/1/comment/store
Content-Type: application/json
Cookie: Authorization=<jwt_token>

{
  "postId": 1,
  "body": "Great post! Thanks for sharing."
}
```

## 📊 Models

### User
```go
{
  "ID": uint,
  "name": string,
  "email": string,
  "CreatedAt": timestamp,
  "UpdatedAt": timestamp,
  "DeletedAt": timestamp (nullable)
}
```

### Category
```go
{
  "ID": uint,
  "name": string,
  "slug": string,
  "CreatedAt": timestamp,
  "UpdatedAt": timestamp,
  "DeletedAt": timestamp (nullable)
}
```

### Post
```go
{
  "ID": uint,
  "categoryID": uint,
  "title": string,
  "body": text,
  "userID": uint,
  "CreatedAt": timestamp,
  "UpdatedAt": timestamp,
  "DeletedAt": timestamp (nullable)
}
```

### Comment
```go
{
  "ID": uint,
  "postID": uint,
  "userID": uint,
  "body": text,
  "CreatedAt": timestamp,
  "UpdatedAt": timestamp,
  "DeletedAt": timestamp (nullable)
}
```

## 🧪 Testing

Run tests using:

```bash
go test ./tests/...
```

Run tests with coverage:

```bash
go test ./tests/... -cover
```

Run tests with verbose output:

```bash
go test ./tests/... -v
```

## 🔒 Security Features

- **Password Hashing**: All passwords are hashed using bcrypt before storage
- **HTTP-Only Cookies**: JWT tokens are stored in HTTP-only cookies to prevent XSS attacks
- **Input Validation**: All inputs are validated using go-playground/validator
- **SQL Injection Prevention**: GORM ORM provides protection against SQL injection
- **Unique Email Constraint**: Database-level unique constraint on user emails

## 📝 API Request Examples

### Using cURL

**Signup**:
```bash
curl -X POST http://localhost:8080/api/signup \
  -H "Content-Type: application/json" \
  -d '{"name":"John Doe","email":"john@example.com","password":"secret123"}'
```

**Login**:
```bash
curl -X POST http://localhost:8080/api/login \
  -H "Content-Type: application/json" \
  -c cookies.txt \
  -d '{"email":"john@example.com","password":"secret123"}'
```

**Get Users** (authenticated):
```bash
curl -X GET http://localhost:8080/api/users?page=1&perPage=10 \
  -b cookies.txt
```

**Create Category** (authenticated):
```bash
curl -X POST http://localhost:8080/api/categories/create \
  -H "Content-Type: application/json" \
  -b cookies.txt \
  -d '{"name":"Technology"}'
```

**Create Post** (authenticated):
```bash
curl -X POST http://localhost:8080/api/posts/create \
  -H "Content-Type: application/json" \
  -b cookies.txt \
  -d '{"title":"Hello World","body":"This is my first post","categoryID":1}'
```

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Coding Standards

- Follow Go conventions and best practices
- Write clear, descriptive commit messages
- Add tests for new features
- Update documentation as needed
- Run `go fmt` before committing
- Ensure all tests pass before submitting PR

## 📝 License

This project is open source and available under the [MIT License](LICENSE).

## 👤 Author

**Aikansh**
- GitHub: [@aikansh008](https://github.com/aikansh008)
- Repository: [golang-crud](https://github.com/aikansh008/golang-crud)

## 🙏 Acknowledgments

- [Gin Web Framework](https://github.com/gin-gonic/gin)
- [GORM ORM](https://gorm.io/)
- [JWT-Go](https://github.com/golang-jwt/jwt)
- Go Community

## 📞 Support

For support, please open an issue in the GitHub repository or contact the maintainer.

## 🔄 Version History

- **v1.0.0** - Initial release with full CRUD operations, JWT authentication, and soft delete functionality

---

**Happy Coding! 🚀**