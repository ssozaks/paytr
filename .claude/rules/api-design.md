---
paths:
  - "src/modules/**"
  - "apps/**"
  - "**/views.py"
  - "**/controllers.ts"
  - "**/routes/**"
---

# API Design Rules

## REST Conventions
- GET: Retrieve resource(s) - idempotent
- POST: Create new resource
- PUT: Full update of resource
- PATCH: Partial update of resource
- DELETE: Remove resource

## URL Structure
- Collection: `/api/v1/users`
- Resource: `/api/v1/users/:id`
- Nested: `/api/v1/users/:id/orders`
- Actions: `/api/v1/users/:id/activate` (POST)

## Response Format
```json
{
  "data": { ... },
  "meta": {
    "page": 1,
    "pageSize": 20,
    "total": 100
  }
}
```

## Error Format
```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input",
    "details": [
      { "field": "email", "message": "Invalid email format" }
    ]
  }
}
```

## Pagination
- Default page size: 20
- Max page size: 100
- Use cursor-based pagination for large datasets
- Always include total count in meta

## Authentication
- JWT in Authorization header: `Bearer <token>`
- Refresh token in httpOnly cookie
- Token expiry: access 15min, refresh 7days

## Rate Limiting
- Auth endpoints: 5 req/min
- API endpoints: 100 req/min
- Public endpoints: 30 req/min
