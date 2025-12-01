# User Management API Documentation

This API provides a simple user management system with features including user creation, login, token generation, role management, and user listing. It supports standard user authentication.

**Content-Type:** `application/json`

---

## Table of Contents

1. [Authentication](#authentication)
2. [Endpoints](#endpoints)

   * [Create User](#create-user)
   * [Login](#login)
   * [Delete Account](#delete-account)
   * [List Users](#list-users)
   * [Get User by ID](#get-user-by-id)
   * [Validate Token](#validate-token)
   * [Upgrade Role](#upgrade-role)
   * [Generate NanoID](#generate-nanoid)
3. [Response Codes](#response-codes)
4. [User Roles](#user-roles)

---

## Authentication

### Tokens

* Users receive tokens upon login (`createToken`).
* Tokens are required for operations like deleting an account, upgrading roles, or listing users.

---

## Endpoints

### Create User

**URL:** `/create-user`
**Method:** `POST`

**Request Body:**

```json
{
  "username": "user1",
  "password": "pass123",
  "userId": "optional-uuid",
  "origin": "optional-origin",
  "role": "100",
  "token": "optional-admin-token"
}
```

**Notes:**

* `role` defaults to `"100"` (standard user).
* Only admins (role `"001"`) can assign roles other than `"100"`.

**Response Example:**

```json
{
  "code": "A100-00",
  "message": "Success",
  "userId": "generated-uuid"
}
```

---

### Login

**URL:** `/login`
**Method:** `POST`

**Request Body:**

```json
{
  "username": "user1",
  "password": "pass123"
}
```

**Response Example:**

```json
{
  "code": "A100-00",
  "message": "Success",
  "token": "generated-token",
  "expires_at": 1700000000
}
```

---

### Delete Account

**URL:** `/delete-account`
**Method:** `DELETE`

**Request Body:**

```json
{
  "token": "user-token"
}
```

**Response Example:**

```json
{
  "code": "A100-00",
  "message": "Success"
}
```

---

### List Users

**URL:** `/list-users`
**Method:** `GET`

**Query Parameters:**

```http
?token=<user-token>
```

**Response Example:**

```json
{
  "code": "A100-00",
  "message": "Success",
  "users": {
    "user1": { "usr-id": "uuid1", "usr-role": "100" },
    "user2": { "usr-id": "uuid2", "usr-role": "001" }
  }
}
```

---

### Get User by ID

**URL:** `/get-user-by-id`
**Method:** `GET`

**Query Parameters:**

```http
?userId=<uuid>
```

**Response Example:**

```json
{
  "code": "A100-00",
  "message": "Success",
  "user": { "username": "user1", "usr-id": "uuid1" }
}
```

---

### Validate Token

**URL:** `/validate-token`
**Method:** `POST`

**Request Body:**

```json
{
  "token": "user-token"
}
```

**Response Example:**

```json
{
  "code": "A100-00",
  "message": "Success",
  "username": "user1"
}
```

---

### Upgrade Role

**URL:** `/upgrade-role`
**Method:** `POST`

**Request Body:**

```json
{
  "token": "admin-token",
  "targetUsername": "user1",
  "newRole": "001"
}
```

**Response Example:**

```json
{
  "code": "A100-00",
  "message": "Success"
}
```

---

### Generate NanoID

**URL:** `/generate-nanoid`
**Method:** `GET`

**Query Parameters:**

```http
?length=32
```

**Response Example:**

```json
{
  "code": "A100-00",
  "message": "Success",
  "nanoid": "random-generated-id"
}
```

---

## Response Codes

| Code | Message                  |
| ---- | ------------------------ |
| A100 | Success                  |
| A200 | Error occurred           |
| A201 | Failed                   |
| A202 | Invalid input            |
| A203 | Permission denied        |
| A204 | Invalid or expired token |
| A205 | Item not found           |
| A206 | Invalid HTTP method      |

---

## User Roles

| Role Code | Description   |
| --------- | ------------- |
| 001       | Admin         |
| 100       | Standard User |

---
