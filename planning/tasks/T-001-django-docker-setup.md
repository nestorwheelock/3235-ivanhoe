# T-001: Django + Docker Setup

**Task Type**: Implementation
**Priority**: High
**Estimate**: 2 hours
**Sprint**: Sprint 1
**Status**: 📋 PENDING
**User Story**: Foundation for all stories

---

## Description

Set up the Django project structure, Docker configuration, and development environment for the 3235 Ivanhoe property platform.

## Deliverables

- [ ] Django project created (`ivanhoe/`)
- [ ] Django app created (`property/`)
- [ ] settings.py configured (database, static, media, security)
- [ ] requirements.txt with all dependencies
- [ ] requirements-dev.txt with dev dependencies
- [ ] Dockerfile for Django app
- [ ] docker-compose.yml for local development
- [ ] nginx configuration for reverse proxy
- [ ] .env.example for environment variables
- [ ] .gitignore configured
- [ ] Media directory configuration
- [ ] Static files configuration
- [ ] Initial migration run successfully
- [ ] Development server runs in Docker

## Acceptance Criteria

- Django project starts successfully
- Docker containers build without errors
- Can access site at http://localhost:8000
- Admin interface accessible at http://localhost:8000/admin
- Static files serve correctly
- Media uploads work correctly
- Database migrations apply successfully

## Technical Notes

**Django Project Structure:**
```
3235-ivanhoe/
├── ivanhoe/                 # Django project
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   ├── wsgi.py
│   └── asgi.py
├── property/                # Django app
│   ├── __init__.py
│   ├── apps.py
│   ├── models.py
│   ├── views.py
│   ├── urls.py
│   ├── forms.py
│   ├── admin.py
│   ├── templates/
│   │   └── property/
│   ├── static/
│   │   └── property/
│   │       ├── css/
│   │       ├── js/
│   │       └── images/
│   ├── migrations/
│   └── tests/
├── docker/
│   ├── Dockerfile
│   ├── docker-compose.yml
│   └── nginx/
│       └── nginx.conf
├── media/                   # User uploads
├── static/                  # Collected static files
├── manage.py
├── requirements.txt
├── requirements-dev.txt
├── .env.example
├── .gitignore
└── README.md
```

**requirements.txt:**
```
Django>=4.2,<5.0
Pillow>=10.0
gunicorn>=21.0
bleach>=6.0
```

**requirements-dev.txt:**
```
pytest>=7.0
pytest-django>=4.5
pytest-cov>=4.0
black>=23.0
flake8>=6.0
```

**Dockerfile:**
```dockerfile
FROM python:3.11-slim

ENV PYTHONUNBUFFERED=1

WORKDIR /app

COPY requirements.txt /app/
RUN pip install --no-cache-dir -r requirements.txt

COPY . /app/

RUN python manage.py collectstatic --noinput

CMD ["gunicorn", "--bind", "0.0.0.0:8000", "ivanhoe.wsgi:application"]
```

**docker-compose.yml:**
```yaml
version: '3.8'

services:
  web:
    build: .
    command: python manage.py runserver 0.0.0.0:8000
    volumes:
      - .:/app
      - media_files:/app/media
    ports:
      - "8000:8000"
    environment:
      - DEBUG=True
      - SECRET_KEY=${SECRET_KEY}
      - ALLOWED_HOSTS=localhost,127.0.0.1

  nginx:
    image: nginx:alpine
    volumes:
      - ./docker/nginx/nginx.conf:/etc/nginx/nginx.conf
      - media_files:/app/media
      - static_files:/app/static
    ports:
      - "80:80"
    depends_on:
      - web

volumes:
  media_files:
  static_files:
```

**settings.py Key Configurations:**

```python
# Security
SECRET_KEY = os.environ.get('SECRET_KEY', 'dev-key-change-in-production')
DEBUG = os.environ.get('DEBUG', 'False') == 'True'
ALLOWED_HOSTS = os.environ.get('ALLOWED_HOSTS', 'localhost').split(',')

# Database (SQLite)
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}

# Static files
STATIC_URL = '/static/'
STATIC_ROOT = BASE_DIR / 'static'

# Media files
MEDIA_URL = '/media/'
MEDIA_ROOT = BASE_DIR / 'media'

# Installed apps
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'property',
]
```

**.gitignore:**
```
*.pyc
__pycache__/
db.sqlite3
/media/
/static/
.env
*.log
.DS_Store
```

**.env.example:**
```
SECRET_KEY=your-secret-key-here
DEBUG=True
ALLOWED_HOSTS=localhost,127.0.0.1,3235ivanhoe.com
```

---

## Testing

- [ ] Django project creates successfully
- [ ] App registers in INSTALLED_APPS
- [ ] Migrations run without errors
- [ ] Admin interface loads
- [ ] Static files serve correctly
- [ ] Media upload directory writable
- [ ] Docker containers build successfully
- [ ] Docker compose up works

---

## Dependencies

- Docker installed
- Docker Compose installed
- Python 3.8+ (for local development)

---

**Closes #TBD** (GitHub issue to be created)
