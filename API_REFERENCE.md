# API Endpoints Reference

## Authentication Endpoints

### POST `/api/v1/auth/register`
Register a new user account.

**Request Body:**
```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "securepassword123"
}
```

**Response:** `201 Created`
```json
{
  "id": 1,
  "name": "John Doe",
  "email": "john@example.com",
  "role": "user"
}
```

### POST `/api/v1/auth/login`
Authenticate and get JWT token.

**Request Body:**
```json
{
  "email": "john@example.com",
  "password": "securepassword123"
}
```

**Response:** `200 OK`
```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIs...",
  "token_type": "bearer",
  "user": {
    "id": 1,
    "email": "john@example.com",
    "role": "user"
  }
}
```

## Project Endpoints

### GET `/api/v1/projects`
Get all projects for the current user.

**Headers:** `Authorization: Bearer <token>`

**Response:** `200 OK`
```json
[
  {
    "id": 1,
    "user_id": 1,
    "project_name": "Manhattan Residential",
    "city": "New York",
    "building_type": "residential",
    "construction_area": 6000,
    "created_at": "2024-01-15T10:30:00Z"
  }
]
```

### POST `/api/v1/projects`
Create a new project.

**Headers:** `Authorization: Bearer <token>`

**Request Body:**
```json
{
  "project_name": "My Building",
  "country": "USA",
  "state": "NY",
  "city": "New York",
  "postal_code": "10001",
  "locality": "Manhattan",
  "plot_area": 8000,
  "construction_area": 6000,
  "building_type": "residential",
  "floors": 4,
  "material_type": "concrete",
  "design_style": "modern"
}
```

**Response:** `201 Created`

### GET `/api/v1/projects/{project_id}`
Get project details.

### PUT `/api/v1/projects/{project_id}`
Update project information.

### DELETE `/api/v1/projects/{project_id}`
Delete a project.

## Cost Prediction Endpoints

### POST `/api/v1/cost/estimate`
Estimate construction cost using AI model.

**Headers:** `Authorization: Bearer <token>`

**Request Body:**
```json
{
  "project_id": 1,
  "plot_area": 8000,
  "construction_area": 6000,
  "building_type": "residential",
  "floors": 4,
  "material_type": "concrete",
  "city": "New York"
}
```

**Response:** `200 OK`
```json
{
  "estimated_cost": 2500000,
  "cost_per_sqft": 416.67,
  "material_cost": 1000000,
  "labor_cost": 1000000,
  "overhead_cost": 500000,
  "confidence_score": 0.87,
  "risk_score": 25
}
```

## Design Generation Endpoints

### POST `/api/v1/design/generate`
Generate floor plans and design layouts.

**Headers:** `Authorization: Bearer <token>`

**Request Body:**
```json
{
  "project_id": 1,
  "plot_area": 8000,
  "construction_area": 6000,
  "building_type": "residential",
  "floors": 2,
  "design_style": "modern"
}
```

**Response:** `200 OK`
```json
{
  "ground_floor": {
    "floor_name": "Ground Floor",
    "rooms": [
      {
        "room_name": "Living Room",
        "length": 30,
        "width": 25,
        "area": 750
      }
    ]
  },
  "upper_floors": []
}
```

## Bill of Quantities (BOQ) Endpoints

### POST `/api/v1/boq/generate`
Generate material list and quantities.

**Headers:** `Authorization: Bearer <token>`

**Request Body:**
```json
{
  "project_id": 1,
  "construction_area": 6000,
  "building_type": "residential",
  "material_type": "concrete",
  "city": "New York"
}
```

**Response:** `200 OK`
```json
{
  "total_cost": 1000000,
  "items": [
    {
      "item_name": "Concrete",
      "quantity": 500,
      "unit": "cubic meters",
      "total_cost": 100000
    }
  ]
}
```

## Timeline Prediction Endpoints

### POST `/api/v1/timeline/predict`
Predict project timeline and phases.

**Headers:** `Authorization: Bearer <token>`

**Request Body:**
```json
{
  "project_id": 1,
  "building_type": "residential",
  "floors": 2,
  "construction_area": 6000,
  "city": "New York"
}
```

**Response:** `200 OK`
```json
{
  "total_days": 180,
  "foundation_days": 30,
  "structure_days": 45,
  "roofing_days": 20,
  "electrical_days": 25,
  "plumbing_days": 25,
  "finishing_days": 35
}
```

## Risk Analysis Endpoints

### POST `/api/v1/risk/analyze`
Analyze project risks and mitigation strategies.

**Headers:** `Authorization: Bearer <token>`

**Request Body:**
```json
{
  "project_id": 1,
  "estimated_cost": 2500000,
  "timeline_days": 180,
  "building_type": "residential",
  "city": "New York"
}
```

**Response:** `200 OK`
```json
{
  "overall_risk_score": 35,
  "overall_risk_level": "low",
  "budget_overrun_risk": 25,
  "delay_risk": 20,
  "material_shortage_risk": 15,
  "weather_risk": 30
}
```

## Chat Endpoints

### POST `/api/v1/chat/ask`
Ask AI assistant a question.

**Headers:** `Authorization: Bearer <token>`

**Request Body:**
```json
{
  "question": "How much will a residential building cost?",
  "building_type": "residential",
  "city": "New York"
}
```

**Response:** `200 OK`
```json
{
  "question": "How much will a residential building cost?",
  "response": "Cost estimation depends on location, building type...",
  "category": "cost"
}
```

### GET `/api/v1/chat/history`
Get chat conversation history.

**Headers:** `Authorization: Bearer <token>`

**Response:** `200 OK`
```json
[
  {
    "id": 1,
    "question": "How much will it cost?",
    "response": "Cost depends on...",
    "created_at": "2024-01-15T10:30:00Z"
  }
]
```

## Report Endpoints

### GET `/api/v1/projects/{project_id}/report/cost`
Get cost breakdown report.

**Headers:** `Authorization: Bearer <token>`

**Response:** `200 OK`
```json
{
  "report_type": "cost",
  "project_id": 1,
  "summary": {
    "total_estimated_cost": 2500000,
    "cost_per_sqft": 416.67
  },
  "cost_breakdown": {
    "material_cost": 1000000,
    "labor_cost": 1000000,
    "overhead_cost": 500000
  }
}
```

### GET `/api/v1/projects/{project_id}/report/timeline`
Get timeline report.

### GET `/api/v1/projects/{project_id}/report/risk`
Get risk assessment report.

### GET `/api/v1/projects/{project_id}/report/comprehensive`
Get comprehensive project report.

## Admin Endpoints

### GET `/api/v1/admin/statistics`
Get system statistics (Admin only).

**Headers:** `Authorization: Bearer <admin_token>`

**Response:** `200 OK`
```json
{
  "total_users": 42,
  "total_projects": 128,
  "total_predictions": 256,
  "average_cost": 1800000,
  "average_timeline_days": 165
}
```

### GET `/api/v1/admin/statistics/users`
Get user statistics.

### GET `/api/v1/admin/statistics/projects`
Get project statistics.

### GET `/api/v1/admin/statistics/costs`
Get cost statistics.

### POST `/api/v1/admin/users/{user_id}/disable`
Disable user account (Admin only).

### POST `/api/v1/admin/users/{user_id}/enable`
Enable user account (Admin only).

### POST `/api/v1/admin/users/{user_id}/role`
Update user role (Admin only).

**Request Body:**
```json
{
  "role": "admin"
}
```

### GET `/api/v1/admin/audit-log`
Get system audit log (Admin only).

## Error Responses

All endpoints return standardized error responses:

### 401 Unauthorized
```json
{
  "detail": "Authentication failed"
}
```

### 403 Forbidden
```json
{
  "detail": "Insufficient permissions"
}
```

### 404 Not Found
```json
{
  "detail": "Resource not found"
}
```

### 422 Unprocessable Entity
```json
{
  "detail": "Validation failed"
}
```

### 500 Internal Server Error
```json
{
  "detail": "Internal server error"
}
```

## Rate Limiting

API requests are rate-limited to prevent abuse:
- Standard: 100 requests per minute
- Authenticated: 500 requests per minute
- Admin: Unlimited

## Headers Required

All authenticated endpoints require:
```
Authorization: Bearer <JWT_TOKEN>
Content-Type: application/json
```

## Response Format

All successful responses follow:
```json
{
  "data": {},
  "message": "Success",
  "status": "success"
}
```

Failed responses include error details and appropriate HTTP status codes.
