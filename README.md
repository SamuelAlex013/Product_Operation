# Scalable REST API with Authentication & Role-Based Access

A production-ready FastAPI application with JWT authentication, role-based access control, and a React frontend.
- ✅ **Modern UI** - Clean, responsive React application
- ✅ **Product Management** - Full CRUD interface for products

## 🛠️ Installation

### Option A: Docker (Recommended) 🐳

Run everything in containers (database, backend, frontend):

```bash
# From project root
docker-compose up -d --build
```

Access the services after startup:

- Frontend: http://localhost:3000
- Backend API: http://localhost:8000
- API Docs: http://localhost:8000/api/docs

View logs or stop services:

```bash
docker-compose logs -f        # stream logs
docker-compose down          # stop and remove containers
docker-compose down -v       # also remove volumes (data)
```

The compose stack includes:

- PostgreSQL (port 5432)
- FastAPI backend (port 8000)
- React frontend served by nginx (port 3000)


### Option B: Manual (Development)

Run backend and frontend on your host for fast development:

Backend (Python venv):

```bash
cd F:\\college\\projects\\Assignment1
.\\venv\\Scripts\\Activate.ps1   # Windows PowerShell
# or: source venv/bin/activate  # Linux/Mac
pip install -r requirements.txt
uvicorn app.main:app --reload
```

Frontend (Vite dev server):

```bash
cd frontend
npm install
npm run dev
```

Frontend will run at `http://localhost:3000` and proxy requests to the backend at `http://localhost:8000` (CORS is enabled in the backend).

### Option C: Hybrid (Backend in Docker, Frontend local)

```bash
# Start DB + backend only


# In another terminal, run frontend locally
cd frontend
npm run dev
```

This is convenient for frontend development while keeping the backend environment consistent.
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


