# sample-case Test Case

This sample demonstrates the main features of Mockstation.

## Included Endpoints

### GET /api/users

Demonstrates query parameter conditional branching:

- `GET.res` or `GET@limit__10.res` - limit=10 (default, returns 5 users)
- `GET@limit__0.res` - limit=0 (returns 400 Bad Request)

#### Usage Examples

```bash
# Default request
curl http://localhost:8080/api/users

# Explicit limit=10
curl http://localhost:8080/api/users?limit=10

# limit=0 (error)
curl http://localhost:8080/api/users?limit=0
# → HTTP 400 Bad Request
```

### POST /api/users

Simulates user creation.

- Request: JSON body `{"name":"...","email":"..."}`
- Response: 201 Created
- Body: `{"id":"new-id","name":"...","email":"...","status":"active"}`

#### Usage Example

```bash
curl -X POST http://localhost:8080/api/users \
  -H "Content-Type: application/json" \
  -d '{"name":"John","email":"john@example.com"}'
```

## Learning Points

1. **Parameter-based branching**: Naming convention `GET@limit__10.res`
2. **Different status codes**: 200 (success) vs 400 (error)
3. **Request body handling**: POST body is ignored (always returns same response)

Customize response files according to your actual API specifications.
