# Deployment Guide

## Prerequisites

- Docker & Docker Compose (for containerized deployment)
- Kubernetes cluster (for production deployment)
- AWS account (for cloud deployment)
- PostgreSQL 15+
- Node.js 20+
- Python 3.12+

## Local Development Deployment

### Option 1: Docker Compose (Recommended)

1. **Clone and configure**:
```bash
git clone https://github.com/buildsmart/buildsmart-ai.git
cd buildsmart-ai
cp backend/.env.example backend/.env
cp frontend/.env.example frontend/.env.local
```

2. **Update environment variables**:
```bash
# backend/.env
SECRET_KEY=your-secure-random-key
OPENAI_API_KEY=sk-your-key
DATABASE_URL=postgresql://buildsmart:buildsmart_pass@postgres:5432/buildsmart_db
```

3. **Start services**:
```bash
docker-compose up -d
```

4. **Initialize database**:
```bash
docker-compose exec backend alembic upgrade head
docker-compose exec backend python -m app.scripts.seed
```

5. **Access application**:
- Frontend: http://localhost:3000
- API: http://localhost:8000
- API Docs: http://localhost:8000/docs

### Option 2: Manual Setup

**Backend**:
```bash
cd backend
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python -m uvicorn app.main:app --reload
```

**Frontend**:
```bash
cd frontend
npm install
npm run dev
```

## Docker Image Building

### Build Images

```bash
# Backend image
docker build -t buildsmart-api:1.0.0 -f backend/Dockerfile .

# Frontend image
docker build -t buildsmart-web:1.0.0 -f frontend/Dockerfile .

# Test images
docker run --rm buildsmart-api:1.0.0 python -m pytest tests/
docker run --rm buildsmart-web:1.0.0 npm test
```

### Push to Registry

```bash
# Configure registry credentials
docker login -u username -p password registry.example.com

# Tag images
docker tag buildsmart-api:1.0.0 registry.example.com/buildsmart-api:1.0.0
docker tag buildsmart-web:1.0.0 registry.example.com/buildsmart-web:1.0.0

# Push images
docker push registry.example.com/buildsmart-api:1.0.0
docker push registry.example.com/buildsmart-web:1.0.0
```

## Kubernetes Deployment

### Prerequisites

- kubectl configured with cluster access
- Helm (optional, for templating)

### Deploy to Kubernetes

```bash
# Create namespace
kubectl create namespace buildsmart

# Apply configurations
kubectl apply -f devops/kubernetes-deployment.yaml

# Verify deployment
kubectl get pods -n buildsmart
kubectl get svc -n buildsmart

# Port forward (if needed)
kubectl port-forward -n buildsmart svc/frontend 3000:3000
kubectl port-forward -n buildsmart svc/backend 8000:8000

# Check logs
kubectl logs -n buildsmart deployment/backend -f
kubectl logs -n buildsmart deployment/frontend -f
```

### Update Deployment

```bash
# Update image
kubectl set image deployment/backend backend=registry.example.com/buildsmart-api:1.0.1 -n buildsmart

# Check rollout status
kubectl rollout status deployment/backend -n buildsmart

# Rollback if needed
kubectl rollout undo deployment/backend -n buildsmart
```

## AWS Deployment

### Using ECS (Elastic Container Service)

1. **Create ECR repositories**:
```bash
aws ecr create-repository --repository-name buildsmart-api --region us-east-1
aws ecr create-repository --repository-name buildsmart-web --region us-east-1
```

2. **Push images**:
```bash
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 123456789.dkr.ecr.us-east-1.amazonaws.com

docker tag buildsmart-api:1.0.0 123456789.dkr.ecr.us-east-1.amazonaws.com/buildsmart-api:1.0.0
docker push 123456789.dkr.ecr.us-east-1.amazonaws.com/buildsmart-api:1.0.0

docker tag buildsmart-web:1.0.0 123456789.dkr.ecr.us-east-1.amazonaws.com/buildsmart-web:1.0.0
docker push 123456789.dkr.ecr.us-east-1.amazonaws.com/buildsmart-web:1.0.0
```

3. **Create RDS Database**:
```bash
aws rds create-db-instance \
  --db-instance-identifier buildsmart-db \
  --db-instance-class db.t3.micro \
  --engine postgres \
  --master-username postgres \
  --master-user-password YourSecurePassword123 \
  --allocated-storage 20 \
  --region us-east-1
```

4. **Create ElastiCache Redis**:
```bash
aws elasticache create-cache-cluster \
  --cache-cluster-id buildsmart-cache \
  --cache-node-type cache.t3.micro \
  --engine redis \
  --region us-east-1
```

5. **Create ECS Cluster and Services** (use AWS Console or CloudFormation)

### Using AWS CodeDeploy

1. **Create deployment configuration**:
```bash
aws deploy create-application --application-name BuildSmartAI

aws deploy create-deployment \
  --application-name BuildSmartAI \
  --deployment-group-name production \
  --github-location repository=buildsmart-ai,commitId=abc123
```

## CI/CD Pipeline

### GitHub Actions

The project includes a complete CI/CD pipeline in `.github/workflows/ci-cd.yml`:

1. **On every push**:
   - Run tests
   - Lint code
   - Build Docker images

2. **On push to main**:
   - Push images to registry
   - Deploy to Kubernetes
   - Run health checks

### Manual Workflow

```bash
# 1. Run tests locally
cd backend && pytest tests/
cd ../frontend && npm test

# 2. Build images
docker build -t buildsmart-api:latest -f backend/Dockerfile .
docker build -t buildsmart-web:latest -f frontend/Dockerfile .

# 3. Push to registry
docker push registry.example.com/buildsmart-api:latest
docker push registry.example.com/buildsmart-web:latest

# 4. Deploy
kubectl set image deployment/backend backend=registry.example.com/buildsmart-api:latest -n buildsmart
kubectl set image deployment/frontend frontend=registry.example.com/buildsmart-web:latest -n buildsmart
```

## Database Migrations

### Create Migration

```bash
cd backend
alembic revision --autogenerate -m "Add new column"
```

### Apply Migration

```bash
alembic upgrade head
```

### Rollback Migration

```bash
alembic downgrade -1
```

### Verify Status

```bash
alembic current
```

## Health Checks & Monitoring

### Application Health

```bash
# Backend
curl http://localhost:8000/health

# Frontend
curl http://localhost:3000/health
```

### Database Health

```bash
# Connect to database
docker-compose exec postgres psql -U buildsmart -d buildsmart_db

# List tables
\dt

# Check connections
SELECT * FROM pg_stat_activity;
```

### Kubernetes Health

```bash
# Pod status
kubectl get pods -n buildsmart -o wide

# Service endpoints
kubectl get endpoints -n buildsmart

# Resource usage
kubectl top pods -n buildsmart
kubectl top nodes
```

## Backup & Recovery

### Database Backup

```bash
# Manual backup
docker-compose exec postgres pg_dump -U buildsmart buildsmart_db > backup.sql

# Automated backup (cron job)
0 2 * * * pg_dump -U buildsmart buildsmart_db | gzip > /backups/buildsmart_$(date +\%Y\%m\%d).sql.gz
```

### Restore from Backup

```bash
# Restore database
psql -U buildsmart buildsmart_db < backup.sql

# Or from gzipped backup
gunzip < backup.sql.gz | psql -U buildsmart buildsmart_db
```

## Performance Tuning

### PostgreSQL

```sql
-- Connection pooling
shared_buffers = 256MB
effective_cache_size = 1GB
work_mem = 16MB

-- Index on frequently queried columns
CREATE INDEX idx_projects_user_id ON projects(user_id);
CREATE INDEX idx_predictions_project_id ON predictions(project_id);
```

### Redis

```
maxmemory 256mb
maxmemory-policy allkeys-lru
```

### Application

- Enable query result caching
- Use database connection pooling
- Implement request rate limiting
- Enable gzip compression

## Security Hardening

1. **Secrets Management**:
```bash
# Use AWS Secrets Manager
aws secretsmanager create-secret --name buildsmart/db-password
```

2. **Network Security**:
- Enable WAF on ALB
- Security groups for database access
- VPC endpoint for internal services

3. **SSL/TLS**:
- Get certificate from ACM
- Enable HTTPS on ALB
- HSTS headers in frontend

4. **Environment Security**:
- Don't commit secrets
- Use .env files (not in repo)
- Rotate API keys regularly
- Enable audit logging

## Troubleshooting

### Pod CrashLoopBackOff

```bash
# Check logs
kubectl logs -n buildsmart <pod-name>

# Describe pod
kubectl describe pod -n buildsmart <pod-name>

# Check resource limits
kubectl top pod -n buildsmart <pod-name>
```

### Database Connection Issues

```bash
# Test connection
psql -h postgres -U buildsmart -d buildsmart_db

# Check environment variables
docker-compose exec backend env | grep DATABASE
```

### API Not Responding

```bash
# Check service
kubectl get svc -n buildsmart

# Test endpoint
curl http://backend:8000/health

# Check logs
docker-compose logs backend
```

## Support

For deployment assistance:
- Check the troubleshooting guide
- Review application logs
- Contact the development team
- Open an issue on GitHub
