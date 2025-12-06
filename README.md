# Spring Security Demo Application

[![Java](https://img.shields.io/badge/Java-17-orange.svg)](https://openjdk.org/projects/jdk/17/)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.5.3-brightgreen.svg)](https://spring.io/projects/spring-boot)
[![Spring Security](https://img.shields.io/badge/Spring%20Security-6.x-brightgreen.svg)](https://spring.io/projects/spring-security)
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

A comprehensive Spring Security demonstration project featuring JWT authentication, database-backed user management, and RESTful API security. This project serves as a practical reference for implementing modern authentication and authorization patterns in Spring Boot applications.

---

## 📋 Table of Contents

- [Features](#features)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [API Documentation](#api-documentation)
- [Security Features](#security-features)
- [Tech Stack](#tech-stack)
- [Contributing](#contributing)
- [License](#license)

---

## ✨ Features

- **JWT Authentication** - Secure token-based authentication with configurable expiration
- **Database User Management** - PostgreSQL-backed user storage with JPA
- **BCrypt Password Encoding** - Secure password hashing with strength factor 12
- **Stateless Session Management** - RESTful API design without server-side sessions
- **Custom Security Configuration** - Flexible Spring Security filter chain
- **User Registration & Login** - Complete authentication workflow

---

## 📁 Project Structure

```
src/
├── main/
│   ├── java/com/telusko/SpringSecEx/
│   │   ├── SpringSecExApplication.java    # Application entry point
│   │   ├── config/
│   │   │   ├── SecurityConfig.java        # Spring Security configuration
│   │   │   └── JwtFilter.java             # JWT authentication filter
│   │   ├── controller/
│   │   │   ├── HelloController.java       # Welcome endpoint
│   │   │   ├── StudentController.java     # Student resource endpoints
│   │   │   └── UserController.java        # Authentication endpoints
│   │   ├── model/
│   │   │   ├── Student.java               # Student entity
│   │   │   ├── Users.java                 # User entity (JPA)
│   │   │   └── UserPrincipal.java         # Spring Security UserDetails
│   │   ├── repo/
│   │   │   └── UserRepo.java              # User JPA repository
│   │   └── service/
│   │       ├── JWTService.java            # JWT token operations
│   │       ├── MyUserDetailsService.java  # Custom UserDetailsService
│   │       └── UserService.java           # User business logic
│   └── resources/
│       ├── application.properties         # Application configuration (not in git)
│       └── application.properties.example # Configuration template
└── test/
    └── java/com/telusko/SpringSecEx/
        └── SpringSecExApplicationTests.java
```

---

## 📋 Prerequisites

Before running this project, ensure you have the following installed:

- **Java 17** or higher
- **Maven 3.6+** or use the included Maven wrapper
- **PostgreSQL 12+** database server
- **Git** for version control

---

## 🚀 Installation

### 1. Clone the Repository

```bash
git clone https://github.com/Ankur071/Spring-Security.git
cd Spring-Security
```

### 2. Set Up the Database

Create a PostgreSQL database:

```sql
CREATE DATABASE telusko1;
```

### 3. Configure Application Properties

Copy the example configuration file and update with your credentials:

```bash
cp src/main/resources/application.properties.example src/main/resources/application.properties
```

Then edit `src/main/resources/application.properties` with your database credentials:

```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/telusko1
spring.datasource.username=your_username
spring.datasource.password=your_password
```

> **Note:** The `application.properties` file is not tracked in git to protect sensitive credentials.

### 4. Build the Project

```bash
# Using Maven wrapper (recommended)
./mvnw clean install

# Or using installed Maven
mvn clean install
```

### 5. Run the Application

```bash
# Using Maven wrapper
./mvnw spring-boot:run

# Or using Maven
mvn spring-boot:run
```

The application will start on `http://localhost:8080`.

---

## ⚙️ Configuration

### Application Properties

| Property | Description | Default |
|----------|-------------|---------|
| `spring.application.name` | Application name | SpringSecEx |
| `spring.datasource.url` | PostgreSQL connection URL | jdbc:postgresql://localhost:5432/telusko1 |
| `spring.datasource.username` | Database username | - |
| `spring.datasource.password` | Database password | - |
| `spring.security.user.name` | Default admin username | - |
| `spring.security.user.password` | Default admin password | - |

---

## 📖 API Documentation

### Public Endpoints

| Method | Endpoint | Description | Request Body |
|--------|----------|-------------|--------------|
| POST | `/register` | Register a new user | `{ "id": 1, "username": "user", "password": "pass" }` |
| POST | `/login` | Authenticate and get JWT token | `{ "username": "user", "password": "pass" }` |

### Protected Endpoints

> **Note:** These endpoints require a valid JWT token in the `Authorization` header.

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/` | Welcome message with session ID |
| GET | `/students` | Get all students |
| POST | `/students` | Add a new student |
| GET | `/csrf-token` | Get CSRF token (if enabled) |

### Authentication Flow

1. **Register a User**
   ```bash
   curl -X POST http://localhost:8080/register \
     -H "Content-Type: application/json" \
     -d '{"id": 1, "username": "john", "password": "secret123"}'
   ```

2. **Login to Get JWT Token**
   ```bash
   curl -X POST http://localhost:8080/login \
     -H "Content-Type: application/json" \
     -d '{"username": "john", "password": "secret123"}'
   ```

3. **Access Protected Resources**
   ```bash
   curl -X GET http://localhost:8080/students \
     -H "Authorization: Bearer <your_jwt_token>"
   ```

---

## 🔐 Security Features

### JWT Token Configuration

- **Algorithm:** HMAC SHA-256
- **Token Expiration:** 30 minutes
- **Key Generation:** Dynamic key generation at startup

### Password Security

- **Encoder:** BCrypt with strength factor 12
- **Storage:** Passwords are never stored in plain text

### Security Filter Chain

1. CSRF protection disabled (stateless API)
2. Public access to `/login` and `/register`
3. All other endpoints require authentication
4. JWT filter validates tokens before processing requests
5. Stateless session management

---

## 🛠 Tech Stack

| Technology | Version | Purpose |
|------------|---------|---------|
| Java | 17 | Programming language |
| Spring Boot | 3.5.3 | Application framework |
| Spring Security | 6.x | Security framework |
| Spring Data JPA | 3.x | Data persistence |
| PostgreSQL | 12+ | Database |
| JJWT | 0.12.6 | JWT implementation |
| Maven | 3.6+ | Build tool |

---

## 🤝 Contributing

Contributions are welcome! Here's how you can help:

1. **Fork** the repository
2. **Create** a feature branch (`git checkout -b feature/amazing-feature`)
3. **Commit** your changes (`git commit -m 'Add amazing feature'`)
4. **Push** to the branch (`git push origin feature/amazing-feature`)
5. **Open** a Pull Request

### Guidelines

- Follow existing code style and conventions
- Write meaningful commit messages
- Update documentation for significant changes
- Add tests for new features when applicable

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 📚 Learning Resources

- [Spring Security Reference](https://docs.spring.io/spring-security/reference/)
- [Spring Boot Documentation](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/)
- [JWT Introduction](https://jwt.io/introduction)
- [PostgreSQL Documentation](https://www.postgresql.org/docs/)

---

## 👤 Author

**Ankur**

- GitHub: [@Ankur071](https://github.com/Ankur071)

---

<p align="center">
  Made with ❤️ for learning Spring Security
</p>
