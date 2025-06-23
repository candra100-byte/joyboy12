# Panduan Deployment

Panduan lengkap untuk deployment Sistem Monitoring Imunisasi Lombok Tengah.

## Prasyarat

### System Requirements
- Node.js 18.x atau lebih baru
- PostgreSQL 12.x atau lebih baru
- RAM minimal 2GB
- Storage minimal 10GB

### Environment Variables
```bash
# Database Configuration
DATABASE_URL=postgresql://username:password@localhost:5432/immunization_db
PGHOST=localhost
PGPORT=5432
PGUSER=postgres
PGPASSWORD=your_password
PGDATABASE=immunization_db

# Session Configuration
SESSION_SECRET=your-super-secret-session-key-here-change-this-in-production

# Replit OAuth Configuration
REPL_ID=your-replit-app-id
ISSUER_URL=https://replit.com/oidc
REPLIT_DOMAINS=your-domain.replit.app

# Production Configuration
NODE_ENV=production
PORT=5000
```

## Deployment Options

### 1. Replit Deployment (Recommended)

#### Setup Replit
1. Fork repository ke Replit
2. Konfigurasi environment variables di Replit Secrets
3. Aktifkan PostgreSQL database
4. Jalankan aplikasi

#### Konfigurasi Secrets di Replit
```
DATABASE_URL: [auto-generated]
SESSION_SECRET: [generate random string]
REPL_ID: [auto-generated]
ISSUER_URL: https://replit.com/oidc
REPLIT_DOMAINS: [your-repl-domain].replit.app
```

### 2. VPS/Cloud Server Deployment

#### Setup Server
```bash
# Update system
sudo apt update && sudo apt upgrade -y

# Install Node.js
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs

# Install PostgreSQL
sudo apt install postgresql postgresql-contrib

# Install PM2 untuk process management
sudo npm install -g pm2
```

#### Setup Database
```bash
# Login ke PostgreSQL
sudo -u postgres psql

# Buat database dan user
CREATE DATABASE immunization_db;
CREATE USER immunization_user WITH PASSWORD 'your_password';
GRANT ALL PRIVILEGES ON DATABASE immunization_db TO immunization_user;
\q
```

#### Deploy Application
```bash
# Clone repository
git clone https://github.com/your-username/immunization-monitoring-system.git
cd immunization-monitoring-system

# Install dependencies
npm install

# Setup environment variables
cp .env.example .env
# Edit .env dengan konfigurasi yang sesuai

# Build aplikasi
npm run build

# Setup database
npm run db:push

# Start dengan PM2
pm2 start dist/index.js --name immunization-app
pm2 startup
pm2 save
```

### 3. Docker Deployment

#### Dockerfile
```dockerfile
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./
RUN npm ci --only=production

COPY . .
RUN npm run build

EXPOSE 5000

CMD ["npm", "start"]
```

#### docker-compose.yml
```yaml
version: '3.8'

services:
  app:
    build: .
    ports:
      - "5000:5000"
    environment:
      - NODE_ENV=production
      - DATABASE_URL=postgresql://postgres:password@db:5432/immunization_db
      - SESSION_SECRET=your-session-secret
    depends_on:
      - db

  db:
    image: postgres:15
    environment:
      - POSTGRES_DB=immunization_db
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=password
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"

volumes:
  postgres_data:
```

#### Deploy dengan Docker Compose
```bash
# Build dan start services
docker-compose up -d

# Setup database
docker-compose exec app npm run db:push
```

### 4. Heroku Deployment

#### Setup Heroku
```bash
# Install Heroku CLI
# Login ke Heroku
heroku login

# Create app
heroku create immunization-app

# Add PostgreSQL addon
heroku addons:create heroku-postgresql:hobby-dev

# Set environment variables
heroku config:set NODE_ENV=production
heroku config:set SESSION_SECRET=your-session-secret
heroku config:set REPL_ID=your-repl-id
heroku config:set ISSUER_URL=https://replit.com/oidc
heroku config:set REPLIT_DOMAINS=immunization-app.herokuapp.com

# Deploy
git push heroku main

# Setup database
heroku run npm run db:push
```

## Nginx Configuration

### Reverse Proxy Setup
```nginx
server {
    listen 80;
    server_name your-domain.com;
    
    location / {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_cache_bypass $http_upgrade;
    }
}
```

### SSL Configuration dengan Let's Encrypt
```bash
# Install Certbot
sudo apt install certbot python3-certbot-nginx

# Obtain SSL certificate
sudo certbot --nginx -d your-domain.com

# Auto-renewal
sudo crontab -e
# Add line: 0 12 * * * /usr/bin/certbot renew --quiet
```

## Database Migration

### Production Migration
```bash
# Backup database sebelum migration
pg_dump -U username -h localhost immunization_db > backup.sql

# Run migration
npm run db:push

# Jika ada masalah, restore backup
psql -U username -h localhost immunization_db < backup.sql
```

## Monitoring dan Logging

### PM2 Monitoring
```bash
# Monitor aplikasi
pm2 monit

# View logs
pm2 logs immunization-app

# Restart aplikasi
pm2 restart immunization-app
```

### Log Rotation
```bash
# Setup log rotation
pm2 install pm2-logrotate

# Configure log rotation
pm2 set pm2-logrotate:max_size 10M
pm2 set pm2-logrotate:retain 30
pm2 set pm2-logrotate:compress true
```

## Performance Optimization

### Database Optimization
```sql
-- Create indexes untuk performance
CREATE INDEX idx_patients_search ON patients USING gin(to_tsvector('indonesian', name || ' ' || nik));
CREATE INDEX idx_immunizations_patient ON immunizations(patient_id);
CREATE INDEX idx_schedules_date ON schedules(scheduled_date);
CREATE INDEX idx_notifications_status ON notifications(status);
```

### Application Optimization
```bash
# Optimize Node.js
export NODE_OPTIONS="--max-old-space-size=2048"

# Enable gzip compression
# Add to nginx config:
gzip on;
gzip_vary on;
gzip_min_length 1024;
gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;
```

## Security Checklist

### Production Security
- [ ] Environment variables aman dan tidak di-commit
- [ ] HTTPS diaktifkan dengan SSL certificate
- [ ] Database user memiliki minimal privileges
- [ ] Firewall dikonfigurasi dengan benar
- [ ] Regular security updates
- [ ] Session secret yang kuat
- [ ] Rate limiting diaktifkan
- [ ] CORS dikonfigurasi dengan benar

### Database Security
```sql
-- Revoke unnecessary permissions
REVOKE ALL ON SCHEMA public FROM PUBLIC;
GRANT USAGE ON SCHEMA public TO immunization_user;
GRANT SELECT, INSERT, UPDATE, DELETE ON ALL TABLES IN SCHEMA public TO immunization_user;
```

## Backup Strategy

### Automated Backup
```bash
#!/bin/bash
# backup.sh

DATE=$(date +%Y%m%d_%H%M%S)
BACKUP_DIR="/backups"
DB_NAME="immunization_db"

# Create backup
pg_dump -U postgres -h localhost $DB_NAME > $BACKUP_DIR/backup_$DATE.sql

# Keep only last 7 days of backups
find $BACKUP_DIR -name "backup_*.sql" -mtime +7 -delete

# Upload to cloud storage (optional)
# aws s3 cp $BACKUP_DIR/backup_$DATE.sql s3://your-backup-bucket/
```

### Cron Job untuk Backup
```bash
# Add to crontab
0 2 * * * /path/to/backup.sh
```

## Troubleshooting

### Common Issues

#### Database Connection Error
```bash
# Check PostgreSQL status
sudo systemctl status postgresql

# Check connections
sudo -u postgres psql -c "SELECT * FROM pg_stat_activity;"

# Restart PostgreSQL
sudo systemctl restart postgresql
```

#### Application Not Starting
```bash
# Check logs
pm2 logs immunization-app

# Check port availability
sudo netstat -tlnp | grep :5000

# Check environment variables
pm2 env immunization-app
```

#### Memory Issues
```bash
# Check memory usage
free -h

# Increase swap space
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
```

## Health Checks

### Application Health
```bash
# Check application status
curl -f http://localhost:5000/health || exit 1

# Check database connectivity
psql -U username -h localhost -d immunization_db -c "SELECT 1;"
```

### Monitoring Script
```bash
#!/bin/bash
# health_check.sh

APP_URL="http://localhost:5000"
HEALTH_ENDPOINT="$APP_URL/health"

if curl -f $HEALTH_ENDPOINT > /dev/null 2>&1; then
    echo "Application is healthy"
else
    echo "Application is unhealthy, restarting..."
    pm2 restart immunization-app
fi
```

## Support

Untuk bantuan deployment:
- Buat issue di GitHub repository
- Konsultasi dengan tim DevOps
- Dokumentasi troubleshooting di wiki