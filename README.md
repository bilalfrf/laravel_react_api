# Laravel RESTful API for React Frontend

This project implements a comprehensive RESTful API using Laravel, designed to serve a React-based frontend application. It provides a secure, structured, and dynamic backend system for managing various data resources, including user authentication, posts, and categories.

## Features

This API is built to be robust and detailed, moving beyond simple data retrieval to include:

*   **User Authentication & Authorization:** Token-based authentication using Laravel Sanctum, supporting user registration, login, and logout. Includes role-based access control (RBAC) with `admin` and `user` roles.
*   **Secure Data Transmission:** All communications are handled via JSON, ensuring secure and standardized data exchange.
*   **Comprehensive Error Handling:** Provides clear, JSON-formatted error responses for various scenarios (e.g., validation errors, authentication failures, unauthorized access, resource not found).
*   **Data Filtering & Pagination:** Advanced filtering options for posts (by title search, category, and user) and efficient data retrieval through pagination.
*   **Relational Data Management:** Manages relationships between entities (e.g., a user can have multiple posts, a post belongs to a category).
*   **Dynamic & Complex API Structure:** Features multiple endpoints and controllers for different resources, with robust data validation and user-specific access rules.
*   **Database Integration:** Utilizes MySQL for data storage, managed through Laravel Migrations and Eloquent ORM.

## Setup & Installation

Follow these steps to get the API up and running on your local machine.

### 1. Clone the Repository (if not already done)

```bash
git clone <your-repository-url>
cd your-api-project-directory
```

### 2. Install Composer Dependencies

```bash
composer install
```

### 3. Configure Environment Variables

Create a `.env` file in the project root by copying the `.env.example` file:

```bash
cp .env.example .env
```

Open `.env` and configure your database connection details (MySQL):

```dotenv
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=your_database_name
DB_USERNAME=your_username
DB_PASSWORD=your_password
```

Replace `your_database_name`, `your_username`, and `your_password` with your actual MySQL credentials.

### 4. Generate Application Key

```bash
php artisan key:generate
```

### 5. Run Database Migrations

This will create all necessary tables, including `users`, `personal_access_tokens`, `posts`, and `categories`, along with their relationships.

```bash
php artisan migrate:fresh
```

### 6. Start the Laravel Development Server

```bash
php artisan serve
```

The API will typically be accessible at `http://127.0.0.1:8000`.

## API Usage (Getting Started)

You can use tools like Postman, Insomnia, or cURL to test the API endpoints. Ensure you send `Content-Type: application/json` and `Accept: application/json` headers with your requests.

### 1. Register a New User

*   **Endpoint:** `POST /api/register`
*   **Body (JSON):**
    ```json
    {
        "name": "Your Name",
        "email": "your_email@example.com",
        "password": "your_password",
        "password_confirmation": "your_password"
    }
    ```
*   **Response:** You will receive a `message`, `access_token`, and `token_type` (Bearer). **Copy this `access_token` for subsequent authenticated requests.**

### 2. Login User (to get a new token or verify credentials)

*   **Endpoint:** `POST /api/login`
*   **Body (JSON):**
    ```json
    {
        "email": "your_email@example.com",
        "password": "your_password"
    }
    ```
*   **Response:** Similar to registration, you'll receive a `message`, `access_token`, and `token_type`.

### 3. Access Authenticated Endpoints (e.g., Get All Posts)

For all protected routes, you must include the `Authorization` header:

*   **Header:** `Authorization: Bearer <YOUR_ACCESS_TOKEN>`

*   **Example Endpoint:** `GET /api/posts`
*   **Headers:**
    *   `Accept: application/json`
    *   `Authorization: Bearer <YOUR_ACCESS_TOKEN>`

## Documentation for Specific Features

For detailed information on each API resource and functionality, refer to the respective Markdown files in the `docs/` directory:

*   [Authentication API](docs/authentication.md)
*   [Posts API](docs/posts.md)
*   [Categories API](docs/categories.md)
*   [API Error Handling](docs/error-handling.md)

## Technologies Used

*   **Backend Framework:** Laravel
*   **Authentication:** Laravel Sanctum (Token-based API Authentication)
*   **Database:** MySQL
*   **ORM:** Eloquent
*   **HTTP Client (for testing):** Postman / Insomnia / cURL (recommended)

---
Developed as a comprehensive backend solution for a React frontend application.
