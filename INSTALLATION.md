# BuildSmart AI - Installation and Setup Guide

## Quick Start with Docker

### 1. Clone Repository
```bash
git clone https://github.com/yourusername/buildsmart-ai.git
cd buildsmart-ai
```

### 2. Configure Environment
```bash
cp backend/.env.example backend/.env
cp frontend/.env.example frontend/.env.local
```

Edit `backend/.env`:
- Set `SECRET_KEY` to a random string
- Configure database credentials
- Add OpenAI API key

### 3. Start with Docker Compose
```bash
docker-compose up -d
```

Wait for all services to start:
```bash
docker-compose ps
```

### 4. Initialize Database
```bash
docker-compose exec backend alembic upgrade head
docker-compose exec backend python -m app.scripts.seed
```

### 5. Access Application
- Frontend: http://localhost:3000
- API: http://localhost:8000
- API Docs: http://localhost:8000/docs

### Test Login Credentials
- Email: `demo@buildsmart.ai`
- Password: `demo123`

## Manual Setup (Development)

### Backend Setup

1. Create Python virtual environment:
```bash
cd backend
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

2. Install dependencies:
```bash
pip install -r requirements.txt
```

3. Set up database:
```bash
createdb buildsmart_db -U buildsmart
alembic upgrade head
```

4. Run server:
```bash
uvicorn app.main:app --reload
```

### Frontend Setup

1. Install dependencies:
```bash
cd frontend
npm install
```

2. Start development server:
```bash
npm run dev
```

3. Open http://localhost:3000

## Database Migrations

### Create New Migration
```bash
alembic revision --autogenerate -m "Description"
```

### Apply Migration
```bash
alembic upgrade head
```

### Rollback Migration
```bash
alembic downgrade -1
```

## Running Tests

### Backend Tests
```bash
cd backend
pytest tests/ -v --cov=app
```

### Frontend Tests
```bash
cd frontend
npm test
```

## Deployment

### AWS ECS Deployment
```bash
# Build images
docker build -t buildsmart-api:latest -f backend/Dockerfile .
docker build -t buildsmart-web:latest -f frontend/Dockerfile .

# Push to ECR
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <account>.dkr.ecr.us-east-1.amazonaws.com

docker tag buildsmart-api:latest <account>.dkr.ecr.us-east-1.amazonaws.com/buildsmart-api:latest
docker push <account>.dkr.ecr.us-east-1.amazonaws.com/buildsmart-api:latest

docker tag buildsmart-web:latest <account>.dkr.ecr.us-east-1.amazonaws.com/buildsmart-web:latest
docker push <account>.dkr.ecr.us-east-1.amazonaws.com/buildsmart-web:latest
```

### Kubernetes Deployment
```bash
# Create namespace and deploy
kubectl apply -f devops/kubernetes-deployment.yaml

# Check status
kubectl get pods -n buildsmart

# Port forward
kubectl port-forward -n buildsmart svc/frontend 3000:3000
kubectl port-forward -n buildsmart svc/backend 8000:8000
```

## Troubleshooting

### Database Connection Issues
```bash
# Check PostgreSQL container
docker-compose ps postgres

# View logs
docker-compose logs postgres

# Restart database
docker-compose restart postgres
```

### API Connection Issues
```bash
# Check backend container
docker-compose logs backend

# Test API
curl http://localhost:8000/health
```

### Frontend Issues
```bash
# Clear node modules and reinstall
rm -rf frontend/node_modules
cd frontend && npm install
npm run dev
```

## Environment Variables

### Backend (.env)
```
DATABASE_URL=postgresql://user:password@localhost:5432/buildsmart_db
REDIS_URL=redis://localhost:6379
SECRET_KEY=your-secret-key
OPENAI_API_KEY=your-api-key
ENVIRONMENT=development
DEBUG=True
```

### Frontend (.env.local)
```
NEXT_PUBLIC_API_URL=http://localhost:8000
```

## Common Commands

```bash
# Start all services
docker-compose up -d

# Stop all services
docker-compose down

# View logs
docker-compose logs -f backend
docker-compose logs -f frontend

# Run migrations
docker-compose exec backend alembic upgrade head

# Seed database
docker-compose exec backend python -m app.scripts.seed

# Access database
docker-compose exec postgres psql -U buildsmart -d buildsmart_db
```

## Performance Optimization

### Backend
- Enable query caching with Redis
- Use database connection pooling
- Implement request rate limiting
- Add database indexes

### Frontend
- Enable Next.js image optimization
- Use code splitting
- Implement lazy loading
- Cache API responses

## Security Checklist

- [ ] Change SECRET_KEY in production
- [ ] Use strong database passwords
- [ ] Enable HTTPS
- [ ] Set CORS origins correctly
- [ ] Enable authentication on all endpoints
- [ ] Implement rate limiting
- [ ] Enable database backups
- [ ] Use environment variables for secrets
- [ ] Enable logging and monitoring
- [ ] Regular security updates

## Support

For issues or questions:
1. Check the GitHub issues
2. Review the API documentation at `/docs`
3. Contact support@buildsmart.ai
