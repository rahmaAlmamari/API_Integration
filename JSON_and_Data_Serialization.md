# **JSON & Data Serialization in API**

**Overview:**

This phase covers how data is formatted and exchanged between clients and servers. 
Think of this as the language and packaging for your API communication.

## 1. JSON (JavaScript Object Notation)

**Core Concept:**

JSON is a lightweight, human-readable data format that uses key-value pairs and 
arrays to structure data. It's become the universal language for web APIs.

**Real-World Example:**

- Filling out a form:
```json
{
  "firstName": "John",
  "lastName": "Doe",
  "age": 30,
  "isEmployed": true,
  "hobbies": ["reading", "hiking", "coding"],
  "address": {
    "street": "123 Main St",
    "city": "Boston",
    "zipCode": "02101"
  }
}
```

- Technical Details

  - **Lightweight:** Much smaller than XML.
  - **Language Independent:** Works with any programming language.
  - **Easy to Parse:** Both humans and machines can read it easily.

## 2. Data Serialization & Deserialization

**Core Concept:**

The process of converting data structures or objects into 
a format that can be stored/transmitted (serialization) and 
then reconstructed later (deserialization).

**Real-World Example:**

**Packing for moving vs. Unpacking:**

- Serialization = Carefully packing your dishes into boxes with labels.
- Deserialization = Unpacking the boxes and putting dishes back in cabinets.

**Technical Example:**
```javascript
// Serialization (Object → JSON string)
const user = { name: "Alice", age: 25 };
const jsonString = JSON.stringify(user);
// Result: '{"name":"Alice","age":25}'

// Deserialization (JSON string → Object)
const jsonString = '{"name":"Alice","age":25}';
const userObject = JSON.parse(jsonString);
// Result: { name: "Alice", age: 25 }
```

## 3. JSON Data Types

**Supported Types:**

|Type    |Example          |Description                |
|--------|-----------------|---------------------------|
|String  |"hello"          |Text in double quotes      |
|Number  |42, 3.14         |Integer or floating point  |
|Boolean |true, false      |True/false values          |
|Array   |[1, 2, 3]        |Ordered list of values     |
|Object	 |{"key": "value"} |Unordered key-value pairs  |
|null	 |null	           |Empty value                |

**Important Notes**

- **No Comments:** JSON doesn't support comments.
- **Double Quotes Only:** Keys and strings must use double quotes.
- **No Functions:** Cannot store executable code.
- **No Dates:** Dates are typically stored as strings.

## 4. JSON Schema

**Core Concept:**

A vocabulary that allows you to annotate and validate JSON documents. 
It's like a blueprint for your JSON data.

**Real-World Example:**

Building permit requirements that specify what information must be provided and in what format.

**Technical Example:**
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "properties": {
    "name": {
      "type": "string",
      "minLength": 1,
      "maxLength": 100
    },
    "email": {
      "type": "string",
      "format": "email"
    },
    "age": {
      "type": "integer",
      "minimum": 0,
      "maximum": 150
    }
  },
  "required": ["name", "email"]
}
```
## 5. Data Validation

**Core Concept:**

Ensuring that incoming data meets specific criteria before processing it.

**Real-World Example:**

Airport security check - verifying you have a valid ticket, passport, and luggage meets requirements before boarding.

- Common Validation Rules

  - **Required Fields:** Must be present
  - **Data Types:** String, number, boolean, etc.
  - **Formats:** Email, URL, date, etc.
  - **Ranges:** Minimum/maximum values
  - **Patterns:** Regular expression matching

**Technical Example:**

```javascript
// Example validation for user registration
const userSchema = {
  username: { type: 'string', minLength: 3, maxLength: 20 },
  email: { type: 'string', format: 'email' },
  age: { type: 'number', minimum: 18 },
  password: { type: 'string', minLength: 8 }
};
```

## 6. Data Transformation

**Core Concept:**

Converting data from one structure to another, often to match different system requirements.

**Real-World Example:**

Currency exchange - converting USD to EUR, maintaining the value but changing the representation.

**Technical Examples:**

Snake Case vs Camel Case

```json
// API Response (snake_case)
{
  "user_id": 123,
  "first_name": "John",
  "created_at": "2023-01-15"
}

// Frontend Expects (camelCase)
{
  "userId": 123,
  "firstName": "John",
  "createdAt": "2023-01-15"
}
```

Nested Object Flattening

```json
// Complex nested structure
{
  "user": {
    "profile": {
      "personal": {
        "name": "Alice",
        "age": 30
      }
    }
  }
}

// Flattened structure
{
  "userName": "Alice",
  "userAge": 30
}
```

## 7. Common JSON Patterns in APIs

**Standard Response Format:**

```json
{
  "status": "success",
  "data": {
    "id": 1,
    "name": "John Doe",
    "email": "john@example.com"
  },
  "message": "User retrieved successfully"
}
```

**Error Response Format:**

```json
{
  "status": "error",
  "message": "Validation failed",
  "code": "VALIDATION_ERROR",
  "details": [
    {
      "field": "email",
      "message": "Invalid email format"
    }
  ]
}
```

**Paginated Response:**

```json
{
  "data": [
    { "id": 1, "name": "User 1" },
    { "id": 2, "name": "User 2" }
  ],
  "pagination": {
    "currentPage": 1,
    "totalPages": 5,
    "totalItems": 50,
    "itemsPerPage": 10
  }
}
```

## 8. Best Practices for JSON in APIs

**1. Consistent Naming Conventions**

- Choose snake_case or camelCase and stick with it.
- Be consistent across your entire API.

**2. Meaningful Data Structures**

```json
// Good - Clear structure
{
  "user": {
    "id": 1,
    "profile": {
      "name": "John",
      "avatar": "url"
    }
  }
}

// Avoid - Overly flat or nested
{
  "userId": 1,
  "userProfileName": "John",
  "userProfileAvatar": "url"
}
```

**3. Proper Error Handling**

```json
{
  "error": {
    "code": "INVALID_EMAIL",
    "message": "The provided email is invalid",
    "field": "email",
    "suggestion": "Please check the email format"
  }
}
```

**4. Date and Time Formatting**

- Use ISO 8601 format: "2023-12-25T10:30:00Z".
- Include timezone information.
- Be consistent across all date fields.

**5. Handling Empty Values**

```json
{
  "activeUsers": [],
  "recentActivity": null,
  "totalCount": 0
}
```

**9. Common Pitfalls to Avoid**

**1. Mixing Data Types**

```json
// Avoid - Inconsistent types
{
  "users": [
    { "id": "1", "name": "John" },    // ID as string
    { "id": 2, "name": "Jane" }       // ID as number
  ]
}
```

**2. Over-nesting**

```json
// Avoid - Too deep nesting
{
  "response": {
    "data": {
      "users": {
        "list": {
          "items": [
            { "name": "John" }
          ]
        }
      }
    }
  }
}
```

**3. Inconsistent Error Responses**

```json
// Avoid - Different error formats
// Endpoint 1
{ "error": "Invalid email" }

// Endpoint 2  
{ "message": "Email is not valid", "code": 400 }

// Endpoint 3
{ "success": false, "err_msg": "Bad email" }
```

