# Docker Staging Configuration Guide

## Quick Commands:
```bash
# Start API
docker-compose -f docker-compose.staging.yml --env-file .env.staging up api -d

# Stop services  
docker-compose -f docker-compose.staging.yml down

# View logs
docker logs -f nemui-api-staging

# Check status
docker ps
```

## Cách sử dụng:

### 1. Chuẩn bị Environment Variables
```bash
# Copy và edit file environment
cp .env.staging.example .env.staging

# Edit file .env.staging và điền các thông tin:
# - Database connection string
# - Redis configuration 
# - JWT secret keys
# - Cloudinary credentials
# - Gemini AI API key
# - VPS IP address
```

### 2. Chạy API service
```bash
# Chỉ chạy API service
docker-compose -f docker-compose.staging.yml --env-file .env.staging up api -d

# Hoặc build lại và chạy
docker-compose -f docker-compose.staging.yml --env-file .env.staging up --build api -d

# Stop services
docker-compose -f docker-compose.staging.yml down

# Remove containers và volumes
docker-compose -f docker-compose.staging.yml down -v
```

### 3. Kiểm tra logs
```bash
# Xem logs của API
docker-compose -f docker-compose.staging.yml logs -f api

# Xem logs realtime
docker logs -f nemui-api-staging

# Check container status
docker-compose -f docker-compose.staging.yml ps
```

### 4. Health Check
- Health check endpoint: `http://localhost:8080`
- Health check interval: 30s
- Start period: 30s (time to wait before first health check)

### 5. Troubleshooting

#### Docker Commands:
```bash
# Stop all services (correct way)
docker-compose -f docker-compose.staging.yml down

# Force rebuild và restart
docker-compose -f docker-compose.staging.yml down
docker-compose -f docker-compose.staging.yml --env-file .env.staging up --build api -d

# Remove all containers, networks, and volumes
docker-compose -f docker-compose.staging.yml down -v --remove-orphans

# Check running containers
docker ps

# Check all containers (including stopped)
docker ps -a

# Remove specific container
docker rm -f nemui-api-staging
```

#### API không start được:
- Kiểm tra environment variables trong `.env.staging`
- Kiểm tra database connection
- Kiểm tra Redis connection
- Xem logs: `docker logs nemui-api-staging`

#### Database connection issues:
- Verify `CLOUD_DATABASE_URL` format
- Check if database server is accessible
- Verify username/password

#### Redis connection issues:
- Verify Redis endpoint, port, username, password
- Check if Redis server allows connections from your IP
- Verify SSL settings

#### JWT issues:
- Ensure `JWT_SECRET_KEY` is at least 32 characters
- Check JWT issuer and audience settings

## File Structure:
```
deploy/
├── docker-compose.staging.yml    # Main compose file
├── staging.env.template          # Environment template
├── .env.staging.example          # Example with some real values
├── .env.staging                  # Your actual environment file (create this)
├── nginx/                        # Nginx configurations
└── logs/                         # Log directories
```

## Important Notes:
1. Never commit `.env.staging` file với production credentials
2. Set `ADMIN_ENABLE_SEED_ENDPOINTS=false` for production
3. Use strong JWT secret keys (generate with: `openssl rand -base64 32`)
4. Ensure Redis and Database are accessible from Docker network
5. Configure proper CORS origins for your frontend domain
