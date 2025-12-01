# Inventory API Documentation

**Base URL:** `http://lifetheuniverseandeverything.co.uk/api/BSA/pra/`

This API allows authorized users to manage an inventory through `list` and `add` operations. Access is controlled using **API keys**.

---

## **Authentication**

All requests require a **valid API key**.

Request format for the key:

```json
{
    "key": {
        "key": "YOUR_API_KEY_HERE"
    }
}
```

The API checks the provided key against a list of valid keys defined in `keys.php`.

---

## **Endpoints**

### 1. **List Items**

**Endpoint:** `/list`
**Method:** `POST`

**Request Example:**

```json
{
    "key": {
        "key": "YOUR_API_KEY_HERE"
    }
}
```

**Response Example:**

```json
{
    "code": "S100-00",
    "message": "Success",
    "items": {
        "item1": {
            "amount": 10,
            "storage-source": "warehouse"
        },
        "item2": {
            "amount": 5,
            "storage-source": "storefront"
        }
    }
}
```

**Errors:**

| Code    | Message             | Description                 |
| ------- | ------------------- | --------------------------- |
| S201-01 | Invalid input       | Key missing or wrong format |
| S205-01 | Invalid API key     | Key not in valid keys       |
| S204-01 | Invalid HTTP method | Only POST allowed           |

---

### 2. **Add Items**

**Endpoint:** `/add`
**Method:** `POST`

**Request Example:**

```json
{
    "key": {
        "key": "YOUR_API_KEY_HERE"
    },
    "items": {
        "item1": {
            "amount": 10,
            "storage-source": "warehouse"
        },
        "item2": {
            "amount": 5,
            "storage-source": "storefront"
        }
    }
}
```

**Request Rules:**

* `items` must be a JSON object.
* Each item must include:

  * `amount` (number)
  * `storage-source` (string)

**Response Example:**

```json
{
    "code": "S100-00",
    "message": "Success",
    "added": ["item1", "item2"]
}
```

**Errors:**

| Code    | Message             | Description                      |
| ------- | ------------------- | -------------------------------- |
| S201-02 | Invalid input       | `items` missing or not an object |
| S201-03 | Invalid input       | Item missing required fields     |
| S205-01 | Invalid API key     | Key not in valid keys            |
| S204-01 | Invalid HTTP method | Only POST allowed                |

---

## **Error Codes**

| Code | Message             | Description                             |
| ---- | ------------------- | --------------------------------------- |
| S100 | Success             | Operation completed successfully        |
| S200 | Error               | General server error                    |
| S201 | Invalid input       | Input JSON is invalid or missing fields |
| S202 | Permission denied   | Key is missing or invalid               |
| S203 | Item not found      | Requested item does not exist           |
| S204 | Invalid HTTP method | Request method not allowed              |
| S205 | Invalid API key     | Provided API key is invalid             |

---
