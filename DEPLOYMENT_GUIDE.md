# 🚀 Deployment Guide

## 📋 Overview

This guide covers various deployment options for the ERP CRM system, from local development to production environments.

## 🏠 Local Development

### Prerequisites
- Node.js 18+
- MongoDB 6+
- Git

### Quick Start
```bash
# Clone the repository
git clone https://github.com/PremPMehta/ERP-CRM-Accounting-Invoicing-Software.git
cd ERP-CRM-Accounting-Invoicing-Software

# Install dependencies
cd backend && npm install
cd ../frontend && npm install

# Start development servers
cd ../backend && npm run dev
cd ../frontend && npm run dev
```

### Environment Setup
```bash
# Backend environment
cp backend/.env.example backend/.env
# Edit backend/.env with your configuration

# Frontend environment
cp frontend/.env.example frontend/.env
# Edit frontend/.env with your configuration
```

## 🐳 Docker Deployment

### Using Docker Compose

Create `docker-compose.yml`:
```yaml
version: '3.8'

services:
  mongodb:
    image: mongo:6.0
    container_name: erp-crm-mongodb
    restart: unless-stopped
    environment:
      MONGO_INITDB_ROOT_USERNAME: admin
      MONGO_INITDB_ROOT_PASSWORD: password
    ports:
      - "27017:27017"
    volumes:
      - mongodb_data:/data/db

  backend:
    build:
      context: ./backend
      dockerfile: Dockerfile
    container_name: erp-crm-backend
    restart: unless-stopped
    environment:
      - NODE_ENV=production
      - MONGODB_URI=mongodb://admin:password@mongodb:27017/erp-crm?authSource=admin
      - JWT_SECRET=your-secret-key
      - PORT=5000
    ports:
      - "5000:5000"
    depends_on:
      - mongodb

  frontend:
    build:
      context: ./frontend
      dockerfile: Dockerfile
    container_name: erp-crm-frontend
    restart: unless-stopped
    environment:
      - VITE_API_URL=http://localhost:5000/api
    ports:
      - "3000:3000"
    depends_on:
      - backend

volumes:
  mongodb_data:
```

### Backend Dockerfile
```dockerfile
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./
RUN npm ci --only=production

COPY . .

EXPOSE 5000

CMD ["npm", "start"]
```

### Frontend Dockerfile
```dockerfile
FROM node:18-alpine as build

WORKDIR /app

COPY package*.json ./
RUN npm ci

COPY . .
RUN npm run build

FROM nginx:alpine
COPY --from=build /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/nginx.conf

EXPOSE 3000

CMD ["nginx", "-g", "daemon off;"]
```

### Deploy with Docker
```bash
# Build and start services
docker-compose up -d

# View logs
docker-compose logs -f

# Stop services
docker-compose down
```

## ☁️ Cloud Deployment

### AWS Deployment

#### Using AWS EC2

1. **Launch EC2 Instance**
```bash
# Connect to your EC2 instance
ssh -i your-key.pem ubuntu@your-ec2-ip

# Update system
sudo apt update && sudo apt upgrade -y

# Install Node.js
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs

# Install MongoDB
wget -qO - https://www.mongodb.org/static/pgp/server-6.0.asc | sudo apt-key add -
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list
sudo apt-get update
sudo apt-get install -y mongodb-org
sudo systemctl start mongod
sudo systemctl enable mongod
```

2. **Deploy Application**
```bash
# Clone repository
git clone https://github.com/PremPMehta/ERP-CRM-Accounting-Invoicing-Software.git
cd ERP-CRM-Accounting-Invoicing-Software

# Install dependencies
cd backend && npm install
cd ../frontend && npm install

# Build frontend
cd ../frontend && npm run build

# Configure environment
cp backend/.env.example backend/.env
# Edit backend/.env with production settings
```

3. **Setup PM2 for Process Management**
```bash
# Install PM2
sudo npm install -g pm2

# Start backend
cd backend
pm2 start npm --name "erp-crm-backend" -- start

# Setup PM2 to start on boot
pm2 startup
pm2 save
```

4. **Setup Nginx**
```bash
# Install Nginx
sudo apt install nginx

# Configure Nginx
sudo nano /etc/nginx/sites-available/erp-crm
```

Nginx configuration:
```nginx
server {
    listen 80;
    server_name your-domain.com;

    # Frontend
    location / {
        root /home/ubuntu/ERP-CRM-Accounting-Invoicing-Software/frontend/dist;
        try_files $uri $uri/ /index.html;
    }

    # Backend API
    location /api {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

```bash
# Enable site
sudo ln -s /etc/nginx/sites-available/erp-crm /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```

### DigitalOcean Deployment

#### Using DigitalOcean App Platform

1. **Create App**
   - Go to DigitalOcean App Platform
   - Connect your GitHub repository
   - Select the repository

2. **Configure Environment**
```yaml
# .do/app.yaml
name: erp-crm
services:
  - name: backend
    source_dir: /backend
    github:
      repo: PremPMehta/ERP-CRM-Accounting-Invoicing-Software
      branch: main
    environment_slug: node-js
    instance_count: 1
    instance_size_slug: basic-xxs
    envs:
      - key: NODE_ENV
        value: production
      - key: MONGODB_URI
        value: ${mongodb.DATABASE_URL}
      - key: JWT_SECRET
        value: your-secret-key

  - name: frontend
    source_dir: /frontend
    github:
      repo: PremPMehta/ERP-CRM-Accounting-Invoicing-Software
      branch: main
    environment_slug: node-js
    instance_count: 1
    instance_size_slug: basic-xxs
    build_command: npm run build
    run_command: npm run preview
    envs:
      - key: VITE_API_URL
        value: ${backend.URL}/api

databases:
  - name: mongodb
    engine: MONGODB
    version: "6.0"
```

### Heroku Deployment

#### Backend Deployment
```bash
# Install Heroku CLI
curl https://cli-assets.heroku.com/install.sh | sh

# Login to Heroku
heroku login

# Create app
heroku create your-erp-crm-backend

# Add MongoDB addon
heroku addons:create mongolab:sandbox

# Set environment variables
heroku config:set NODE_ENV=production
heroku config:set JWT_SECRET=your-secret-key

# Deploy
git push heroku main
```

#### Frontend Deployment
```bash
# Create frontend app
heroku create your-erp-crm-frontend

# Set buildpack
heroku buildpacks:set mars/create-react-app

# Set environment variables
heroku config:set VITE_API_URL=https://your-erp-crm-backend.herokuapp.com/api

# Deploy
git push heroku main
```

## 🔧 Environment Configuration

### Production Environment Variables

#### Backend (.env)
```env
NODE_ENV=production
PORT=5000
MONGODB_URI=mongodb://username:password@host:port/database
JWT_SECRET=your-super-secret-jwt-key
JWT_EXPIRES_IN=7d
CORS_ORIGIN=https://your-domain.com
RATE_LIMIT_WINDOW_MS=900000
RATE_LIMIT_MAX=1000
```

#### Frontend (.env)
```env
VITE_API_URL=https://your-api-domain.com/api
VITE_APP_NAME=ERP CRM
VITE_APP_VERSION=1.0.0
```

## 🔒 Security Configuration

### SSL/HTTPS Setup

#### Using Let's Encrypt
```bash
# Install Certbot
sudo apt install certbot python3-certbot-nginx

# Get SSL certificate
sudo certbot --nginx -d your-domain.com

# Auto-renewal
sudo crontab -e
# Add: 0 12 * * * /usr/bin/certbot renew --quiet
```

#### Using Cloudflare
1. Add your domain to Cloudflare
2. Update nameservers
3. Enable SSL/TLS encryption mode to "Full"
4. Enable "Always Use HTTPS"

### Security Headers
```nginx
# Add to Nginx configuration
add_header X-Frame-Options "SAMEORIGIN" always;
add_header X-XSS-Protection "1; mode=block" always;
add_header X-Content-Type-Options "nosniff" always;
add_header Referrer-Policy "no-referrer-when-downgrade" always;
add_header Content-Security-Policy "default-src 'self' http: https: data: blob: 'unsafe-inline'" always;
```

## 📊 Monitoring & Logging

### Application Monitoring

#### Using PM2
```bash
# Monitor processes
pm2 monit

# View logs
pm2 logs

# Restart application
pm2 restart all
```

#### Using New Relic
```bash
# Install New Relic
npm install newrelic

# Add to your main server file
require('newrelic');
```

### Database Monitoring

#### MongoDB Atlas
- Enable MongoDB Atlas monitoring
- Set up alerts for performance issues
- Monitor connection pool usage

## 🔄 CI/CD Pipeline

### GitHub Actions

Create `.github/workflows/deploy.yml`:
```yaml
name: Deploy to Production

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Setup Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18'
        
    - name: Install dependencies
      run: |
        cd backend && npm install
        cd ../frontend && npm install
        
    - name: Run tests
      run: |
        cd backend && npm test
        cd ../frontend && npm test
        
    - name: Build frontend
      run: cd frontend && npm run build
      
    - name: Deploy to server
      uses: appleboy/ssh-action@v0.1.5
      with:
        host: ${{ secrets.HOST }}
        username: ${{ secrets.USERNAME }}
        key: ${{ secrets.KEY }}
        script: |
          cd /path/to/your/app
          git pull origin main
          cd backend && npm install
          cd ../frontend && npm install && npm run build
          pm2 restart all
```

## 🚨 Troubleshooting

### Common Issues

#### Port Already in Use
```bash
# Find process using port
sudo lsof -i :5000

# Kill process
sudo kill -9 PID
```

#### MongoDB Connection Issues
```bash
# Check MongoDB status
sudo systemctl status mongod

# Restart MongoDB
sudo systemctl restart mongod

# Check MongoDB logs
sudo tail -f /var/log/mongodb/mongod.log
```

#### Memory Issues
```bash
# Check memory usage
free -h

# Increase Node.js memory limit
export NODE_OPTIONS="--max-old-space-size=4096"
```

### Performance Optimization

#### Database Optimization
```javascript
// Add indexes to MongoDB collections
db.customers.createIndex({ "email": 1 })
db.invoices.createIndex({ "customerId": 1, "status": 1 })
db.payments.createIndex({ "invoiceId": 1, "createdAt": -1 })
```

#### Application Optimization
```javascript
// Enable compression
app.use(compression());

// Enable caching
app.use(express.static('public', { maxAge: '1d' }));
```

---

*For more detailed deployment instructions, please refer to the specific cloud provider documentation.*

---

<div align="center">
  <p><strong>Built upon and refined with ❤️ by Prem Mehta</strong></p>
</div> 