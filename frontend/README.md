# Product Manager Frontend

A React-based frontend application for managing products with user authentication.

## Features

- User authentication (register, login, logout)
- Protected routes with JWT token management
- Product CRUD operations (Create, Read, Update, Delete)
- Search and filter products by name/category
- Toast notifications for user feedback
- Responsive design

## Tech Stack

- **React** - UI library
- **React Router** - Client-side routing
- **Axios** - HTTP client for API calls
- **Vite** - Build tool and dev server

## Prerequisites

- Node.js (v16 or higher)
- npm or yarn
- Backend API running on `http://localhost:8000`

## Installation

1. Install dependencies:
```bash
npm install
```

## Running the Application

1. Start the development server:
```bash
npm run dev
```

The application will open at `http://localhost:3000`

2. Make sure the FastAPI backend is running on `http://localhost:8000`

## Building for Production

```bash
npm run build
```

The production-ready files will be in the `dist/` folder.

## Project Structure

```
frontend/
├── src/
│   ├── api/
│   │   └── axios.js          # Axios configuration and API endpoints
│   ├── components/
│   │   ├── Navbar.jsx        # Navigation bar
│   │   ├── ProtectedRoute.jsx # Route protection wrapper
│   │   └── Toast.jsx         # Toast notification component
│   ├── context/
│   │   ├── AuthContext.jsx   # Authentication state management
│   │   └── ToastContext.jsx  # Toast notification state
│   ├── pages/
│   │   ├── Login.jsx         # Login page
│   │   ├── Register.jsx      # Registration page
│   │   ├── Dashboard.jsx     # Product listing page
│   │   └── ProductForm.jsx   # Create/Edit product form
│   ├── App.jsx              # Main app component with routing
│   ├── main.jsx             # Application entry point
│   └── index.css            # Global styles
├── index.html
├── package.json
└── vite.config.js
```

## API Integration

The frontend connects to the FastAPI backend at `http://localhost:8000/api/v1` with the following endpoints:

### Authentication
- `POST /auth/register` - Register new user (JSON body)
- `POST /auth/login` - Login (form-urlencoded)
- `GET /users/me` - Get current user info

### Products
- `GET /products` - List all products
- `GET /products/{id}` - Get product by ID
- `POST /products` - Create new product
- `PUT /products/{id}` - Update product
- `DELETE /products/{id}` - Delete product

## Authentication Flow

1. User registers via `/register` page
2. User logs in via `/login` page (credentials sent as form-urlencoded)
3. JWT token is stored in `localStorage`
4. Token is automatically attached to all API requests via Axios interceptor
5. On 401 response, user is redirected to login page

## Features

### User Authentication
- Registration with email, username, password, and optional full name
- Login with username and password
- Auto-logout on token expiration
- Protected routes requiring authentication

### Product Management
- View all products in a card-based grid layout
- Search products by name or description
- Filter products by category
- Create new products with name, description, price, stock, and category
- Edit existing products
- Delete products with confirmation

### User Experience
- Toast notifications for success/error messages
- Loading spinners during API calls
- Form validation
- Responsive design for mobile and desktop
