# Scalable REST API with Authentication & Role-Based Access

A production-ready FastAPI application with JWT authentication, role-based access control, and a React frontend.

## 🚀 Features

### Backend (FastAPI)
- ✅ **User Authentication** - JWT-based authentication with secure password hashing
- ✅ **Role-Based Access Control** - Three roles: Admin, Manager, User
- ✅ **User Management** - Registration, login, profile management
- ✅ **Product CRUD** - Complete Create, Read, Update, Delete operations
- ✅ **API Versioning** - All endpoints under `/api/v1/`
- ✅ **Input Validation** - Pydantic models with comprehensive validation
- ✅ **Error Handling** - Global exception handlers with proper HTTP status codes
- ✅ **API Documentation** - Auto-generated Swagger UI and ReDoc
- ✅ **Database Integration** - SQLAlchemy ORM with PostgreSQL support
- ✅ **CORS Support** - Cross-Origin Resource Sharing enabled

### Frontend (React)
- ✅ **Modern UI** - Clean, responsive React application
- ✅ **User Authentication** - Register and login with JWT token management
- ✅ **Protected Routes** - Route protection with automatic redirect
- ✅ **Product Management** - Full CRUD interface for products
- ✅ **Search & Filter** - Search products by name/description, filter by category
- ✅ **Toast Notifications** - Success/error feedback for all operations
- ✅ **Loading States** - Spinner indicators during API calls

## 📋 Prerequisites

- Python 3.8+
- Node.js 16+
- PostgreSQL database
- pip (Python package manager)
- npm or yarn

## 🛠️ Installation

### Option A: Docker (Recommended) 🐳

**Easiest way to get started - runs everything in containers:**

```bash
# 1. Build and start all services (backend, frontend, database)
docker-compose up -d --build

# 2. Access the application
# Frontend: http://localhost:3000
# Backend API: http://localhost:8000
# API Docs: http://localhost:8000/api/docs

# 3. View logs
docker-compose logs -f

# 4. Stop all services
docker-compose down
```

**What's included:**
- ✅ PostgreSQL database (port 5432)
- ✅ FastAPI backend (port 8000)
- ✅ React frontend (port 3000)
- ✅ All dependencies installed
- ✅ Auto-restart on failure
- ✅ Hot-reload for development

See **[DOCKER_GUIDE.md](DOCKER_GUIDE.md)** for complete Docker documentation.

### Option B: Manual Setup (Development)

#### 1. Clone or Download the Project

```bash
cd Assignment1
```

#### 2. Setup Backend

**Install Python dependencies:**

```bash
# Create virtual environment (optional but recommended)
python -m venv venv
.\venv\Scripts\Activate.ps1  # Windows
# source venv/bin/activate   # Linux/Mac

# Install dependencies
pip install -r requirements.txt
```

**Setup database:**

```bash
# Make sure PostgreSQL is running
# Update DATABASE_URL in app/config.py or create .env file
```

**Run backend:**

```bash
uvicorn app.main:app --reload
```

Backend will be at http://localhost:8000

#### 3. Setup Frontend

**Install Node dependencies:**

```bash
cd frontend
npm install
```

**Run frontend:**

```bash
npm run dev
```

Frontend will be at http://localhost:3000

### Option C: Backend Docker + Frontend Local

Run backend in Docker, frontend locally for easier frontend development:

```bash
# Start backend and database only
docker-compose up -d postgres backend

# In another terminal, start frontend locally
cd frontend
npm run dev
```
python create_backend.py
```

This will create:
- `app/` directory with all backend modules
- `app/main.py` - FastAPI application entry point
- `app/models.py` - Database models (User, Product)
- `app/schemas.py` - Pydantic schemas for validation
- `app/auth.py` - Authentication and authorization logic
- `app/config.py` - Application configuration
- `app/database.py` - Database connection setup
- `app/routers/` - API route handlers (auth, users, products)

#### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

#### 4. Configure Database

#### Option A: Using PostgreSQL (Recommended)

1. Install PostgreSQL if not already installed
2. Create a database:
   ```sql
   CREATE DATABASE apidb;
   CREATE USER apiuser WITH PASSWORD 'yourpassword';
   GRANT ALL PRIVILEGES ON DATABASE apidb TO apiuser;
   ```

3. Copy `.env.example` to `.env`:
   ```bash
   copy .env.example .env
   ```

4. Update `.env` with your database credentials:
   ```
   DATABASE_URL=postgresql://apiuser:yourpassword@localhost:5432/apidb
   SECRET_KEY=your-secret-key-here-change-in-production
   ALGORITHM=HS256
   ACCESS_TOKEN_EXPIRE_MINUTES=30
   ```

#### Option B: Using SQLite (For Testing)

Modify `app/config.py` to use SQLite:
```python
DATABASE_URL: str = "sqlite:///./test.db"
```

### 5. Run the Application

```bash
uvicorn app.main:app --reload
```

The API will be available at: `http://localhost:8000`

## 📖 API Documentation

Once the server is running, access the interactive API documentation:

- **Swagger UI**: http://localhost:8000/api/docs
- **ReDoc**: http://localhost:8000/api/redoc

## 🎨 Frontend Testing Interface

Open `frontend.html` in your web browser to access the testing dashboard.

### Using the Frontend:

1. **Register a User**
   - Navigate to Authentication tab
   - Fill in registration form
   - Select role (user, manager, or admin)
   - Click "Register"

2. **Login**
   - Enter username and password
   - Click "Login"
   - Token will be automatically saved

3. **Test Products API**
   - Switch to Products tab
   - Create, view, update, or delete products
   - All requests use the saved authentication token

4. **Test Users API**
   - Switch to Users tab
   - View your profile
   - List all users (requires admin/manager role)

## 🔑 API Endpoints

### Authentication (`/api/v1/auth`)

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| POST | `/register` | Register new user | No |
| POST | `/login` | Login and get JWT token | No |

### Users (`/api/v1/users`)

| Method | Endpoint | Description | Auth Required | Roles |
|--------|----------|-------------|---------------|-------|
| GET | `/me` | Get current user profile | Yes | All |
| GET | `/` | List all users | Yes | Admin, Manager |
| GET | `/{user_id}` | Get user by ID | Yes | Admin, Manager |

### Products (`/api/v1/products`)

| Method | Endpoint | Description | Auth Required | Roles |
|--------|----------|-------------|---------------|-------|
| POST | `/` | Create product | Yes | All |
| GET | `/` | List products | Yes | All |
| GET | `/{product_id}` | Get product by ID | Yes | All |
| PUT | `/{product_id}` | Update product | Yes | Owner, Admin, Manager |
| DELETE | `/{product_id}` | Delete product | Yes | Owner, Admin, Manager |

## 🗂️ Database Schema

### Users Table
- `id` - Primary key
- `email` - Unique email address
- `username` - Unique username
- `hashed_password` - Bcrypt hashed password
- `full_name` - User's full name
- `role` - Enum: admin, manager, user
- `is_active` - Boolean status
- `created_at` - Timestamp
- `updated_at` - Timestamp

### Products Table
- `id` - Primary key
- `name` - Product name
- `description` - Product description
- `price` - Product price (integer)
- `stock` - Available stock quantity
- `category` - Product category
- `owner_id` - Foreign key to Users
- `created_at` - Timestamp
- `updated_at` - Timestamp

## 🔒 Security Features

1. **Password Hashing** - Bcrypt algorithm with salt
2. **JWT Tokens** - Secure token-based authentication
3. **Role-Based Access** - Fine-grained permission control
4. **Input Validation** - Pydantic models validate all inputs
5. **SQL Injection Protection** - SQLAlchemy ORM parameterized queries
6. **CORS Configuration** - Controlled cross-origin access

## 📝 Example Usage

### Register a User
```bash
curl -X POST "http://localhost:8000/api/v1/auth/register" \
  -H "Content-Type: application/json" \
  -d '{
    "email": "admin@example.com",
    "username": "admin",
    "password": "securepass123",
    "full_name": "Admin User",
    "role": "admin"
  }'
```

### Login
```bash
curl -X POST "http://localhost:8000/api/v1/auth/login" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "username=admin&password=securepass123"
```

### Create Product (with token)
```bash
curl -X POST "http://localhost:8000/api/v1/products/" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_TOKEN_HERE" \
  -d '{
    "name": "Laptop",
    "description": "High-performance laptop",
    "price": 1200,
    "stock": 10,
    "category": "Electronics"
  }'
```

## 🧪 Testing

### Manual Testing
1. Use the provided `frontend.html` interface
2. Use the Swagger UI at `/api/docs`
3. Use curl commands or Postman

### Automated Testing (Optional)
Create a `tests/` directory and add pytest tests:

```bash
pip install pytest pytest-asyncio httpx
pytest
```

## 📁 Project Structure

```
Assignment1/
├── app/
│   ├── __init__.py
│   ├── main.py              # FastAPI app initialization
│   ├── config.py            # Configuration settings
│   ├── database.py          # Database connection
│   ├── models.py            # SQLAlchemy models
│   ├── schemas.py           # Pydantic schemas
│   ├── auth.py              # Authentication logic
│   └── routers/
│       ├── __init__.py
│       ├── auth.py          # Auth endpoints
│       ├── users.py         # User endpoints
│       └── products.py      # Product endpoints
├── frontend.html            # Testing UI
├── requirements.txt         # Python dependencies
├── .env.example            # Environment variables template
├── .gitignore              # Git ignore file
├── create_backend.py       # Setup script
└── README.md               # This file
```

## 🚧 Troubleshooting

### Database Connection Issues
- Ensure PostgreSQL is running
- Verify database credentials in `.env`
- Check if the database exists

### Token Errors
- Ensure you're logged in and have a valid token
- Check token expiration (default: 30 minutes)
- Clear browser localStorage if needed

### CORS Errors
- Verify CORS middleware is enabled in `main.py`
- Check that frontend is accessing correct API URL

## 🔄 Future Enhancements

- [ ] Password reset functionality
- [ ] Email verification
- [ ] Rate limiting
- [ ] API key authentication
- [ ] Pagination for list endpoints
- [ ] Advanced filtering and search
- [ ] File upload for product images
- [ ] WebSocket support for real-time updates

## 📄 License

This project is for educational purposes.

## 👨‍💻 Author

Created as part of Assignment 1 - Scalable REST API Development

## 📞 Support

For issues or questions:
1. Check the API documentation at `/api/docs`
2. Review error messages in the response
3. Check server logs for detailed error information
