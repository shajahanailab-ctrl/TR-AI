# BuildSmart AI - Complete Enterprise SaaS Application

## Overview

BuildSmart AI is an AI-powered Construction Cost Prediction and Building Design Assistant that helps homeowners, architects, civil engineers, contractors, builders, and construction companies estimate project costs, generate building designs, calculate materials, predict project timelines, analyze risks, and interact with an AI construction chatbot.

## Quick Start

### Prerequisites
- Docker & Docker Compose
- Node.js 20+ (for local frontend development)
- Python 3.12+ (for local backend development)
- PostgreSQL 15+ (optional, if running without Docker)
- Redis 7+ (optional, if running without Docker)

### Local Development with Docker

1. **Clone and Setup**
```bash
cd buildsmart-ai
```

2. **Start Services**
```bash
docker-compose up -d
```

3. **Access the Application**
- Frontend: http://localhost:3000
- API: http://localhost:8000
- API Docs: http://localhost:8000/docs
- API ReDoc: http://localhost:8000/redoc

4. **Database Initialization**
```bash
docker-compose exec backend alembic upgrade head
```

### Local Frontend Development

```bash
cd frontend
npm install
npm run dev
# Runs on http://localhost:3000
```

### Local Backend Development

```bash
cd backend
pip install -r requirements.txt
uvicorn app.main:app --reload
# Runs on http://localhost:8000
```

## Project Structure

```
buildsmart-ai/
├── backend/                 # FastAPI application
│   ├── app/
│   │   ├── api/            # API routers
│   │   ├── core/           # Configuration & security
│   │   ├── db/             # Database setup
│   │   ├── models/         # SQLAlchemy models
│   │   ├── schemas/        # Pydantic schemas
│   │   ├── services/       # Business logic
│   │   ├── ml/             # ML models
│   │   └── main.py         # FastAPI app
│   ├── requirements.txt
│   └── Dockerfile
│
├── frontend/                # Next.js 15 application
│   ├── app/                # App router pages
│   ├── components/         # React components
│   ├── lib/                # Utilities & API client
│   ├── store/              # Zustand state management
│   ├── package.json
│   └── Dockerfile
│
├── devops/                  # DevOps configuration
│   └── kubernetes-deployment.yaml
│
├── docker-compose.yml       # Docker Compose setup
├── .github/workflows/       # CI/CD workflows
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

## Key Features

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

## Technology Stack

### Frontend
- **Next.js 15** - React framework
- **React 19** - UI library
- **TypeScript** - Type safety
- **TailwindCSS** - Styling
- **Framer Motion** - Animations
- **Recharts** - Charts & visualizations
- **Zustand** - State management
- **Axios** - HTTP client

### Backend
- **FastAPI** - Python web framework
- **SQLAlchemy** - ORM
- **Alembic** - Database migrations
- **Pydantic** - Data validation
- **JWT** - Authentication

### Database & Cache
- **PostgreSQL** - Relational database
- **Redis** - Caching & message queue

### AI/ML
- **LangChain** - LLM framework
- **OpenAI API** - GPT models
- **Scikit-Learn** - Machine learning
- **XGBoost** - Gradient boosting
- **CatBoost** - Categorical boosting

### DevOps
- **Docker** - Containerization
- **Docker Compose** - Local development
- **Kubernetes** - Production orchestration
- **GitHub Actions** - CI/CD

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

## Deployment

### Kubernetes
```bash
kubectl apply -f devops/kubernetes-deployment.yaml
```

### Docker Compose
```bash
docker-compose up -d
```

## Environment Variables

### Backend (.env)
```
DATABASE_URL=postgresql://user:password@localhost:5432/buildsmart
REDIS_URL=redis://localhost:6379
SECRET_KEY=your-secret-key
OPENAI_API_KEY=your-openai-key
```

### Frontend (.env.local)
```
NEXT_PUBLIC_API_URL=http://localhost:8000
```

## Security

- JWT authentication with bcrypt password hashing
- Role-based access control (RBAC)
- Input validation and sanitization
- SQL injection prevention via ORM
- CORS protection
- Rate limiting
- Secure headers

## Performance

- Database query optimization with indexes
- Redis caching layer
- API response compression
- Frontend code splitting and lazy loading
- Image optimization
- CDN-ready architecture

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
- [ ] Payment integration (Stripe)
- [ ] Advanced reporting and analytics
- [ ] Integration with building management systems
- [ ] AR/VR visualization

## Documentation

- API Documentation: Available at `/docs` (Swagger UI) and `/redoc` when backend is running
- Architecture documentation: See `docs/` folder
- Environment setup: See individual service README files
