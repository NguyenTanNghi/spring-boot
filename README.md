# Identity Service

![Java](https://img.shields.io/badge/Java-21-orange?style=for-the-badge&logo=openjdk)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.2.2-6DB33F?style=for-the-badge&logo=springboot&logoColor=white)
![React](https://img.shields.io/badge/React-18.3.1-61DAFB?style=for-the-badge&logo=react&logoColor=111)
![MySQL](https://img.shields.io/badge/MySQL-8.x-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-ready-2496ED?style=for-the-badge&logo=docker&logoColor=white)

A production-style full-stack identity management application built with Spring Boot, JWT authentication, role-based authorization, MySQL, and a React + Material UI client.

The project demonstrates a real authentication flow: user registration, secure password hashing, JWT login, token introspection, refresh/logout token invalidation, protected profile retrieval, and admin-only user management.

---

## 🚀 Introduction

**Identity Service** is a full-stack authentication and authorization system designed as a portfolio-ready backend service with a lightweight React client.

The backend exposes REST APIs under `/identity`, uses Spring Security as an OAuth2 Resource Server, signs JWTs with HS512, stores invalidated tokens for logout/refresh flows, and manages users through roles and permissions.

The frontend provides a login page, stores the access token in browser local storage, protects the home route, and fetches the authenticated user's profile from the backend.

---

## ✨ Features

- 🔐 **JWT authentication** with username/password login
- ♻️ **Refresh token flow** with old token invalidation
- 🚪 **Logout support** by storing invalidated JWT IDs
- 👤 **User registration** with validation rules
- 🛡 **Role-based access control** with `ADMIN` and `USER`
- 🔑 **Permission management** through dedicated permission APIs
- 🧂 **BCrypt password hashing** with strength `10`
- ✅ **Custom validation** for username, password, and date of birth
- 🧯 **Centralized exception handling** with consistent API responses
- 🧬 **DTO mapping** using MapStruct
- 🧪 **Backend tests** with Spring Boot Test, MockMvc, H2, Mockito, and Testcontainers dependencies
- 🐳 **Docker-ready backend** with a multi-stage Dockerfile
- 💻 **React client** with protected routing, Material UI components, and token-based profile loading

---

## 🛠 Tech Stack

| Layer | Technology | Usage |
| --- | --- | --- |
| Backend | Java 21 | Main backend language |
| Backend | Spring Boot 3.2.2 | REST API application framework |
| Security | Spring Security + OAuth2 Resource Server | JWT validation and protected endpoints |
| Persistence | Spring Data JPA | Repository layer and ORM |
| Database | MySQL | Primary runtime database |
| Testing | H2 | In-memory database for tests |
| Testing | JUnit 5, MockMvc, Mockito | Unit and controller testing |
| Testing | Testcontainers | MySQL integration testing support |
| Mapping | MapStruct | Entity/DTO mapping |
| Boilerplate | Lombok | Constructor, builder, getter/setter generation |
| Quality | Spotless, JaCoCo | Formatting and coverage reporting |
| Frontend | React 18 | SPA client |
| UI | Material UI 5 | Login, layout, navigation, and profile components |
| Routing | React Router 6 | `/login` and `/` routes |
| Build | Maven, npm | Backend and frontend build tooling |
| Runtime | Docker | Containerized backend deployment |

---

## 📂 Project Structure

```text
.
├── backend/
│   ├── Dockerfile
│   ├── pom.xml
│   ├── Identity Service.postman_collection.json
│   └── src/
│       ├── main/
│       │   ├── java/com/devteria/identityservice/
│       │   │   ├── configuration/     # Security, JWT decoder, app initialization
│       │   │   ├── constant/          # Predefined ADMIN and USER roles
│       │   │   ├── controller/        # Auth, users, roles, permissions APIs
│       │   │   ├── dto/               # Request/response payloads
│       │   │   ├── entity/            # User, Role, Permission, InvalidatedToken
│       │   │   ├── exception/         # AppException, ErrorCode, global handler
│       │   │   ├── mapper/            # MapStruct mappers
│       │   │   ├── repository/        # Spring Data JPA repositories
│       │   │   ├── service/           # Business logic
│       │   │   └── validator/         # Custom date-of-birth validation
│       │   └── resources/
│       │       ├── application.yaml
│       │       └── application-prod.yaml
│       └── test/
│           ├── java/                  # Controller and service tests
│           └── resources/test.properties
└── frontend/
    ├── package.json
    ├── public/
    │   └── logo/devteria-logo.png
    └── src/
        ├── components/                # Login, Home, Header
        ├── routes/                    # React Router configuration
        ├── services/                  # Local storage and auth helpers
        ├── App.jsx
        └── index.js
```

---

## ⚙️ Installation

### Prerequisites

- Java SDK 21
- Maven 3.9+
- Node.js 20+
- npm
- MySQL 8+
- Docker, optional for containerized backend runtime

### 1. Clone the repository

```bash
git clone https://github.com/NguyenTanNghi/spring-boot.git
cd spring-boot
```

### 2. Create the MySQL database

```sql
CREATE DATABASE identity_service;
```

### 3. Configure backend environment variables

The backend reads database configuration from environment variables and falls back to local defaults:

| Variable | Default |
| --- | --- |
| `DBMS_CONNECTION` | `jdbc:mysql://localhost:3306/identity_service` |
| `DBMS_USERNAME` | `root` |
| `DBMS_PASSWORD` | `root` |

Example:

```bash
export DBMS_CONNECTION=jdbc:mysql://localhost:3306/identity_service
export DBMS_USERNAME=root
export DBMS_PASSWORD=root
```

On Windows PowerShell:

```powershell
$env:DBMS_CONNECTION="jdbc:mysql://localhost:3306/identity_service"
$env:DBMS_USERNAME="root"
$env:DBMS_PASSWORD="root"
```

### 4. Install frontend dependencies

```bash
cd frontend
npm install
```

---

## ▶️ Usage

### Run the backend

```bash
cd backend
./mvnw spring-boot:run
```

On Windows:

```powershell
cd backend
.\mvnw.cmd spring-boot:run
```

The backend starts at:

```text
http://localhost:8080/identity
```

When the application starts with MySQL, it seeds:

| Username | Password | Role |
| --- | --- | --- |
| `admin` | `admin` | `ADMIN` |

> Change the default admin password before using this service outside local development.

### Run the frontend

```bash
cd frontend
npm start
```

The React app starts at:

```text
http://localhost:3000
```

### Build the backend

```bash
cd backend
./mvnw clean package
```

### Run backend tests

```bash
cd backend
./mvnw test
```

### Build the frontend

```bash
cd frontend
npm run build
```

### Run with Docker

Build the backend image:

```bash
cd backend
docker build -t identity-service:0.9.0 .
```

Create a Docker network:

```bash
docker network create devteria-network
```

Start MySQL:

```bash
docker run --network devteria-network --name mysql \
  -p 3306:3306 \
  -e MYSQL_ROOT_PASSWORD=root \
  -d mysql:8.0.36-debian
```

Run the backend container:

```bash
docker run --name identity-service --network devteria-network \
  -p 8080:8080 \
  -e DBMS_CONNECTION=jdbc:mysql://mysql:3306/identity_service \
  -e DBMS_USERNAME=root \
  -e DBMS_PASSWORD=root \
  identity-service:0.9.0
```

---

## 🔌 API

Base URL:

```text
http://localhost:8080/identity
```

All successful responses use the shared wrapper:

```json
{
  "code": 1000,
  "result": {}
}
```

### Authentication

| Method | Endpoint | Access | Description |
| --- | --- | --- | --- |
| `POST` | `/auth/token` | Public | Login and receive a JWT |
| `POST` | `/auth/introspect` | Public | Validate whether a token is still active |
| `POST` | `/auth/refresh` | Public | Refresh a token within the refreshable duration |
| `POST` | `/auth/logout` | Public | Invalidate a token by storing its JWT ID |

Login request:

```json
{
  "username": "admin",
  "password": "admin"
}
```

Login response:

```json
{
  "code": 1000,
  "result": {
    "token": "<jwt-token>",
    "authenticated": true
  }
}
```

### Users

| Method | Endpoint | Access | Description |
| --- | --- | --- | --- |
| `POST` | `/users` | Public | Create a new user |
| `GET` | `/users/my-info` | Authenticated | Get the current user's profile |
| `GET` | `/users` | `ADMIN` | List all users |
| `GET` | `/users/{userId}` | `ADMIN` | Get a user by ID |
| `PUT` | `/users/{userId}` | Owner only | Update a user profile |
| `DELETE` | `/users/{userId}` | `ADMIN` | Delete a user |

Create user request:

```json
{
  "username": "john",
  "password": "12345678",
  "firstName": "John",
  "lastName": "Doe",
  "dob": "1990-01-01"
}
```

Authenticated requests must include:

```http
Authorization: Bearer <jwt-token>
```

### Roles

| Method | Endpoint | Access | Description |
| --- | --- | --- | --- |
| `POST` | `/roles` | Authenticated | Create a role with permissions |
| `GET` | `/roles` | Authenticated | List roles |
| `DELETE` | `/roles/{role}` | Authenticated | Delete a role |

Role request:

```json
{
  "name": "MANAGER",
  "description": "Manager role",
  "permissions": ["USER_READ", "USER_UPDATE"]
}
```

### Permissions

| Method | Endpoint | Access | Description |
| --- | --- | --- | --- |
| `POST` | `/permissions` | Authenticated | Create a permission |
| `GET` | `/permissions` | Authenticated | List permissions |
| `DELETE` | `/permissions/{permission}` | Authenticated | Delete a permission |

Permission request:

```json
{
  "name": "USER_READ",
  "description": "Read user information"
}
```

---

## 🧱 Architecture

```text
React Client
    │
    │  JWT stored in localStorage
    ▼
Spring Boot REST API (/identity)
    │
    ├── Controller layer
    │       Handles HTTP requests and response wrapping
    │
    ├── Service layer
    │       Authentication, token lifecycle, users, roles, permissions
    │
    ├── Security layer
    │       Spring Security, custom JWT decoder, RBAC method guards
    │
    ├── Repository layer
    │       Spring Data JPA repositories
    │
    ▼
MySQL Database
```

### Security model

- JWTs are signed with `HS512`.
- Access token lifetime is configured by `jwt.valid-duration`.
- Refresh lifetime is configured by `jwt.refreshable-duration`.
- Logout and refresh invalidate the previous token by storing its `jti` in `InvalidatedToken`.
- `POST /users`, `POST /auth/token`, `POST /auth/introspect`, `POST /auth/logout`, and `POST /auth/refresh` are public.
- Other endpoints require a valid `Authorization: Bearer <token>` header.
- Admin-only operations use method-level security with `@PreAuthorize("hasRole('ADMIN')")`.

### Backend package flow

```text
Controller -> Service -> Mapper -> Repository -> Entity -> Database
```

### Frontend flow

```text
Login.jsx -> POST /identity/auth/token -> localStorage(accessToken)
Home.jsx  -> GET /identity/users/my-info -> authenticated profile card
Header.jsx -> Log Out -> remove accessToken -> redirect to /login
```

---

## 🧪 Future Improvements

- Add refresh-token handling to the React client
- Add user registration UI connected to `POST /users`
- Add role and permission management screens for administrators
- Move backend API base URL into a frontend environment variable
- Replace the hardcoded JWT signing key with secret management
- Add database migrations with Flyway or Liquibase
- Add CI workflow for backend tests, frontend build, and Docker image validation
- Add OpenAPI/Swagger documentation for easier API exploration
- Add production-grade CORS configuration instead of wildcard origins
- Add token cleanup job for expired invalidated tokens

---

## 👨‍💻 Author

**NguyenTanNghi**

- Backend: Spring Boot, Spring Security, JPA, MySQL, JWT
- Frontend: React, Material UI, React Router
- Focus: authentication, authorization, clean API design, and portfolio-ready engineering practices
- GitHub: [NguyenTanNghi](https://github.com/NguyenTanNghi)
