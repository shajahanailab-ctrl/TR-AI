# Architecture & Design Guide

## System Architecture

### Overview
BuildSmart AI follows a modern three-tier architecture:
- **Frontend**: Next.js client-side application
- **API**: FastAPI REST API server
- **Database**: PostgreSQL with Redis caching

### Technology Stack Rationale

#### Frontend
- **Next.js 15**: Server-side rendering, static optimization, built-in API routes
- **React 19**: Latest React with improved performance
- **TypeScript**: Type safety and better IDE support
- **TailwindCSS**: Utility-first CSS for rapid UI development
- **Zustand**: Lightweight state management for better performance

#### Backend
- **FastAPI**: Async support, automatic API documentation, fast performance
- **Python 3.12**: Latest Python with improved performance
- **SQLAlchemy**: Flexible ORM with support for complex queries
- **PostgreSQL**: Robust relational database with JSON support

#### AI/ML
- **XGBoost**: Gradient boosting for cost prediction
- **LangChain**: Framework for building LLM applications
- **OpenAI API**: GPT models for natural language understanding

## Database Schema

### Core Tables

**Users**
- Manages user accounts and roles
- Supports RBAC with role-based access control
- Password hashing with bcrypt

**Projects**
- Stores project information
- Linked to users via foreign key
- Includes location data for locality intelligence

**Predictions**
- Stores cost, timeline, and risk predictions
- ML model outputs
- Linked to projects

**ChatHistory**
- Tracks user-AI interactions
- Enables chat history features
- Useful for analytics

### Indexing Strategy
- Foreign keys indexed for fast joins
- Email indexed for quick user lookups
- Project user_id indexed for efficient filtering
- City indexed in material_rates for location queries

## API Design

### RESTful Endpoints
- Resource-based URLs
- Standard HTTP methods (GET, POST, PUT, DELETE)
- Consistent response format
- Error handling with appropriate status codes

### Request/Response Format
```json
{
  "status": "success|error",
  "data": {},
  "message": "string"
}
```

### Authentication
- JWT token-based authentication
- Token refresh mechanism
- Role-based access control

## Security Implementation

### Authentication
- Secure password hashing with bcrypt
- JWT token generation and validation
- Token expiration (configurable)

### Authorization
- Role-based access control (RBAC)
- User-scoped data access
- Admin privileges for management

### Input Validation
- Pydantic models for request validation
- Type checking with TypeScript
- SQL injection prevention (ORM)

### CORS Protection
- Whitelist specific origins
- Restrict HTTP methods
- Validate headers

## Caching Strategy

### Redis Cache
- User session cache
- API response caching
- Rate limiting counters
- Task queue for async jobs

### Cache Keys
```
user:{user_id}
project:{project_id}
prediction:{prediction_id}
```

## Performance Optimization

### Database
- Connection pooling (20-40 connections)
- Query optimization with proper indexes
- Prepared statements (via SQLAlchemy)

### API
- Response compression
- Pagination for large datasets
- Batch operations when possible

### Frontend
- Code splitting per route
- Image optimization
- Lazy loading components
- Service worker caching

## Scalability Considerations

### Horizontal Scaling
- Stateless API design
- Load balancer distribution
- Database read replicas

### Vertical Scaling
- Connection pooling
- Caching layer (Redis)
- Async task processing

### Database Sharding
- By user_id for multi-tenancy
- By region/location for data residency

## Deployment Architecture

### Development
- Docker Compose for local setup
- SQLite for testing
- Mock OpenAI responses

### Staging
- Kubernetes with 2 replicas
- PostgreSQL with backups
- Redis for caching

### Production
- Kubernetes with auto-scaling
- Multi-AZ database setup
- CDN for static assets
- SSL/TLS encryption

## Monitoring & Logging

### Application Metrics
- Request latency
- Error rates
- Cache hit ratio
- Database query performance

### Logging
- Structured logging (JSON format)
- Different log levels (DEBUG, INFO, WARNING, ERROR)
- Log aggregation with ELK stack

### Health Checks
- API health endpoint
- Database connectivity check
- Cache connectivity check
- External API status

## ML Model Architecture

### Cost Prediction Model
- **Algorithm**: XGBoost Regressor
- **Features**: 
  - Location (city/postal code)
  - Building parameters (type, floors, size)
  - Material type
  - Design style
- **Output**: Cost, confidence score, risk assessment

### Data Features
- Normalized features using StandardScaler
- Categorical encoding for building types
- City price indices for location adjustment

## Testing Strategy

### Unit Tests
- Service layer tests
- Schema validation tests
- Security function tests

### Integration Tests
- API endpoint tests
- Database integration tests
- Authentication flow tests

### Load Testing
- JMeter for concurrent user simulation
- API endpoint performance measurement
- Database connection pool testing

## Future Enhancements

1. **Advanced ML**
   - Neural networks for design generation
   - Time series forecasting for cost trends
   - Image recognition for site analysis

2. **Features**
   - Real-time collaboration
   - Mobile app (React Native)
   - AR/VR visualization
   - Payment integration

3. **Infrastructure**
   - Multi-region deployment
   - Advanced caching (GraphQL)
   - Message queue (Kafka)
   - Search engine (Elasticsearch)

4. **Analytics**
   - User behavior tracking
   - Project success metrics
   - Cost prediction accuracy
   - Timeline estimation accuracy
