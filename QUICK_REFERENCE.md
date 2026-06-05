# Quick Reference Guide

## 🚀 Start Development in 30 Seconds

```bash
docker-compose up -d
# Done! Access at http://localhost:3000
```

## 🔑 Default Credentials

**Demo User**
- Email: `john@example.com`
- Password: `john123456`

**Admin User**
- Email: `admin@example.com`
- Password: `admin123456`

## 🌐 Access Points

| Service | URL | Purpose |
|---------|-----|---------|
| Frontend | http://localhost:3000 | Web application |
| Backend API | http://localhost:8000 | REST API |
| API Docs | http://localhost:8000/docs | Interactive API |
| ReDoc | http://localhost:8000/redoc | API documentation |
| Database | localhost:5432 | PostgreSQL |
| Cache | localhost:6379 | Redis |

## 📋 Common Commands

### Docker Compose
```bash
# Start all services
docker-compose up -d

# Stop all services
docker-compose down

# View logs
docker-compose logs -f

# Restart specific service
docker-compose restart backend
```

### Backend
```bash
# Start backend
cd backend && python -m uvicorn app.main:app --reload

# Run tests
pytest tests/ -v

# Run migrations
alembic upgrade head

# Seed database
python -m app.scripts.seed

# Generate new migration
alembic revision --autogenerate -m "Add new column"
```

### Frontend
```bash
# Start frontend
cd frontend && npm run dev

# Build for production
npm run build

# Run tests
npm test

# Check linting
npm run lint
```

### Database
```bash
# Connect to database
docker-compose exec postgres psql -U buildsmart -d buildsmart_db

# Dump database
docker-compose exec postgres pg_dump -U buildsmart buildsmart_db > backup.sql

# Restore from backup
docker-compose exec postgres psql -U buildsmart buildsmart_db < backup.sql
```

## 📝 API Examples

### Register User
```bash
curl -X POST http://localhost:8000/api/v1/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "name": "John Doe",
    "email": "john@example.com",
    "password": "password123"
  }'
```

### Login
```bash
curl -X POST http://localhost:8000/api/v1/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "john@example.com",
    "password": "password123"
  }'

# Save token
TOKEN="your_token_here"
```

### Create Project
```bash
curl -X POST http://localhost:8000/api/v1/projects \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "project_name": "My Building",
    "country": "USA",
    "state": "NY",
    "city": "New York",
    "postal_code": "10001",
    "locality": "Manhattan",
    "plot_area": 5000,
    "construction_area": 3000,
    "building_type": "residential",
    "floors": 2,
    "material_type": "concrete",
    "design_style": "modern"
  }'
```

### Predict Cost
```bash
curl -X POST http://localhost:8000/api/v1/cost/estimate \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "project_id": 1,
    "plot_area": 5000,
    "construction_area": 3000,
    "building_type": "residential",
    "floors": 2,
    "material_type": "concrete",
    "city": "New York"
  }'
```

## 🗂️ File Structure Quick Reference

```
buildsmart-ai/
├── backend/                    # Python FastAPI
├── frontend/                   # Next.js React app
├── devops/                     # Docker & Kubernetes
├── ml-models/                  # ML training scripts
├── README.md                   # Main documentation
├── INSTALLATION.md             # Setup guide
├── ARCHITECTURE.md             # System design
├── DEPLOYMENT.md               # Deployment guide
├── API_REFERENCE.md            # API docs
├── TESTING_GUIDE.md            # Testing manual
├── CONTRIBUTING.md             # Contributing guide
├── PROJECT_SUMMARY.md          # Complete overview
└── docker-compose.yml          # Local development
```

## 🔍 Troubleshooting Quick Fixes

### Port Already in Use
```bash
# Find and kill process
lsof -i :8000
kill -9 <PID>
```

### Database Connection Error
```bash
# Restart database
docker-compose restart postgres

# Reset database
docker-compose exec postgres psql -U buildsmart -c "DROP DATABASE buildsmart_db;"
docker-compose exec postgres psql -U buildsmart -c "CREATE DATABASE buildsmart_db;"
```

### Frontend Not Loading
```bash
# Clear next cache
rm -rf frontend/.next

# Reinstall dependencies
cd frontend && npm install

# Start again
npm run dev
```

### API Docs Not Loading
```bash
# Check backend is running
curl http://localhost:8000/health

# Check CORS settings
# Verify BACKEND_CORS_ORIGINS in backend/.env
```

## 📊 Key Endpoints Summary

| Method | Endpoint | Purpose |
|--------|----------|---------|
| POST | `/api/v1/auth/register` | Register user |
| POST | `/api/v1/auth/login` | Login user |
| GET | `/api/v1/projects` | List projects |
| POST | `/api/v1/projects` | Create project |
| POST | `/api/v1/cost/estimate` | Predict cost |
| POST | `/api/v1/design/generate` | Generate design |
| POST | `/api/v1/boq/generate` | Generate BOQ |
| POST | `/api/v1/timeline/predict` | Predict timeline |
| POST | `/api/v1/risk/analyze` | Analyze risks |
| POST | `/api/v1/chat/ask` | Chat with AI |
| GET | `/api/v1/projects/{id}/report/comprehensive` | Get report |
| GET | `/api/v1/admin/statistics` | Admin stats |

## 🧪 Running Tests

```bash
# Backend tests
cd backend
pytest tests/ -v

# Frontend tests
cd frontend
npm test

# Both with coverage
pytest tests/ --cov=app
npm test -- --coverage
```

## 🚀 Deployment Checklist

Before deploying to production:

- [ ] Update version numbers
- [ ] Run full test suite
- [ ] Build Docker images
- [ ] Test Docker images locally
- [ ] Update CHANGELOG.md
- [ ] Create git tag
- [ ] Deploy to staging
- [ ] Run smoke tests
- [ ] Deploy to production
- [ ] Monitor logs and metrics

## 📞 Getting Help

1. **Check Documentation**: See README.md and other .md files
2. **Review API Docs**: http://localhost:8000/docs
3. **Search Issues**: GitHub Issues
4. **Ask Questions**: GitHub Discussions
5. **Contact Team**: support@buildsmart.ai

## 🎯 Next Steps

1. **Setup**: Follow INSTALLATION.md
2. **Explore**: Try the API at /docs
3. **Develop**: Read CONTRIBUTING.md
4. **Test**: Follow TESTING_GUIDE.md
5. **Deploy**: Follow DEPLOYMENT.md

---

**Happy coding!** 🎉

Need more help? Check the full documentation files!
