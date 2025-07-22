# 📦 Deploy Directory

Thư mục này chứa tất cả file cấu hình để deploy dự án lên staging environment.

## 📁 Structure

```
deploy/
├── docker-compose.staging.yml  # Docker orchestration file
├── nginx/                     # Nginx configurations (có comments chi tiết)
│   ├── nginx.conf            # Main nginx config
│   └── conf.d/               # Server configurations
│       ├── default.conf      # Server blocks
│       └── locations.conf    # URL routing rules
└── README.md                 # File này
```

## 🚀 Manual Deploy

```bash
# 1. Copy environment template
cp ../staging.env.template ../.env.staging

# 2. Edit environment file với thông tin thật
nano ../.env.staging
# Thay YOUR_VPS_IP bằng IP thật của VPS
# Điền cloud database URLs, JWT secret, API keys

# 3. Load environment variables
export $(grep -v '^#' ../.env.staging | xargs)

# 4. Build và start services
docker-compose -f docker-compose.staging.yml build
docker-compose -f docker-compose.staging.yml up -d

# 5. Check status
docker-compose -f docker-compose.staging.yml ps
```

## 📝 Notes

- Tất cả file trong đây được version control bằng Git
- File `.env.staging` nằm ở root directory và KHÔNG được commit
- Nginx configs có comments chi tiết để customize
- Deploy manual để học cách hoạt động của từng step
- Hỗ trợ cả IP VPS và domain name

## 🔧 Manual Commands

```bash
# Start services
docker-compose -f docker-compose.staging.yml up -d

# View logs
docker-compose -f docker-compose.staging.yml logs -f

# Stop services
docker-compose -f docker-compose.staging.yml down
```
