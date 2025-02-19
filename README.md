# ALX Travel App

## Project Overview
ALX Travel App is a Django-based web application designed for travel listings. This guide will help you set up the project, install dependencies, configure the database, and integrate Swagger for API documentation.

---

## 1. Project Initialization
### Create Django Project & App
```bash
# Navigate to your workspace
cd ~/Documents/Alx_pro_dev/

# create env
python -m venv env
source env/bin/activate  # On macOS/Linux
env\Scripts\activate  # On Windows

# deactivate env
deactivate

# Create the project
django-admin startproject alx_travel_app

# Create the project in the current dir
django-admin startproject alx_travel_app .

# Move into the project directory
cd alx_travel_app

# Create an app named listings
python manage.py startapp listings
```

---

## 2. Install Required Dependencies
```bash
pip install django djangorestframework django-cors-headers mysqlclient drf-yasg django-environ celery
```
💡 **Ensure you have RabbitMQ installed for Celery**  
If not installed, use:
```bash
sudo apt install rabbitmq-server
```
Start RabbitMQ:
```bash
sudo systemctl enable rabbitmq-server
sudo systemctl start rabbitmq-server
```

---

## 3. Configure `settings.py`
### Add Installed Apps
Edit `alx_travel_app/settings.py`:
```python
INSTALLED_APPS = [
    "django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",
    "rest_framework",
    "corsheaders",
    "drf_yasg",
    "listings",
]
```

### Configure CORS
```python
MIDDLEWARE = [
    "corsheaders.middleware.CorsMiddleware",
    "django.middleware.security.SecurityMiddleware",
    "django.contrib.sessions.middleware.SessionMiddleware",
    "django.middleware.common.CommonMiddleware",
    "django.middleware.csrf.CsrfViewMiddleware",
    "django.contrib.auth.middleware.AuthenticationMiddleware",
    "django.contrib.messages.middleware.MessageMiddleware",
    "django.middleware.clickjacking.XFrameOptionsMiddleware",
]

CORS_ALLOW_ALL_ORIGINS = True  # Adjust this for production
```

### Set Up MySQL Database Using Environment Variables
#### 1. Create `.env` file
```bash
touch .env
```
#### 2. Add Database Credentials to `.env`
```ini
DB_NAME=alx_travel_db
DB_USER=root
DB_PASSWORD=mysecurepassword
DB_HOST=127.0.0.1
DB_PORT=3306
```
#### 3. Modify `settings.py` to Use `django-environ`
```python
import environ

env = environ.Env()
environ.Env.read_env()

DATABASES = {
    "default": {
        "ENGINE": "django.db.backends.mysql",
        "NAME": env("DB_NAME"),
        "USER": env("DB_USER"),
        "PASSWORD": env("DB_PASSWORD"),
        "HOST": env("DB_HOST"),
        "PORT": env("DB_PORT"),
    }
}
```

---

## 4. Configure Swagger
### Modify `urls.py`
Edit `alx_travel_app/urls.py`:
```python
from django.contrib import admin
from django.urls import path, re_path
from rest_framework import permissions
from drf_yasg.views import get_schema_view
from drf_yasg import openapi

schema_view = get_schema_view(
    openapi.Info(
        title="ALX Travel API",
        default_version="v1",
        description="API documentation for ALX Travel App",
    ),
    public=True,
    permission_classes=[permissions.AllowAny],
)

urlpatterns = [
    path("admin/", admin.site.urls),
    re_path(r"^swagger/$", schema_view.with_ui("swagger", cache_timeout=0), name="swagger-ui"),
]
```
Swagger documentation will be available at **`/swagger/`**.

---

## 5. Initialize Git and Push to GitHub
### Initialize Git Repository
```bash
git init
git add .
git commit -m "Initial project setup"
```

### Create `requirements.txt`
```bash
pip freeze > requirements.txt
```

### Push to GitHub
1. **Create a GitHub repository named `alx_travel_app`**  
2. **Link your local project to GitHub**
```bash
git remote add origin https://github.com/YOUR_USERNAME/alx_travel_app.git
git branch -M main
git push -u origin main
```

---

## 6. Run Migrations and Start the Server
```bash
python manage.py migrate
python manage.py runserver
```
Access API documentation at [http://127.0.0.1:8000/swagger/](http://127.0.0.1:8000/swagger/).

---

🎉 **Done!** Now your Django project is fully set up with MySQL, Swagger, and version control! 🚀

