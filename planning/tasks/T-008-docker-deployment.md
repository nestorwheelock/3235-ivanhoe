# T-008: Docker Deployment Configuration

**Task Type**: Implementation
**Priority**: High
**Estimate**: 3 hours
**Sprint**: Sprint 1
**Status**: 📋 PENDING
**User Story**: Foundation

---

## Description

Create production-ready Docker configuration for deployment to Vultr with Nginx reverse proxy, Let's Encrypt SSL, and proper volume management.

## Deliverables

- [ ] Production Dockerfile
- [ ] Production docker-compose.yml
- [ ] Nginx configuration with SSL
- [ ] Let's Encrypt certbot configuration
- [ ] Environment variable configuration
- [ ] Volume configuration for media/static
- [ ] Database backup script
- [ ] Deployment documentation
- [ ] Health check endpoint
- [ ] Logging configuration
- [ ] Deployment testing

## Acceptance Criteria

- Docker images build successfully
- Containers start and run stably
- Nginx serves static/media files
- SSL certificate obtains and auto-renews
- Domain points to server correctly
- Site accessible at https://3235ivanhoe.com
- Database persists across restarts
- Media uploads persist across restarts
- Logs accessible for debugging
- Deployment documented

## Technical Notes

**Production docker-compose.yml:**
```yaml
version: '3.8'

services:
  web:
    build: .
    command: gunicorn --bind 0.0.0.0:8000 ivanhoe.wsgi:application
    volumes:
      - media_files:/app/media
      - db_data:/app/data
    environment:
      - DEBUG=False
      - SECRET_KEY=${SECRET_KEY}
      - ALLOWED_HOSTS=3235ivanhoe.com,www.3235ivanhoe.com
    expose:
      - "8000"
    restart: unless-stopped

  nginx:
    image: nginx:alpine
    volumes:
      - ./docker/nginx/nginx.conf:/etc/nginx/nginx.conf
      - ./docker/nginx/ssl:/etc/nginx/ssl
      - media_files:/app/media
      - static_files:/app/static
      - certbot_webroot:/var/www/certbot
      - certbot_certs:/etc/letsencrypt
    ports:
      - "80:80"
      - "443:443"
    depends_on:
      - web
    restart: unless-stopped

  certbot:
    image: certbot/certbot
    volumes:
      - certbot_webroot:/var/www/certbot
      - certbot_certs:/etc/letsencrypt
    command: certonly --webroot --webroot-path=/var/www/certbot --email your@email.com --agree-tos --no-eff-email -d 3235ivanhoe.com -d www.3235ivanhoe.com

volumes:
  media_files:
  static_files:
  db_data:
  certbot_webroot:
  certbot_certs:
```

**Production Nginx Configuration:**
```nginx
upstream django {
    server web:8000;
}

server {
    listen 80;
    server_name 3235ivanhoe.com www.3235ivanhoe.com;

    location /.well-known/acme-challenge/ {
        root /var/www/certbot;
    }

    location / {
        return 301 https://$host$request_uri;
    }
}

server {
    listen 443 ssl http2;
    server_name 3235ivanhoe.com www.3235ivanhoe.com;

    ssl_certificate /etc/letsencrypt/live/3235ivanhoe.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/3235ivanhoe.com/privkey.pem;

    client_max_body_size 20M;

    location /static/ {
        alias /app/static/;
    }

    location /media/ {
        alias /app/media/;
    }

    location / {
        proxy_pass http://django;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

**Production Environment Variables:**
```bash
# .env.production
SECRET_KEY=<generate-strong-secret-key>
DEBUG=False
ALLOWED_HOSTS=3235ivanhoe.com,www.3235ivanhoe.com
DATABASE_PATH=/app/data/db.sqlite3
```

**Database Backup Script:**
```bash
#!/bin/bash
# backup-db.sh
BACKUP_DIR="/backups"
TIMESTAMP=$(date +%Y%m%d_%H%M%S)
docker cp 3235-ivanhoe_web_1:/app/data/db.sqlite3 $BACKUP_DIR/db_$TIMESTAMP.sqlite3
find $BACKUP_DIR -name "db_*.sqlite3" -mtime +7 -delete
```

**Health Check Endpoint:**
```python
# views.py
def health_check(request):
    return JsonResponse({'status': 'healthy'})

# urls.py
path('health/', views.health_check, name='health'),
```

**Deployment Steps Documentation:**

1. **Vultr VPS Setup:**
   - Create VPS (Ubuntu 22.04)
   - Install Docker & Docker Compose
   - Configure firewall (ports 80, 443)

2. **Domain Configuration:**
   - Point 3235ivanhoe.com A record to VPS IP
   - Point www.3235ivanhoe.com CNAME to 3235ivanhoe.com

3. **Deploy Application:**
   ```bash
   git clone <repo>
   cd 3235-ivanhoe
   cp .env.example .env.production
   # Edit .env.production with SECRET_KEY
   docker-compose -f docker-compose.prod.yml up -d
   ```

4. **Obtain SSL Certificate:**
   ```bash
   docker-compose -f docker-compose.prod.yml run --rm certbot
   docker-compose -f docker-compose.prod.yml restart nginx
   ```

5. **Create Superuser:**
   ```bash
   docker-compose -f docker-compose.prod.yml exec web python manage.py createsuperuser
   ```

6. **Setup Cron for SSL Renewal:**
   ```bash
   0 3 * * 0 docker-compose -f /path/to/docker-compose.prod.yml run --rm certbot renew
   5 3 * * 0 docker-compose -f /path/to/docker-compose.prod.yml restart nginx
   ```

---

## Testing

- [ ] Test Docker build locally
- [ ] Test production compose file
- [ ] Test Nginx configuration
- [ ] Test SSL certificate issuance
- [ ] Test volume persistence
- [ ] Test health check endpoint
- [ ] Test deployment on clean server
- [ ] Test backup script

---

## Dependencies

- Docker installed on Vultr VPS
- Domain DNS configured
- Application code complete (T-001 through T-007)

---

**Closes #TBD**
