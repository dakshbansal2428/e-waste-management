# E-Waste Management System - Setup Guide

This guide provides detailed instructions for setting up both the frontend and backend components of the E-Waste Management System for development.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Backend Setup](#backend-setup)
- [Frontend Setup](#frontend-setup)
- [Running the Application](#running-the-application)
- [Troubleshooting](#troubleshooting)
- [Additional Resources](#additional-resources)

---

## Prerequisites

Before you begin, ensure you have the following installed on your system:

### Common Requirements
- **Git**: [Download and Install](https://git-scm.com/)
- **Node.js**: Version 14.0 or higher ([Download](https://nodejs.org/))
- **npm** or **yarn**: Comes with Node.js installation

### Backend Requirements
- **Python**: Version 3.8 or higher ([Download](https://www.python.org/downloads/))
- **pip**: Python package manager (comes with Python)
- **Virtual Environment**: `venv` (built-in with Python 3.3+)

### Optional Tools
- **Docker**: For containerized development ([Download](https://www.docker.com/products/docker-desktop))
- **Postman**: For API testing ([Download](https://www.postman.com/downloads/))
- **VS Code**: Recommended code editor ([Download](https://code.visualstudio.com/))

---

## Backend Setup

### Step 1: Clone the Repository

```bash
git clone https://github.com/dakshbansal2428/e-waste-management.git
cd e-waste-management
```

### Step 2: Navigate to Backend Directory

```bash
cd backend
# or wherever your backend code is located
```

### Step 3: Create a Virtual Environment

**On macOS/Linux:**
```bash
python3 -m venv venv
source venv/bin/activate
```

**On Windows:**
```bash
python -m venv venv
venv\Scripts\activate
```

### Step 4: Install Dependencies

```bash
pip install --upgrade pip
pip install -r requirements.txt
```

### Step 5: Environment Configuration

Create a `.env` file in the backend directory with the following variables:

```env
# Database Configuration
DATABASE_URL=sqlite:///db.sqlite3
# or for PostgreSQL: DATABASE_URL=postgresql://user:password@localhost:5432/dbname

# Django Settings
DEBUG=True
SECRET_KEY=your_secret_key_here
ALLOWED_HOSTS=localhost,127.0.0.1

# Email Configuration (Optional)
EMAIL_BACKEND=django.core.mail.backends.console.EmailBackend
EMAIL_HOST=smtp.gmail.com
EMAIL_PORT=587
EMAIL_USE_TLS=True
EMAIL_HOST_USER=your_email@gmail.com
EMAIL_HOST_PASSWORD=your_app_password

# JWT Configuration (If using JWT)
JWT_SECRET_KEY=your_jwt_secret_key

# AWS/Storage Configuration (Optional)
AWS_ACCESS_KEY_ID=your_aws_key
AWS_SECRET_ACCESS_KEY=your_aws_secret
AWS_STORAGE_BUCKET_NAME=your_bucket_name

# Other Settings
CORS_ALLOWED_ORIGINS=http://localhost:3000,http://localhost:8000
```

### Step 6: Database Migration

```bash
python manage.py migrate
# or for custom project structure:
# python manage.py makemigrations
# python manage.py migrate
```

### Step 7: Create a Superuser (Admin Account)

```bash
python manage.py createsuperuser
# Follow the prompts to create an admin account
```

### Step 8: Run the Backend Server

```bash
python manage.py runserver
# Server will run on http://localhost:8000
```

The backend API will be accessible at `http://localhost:8000/api/`

### Backend Structure (Expected)

```
backend/
├── manage.py
├── requirements.txt
├── .env
├── venv/
├── project_name/
│   ├── settings.py
│   ├── urls.py
│   ├── wsgi.py
│   └── asgi.py
├── app_name/
│   ├── models.py
│   ├── views.py
│   ├── serializers.py
│   ├── urls.py
│   └── tests.py
└── static/
```

---

## Frontend Setup

### Step 1: Navigate to Frontend Directory

```bash
cd frontend
# or wherever your frontend code is located
```

### Step 2: Install Dependencies

Using npm:
```bash
npm install
```

Or using yarn:
```bash
yarn install
```

### Step 3: Environment Configuration

Create a `.env` file in the frontend directory with the following variables:

```env
REACT_APP_API_BASE_URL=http://localhost:8000/api
REACT_APP_API_TIMEOUT=30000
REACT_APP_ENV=development

# Optional: Google Maps API
REACT_APP_GOOGLE_MAPS_API_KEY=your_api_key_here

# Optional: File Upload
REACT_APP_MAX_FILE_SIZE=5242880
```

**Note for Create React App users:** Environment variables must start with `REACT_APP_` to be exposed to the browser.

### Step 4: Install Additional Tools (Optional)

If you're using Tailwind CSS or other preprocessors:

```bash
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

### Step 5: Run the Frontend Development Server

Using npm:
```bash
npm start
# Frontend will run on http://localhost:3000
```

Or using yarn:
```bash
yarn start
```

The application will automatically open in your default browser.

### Frontend Structure (Expected)

```
frontend/
├── node_modules/
├── public/
│   ├── index.html
│   └── favicon.ico
├── src/
│   ├── components/
│   ├── pages/
│   ├── services/
│   ├── utils/
│   ├── App.js
│   ├── App.css
│   └── index.js
├── .env
├── package.json
└── package-lock.json
```

---

## Running the Application

### Running Both Backend and Frontend Simultaneously

#### Option 1: Using Separate Terminals

**Terminal 1 - Backend:**
```bash
cd backend
source venv/bin/activate  # On Windows: venv\Scripts\activate
python manage.py runserver
```

**Terminal 2 - Frontend:**
```bash
cd frontend
npm start
```

#### Option 2: Using npm-run-all (Recommended)

1. Install npm-run-all in the root directory:
```bash
npm install -g npm-run-all
```

2. Create a root `package.json` if it doesn't exist:
```json
{
  "name": "e-waste-management",
  "scripts": {
    "start:backend": "cd backend && python manage.py runserver",
    "start:frontend": "cd frontend && npm start",
    "start": "npm-run-all -p start:backend start:frontend"
  }
}
```

3. Run both servers:
```bash
npm start
```

#### Option 3: Using Docker

Create a `docker-compose.yml` in the root directory:

```yaml
version: '3.8'

services:
  backend:
    build: ./backend
    command: python manage.py runserver 0.0.0.0:8000
    volumes:
      - ./backend:/code
    ports:
      - "8000:8000"
    environment:
      - DEBUG=True
      - DATABASE_URL=sqlite:///db.sqlite3

  frontend:
    build: ./frontend
    command: npm start
    volumes:
      - ./frontend:/app
    ports:
      - "3000:3000"
    depends_on:
      - backend
```

Run with Docker:
```bash
docker-compose up
```

---

## Troubleshooting

### Common Issues and Solutions

#### Backend Issues

**1. ModuleNotFoundError: No module named 'django'**
```bash
# Ensure virtual environment is activated
pip install -r requirements.txt
```

**2. Database migration errors**
```bash
# Reset migrations (CAUTION: Use only in development)
python manage.py migrate app_name zero
python manage.py migrate
```

**3. Port 8000 already in use**
```bash
# Run on a different port
python manage.py runserver 8001
```

**4. CORS errors**
- Ensure `CORS_ALLOWED_ORIGINS` in `.env` includes your frontend URL
- Install django-cors-headers: `pip install django-cors-headers`

#### Frontend Issues

**1. Dependencies not installing**
```bash
# Clear npm cache and reinstall
rm -rf node_modules package-lock.json
npm install
```

**2. Port 3000 already in use (macOS/Linux)**
```bash
# Run on a different port
PORT=3001 npm start
```

**3. API connection errors**
- Check that backend is running on the correct port
- Verify `REACT_APP_API_BASE_URL` in `.env` file
- Check browser console for detailed error messages

**4. Environment variables not loading**
- Restart the development server after changing `.env`
- Ensure variable names start with `REACT_APP_` for React apps

### Checking Services Status

```bash
# Check if backend is running
curl http://localhost:8000/api/

# Check if frontend is running
curl http://localhost:3000/
```

---

## Development Workflow

### Backend Development Tips

- Use Django shell for testing: `python manage.py shell`
- Create test files: `python manage.py test app_name`
- Use Django admin panel: `http://localhost:8000/admin/`
- Monitor logs with: `python -m django.core.management.call_command`

### Frontend Development Tips

- Use React DevTools browser extension
- Enable source maps in development for better debugging
- Use `console.log()` or debugger statements
- Check Network tab in browser DevTools for API calls

### Code Formatting (Optional but Recommended)

**Backend - Black & Flake8:**
```bash
pip install black flake8
black backend/
flake8 backend/
```

**Frontend - Prettier & ESLint:**
```bash
npm install -D prettier eslint
npx prettier --write src/
npx eslint src/
```

---

## Additional Resources

### Documentation
- [Django Documentation](https://docs.djangoproject.com/)
- [React Documentation](https://react.dev/)
- [Django REST Framework](https://www.django-rest-framework.org/)

### Learning Resources
- [Django for Beginners](https://djangoforbeginners.com/)
- [React Tutorial](https://react.dev/learn)
- [REST API Best Practices](https://restfulapi.net/)

### Useful Tools
- [Postman API Testing](https://www.postman.com/)
- [Django Debug Toolbar](https://django-debug-toolbar.readthedocs.io/)
- [React Query DevTools](https://tanstack.com/query/latest/docs/react/devtools)

---

## Getting Help

If you encounter any issues:

1. Check this SETUP.md file for common solutions
2. Review the project's issue tracker on GitHub
3. Consult the official documentation for Django and React
4. Create a new GitHub issue with detailed error information

---

**Last Updated:** December 31, 2025

For questions or contributions, please open an issue or pull request on the repository.
