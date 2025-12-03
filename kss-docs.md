# Application & API Key Management System Documentation

## Base URL
`http://lifetheuniverseandeverything.co.uk/api/BSA/kss/`

## Overview
This API provides comprehensive application and API key management functionality with secure token-based authentication. Each user can create multiple applications, each with multiple API keys. The system validates tokens against an external authentication service and stores application data in a local JSON database.

## Authentication
All endpoints (except `/verify/{appId}/{key}`) require authentication via Bearer token.

### Token Format
```
Authorization: Bearer {your_token}
```

Alternatively, you can pass the token as a query parameter:
```
?token={your_token}
```

## Endpoints

### 1. Initialize User
**GET** `/int`

Initializes a user session and generates an internal ID.

**Response:**
```json
{
  "code": "A100-00",
  "message": "User initialized successfully",
  "timestamp": 1640995200,
  "data": {
    "user_id": "external-user-id",
    "username": "john_doe",
    "internal_id": "random-nanoid",
    "role": "user",
    "authenticated": true
  }
}
```

---

### 2. List All Applications
**GET** `/list`

Retrieves all applications owned by the authenticated user.

**Response:**
```json
{
  "code": "A100-00",
  "message": "Success",
  "timestamp": 1640995200,
  "data": {
    "user_id": "external-user-id",
    "username": "john_doe",
    "applications": {
      "app-id-123": {
        "name": "My App",
        "created_at": "2024-01-01 12:00:00",
        "updated_at": "2024-01-01 12:00:00",
        "api_keys": {
          "key_123": {
            "name": "Production Key",
            "created_at": "2024-01-01 12:00:00",
            "last_used": "2024-01-02 14:30:00",
            "usage_count": 42,
            "is_active": true,
            "revoked": false,
            "revoked_at": null
          }
        }
      }
    },
    "total_applications": 1
  }
}
```

---

### 3. Get Application Details
**GET** `/get/{applicationId}`

Retrieves detailed information about a specific application.

**Path Parameters:**
- `applicationId`: The ID of the application to retrieve

**Response:**
```json
{
  "code": "A100-00",
  "message": "Success",
  "timestamp": 1640995200,
  "data": {
    "application_id": "app-id-123",
    "name": "My Application",
    "owner_id": "external-user-id",
    "created_at": "2024-01-01 12:00:00",
    "updated_at": "2024-01-01 12:00:00",
    "api_keys": {
      "key_123": {
        "name": "Production Key",
        "created_at": "2024-01-01 12:00:00",
        "last_used": "2024-01-02 14:30:00",
        "usage_count": 42,
        "is_active": true,
        "revoked": false,
        "revoked_at": null,
        "revoked_reason": null
      }
    },
    "total_keys": 1
  }
}
```

---

### 4. Create Application
**POST** `/create-application/{applicationName}`

Creates a new application. The system automatically generates an application ID.

**Path Parameters:**
- `applicationName`: URL-encoded name for the new application

**Response:**
```json
{
  "code": "A100-00",
  "message": "Application created successfully",
  "timestamp": 1640995200,
  "data": {
    "application_id": "generated-app-id",
    "name": "My New App",
    "owner_id": "external-user-id",
    "created_at": "2024-01-01 12:00:00"
  }
}
```

---

### 5. Update Application
**PUT** `/update-application/{applicationId}/{newName}`

Updates the name of an existing application.

**Path Parameters:**
- `applicationId`: The ID of the application to update
- `newName`: URL-encoded new name for the application

**Response:**
```json
{
  "code": "A100-00",
  "message": "Application updated successfully",
  "timestamp": 1640995200,
  "data": {
    "application_id": "app-id-123",
    "name": "Updated App Name",
    "owner_id": "external-user-id",
    "updated_at": "2024-01-02 14:30:00"
  }
}
```

---

### 6. Delete Application
**DELETE** `/delete-application/{applicationId}`

Deletes an application and all associated API keys.

**Path Parameters:**
- `applicationId`: The ID of the application to delete

**Response:**
```json
{
  "code": "A100-00",
  "message": "Application deleted successfully",
  "timestamp": 1640995200,
  "data": {
    "application_id": "app-id-123",
    "name": "My App",
    "deleted_at": "2024-01-02 14:30:00",
    "api_keys_deleted": 3
  }
}
```

---

### 7. Create API Key
**POST** `/create-key/{applicationId}/{keyName}`

Creates a new API key for an application. **Important:** The API key is only shown once in the response.

**Path Parameters:**
- `applicationId`: The ID of the application
- `keyName`: URL-encoded name for the API key

**Response:**
```json
{
  "code": "A100-00",
  "message": "API key created successfully",
  "timestamp": 1640995200,
  "data": {
    "key_id": "key_67890abcdef",
    "key_name": "Production Key",
    "api_key": "generated-api-key-value",
    "application_id": "app-id-123",
    "created_at": "2024-01-01 12:00:00",
    "warning": "Store this API key securely. It will not be shown again."
  }
}
```

---

### 8. Verify API Key
**GET** `/verify/{applicationId}/{apiKey}`

Verifies if an API key is valid for an application. This endpoint does NOT require authentication.

**Path Parameters:**
- `applicationId`: The ID of the application
- `apiKey`: The API key to verify

**Response (Valid Key):**
```json
{
  "code": "A100-00",
  "message": "API key is valid",
  "timestamp": 1640995200,
  "data": {
    "valid": true,
    "application_id": "app-id-123",
    "key_id": "key_67890abcdef",
    "key_name": "Production Key",
    "verified_at": "2024-01-02 14:30:00"
  }
}
```

**Response (Invalid Key - Status 401):**
```json
{
  "code": "A204",
  "message": "Invalid API key",
  "timestamp": 1640995200
}
```

---

### 9. List API Keys for Application
**GET** `/keys/{applicationId}`

Lists all API keys for a specific application.

**Path Parameters:**
- `applicationId`: The ID of the application

**Response:**
```json
{
  "code": "A100-00",
  "message": "Success",
  "timestamp": 1640995200,
  "data": {
    "application_id": "app-id-123",
    "application_name": "My Application",
    "api_keys": {
      "key_123": {
        "name": "Production Key",
        "created_at": "2024-01-01 12:00:00",
        "last_used": "2024-01-02 14:30:00",
        "usage_count": 42,
        "is_active": true,
        "revoked": false,
        "revoked_at": null,
        "revoked_reason": null
      },
      "key_456": {
        "name": "Test Key",
        "created_at": "2024-01-01 13:00:00",
        "last_used": null,
        "usage_count": 0,
        "is_active": false,
        "revoked": true,
        "revoked_at": "2024-01-01 14:00:00",
        "revoked_reason": "Compromised"
      }
    },
    "total_keys": 2,
    "active_keys": 1,
    "revoked_keys": 1
  }
}
```

---

### 10. Get API Key Details
**GET** `/key/{keyId}`

Retrieves detailed information about a specific API key.

**Path Parameters:**
- `keyId`: The ID of the API key

**Response:**
```json
{
  "code": "A100-00",
  "message": "Success",
  "timestamp": 1640995200,
  "data": {
    "key_id": "key_123",
    "name": "Production Key",
    "application_id": "app-id-123",
    "application_name": "My Application",
    "created_at": "2024-01-01 12:00:00",
    "last_used": "2024-01-02 14:30:00",
    "usage_count": 42,
    "is_active": true,
    "revoked": false,
    "revoked_at": null,
    "revoked_reason": null
  }
}
```

---

### 11. Revoke API Key
**POST** `/revoke-key/{keyId}`

Revokes an active API key. Optionally provide a reason in the request body.

**Path Parameters:**
- `keyId`: The ID of the API key to revoke

**Request Body (Optional):**
```json
{
  "reason": "Key compromised"
}
```

**Response:**
```json
{
  "code": "A100-00",
  "message": "API key revoked successfully",
  "timestamp": 1640995200,
  "data": {
    "key_id": "key_123",
    "key_name": "Production Key",
    "application_id": "app-id-123",
    "application_name": "My Application",
    "revoked_at": "2024-01-02 14:30:00",
    "revoked_reason": "Key compromised",
    "status": "revoked"
  }
}
```

---

### 12. Reactivate API Key
**POST** `/reactivate-key/{keyId}`

Reactivates a previously revoked API key.

**Path Parameters:**
- `keyId`: The ID of the API key to reactivate

**Response:**
```json
{
  "code": "A100-00",
  "message": "API key reactivated successfully",
  "timestamp": 1640995200,
  "data": {
    "key_id": "key_123",
    "key_name": "Production Key",
    "application_id": "app-id-123",
    "application_name": "My Application",
    "reactivated_at": "2024-01-02 14:30:00",
    "status": "active"
  }
}
```

---

### 13. Delete API Key Permanently
**POST** `/delete-key/{keyId}`

Permanently deletes an API key. This action cannot be undone.

**Path Parameters:**
- `keyId`: The ID of the API key to delete

**Response:**
```json
{
  "code": "A100-00",
  "message": "API key deleted permanently",
  "timestamp": 1640995200,
  "data": {
    "key_id": "key_123",
    "key_name": "Production Key",
    "application_id": "app-id-123",
    "application_name": "My Application",
    "deleted_at": "2024-01-02 14:30:00",
    "status": "permanently deleted"
  }
}
```

---

## Error Codes

| Code | Message | Status Code | Description |
|------|---------|-------------|-------------|
| A100-00 | Success | 200 | Request completed successfully |
| A200 | Internal server error | 500 | Unexpected server error |
| A201 | Handler not found / Failed to create/delete | 500 | Server configuration issue or operation failed |
| A202 | Token required | 401 | No authentication token provided |
| A203 | Failed to update/revoke/reactivate | 400 | Operation failed due to validation or permissions |
| A204 | Invalid or expired token / Invalid API key | 401 | Authentication or verification failed |
| A205 | Not found | 404 | Resource not found or no permission to access |

---

## Rate Limiting
Currently no rate limiting is implemented.

---

## Data Storage
- Applications and API keys are stored in `applications_db.json`
- Each user's data is isolated by their user ID
- API keys are stored as hashed values (in the actual implementation, consider adding hashing)
- The database is automatically initialized if it doesn't exist

---

## Important Notes
1. **API Key Security**: When creating an API key, store it immediately as it's only shown once
2. **Token Expiry**: Authentication tokens expire after 24 hours
3. **Ownership**: Users can only access their own applications and keys
4. **URL Encoding**: All path parameters containing special characters should be URL-encoded
5. **CORS**: The API supports CORS for cross-origin requests

---

## Example Usage Flow

1. **Authenticate** with external system to get a token
2. **Initialize** user with `/int` endpoint
3. **Create** an application with `/create-application/MyApp`
4. **Create** API keys with `/create-key/{appId}/Production`
5. **Use** the API key in your application
6. **Verify** keys with `/verify/{appId}/{key}` endpoint
7. **Manage** keys with revoke/reactivate/delete endpoints as needed