# Authentication API Endpoints

This section details the authentication process for the API, including user registration, login, and logout functionalities. Authentication is handled using **Laravel Sanctum** for token-based API authentication.

## 1. User Registration

Registers a new user in the system. Upon successful registration, an access token is returned, which should be used for subsequent authenticated requests.

*   **Endpoint:** `/api/register`
*   **Method:** `POST`
*   **Headers:**
    *   `Accept: application/json`
    *   `Content-Type: application/json`
*   **Request Body (JSON):**
    ```json
    {
        "name": "John Doe",
        "email": "john.doe@example.com",
        "password": "password123",
        "password_confirmation": "password123"
    }
    ```
*   **Response (JSON - Success):**
    ```json
    {
        "message": "User registered successfully",
        "access_token": "<SANCTUM_GENERATED_TOKEN>",
        "token_type": "Bearer"
    }
    ```
*   **Response (JSON - Error - Validation):** (HTTP 422 Unprocessable Entity)
    ```json
    {
        "message": "The given data was invalid.",
        "errors": {
            "email": ["The email has already been taken."]
        }
    }
    ```

## 2. User Login

Authenticates an existing user and returns a new access token. This token should be used for all protected API routes.

*   **Endpoint:** `/api/login`
*   **Method:** `POST`
*   **Headers:**
    *   `Accept: application/json`
    *   `Content-Type: application/json`
*   **Request Body (JSON):**
    ```json
    {
        "email": "john.doe@example.com",
        "password": "password123"
    }
    ```
*   **Response (JSON - Success):**
    ```json
    {
        "message": "Login successful",
        "access_token": "<SANCTUM_GENERATED_TOKEN>",
        "token_type": "Bearer"
    }
    ```
*   **Response (JSON - Error - Invalid Credentials):** (HTTP 422 Unprocessable Entity)
    ```json
    {
        "message": "The given data was invalid.",
        "errors": {
            "email": ["These credentials do not match our records."]
        }
    }
    ```

## 3. User Logout

Invalidates the current access token, logging out the user from the current session. The token will no longer be valid for protected routes.

*   **Endpoint:** `/api/logout`
*   **Method:** `POST`
*   **Headers:**
    *   `Accept: application/json`
    *   `Authorization: Bearer <YOUR_ACCESS_TOKEN>`
*   **Response (JSON - Success):** (HTTP 200 OK)
    ```json
    {
        "message": "Logged out successfully"
    }
    ```

## 4. Get Authenticated User Details

Retrieves the details of the currently authenticated user.

*   **Endpoint:** `/api/user`
*   **Method:** `GET`
*   **Headers:**
    *   `Accept: application/json`
    *   `Authorization: Bearer <YOUR_ACCESS_TOKEN>`
*   **Response (JSON - Success):**
    ```json
    {
        "data": {
            "id": 1,
            "name": "John Doe",
            "email": "john.doe@example.com",
            "role": "user",
            "created_at": "2023-10-27 10:00:00",
            "updated_at": "2023-10-27 10:00:00"
        }
    }
    ```
*   **Response (JSON - Error - Unauthenticated):** (HTTP 401 Unauthorized)
    ```json
    {
        "message": "Unauthenticated."
    }
    ``` 