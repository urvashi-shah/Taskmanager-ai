# Taskmanager AI API

A Spring Boot REST API for task management with JWT authentication, MongoDB persistence, and AI-powered task enrichment. Users can create tasks, and the backend calls an external AI service to generate a short summary, suggest priority, and provide a first action step.

## Features

- User registration and login with JWT authentication
- Secure task CRUD APIs
- MongoDB database integration
- AI enrichment for newly created tasks
- Priority, summary, and next-step suggestions
- Centralized error handling
- Environment-based configuration for secrets

## Tech Stack

- Java 17
- Spring Boot
- Spring Security
- MongoDB
- Maven
- OpenAI-compatible REST API integration

## Project Structure

```text
src/main/java/com/taskmanager
├── config        # Spring security, app config, exception handling
├── controller    # REST API endpoints
├── dto           # Request and response models
├── model         # MongoDB documents and enums
├── repository    # MongoDB repositories
├── security      # JWT filter, token service, user details service
└── service       # Business logic and AI integration
```

## Getting Started

### Prerequisites

- Java 17
- Maven 3.9+
- MongoDB local instance or MongoDB Atlas connection string

### Configuration

Create your local config file from the example:

```bash
cp src/main/resources/application.example.properties src/main/resources/application.properties
```

Then set these environment variables as needed:

| Variable | Description |
| --- | --- |
| `MONGODB_URI` | MongoDB connection string |
| `JWT_SECRET` | Long random secret used to sign JWT tokens |
| `OPENAI_API_KEY` | API key for AI task enrichment |

The real `application.properties` file is intentionally ignored by Git so credentials are not committed.

## Run Locally

```bash
mvn spring-boot:run
```

The API starts on the default Spring Boot port:

```text
http://localhost:8080
```

## API Endpoints

### Auth

| Method | Endpoint | Description |
| --- | --- | --- |
| `POST` | `/api/auth/register` | Register a new user |
| `POST` | `/api/auth/login` | Login and receive a JWT |

### Tasks

Task endpoints require a JWT in the `Authorization` header:

```text
Authorization: Bearer <token>
```

| Method | Endpoint | Description |
| --- | --- | --- |
| `POST` | `/api/tasks` | Create a task and enrich it with AI |
| `GET` | `/api/tasks` | Get all tasks for the logged-in user |
| `PUT` | `/api/tasks/{taskId}` | Update a task |
| `DELETE` | `/api/tasks/{taskId}` | Delete a task |

## Example Flow

1. Register a user with `/api/auth/register`.
2. Login with `/api/auth/login`.
3. Use the returned JWT to create tasks.
4. When a task is created, the server sends the task description to the AI service.
5. The response includes AI-generated summary, priority, and suggested first step.

## Testing

```bash
mvn test
```

## Security Note

Do not commit local credentials, API keys, database URLs, or JWT secrets. Use environment variables and keep `src/main/resources/application.properties` local only.
