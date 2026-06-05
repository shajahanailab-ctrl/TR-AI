# Development & Testing Guide

## Local Development Setup

### Prerequisites
- Node.js 20+
- Python 3.12+
- Docker & Docker Compose
- PostgreSQL 15+ (or use Docker)
- Redis 7+ (or use Docker)

### Quick Start with Docker Compose

```bash
# Clone repository
git clone https://github.com/buildsmart/buildsmart-ai.git
cd buildsmart-ai

# Create environment files
cp backend/.env.example backend/.env
cp frontend/.env.example frontend/.env.local

# Start services
docker-compose up -d

# Initialize database
docker-compose exec backend python -m app.scripts.seed

# Application is ready at http://localhost:3000
```

### Manual Development Setup

**Backend Setup:**

```bash
cd backend

# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Create .env file
cp .env.example .env

# Update DATABASE_URL in .env
DATABASE_URL=postgresql://user:password@localhost:5432/buildsmart_db

# Run migrations
alembic upgrade head

# Seed database
python -m app.scripts.seed

# Start server
python -m uvicorn app.main:app --reload
# API available at http://localhost:8000
```

**Frontend Setup:**

```bash
cd frontend

# Install dependencies
npm install

# Create .env.local
cp .env.example .env.local

# Start development server
npm run dev
# Frontend available at http://localhost:3000
```

## Testing

### Unit Tests

**Backend Unit Tests:**

```bash
cd backend

# Run all tests
pytest tests/

# Run with coverage
pytest tests/ --cov=app --cov-report=html

# Run specific test file
pytest tests/test_auth.py -v

# Run specific test
pytest tests/test_auth.py::TestAuthentication::test_login -v
```

**Frontend Unit Tests:**

```bash
cd frontend

# Run all tests
npm test

# Run in watch mode
npm test -- --watch

# Run with coverage
npm test -- --coverage
```

### Integration Tests

```bash
cd backend

# Run integration tests only
pytest tests/test_integration.py -v

# Run with database fixtures
pytest tests/test_integration.py::TestProjects::test_create_project -v
```

### Load Testing

```bash
# Using Apache JMeter
jmeter -n -t load_test_plan.jmx -l results.jtl

# Using pytest-benchmark
pytest tests/ --benchmark-only

# Using k6
k6 run load_test.js
```

## Testing Scenarios

### User Registration & Login

```bash
# 1. Register new user
curl -X POST http://localhost:8000/api/v1/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Test User",
    "email": "test@example.com",
    "password": "testpass123"
  }'

# 2. Login
curl -X POST http://localhost:8000/api/v1/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "test@example.com",
    "password": "testpass123"
  }'

# Response contains access_token - save this for authenticated requests
TOKEN="eyJhbGciOiJIUzI1NiIs..."
```

### Project Management

```bash
# 1. Create project
curl -X POST http://localhost:8000/api/v1/projects \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "project_name": "Test Building",
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

# 2. Get projects
curl http://localhost:8000/api/v1/projects \
  -H "Authorization: Bearer $TOKEN"

# 3. Get specific project
curl http://localhost:8000/api/v1/projects/1 \
  -H "Authorization: Bearer $TOKEN"
```

### Cost Prediction

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

### Chat Assistant

```bash
curl -X POST http://localhost:8000/api/v1/chat/ask \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "question": "How much will a residential building cost?",
    "building_type": "residential",
    "city": "New York"
  }'
```

### Admin Operations

```bash
# Get system statistics (requires admin token)
curl http://localhost:8000/api/v1/admin/statistics \
  -H "Authorization: Bearer $ADMIN_TOKEN"

# Get user statistics
curl http://localhost:8000/api/v1/admin/statistics/users \
  -H "Authorization: Bearer $ADMIN_TOKEN"

# Disable user
curl -X POST http://localhost:8000/api/v1/admin/users/2/disable \
  -H "Authorization: Bearer $ADMIN_TOKEN"

# Get audit log
curl http://localhost:8000/api/v1/admin/audit-log \
  -H "Authorization: Bearer $ADMIN_TOKEN"
```

## Database Testing

### Connect to Database

```bash
# Using Docker
docker-compose exec postgres psql -U buildsmart -d buildsmart_db

# Using psql locally
psql -h localhost -U buildsmart -d buildsmart_db

# List tables
\dt

# Query users
SELECT * FROM users;

# Query projects
SELECT * FROM projects;

# Check database size
SELECT pg_size_pretty(pg_database_size('buildsmart_db'));
```

### Reset Database

```bash
# Drop and recreate database
docker-compose exec postgres psql -U buildsmart -c "DROP DATABASE buildsmart_db;"
docker-compose exec postgres psql -U buildsmart -c "CREATE DATABASE buildsmart_db;"
docker-compose exec backend alembic upgrade head
docker-compose exec backend python -m app.scripts.seed
```

## API Documentation

### Access Swagger UI

- **Backend API Docs**: http://localhost:8000/docs
- **ReDoc**: http://localhost:8000/redoc

### Test Endpoints Interactively

1. Go to http://localhost:8000/docs
2. Click "Authorize" button
3. Paste your JWT token
4. Execute requests directly from UI

## Performance Testing

### Database Query Performance

```bash
# Enable query logging
docker-compose exec postgres psql -U buildsmart -d buildsmart_db

# In psql:
SET log_statement = 'all';
SET log_min_duration_statement = 0;

# Then run queries and check /var/log/postgresql/postgresql.log
```

### API Response Time

```bash
# Using curl with timing
curl -w "\nTotal time: %{time_total}s\n" \
  http://localhost:8000/api/v1/projects \
  -H "Authorization: Bearer $TOKEN"

# Using Apache Bench
ab -n 100 -c 10 http://localhost:3000/

# Using wrk
wrk -t4 -c100 -d30s http://localhost:3000/
```

### Frontend Performance

```bash
# Lighthouse audit
npm install -g lighthouse
lighthouse http://localhost:3000

# Bundle analysis
npm run build
npm install -g @next/bundle-analyzer
ANALYZE=true npm run build
```

## Debugging

### Backend Debugging

```bash
# Start with debugger
python -m pdb -c continue -m uvicorn app.main:app --reload

# Or use VS Code debugger with launch.json:
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Python: FastAPI",
      "type": "python",
      "request": "launch",
      "module": "uvicorn",
      "args": ["app.main:app", "--reload"],
      "jinja": true,
      "cwd": "${workspaceFolder}/backend"
    }
  ]
}
```

### Frontend Debugging

```bash
# Browser DevTools
1. Open http://localhost:3000
2. Right-click → Inspect
3. Check Console for errors
4. Use Network tab to see API calls

# VS Code Debugger
Add to .vscode/launch.json:
{
  "type": "chrome",
  "request": "launch",
  "name": "Launch Chrome",
  "url": "http://localhost:3000",
  "webRoot": "${workspaceFolder}/frontend"
}
```

### Logging

```bash
# View backend logs
docker-compose logs backend -f

# View frontend logs
docker-compose logs frontend -f

# View all logs
docker-compose logs -f
```

## Common Issues & Solutions

### PostgreSQL Connection Failed
```bash
# Check if database is running
docker-compose ps postgres

# Check connection
docker-compose exec postgres pg_isready

# Restart database
docker-compose restart postgres
```

### Port Already in Use
```bash
# Find process using port 8000
lsof -i :8000
kill -9 <PID>

# Or use different port
python -m uvicorn app.main:app --port 8001
```

### Migration Issues
```bash
# Check current migration
alembic current

# Downgrade one version
alembic downgrade -1

# Upgrade to specific revision
alembic upgrade abc123def456
```

### Token Expired
```bash
# Get new token by logging in again
curl -X POST http://localhost:8000/api/v1/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "test@example.com",
    "password": "testpass123"
  }'
```

## Continuous Integration

### Local CI Simulation

```bash
# Run all checks locally before pushing
./scripts/ci-check.sh

# Or manually:
cd backend
pytest tests/ --cov=app
pylint app/
mypy app/

cd ../frontend
npm test
npm run lint
npm run type-check
```

## Performance Optimization

### Database Optimization
- Add indexes on frequently queried columns
- Use query optimization for complex joins
- Implement query result caching with Redis

### API Optimization
- Enable gzip compression
- Implement response caching
- Use pagination for large datasets
- Implement rate limiting

### Frontend Optimization
- Code splitting per route
- Image optimization with next/image
- CSS minification
- Service worker caching

## Monitoring & Alerts

### Health Check Endpoints

```bash
# Backend health
curl http://localhost:8000/health

# Frontend health
curl http://localhost:3000/health
```

### Metrics Collection

```bash
# Prometheus metrics
curl http://localhost:8000/metrics

# View in Grafana
http://localhost:3000/grafana
```

## Deployment Testing

### Test Docker Build

```bash
# Build images
docker build -t buildsmart-api:test -f backend/Dockerfile .
docker build -t buildsmart-web:test -f frontend/Dockerfile .

# Run containers
docker run -p 8000:8000 buildsmart-api:test
docker run -p 3000:3000 buildsmart-web:test
```

### Test Kubernetes Deployment

```bash
# Create local cluster (minikube)
minikube start

# Deploy
kubectl apply -f devops/kubernetes-deployment.yaml

# Check pods
kubectl get pods -n buildsmart

# View logs
kubectl logs -n buildsmart deployment/backend
```
