# Categories API Endpoints

This section details the API endpoints for managing categories. Categories are used to organize posts.

## Base Endpoint

All category-related API endpoints are prefixed with `/api/categories`.

## 1. Get All Categories

Retrieves a list of all categories.

*   **Endpoint:** `/api/categories`
*   **Method:** `GET`
*   **Headers:**
    *   `Accept: application/json`
    *   `Authorization: Bearer <YOUR_ACCESS_TOKEN>`
*   **Response (JSON - Success):**
    ```json
    {
        "data": [
            {
                "id": 1,
                "name": "Programming",
                "created_at": "2023-10-27T10:00:00.000000Z",
                "updated_at": "2023-10-27T10:00:00.000000Z"
            },
            {
                "id": 2,
                "name": "Web Development",
                "created_at": "2023-10-27T10:01:00.000000Z",
                "updated_at": "2023-10-27T10:01:00.000000Z"
            }
        ]
    }
    ```

## 2. Create a New Category

Creates a new category. Only users with the `admin` role can create categories.

*   **Endpoint:** `/api/categories`
*   **Method:** `POST`
*   **Headers:**
    *   `Accept: application/json`
    *   `Content-Type: application/json`
    *   `Authorization: Bearer <YOUR_ACCESS_TOKEN>`
*   **Request Body (JSON):**
    ```json
    {
        "name": "New Category Name"
    }
    ```
*   **Response (JSON - Success):** (HTTP 201 Created)
    ```json
    {
        "data": {
            "id": 3,
            "name": "New Category Name",
            "created_at": "2023-10-27T10:10:00.000000Z",
            "updated_at": "2023-10-27T10:10:00.000000Z"
        }
    }
    ```
*   **Response (JSON - Error - Validation):** (HTTP 422 Unprocessable Entity)
    ```json
    {
        "message": "The given data was invalid.",
        "errors": {
            "name": ["The name field is required."]
        }
    }
    ```
*   **Response (JSON - Error - Unauthorized):** (HTTP 403 Forbidden)
    ```json
    {
        "message": "This action is unauthorized."
    }
    ```

## 3. Get a Specific Category

Retrieves details of a single category by its ID.

*   **Endpoint:** `/api/categories/{id}`
*   **Method:** `GET`
*   **Headers:**
    *   `Accept: application/json`
    *   `Authorization: Bearer <YOUR_ACCESS_TOKEN>`
*   **URL Parameters:**
    *   `{id}`: The ID of the category.
*   **Response (JSON - Success):**
    ```json
    {
        "data": {
            "id": 1,
            "name": "Programming",
            "created_at": "2023-10-27T10:00:00.000000Z",
            "updated_at": "2023-10-27T10:00:00.000000Z"
        }
    }
    ```
*   **Response (JSON - Error - Not Found):** (HTTP 404 Not Found)
    ```json
    {
        "message": "Resource not found."
    }
    ```

## 4. Update a Category

Updates an existing category by its ID. Only users with the `admin` role can update categories.

*   **Endpoint:** `/api/categories/{id}`
*   **Method:** `PUT` (or `PATCH`)
*   **Headers:**
    *   `Accept: application/json`
    *   `Content-Type: application/json`
    *   `Authorization: Bearer <YOUR_ACCESS_TOKEN>`
*   **URL Parameters:**
    *   `{id}`: The ID of the category.
*   **Request Body (JSON - Partial or Full Update):**
    ```json
    {
        "name": "Updated Category Name"
    }
    ```
*   **Response (JSON - Success):**
    ```json
    {
        "data": {
            "id": 1,
            "name": "Updated Category Name",
            "created_at": "2023-10-27T10:00:00.000000Z",
            "updated_at": "2023-10-27T10:15:00.000000Z"
        }
    }
    ```
*   **Response (JSON - Error - Unauthorized):** (HTTP 403 Forbidden)
    ```json
    {
        "message": "This action is unauthorized."
    }
    ```

## 5. Delete a Category

Deletes an existing category by its ID. Only users with the `admin` role can delete categories.

*   **Endpoint:** `/api/categories/{id}`
*   **Method:** `DELETE`
*   **Headers:**
    *   `Accept: application/json`
    *   `Authorization: Bearer <YOUR_ACCESS_TOKEN>`
*   **URL Parameters:**
    *   `{id}`: The ID of the category.
*   **Response (JSON - Success):** (HTTP 204 No Content)
    ```json
    {}
    ```
*   **Response (JSON - Error - Unauthorized):** (HTTP 403 Forbidden)
    ```json
    {
        "message": "This action is unauthorized."
    }
    ``` 