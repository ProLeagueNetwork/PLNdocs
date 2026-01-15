# PLN API Documentation

## Overview

The PLN API provides programmatic access to tournament data, scores, player statistics, and event management through a role-based access control system. The API is organized into multiple levels based on operational requirements and security needs.

## 🎯 API Endpoints

### 🟢 Client API (Public Read-Only)
**Endpoint:** `https://api.proleaguenetwork.com/public/docs/`  
**Access Level:** Public  
**Authentication:** Optional

Read-only access to public PLN data. Perfect for external integrations, websites, and mobile apps.

**Allowed Operations:**
- ✅ GET (Read data)
- ❌ POST, PUT, DELETE (Not allowed)

**Use Cases:**
- Display tournament information on websites
- Show real-time scores and standings
- Access player and team statistics
- Retrieve historical data

**Example Request:**
```bash
curl -X GET "https://api.proleaguenetwork.com/public/api/tournaments" \
  -H "Accept: application/json"
```

---

### 🟡 Internal API (Create/Update Operations)
**Endpoint:** `https://api.proleaguenetwork.com/internal/docs/`  
**Access Level:** Internal Team Members  
**Authentication:** Required (JWT Token)

For PLN team members who need to create and update data.

**Allowed Operations:**
- ✅ GET (Read data)
- ✅ POST (Create new records)
- ✅ PUT/PATCH (Update existing records)
- ❌ DELETE (Requires admin access)

**Use Cases:**
- Create new tournaments, events, matches
- Update scores and statistics
- Modify player and team information
- Manage real-time data updates

**Example Request:**
```bash
curl -X POST "https://api.proleaguenetwork.com/internal/api/tournaments" \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"name": "Summer Championship 2026", "sport": "pickleball"}'
```

---

### 🔴 Admin API (Full CRUD Operations)
**Endpoint:** `https://api.proleaguenetwork.com/admin/docs/`  
**Access Level:** Administrator Only  
**Authentication:** Required (Admin JWT Token)

Full administrative access to all PLN data and operations. Use with caution.

**Allowed Operations:**
- ✅ GET (Read data)
- ✅ POST (Create new records)
- ✅ PUT/PATCH (Update existing records)
- ✅ DELETE (Remove records permanently)

**Use Cases:**
- ⚠️ Delete tournaments, events, matches (PERMANENT)
- Manage user accounts and permissions
- Configure system settings
- Perform bulk operations
- Access sensitive data

**Example Request:**
```bash
curl -X DELETE "https://api.proleaguenetwork.com/admin/api/tournaments/123" \
  -H "Authorization: Bearer YOUR_ADMIN_JWT_TOKEN"
```

**⚠️ Security Notice:**
- All admin actions are logged
- Unauthorized access attempts are tracked
- DELETE operations are permanent and cannot be undone

---

### 📘 Legacy API (Backward Compatibility)
**Endpoint:** `/swagger/`  
**Access Level:** Legacy Support  
**Authentication:** Optional

Original API endpoint maintained for backward compatibility with existing integrations.

**Migration Notice:**  
New integrations should use the appropriate role-based endpoint above. This endpoint may be deprecated in the future.

---

## 🔐 Authentication

### Getting Your JWT Token

1. **For Internal/Admin Access:**
   ```bash
   curl -X 'POST' \
     'https://api.proleaguenetwork.com/internal/api/auth/login' \
     -H 'accept: application/json' \
     -H 'Content-Type: application/json' \
     -d '{
     "EmailOrUserName": "your_email@example.com",
     "Password": "your_password"
   }'
   ```

2. **Response:**
   ```json
   {
     "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
     "role": "internal",
     "expires_in": 3600
   }
   ```

3. **Using the Token:**
   Add the token to your request headers:
   ```
   Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
   ```

### Token Requirements

| API Level | Token Required | Role Requirement |
|-----------|---------------|------------------|
| Client    | No            | N/A              |
| Internal  | Yes           | `internal` or `admin` |
| Admin     | Yes           | `admin` only     |
| Legacy    | Optional      | N/A              |

---

## 🚀 Quick Start Guide

### 1. Choose Your API Level

Determine which API level you need based on your requirements:
- **Just reading data?** → Use Client API
- **Creating/updating data?** → Use Internal API
- **Need full control?** → Use Admin API

### 2. Access Interactive Documentation

Visit the appropriate Swagger UI endpoint to explore available endpoints:

- **Client API:** `https://api.proleaguenetwork.com/public/docs/`
- **Internal API:** `https://api.proleaguenetwork.com/internal/docs/`
- **Admin API:** `https://api.proleaguenetwork.com/admin/docs/`

### 3. Credentials:

https://docs.google.com/document/d/1fInixNm-vMKe649MbhMGCb6xPWh8-vA6ASzxXLPxbgM/edit?tab=t.0

### 4. Test Your First Request

**Example: Get all tournaments (Client API)**
```bash
curl -X GET "https://api.proleaguenetwork.com/public/api/tournaments" \
  -H "Accept: application/json"
```

**Example: Create a tournament (Internal API)**
```bash
# First, get your token
TOKEN=$(curl -X 'POST' \
  'https://api.proleaguenetwork.com/internal/api/auth/login' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{"EmailOrUserName":"your_email@example.com","Password":"your_password"}' \
  | jq -r '.token')

# Then, create the tournament
curl -X POST "https://api.proleaguenetwork.com/internal/api/tournaments" \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Winter Championship 2026",
    "sport": "pickleball",
    "start_date": "2026-02-01",
    "end_date": "2026-02-15"
  }'
```

---

## 📊 Common Operations

### Tournaments

```bash
# Get all tournaments (Public)
GET /public/api/tournaments

# Get a specific tournament (Public)
GET /public/api/tournaments/{tournament_id}

# Create a tournament (Internal)
POST /internal/api/tournaments

# Update a tournament (Internal)
PUT /internal/api/tournaments/{tournament_id}

# Delete a tournament (Admin Only)
DELETE /admin/api/tournaments/{tournament_id}
```

### Events

```bash
# Get events for a tournament (Public)
GET /public/api/tournaments/{tournament_id}/events

# Create an event (Internal)
POST /internal/api/events

# Update an event (Internal)
PUT /internal/api/events/{event_id}
```

### Scores & Statistics

```bash
# Get real-time scores (Public)
GET /public/api/scores/live

# Update scores (Internal)
POST /internal/api/scores

# Get player statistics (Public)
GET /public/api/players/{player_id}/stats
```

---

## 🔒 Security Best Practices

1. **Never expose your JWT tokens** in client-side code or public repositories
2. **Use HTTPS** for all API requests in production
3. **Rotate tokens regularly** - tokens expire after a set period
4. **Implement proper error handling** for authentication failures
5. **Use the minimum required access level** for your use case
6. **Store tokens securely** using environment variables or secure vaults

---

## ⚡ Rate Limiting

Standard rate limits apply to all API endpoints:

- **Client API:** 1000 requests per hour per IP
- **Internal API:** 5000 requests per hour per token
- **Admin API:** 10000 requests per hour per token

Rate limit headers are included in responses:
```
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 999
X-RateLimit-Reset: 1642252800
```

---

## 🐛 Error Handling

### Common HTTP Status Codes

| Code | Meaning | Description |
|------|---------|-------------|
| 200  | OK | Request succeeded |
| 201  | Created | Resource created successfully |
| 400  | Bad Request | Invalid request parameters |
| 401  | Unauthorized | Missing or invalid authentication token |
| 403  | Forbidden | Insufficient permissions for this operation |
| 404  | Not Found | Resource not found |
| 429  | Too Many Requests | Rate limit exceeded |
| 500  | Internal Server Error | Server error occurred |

### Error Response Format

```json
{
  "error": {
    "code": "UNAUTHORIZED",
    "message": "Invalid or expired JWT token",
    "details": "Token expired at 2026-01-15T10:30:00Z"
  }
}
```

---

## 🗺️ Roadmap & Migration

### Current Status (January 2026)
- ✅ All API levels fully operational
- ✅ Legacy endpoint maintained for backward compatibility
- ✅ Role-based access control implemented

### Future Plans
- 🔄 Enhanced webhook support for real-time updates
- 🔄 GraphQL endpoint for complex queries
- 🔄 WebSocket support for live score streaming
- ⚠️ Legacy endpoint may be deprecated (timeline TBD)

### Migration Guide
If you're using the Legacy API (`/swagger/`), consider migrating to:
- `https://api.proleaguenetwork.com/public/docs/` for read-only access
- `https://api.proleaguenetwork.com/internal/docs/` for create/update operations
- `https://api.proleaguenetwork.com/admin/docs/` for full administrative access

---

## 📝 Examples & SDKs

### Python Example
```python
import requests

# Client API - Read tournaments
response = requests.get('https://api.proleaguenetwork.com/public/api/tournaments')
tournaments = response.json()

# Internal API - Create a tournament
headers = {
    'Authorization': 'Bearer YOUR_JWT_TOKEN',
    'Content-Type': 'application/json'
}
data = {
    'name': 'Spring Championship',
    'sport': 'pickleball'
}
response = requests.post(
    'https://api.proleaguenetwork.com/internal/api/tournaments',
    headers=headers,
    json=data
)
```

### JavaScript Example
```javascript
// Client API - Fetch tournaments
fetch('https://api.proleaguenetwork.com/public/api/tournaments')
  .then(response => response.json())
  .then(data => console.log(data));

// Internal API - Create tournament
fetch('https://api.proleaguenetwork.com/internal/api/tournaments', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer YOUR_JWT_TOKEN',
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    name: 'Summer Championship',
    sport: 'pickleball'
  })
})
  .then(response => response.json())
  .then(data => console.log(data));
```

---

## 📄 License & Terms

This API is provided by PLN for authorized use only. By accessing this API, you agree to:
- Use the API in accordance with its intended purpose
- Not attempt to bypass security measures
- Not exceed rate limits through distributed requests
- Report any security vulnerabilities responsibly

---

## 🔄 Changelog

### Version 1.0 (January 2026)
- Initial release of role-based API structure
- Client, Internal, and Admin API levels implemented
- Legacy endpoint maintained for backward compatibility
- JWT authentication system deployed
- Rate limiting implemented

---

**Last Updated:** January 15, 2026  
**API Version:** 1.0  
**Base URL:** `https://api.proleaguenetwork.com`
