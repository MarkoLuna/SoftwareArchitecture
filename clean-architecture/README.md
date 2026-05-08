# 🧼 Clean Architecture

Clean Architecture is a software design philosophy introduced by Robert C. Martin ("Uncle Bob"). Its objective is the separation of concerns, allowing developers to build applications that are independent of frameworks, UI, databases, or any external agency.

---

## 🏗️ The Concentric Circles (The Dependency Rule)

The core principle of Clean Architecture is the **Dependency Rule**: *Dependencies must only point inwards.*

1. **Entities (Inmost Circle)**: These represent the business objects of the application and encapsulate the most general and high-level rules.
2. **Use Cases**: These contain the application-specific business rules. They orchestrate the flow of data to and from the entities.
3. **Interface Adapters**: This layer converts data from the format most convenient for use cases and entities to the format most convenient for external agencies (e.g., Controllers, Presenters, Gateways).
4. **Frameworks & Drivers (Outermost Circle)**: This layer is where all the details go (Database, Web Framework, UI, External Devices).

---

## 🔑 Key Characteristics

- **Independent of Frameworks**: The architecture does not depend on the existence of some library of feature-laden software. This allows you to use such frameworks as tools, rather than having to cram your system into their limited constraints.
- **Testable**: The business rules can be tested without the UI, Database, Web Server, or any other external element.
- **Independent of UI**: The UI can change easily, without changing the rest of the system. A Web UI could be replaced with a console UI, for example, without changing the business rules.
- **Independent of Database**: You can swap out Oracle or SQL Server, for Mongo, BigTable, CouchDB, or something else. Your business rules are not bound to the database.
- **Independent of any external agency**: In fact your business rules simply don’t know anything at all about the outside world.

---

## 📐 Implementation Guidelines

### 1. Entities
- Should not be affected by changes to external layers.
- Example: `User`, `Account`, `Transaction`.

### 2. Use Cases
- Should not be affected by changes to the database or UI.
- Example: `CreateAccount`, `TransferFunds`.

### 3. Interface Adapters
- Includes the MVC architecture of a GUI.
- Converts data from Use Cases into a format ready for the UI or DB.

### 4. Frameworks and Drivers
- Generally, you don’t write much code in this layer, other than glue code that communicates to the next circle inwards.

---

## 🏗️ Sample Project Structures

### **Go Service Structure**
```
clean-architecture-go/
├── cmd/
│   └── server/
│       └── main.go                 # Application entry point
├── internal/
│   ├── domain/
│   │   ├── entity/
│   │   │   ├── user.go           # Business entities
│   │   │   └── order.go
│   │   ├── repository/
│   │   │   └── user_repository.go # Repository interfaces
│   │   └── service/
│   │       └── user_service.go     # Use case interfaces
│   ├── application/
│   │   ├── usecase/
│   │   │   ├── create_user.go   # Use case implementations
│   │   │   └── get_user.go
│   │   └── presenter/
│   │       └── user_presenter.go # Output formatting
│   ├── infrastructure/
│   │   ├── database/
│   │   │   ├── postgres.go     # Database implementation
│   │   │   └── migrations/      # Database migrations
│   │   ├── http/
│   │   │   ├── handlers/        # HTTP handlers
│   │   │   └── middleware/      # HTTP middleware
│   │   └── config/
│   │       └── config.go        # Configuration
│   └── interfaces/
│       └── http_client.go          # External service interfaces
├── pkg/
│   └── logger/
│       └── logger.go              # Shared utilities
├── configs/
│   └── config.yaml               # Configuration files
├── migrations/
│   └── 001_initial_schema.sql     # Database migrations
├── tests/
│   ├── unit/                    # Unit tests
│   ├── integration/             # Integration tests
│   └── e2e/                    # End-to-end tests
├── scripts/
│   ├── build.sh                 # Build scripts
│   └── deploy.sh               # Deployment scripts
├── Dockerfile
├── docker-compose.yml
├── go.mod
└── README.md
```

#### **Go Implementation Examples**

**Entity Layer**
```go
// internal/domain/entity/user.go
package entity

import "time"

type User struct {
    ID        string    `json:"id"`
    Email     string    `json:"email"`
    Name      string    `json:"name"`
    CreatedAt time.Time `json:"created_at"`
    UpdatedAt time.Time `json:"updated_at"`
}

type Order struct {
    ID          string    `json:"id"`
    UserID      string    `json:"user_id"`
    TotalAmount float64   `json:"total_amount"`
    Status      string    `json:"status"`
    CreatedAt   time.Time `json:"created_at"`
}
```

**Repository Interface**
```go
// internal/domain/repository/user_repository.go
package repository

import "github.com/company/clean-architecture-go/internal/domain/entity"

type UserRepository interface {
    Create(user *entity.User) error
    GetByID(id string) (*entity.User, error)
    GetByEmail(email string) (*entity.User, error)
    Update(user *entity.User) error
    Delete(id string) error
}
```

**Use Case**
```go
// internal/application/usecase/create_user.go
package usecase

import (
    "github.com/company/clean-architecture-go/internal/domain/entity"
    "github.com/company/clean-architecture-go/internal/domain/repository"
)

type CreateUserInput struct {
    Email string `json:"email" validate:"required,email"`
    Name  string `json:"name" validate:"required"`
}

type CreateUserOutput struct {
    User *entity.User `json:"user"`
}

type CreateUserUseCase struct {
    userRepo repository.UserRepository
}

func NewCreateUserUseCase(userRepo repository.UserRepository) *CreateUserUseCase {
    return &CreateUserUseCase{
        userRepo: userRepo,
    }
}

func (uc *CreateUserUseCase) Execute(input CreateUserInput) (*CreateUserOutput, error) {
    user := &entity.User{
        ID:        generateID(),
        Email:     input.Email,
        Name:      input.Name,
        CreatedAt: time.Now(),
        UpdatedAt: time.Now(),
    }

    err := uc.userRepo.Create(user)
    if err != nil {
        return nil, err
    }

    return &CreateUserOutput{User: user}, nil
}
```

**HTTP Handler**
```go
// internal/infrastructure/http/handlers/user_handler.go
package handlers

import (
    "encoding/json"
    "net/http"
    
    "github.com/company/clean-architecture-go/internal/application/usecase"
    "github.com/company/clean-architecture-go/pkg/logger"
)

type UserHandler struct {
    createUserUseCase *usecase.CreateUserUseCase
    logger           logger.Logger
}

func NewUserHandler(createUserUseCase *usecase.CreateUserUseCase, logger logger.Logger) *UserHandler {
    return &UserHandler{
        createUserUseCase: createUserUseCase,
        logger:           logger,
    }
}

func (h *UserHandler) CreateUser(w http.ResponseWriter, r *http.Request) {
    var input usecase.CreateUserInput
    if err := json.NewDecoder(r.Body).Decode(&input); err != nil {
        h.logger.Error("Failed to decode request", err)
        http.Error(w, err.Error(), http.StatusBadRequest)
        return
    }

    output, err := h.createUserUseCase.Execute(input)
    if err != nil {
        h.logger.Error("Failed to create user", err)
        http.Error(w, err.Error(), http.StatusInternalServerError)
        return
    }

    w.Header().Set("Content-Type", "application/json")
    w.WriteHeader(http.StatusCreated)
    json.NewEncoder(w).Encode(output)
}
```

### **Java Service Structure**
```
clean-architecture-java/
├── src/
│   └── main/
│       └── java/
│           └── com/
│               └── company/
│                   └── application/
│                       └── Application.java  # Application entry point
├── domain/
│   ├── src/
│   │   └── main/
│   │       └── java/
│   │           └── com/
│   │               └── company/
│   │                   └── domain/
│   │                       ├── entity/
│   │                       │   ├── User.java
│   │                       │   └── Order.java
│   │                       ├── repository/
│   │                       │   └── UserRepository.java
│   │                       └── service/
│   │                           └── UserService.java
│   └── test/
│       └── java/
│           └── com/
│               └── company/
│                   └── domain/
│                       ├── entity/
│                       └── service/
├── application/
│   ├── src/
│   │   └── main/
│   │       └── java/
│   │           └── com/
│   │               └── company/
│   │                   └── application/
│   │                       ├── usecase/
│   │                       │   ├── CreateUser.java
│   │                       │   └── GetUser.java
│   │                       ├── dto/
│   │                       │   ├── CreateUserRequest.java
│   │                       │   └── UserResponse.java
│   │                       └── presenter/
│   │                           └── UserPresenter.java
│   └── test/
│       └── java/
│           └── com/
│               └── company/
│                   └── application/
│                       └── usecase/
├── infrastructure/
│   ├── src/
│   │   └── main/
│   │       └── java/
│   │           └── com/
│   │               └── company/
│   │                   └── infrastructure/
│   │                       ├── database/
│   │                       │   ├── jpa/
│   │                       │   │   ├── UserRepositoryImpl.java
│   │                       │   │   └── UserEntity.java
│   │                       │   └── migration/
│   │                       │       └── V1__Create_user_table.sql
│   │                       ├── rest/
│   │                       │   ├── UserController.java
│   │                       │   └── UserExceptionHandler.java
│   │                       └── config/
│   │                           └── DatabaseConfig.java
│   └── test/
│       └── java/
│           └── com/
│               └── company/
│                   └── infrastructure/
│                       └── database/
├── pom.xml
├── Dockerfile
├── docker-compose.yml
└── README.md
```

#### **Java Implementation Examples**

**Entity Layer**
```java
// domain/src/main/java/com/company/domain/entity/User.java
package com.company.domain.entity;

import java.time.LocalDateTime;

public class User {
    private String id;
    private String email;
    private String name;
    private LocalDateTime createdAt;
    private LocalDateTime updatedAt;

    // Constructors
    public User() {}

    public User(String id, String email, String name) {
        this.id = id;
        this.email = email;
        this.name = name;
        this.createdAt = LocalDateTime.now();
        this.updatedAt = LocalDateTime.now();
    }

    // Getters and setters
    public String getId() { return id; }
    public void setId(String id) { this.id = id; }
    
    public String getEmail() { return email; }
    public void setEmail(String email) { this.email = email; }
    
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    
    public LocalDateTime getCreatedAt() { return createdAt; }
    public void setCreatedAt(LocalDateTime createdAt) { this.createdAt = createdAt; }
    
    public LocalDateTime getUpdatedAt() { return updatedAt; }
    public void setUpdatedAt(LocalDateTime updatedAt) { this.updatedAt = updatedAt; }
}
```

**Repository Interface**
```java
// domain/src/main/java/com/company/domain/repository/UserRepository.java
package com.company.domain.repository;

import com.company.domain.entity.User;
import java.util.Optional;

public interface UserRepository {
    User save(User user);
    Optional<User> findById(String id);
    Optional<User> findByEmail(String email);
    User update(User user);
    void deleteById(String id);
}
```

**Use Case**
```java
// application/src/main/java/com/company/application/usecase/CreateUser.java
package com.company.application.usecase;

import com.company.domain.entity.User;
import com.company.domain.repository.UserRepository;
import com.company.application.dto.CreateUserRequest;
import com.company.application.dto.UserResponse;

public class CreateUser {
    private final UserRepository userRepository;

    public CreateUser(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    public UserResponse execute(CreateUserRequest request) {
        User user = new User(
            generateId(),
            request.getEmail(),
            request.getName()
        );

        User savedUser = userRepository.save(user);
        
        return new UserResponse(savedUser);
    }

    private String generateId() {
        return java.util.UUID.randomUUID().toString();
    }
}
```

**REST Controller**
```java
// infrastructure/src/main/java/com/company/infrastructure/rest/UserController.java
package com.company.infrastructure.rest;

import com.company.application.usecase.CreateUser;
import com.company.application.dto.CreateUserRequest;
import com.company.application.dto.UserResponse;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api/users")
public class UserController {
    private final CreateUser createUser;

    public UserController(CreateUser createUser) {
        this.createUser = createUser;
    }

    @PostMapping
    public ResponseEntity<UserResponse> createUser(@RequestBody CreateUserRequest request) {
        UserResponse response = createUser.execute(request);
        return ResponseEntity.ok(response);
    }

    @GetMapping("/{id}")
    public ResponseEntity<UserResponse> getUser(@PathVariable String id) {
        // Implementation would call GetUser use case
        return ResponseEntity.ok().build();
    }
}
```

---

## 🧪 Testing Strategies

### **Unit Testing**
- Test each layer independently
- Use mocks for external dependencies
- Focus on business logic validation

### **Integration Testing**
- Test layer interactions
- Use test databases
- Validate dependency injection

### **End-to-End Testing**
- Test complete user workflows
- Use real infrastructure
- Validate API contracts

---

## ⚖️ Pro vs Contra

| Pro | Contra |
| :--- | :--- |
| High testability | Increased boilerplate |
| Easy to maintain | Steeper learning curve |
| Decoupled code | Can be over-engineering for simple apps |
| Future-proof | More files and abstractions |
