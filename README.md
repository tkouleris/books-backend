# Books Backend API

This project is a Flask-based backend for a book tracking application. It provides APIs for user authentication, library management, reading progress tracking, and dashboard statistics.

## Features

- **Authentication**: User registration, login, and email verification.
- **Library Management**: Store, update, and delete books in a personal library.
- **Reading Progress**: Track current readings and finished books.
- **To Read List**: Manage a prioritized list of books to read next.
- **Profile Management**: Update user profile and avatar.
- **Dashboard**: Get statistics on reading progress and library status.
- **Public Display**: Share reading progress with others via a public profile.
- **Swagger Documentation**: Interactive API documentation at `/apidocs/`.

## Setup and Installation

### Prerequisites

- Python 3.x
- Virtual Environment (recommended)

### Installation Steps

1. **Clone the repository**:
   ```bash
   git clone <repository-url>
   cd books-backend
   ```

2. **Create and activate a virtual environment**:
   ```bash
   py -m venv venv
   .\venv\Scripts\activate
   ```

3. **Install dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

4. **Configure environment variables**:
   Create a `.env` file based on `env.example`:
   ```bash
   cp env.example .env
   ```
   Fill in the required values in `.env`:
   - `SQLALCHEMY_DATABASE_URI`: Database connection string (e.g., MySQL or SQLite).
   - `SECRET_KEY`: A secret key for JWT and session management.
   - `APP_BASE_URL`: The base URL of this backend API.
   - `FRONT_BASE_URL`: The base URL of the frontend application.
   - `BOOK_IMAGES_FOLDER`: Path to store book cover images.
   - `USER_IMAGES_FOLDER`: Path to store user avatar images.
   - `MAILGUN_*`: Configuration for email sending via Mailgun.

5. **Run database migrations**:
   ```bash
   flask db upgrade
   ```

6. **Start the application**:
   ```bash
   flask run
   ```

## API Documentation

The API documentation is available via Swagger UI. Once the application is running, navigate to:
`http://localhost:5000/apidocs/` (or your configured `APP_BASE_URL/apidocs/`)

### Endpoints Overview

#### Auth
- `POST /api/register`: Register a new user.
  - Parameters: `username`, `email`, `password` (JSON).
- `POST /api/login`: User login and receive JWT.
  - Parameters: `email`, `password` (JSON).
- `POST /api/user/verify`: Verify user email using token.
  - Parameters: `token` (JSON).
- `POST /api/user/verify/request`: Request a new verification email. (Requires Token)
- `POST /api/user/forgot-password`: Request a password reset email.
  - Parameters: `email` (JSON).
- `POST /api/user/recover-password`: Recover password using token.
  - Parameters: `token`, `password` (JSON).

#### Library
- `POST /api/library/book/`: Store or update a book. (Requires Token)
  - Parameters: `title` (required), `author`, `book_id` (optional for update), `file` (image).
- `GET /api/library/book/<book_id>`: Get specific book details. (Requires Token)
  - Parameters: `book_id` (path).
- `GET /api/library/books/`: Get all books for the current user. (Requires Token)
  - Parameters: `page` (query), `title` (query filter).
- `DELETE /api/library/book/<book_id>`: Delete a book. (Requires Token)
  - Parameters: `book_id` (path).

#### Readings
- `POST /api/readings/store/`: Store or update a reading record. (Requires Token)
  - Parameters: `book_id`, `reading_id` (optional), `start_date`, `end_date`, `status` (JSON).
- `GET /api/readings/latest/<username>`: Get latest and current readings for a user.
  - Parameters: `username` (path).
- `GET /api/readings/all/`: Get all readings for the current user. (Requires Token)
  - Parameters: `page` (query).
- `GET /api/readings/reading/<id>`: Get a specific reading record. (Requires Token)
  - Parameters: `id` (path).
- `DELETE /api/readings/reading/<id>`: Delete a reading record. (Requires Token)
  - Parameters: `id` (path).

#### To Read
- `POST /api/to_read/store/`: Add a book to the 'to read' list. (Requires Token)
  - Parameters: `book_id` (JSON).
- `POST /api/to_read/reorder/`: Reorder books in the 'to read' list. (Requires Token)
  - Parameters: `id_1`, `id_2` (JSON).
- `GET /api/to_read/list/`: Get the 'to read' list for the current user. (Requires Token)
- `DELETE /api/to_read/book/<book_id>`: Remove a book from the 'to read' list. (Requires Token)
  - Parameters: `book_id` (path).

#### Profile
- `GET /api/profile`: Get the current user's profile. (Requires Token)
- `POST /api/profile`: Update user profile and avatar. (Requires Token)
  - Parameters: `username`, `email`, `file` (image).

#### Dashboard
- `GET /api/dashboard/`: Get dashboard statistics (book counts, reading progress). (Requires Token)

#### Display
- `GET /api/display/<username>`: Public display of user's readings and profile.
  - Parameters: `username` (path).

## Built With

- **Flask**: Web framework.
- **SQLAlchemy**: ORM for database management.
- **Flasgger**: Swagger documentation.
- **PyJWT**: JWT authentication.
- **Flask-Migrate**: Database migrations.
