# BuildSmart AI - Complete Enterprise SaaS Application

## Overview

BuildSmart AI is an AI-powered Construction Cost Prediction and Building Design Assistant that helps homeowners, architects, civil engineers, contractors, builders, and construction companies estimate project costs, generate building designs, calculate materials, predict project timelines, analyze risks, and interact with an AI construction chatbot.

## Technology Stack

### Frontend
- **Next.js 15** - React framework for production
- **React 19** - UI library
- **TypeScript** - Type safety
- **TailwindCSS** - Utility-first CSS framework
- **ShadCN UI** - Component library
- **Framer Motion** - Animation library
- **Recharts** - Charts and visualizations
- **React Hook Form** - Form management
- **Zustand** - State management
- **Axios** - HTTP client

### Backend
- **FastAPI** - Modern Python web framework
- **Python 3.12** - Programming language
- **SQLAlchemy** - ORM
- **Alembic** - Database migrations

### Database
- **PostgreSQL** - Relational database
- **Redis** - Caching and message queue

### AI/ML
- **Scikit-Learn** - Machine learning library
- **XGBoost** - Gradient boosting
- **CatBoost** - Categorical boosting
- **LangChain** - LLM framework
- **OpenAI API** - GPT models

### DevOps
- **Docker** - Container technology
- **Kubernetes** - Orchestration
- **AWS** - Cloud infrastructure
- **GitHub Actions** - CI/CD

## Project Structure

```
buildsmart-ai/
├── backend/
│   ├── app/
│   │   ├── api/
│   │   │   ├── endpoints/
│   │   │   │   ├── auth.py
│   │   │   │   ├── projects.py
│   │   │   │   ├── cost.py
│   │   │   │   ├── design.py
│   │   │   │   ├── boq.py
│   │   │   │   ├── timeline.py
│   │   │   │   ├── risk.py
│   │   │   │   └── chat.py
│   │   ├── core/
│   │   │   ├── config.py
│   │   │   └── security.py
│   │   ├── db/
│   │   │   ├── base.py
│   │   │   └── session.py
│   │   ├── models/
│   │   │   ├── user.py
│   │   │   ├── project.py
│   │   │   ├── material_rate.py
│   │   │   ├── building_template.py
│   │   │   ├── prediction.py
│   │   │   └── chat_history.py
│   │   ├── schemas/
│   │   │   ├── user.py
│   │   │   ├── project.py
│   │   │   ├── cost.py
│   │   │   ├── design.py
│   │   │   ├── boq.py
│   │   │   ├── timeline.py
│   │   │   ├── risk.py
│   │   │   └── chat.py
│   │   ├── services/
│   │   │   ├── user_service.py
│   │   │   ├── project_service.py
│   │   │   ├── cost_service.py
│   │   │   ├── design_service.py
│   │   │   ├── boq_service.py
│   │   │   ├── timeline_service.py
│   │   │   ├── risk_service.py
│   │   │   └── chat_service.py
│   │   ├── ml/
│   │   └── main.py
│   ├── tests/
│   ├── requirements.txt
│   ├── Dockerfile
│   └── .env.example
│
├── frontend/
│   ├── app/
│   │   ├── page.tsx (Home)
│   │   ├── login/
│   │   ├── register/
│   │   ├── dashboard/
│   │   ├── cost-estimator/
│   │   ├── design-studio/
│   │   ├── timeline/
│   │   ├── risk-analysis/
│   │   ├── boq/
│   │   ├── ai-assistant/
│   │   └── layout.tsx
│   ├── components/
│   │   ├── Header.tsx
│   │   └── ui/
│   ├── lib/
│   │   ├── api-client.ts
│   │   └── api-config.ts
│   ├── store/
│   │   ├── auth.ts
│   │   └── projects.ts
│   ├── package.json
│   ├── Dockerfile
│   └── next.config.js
│
├── devops/
│   └── kubernetes-deployment.yaml
│
├── docker-compose.yml
├── .github/
│   └── workflows/
│       └── ci-cd.yml
└── README.md
```

## API Endpoints

### Authentication
- `POST /api/v1/auth/register` - Register new user
- `POST /api/v1/auth/login` - Login user
- `GET /api/v1/auth/me` - Get current user

### Projects
- `GET /api/v1/projects/` - List projects
- `POST /api/v1/projects/` - Create project
- `GET /api/v1/projects/{id}` - Get project
- `PUT /api/v1/projects/{id}` - Update project
- `DELETE /api/v1/projects/{id}` - Delete project

### Cost Prediction
- `POST /api/v1/cost/predict` - Predict cost

### Design Generation
- `POST /api/v1/design/generate` - Generate design

### BOQ
- `POST /api/v1/boq/generate` - Generate BOQ

### Timeline
- `POST /api/v1/timeline/predict` - Predict timeline

### Risk Analysis
- `POST /api/v1/risk/analyze` - Analyze risks

### Chat
- `POST /api/v1/chat/ask` - Ask AI assistant
- `GET /api/v1/chat/history/{user_id}` - Get chat history

## Getting Started

### Prerequisites
- Docker & Docker Compose
- Node.js 20+
- Python 3.12+
- PostgreSQL 15+
- Redis 7+

### Local Development

1. Clone the repository
```bash
git clone https://github.com/yourusername/buildsmart-ai.git
cd buildsmart-ai
```

2. Set up environment variables
```bash
cp backend/.env.example backend/.env
cp frontend/.env.example frontend/.env.local
```

3. Start services with Docker Compose
```bash
docker-compose up -d
```

4. Access the application
- Frontend: http://localhost:3000
- API: http://localhost:8000
- API Docs: http://localhost:8000/docs

### Database Setup

```bash
# Apply migrations
docker-compose exec backend alembic upgrade head

# Create initial data (optional)
docker-compose exec backend python -m app.scripts.seed
```

## Features

### 1. Cost Prediction Engine
- AI-powered cost estimation using XGBoost
- Location-based cost indices
- Material and labor cost breakdown
- Confidence scores and risk assessment

### 2. Building Design Generator
- Automatic floor plan generation
- Space optimization
- Design style variations
- 2D and 3D visualization

### 3. Material Recommendation Engine
- Bill of Quantities (BOQ) generation
- Material cost estimation
- Supplier recommendations
- Export to PDF and Excel

### 4. Timeline Prediction
- Project schedule generation
- Phase-based breakdown
- Gantt chart visualization
- Risk-adjusted timelines

### 5. Risk Analysis
- Budget overrun risk assessment
- Schedule delay risk analysis
- Material shortage prediction
- Weather and geological risk evaluation

### 6. AI Chatbot
- Construction cost queries
- Material recommendations
- Design suggestions
- Building regulation guidance

## Deployment

### Docker Compose (Development)
```bash
docker-compose up -d
```

### Kubernetes (Production)
```bash
kubectl apply -f devops/kubernetes-deployment.yaml
kubectl port-forward -n buildsmart svc/frontend 3000:3000
kubectl port-forward -n buildsmart svc/backend 8000:8000
```

### AWS Deployment
```bash
# Build images
docker build -t buildsmart-api:latest -f backend/Dockerfile .
docker build -t buildsmart-web:latest -f frontend/Dockerfile .

# Push to ECR
aws ecr get-login-password | docker login --username AWS --password-stdin <account-id>.dkr.ecr.<region>.amazonaws.com
docker tag buildsmart-api:latest <account-id>.dkr.ecr.<region>.amazonaws.com/buildsmart-api:latest
docker push <account-id>.dkr.ecr.<region>.amazonaws.com/buildsmart-api:latest
```

## Testing

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

## Security

- JWT authentication
- Role-based access control (RBAC)
- Input validation and sanitization
- SQL injection prevention (SQLAlchemy ORM)
- CORS protection
- Rate limiting
- Secure password hashing (bcrypt)

## Performance

- Database query optimization with indexes
- Redis caching layer
- API response compression
- Frontend code splitting
- Image optimization
- CDN integration ready

## Documentation

- API documentation: `/docs` (Swagger UI)
- API ReDoc: `/redoc`
- Architecture documentation: See `docs/` folder

## Contributing

1. Create a feature branch: `git checkout -b feature/amazing-feature`
2. Commit your changes: `git commit -m 'Add amazing feature'`
3. Push to the branch: `git push origin feature/amazing-feature`
4. Open a Pull Request

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Support

For support, email support@buildsmart.ai or create an issue on GitHub.

## Roadmap

- [ ] Mobile app (React Native)
- [ ] Advanced 3D visualization
- [ ] ML model training pipeline
- [ ] Real-time collaboration features
- [ ] Payment integration
- [ ] Advanced reporting and analytics
- [ ] Integration with building management systems
- [ ] AR/VR visualization
