# API Error Handling

This section describes the standardized error response format and common error scenarios handled by the API. The API is designed to return consistent, JSON-formatted error messages to facilitate easier debugging and integration with the frontend.

## Standard Error Response Format

Most errors will adhere to the following JSON structure, providing a clear message and, where applicable, detailed validation errors.

```json
{
    "message": "A descriptive error message.",
    "errors": {
        "field_name": [
            "Error message for field_name 1.",
            "Error message for field_name 2."
        ],
        "another_field": [
            "Error message for another_field."
        ]
    }
}
```

*   `message`: A general, human-readable message indicating the nature of the error.
*   `errors`: (Optional) An object containing specific validation errors. The keys are the field names, and the values are arrays of error messages for that field. This object is typically present for `HTTP 422 Unprocessable Entity` errors.

## Common Error Scenarios

Here are some common HTTP status codes and their corresponding JSON error responses you might encounter:

### 1. HTTP 401 Unauthorized

This error occurs when a request is made to a protected endpoint without a valid authentication token, or if the token is expired/invalid.

*   **Status Code:** `401 Unauthorized`
*   **Example Response:**
    ```json
    {
        "message": "Unauthenticated."
    }
    ```

### 2. HTTP 403 Forbidden

This error indicates that the authenticated user does not have the necessary permissions to perform the requested action. This is commonly seen with role-based access control (e.g., a `user` role trying to perform an `admin` action).

*   **Status Code:** `403 Forbidden`
*   **Example Response:**
    ```json
    {
        "message": "This action is unauthorized."
    }
    ```

### 3. HTTP 404 Not Found

This error is returned when the requested resource (e.g., a post, category, or user) does not exist.

*   **Status Code:** `404 Not Found`
*   **Example Response:**
    ```json
    {
        "message": "Resource not found."
    }
    ```

### 4. HTTP 405 Method Not Allowed

This error occurs when the request method (e.g., `GET`, `POST`, `PUT`, `DELETE`) used for an endpoint is not supported by that endpoint.

*   **Status Code:** `405 Method Not Allowed`
*   **Example Response:**
    ```json
    {
        "message": "The GET method is not supported for this route. Supported methods: POST."
    }
    ```

### 5. HTTP 422 Unprocessable Entity (Validation Errors)

This is the most common error for invalid user input. It signifies that the server understands the content type of the request entity, and the syntax of the request entity is correct, but it was unable to process the contained instructions.

*   **Status Code:** `422 Unprocessable Entity`
*   **Example Response:**
    ```json
    {
        "message": "The given data was invalid.",
        "errors": {
            "email": ["The email field is required.", "The email must be a valid email address."],
            "password": ["The password field is required."]
        }
    }
    ```

### 6. HTTP 500 Internal Server Error

This is a generic error message, given when an unexpected condition was encountered and no more specific message is suitable. This usually indicates a server-side issue that needs to be investigated by the API developers.

*   **Status Code:** `500 Internal Server Error`
*   **Example Response (during development/debug mode):
    ```json
    {
        "message": "Server Error",
        "exception": "Illuminate\\Database\\QueryException",
        "file": "/path/to/laravel/vendor/laravel/framework/src/Illuminate/Database/Connection.php",
        "line": 712,
        "trace": [
            // ... stack trace ...
        ]
    }
    ```
    *Note: In production environments, the detailed `exception`, `file`, `line`, and `trace` information should be hidden for security reasons.*

## Custom Error Handling

Specific error handling logic is implemented in `bootstrap/app.php` within the `withExceptions` method to standardize JSON responses for common HTTP errors (401, 403, 404, 405, 422). 