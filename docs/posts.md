# Posts API Endpoints

This section outlines the API endpoints for managing blog posts, including CRUD operations, data filtering, and pagination. Posts are associated with users and categories.

## Base Endpoint

All post-related API endpoints are prefixed with `/api/posts`.

## 1. Get All Posts

Retrieves a paginated list of all posts. Supports filtering by `title` (search), `category_id`, and `user_id`.

*   **Endpoint:** `/api/posts`
*   **Method:** `GET`
*   **Headers:**
    *   `Accept: application/json`
    *   `Authorization: Bearer <YOUR_ACCESS_TOKEN>`
*   **Query Parameters (Optional):**
    *   `title`: Filter posts by title (case-insensitive partial match).
    *   `category_id`: Filter posts by a specific category ID.
    *   `user_id`: Filter posts by a specific user ID.
    *   `page`: The page number for pagination (e.g., `?page=2`).
    *   `per_page`: Number of items per page (e.g., `?per_page=10`).
*   **Example Request:** `GET /api/posts?title=laravel&category_id=1&page=1`
*   **Response (JSON - Success):**
    ```json
    {
        "data": [
            {
                "id": 1,
                "user": {
                    "id": 1,
                    "name": "John Doe",
                    "email": "john.doe@example.com"
                },
                "category": {
                    "id": 1,
                    "name": "Programming"
                },
                "title": "My First Laravel Post",
                "content": "This is the content of my first Laravel post.",
                "created_at": "2023-10-27T10:00:00.000000Z",
                "updated_at": "2023-10-27T10:00:00.000000Z"
            }
        ],
        "links": {
            "first": "http://127.0.0.1:8000/api/posts?page=1",
            "last": "http://127.0.0.1:8000/api/posts?page=1",
            "prev": null,
            "next": null
        },
        "meta": {
            "current_page": 1,
            "from": 1,
            "last_page": 1,
            "links": [
                // ... pagination links ...
            ],
            "path": "http://127.0.0.1:8000/api/posts",
            "per_page": 15,
            "to": 1,
            "total": 1
        }
    }
    ```

## 2. Create a New Post

Creates a new post. The authenticated user will be automatically assigned as the author. Requires `admin` role or the authenticated user to be the owner.

*   **Endpoint:** `/api/posts`
*   **Method:** `POST`
*   **Headers:**
    *   `Accept: application/json`
    *   `Content-Type: application/json`
    *   `Authorization: Bearer <YOUR_ACCESS_TOKEN>`
*   **Request Body (JSON):**
    ```json
    {
        "title": "New Post Title",
        "content": "This is the content for the new post.",
        "category_id": 1 
    }
    ```
*   **Response (JSON - Success):** (HTTP 201 Created)
    ```json
    {
        "data": {
            "id": 2,
            "user": {
                "id": 1,
                "name": "John Doe",
                "email": "john.doe@example.com"
            },
            "category": {
                "id": 1,
                "name": "Programming"
            },
            "title": "New Post Title",
            "content": "This is the content for the new post.",
            "created_at": "2023-10-27T10:05:00.000000Z",
            "updated_at": "2023-10-27T10:05:00.000000Z"
        }
    }
    ```
*   **Response (JSON - Error - Validation):** (HTTP 422 Unprocessable Entity)
    ```json
    {
        "message": "The given data was invalid.",
        "errors": {
            "title": ["The title field is required."],
            "category_id": ["The selected category id is invalid."]
        }
    }
    ```

## 3. Get a Specific Post

Retrieves details of a single post by its ID.

*   **Endpoint:** `/api/posts/{id}`
*   **Method:** `GET`
*   **Headers:**
    *   `Accept: application/json`
    *   `Authorization: Bearer <YOUR_ACCESS_TOKEN>`
*   **URL Parameters:**
    *   `{id}`: The ID of the post.
*   **Response (JSON - Success):**
    ```json
    {
        "data": {
            "id": 1,
            "user": {
                "id": 1,
                "name": "John Doe",
                "email": "john.doe@example.com"
            },
            "category": {
                "id": 1,
                "name": "Programming"
            },
            "title": "My First Laravel Post",
            "content": "This is the content of my first Laravel post.",
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

## 4. Update a Post

Updates an existing post by its ID. Only the post owner or an `admin` can update a post.

*   **Endpoint:** `/api/posts/{id}`
*   **Method:** `PUT` (or `PATCH`)
*   **Headers:**
    *   `Accept: application/json`
    *   `Content-Type: application/json`
    *   `Authorization: Bearer <YOUR_ACCESS_TOKEN>`
*   **URL Parameters:**
    *   `{id}`: The ID of the post.
*   **Request Body (JSON - Partial or Full Update):**
    ```json
    {
        "title": "Updated Post Title",
        "content": "Updated content for the post.",
        "category_id": 2
    }
    ```
*   **Response (JSON - Success):**
    ```json
    {
        "data": {
            "id": 1,
            "user": {
                "id": 1,
                "name": "John Doe",
                "email": "john.doe@example.com"
            },
            "category": {
                "id": 2,
                "name": "Web Development"
            },
            "title": "Updated Post Title",
            "content": "Updated content for the post.",
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

## 5. Delete a Post

Deletes an existing post by its ID. Only the post owner or an `admin` can delete a post.

*   **Endpoint:** `/api/posts/{id}`
*   **Method:** `DELETE`
*   **Headers:**
    *   `Accept: application/json`
    *   `Authorization: Bearer <YOUR_ACCESS_TOKEN>`
*   **URL Parameters:**
    *   `{id}`: The ID of the post.
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