# ☕ Java Class Types — A Developer's Reference Guide

> A comprehensive reference of the most commonly used Java class categories in real-world backend development (Spring Boot) and interviews.

---

## 📌 Why This Matters

Java has no *official* classification of class types — but in practice, developers follow well-established patterns. Knowing these helps you:

- Write cleaner, more organized code
- Communicate effectively in team settings
- Ace Java/Spring Boot backend interviews

---

## 🗂️ Class Types at a Glance

| Class Type | Purpose | Common Example |
|---|---|---|
| **Utility Class** | Contains only `static` methods; no state | `Math`, `Arrays`, `Collections`, `Objects` |
| **POJO Class** | Plain Old Java Object — fields, constructor, getters/setters | `User.java` |
| **Entity Class** | Maps to a database table (JPA/Hibernate) | `@Entity User.java` |
| **DTO Class** | Transfers data between application layers | `UserDTO.java` |
| **Model / Bean Class** | Holds application or domain data | `ProductModel.java` |
| **Service Class** | Contains business logic | `UserService.java` |
| **Controller Class** | Handles HTTP requests (Spring MVC / REST) | `UserController.java` |
| **Repository / DAO Class** | Handles database operations (CRUD) | `UserRepository.java` |
| **Abstract Class** | Partial implementation; cannot be instantiated directly | `abstract class BaseEntity` |
| **Concrete Class** | Fully implemented; can be instantiated with `new` | Any regular class |
| **Inner Class** | Non-static class defined inside another class | `class Outer { class Inner {} }` |
| **Nested Static Class** | Static class defined inside another class | `class Outer { static class Nested {} }` |
| **Singleton Class** | Only one instance can exist (controlled creation) | `getInstance()` pattern |
| **Immutable Class** | Object state cannot change after creation | `String`, custom `final` classes |
| **Enum Class** | Represents a fixed set of constants | `enum Status { ACTIVE, INACTIVE }` |
| **Record Class** | Immutable data carrier with auto-generated boilerplate *(Java 16+)* | `record UserDTO(String name, int age) {}` |
| **Exception Class** | Custom exception for error handling | `class UserNotFoundException extends RuntimeException` |

---

## 🏗️ Interview-Friendly Naming Convention

```
User.java                →  POJO / Entity
UserDTO.java             →  DTO
UserService.java         →  Service
UserController.java      →  Controller
UserRepository.java      →  Repository / DAO
Math.java                →  Utility Class
RuntimeException.java    →  Exception Class
```

---

## 🔍 Deep Dive — Most Important Class Types

### 1. 🔧 Utility Class
- Only `static` methods, no instance variables
- Cannot (and should not) be instantiated
- Often has a `private` constructor to prevent misuse

```java
public final class MathUtils {
    private MathUtils() {} // prevent instantiation

    public static int square(int n) {
        return n * n;
    }
}
```

---

### 2. 📦 POJO Class
- Plain Old Java Object
- No annotations, no framework dependencies
- Just fields, constructors, getters/setters

```java
public class User {
    private String name;
    private int age;

    public User(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // getters & setters...
}
```

---

### 3. 🗄️ Entity Class
- Annotated with `@Entity` for JPA/Hibernate
- Represents a table in the database
- Fields map to columns

```java
@Entity
@Table(name = "users")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private String email;
}
```

---

### 4. 📨 DTO Class (Data Transfer Object)
- Carries data between layers (e.g., Controller → Service, or API response)
- No business logic
- Decouples your API contract from your database model

```java
public class UserDTO {
    private String name;
    private String email;
    // constructors, getters, setters
}
```

---

### 5. ⚙️ Service Class
- Annotated with `@Service`
- Holds all business logic
- Sits between Controller and Repository

```java
@Service
public class UserService {
    @Autowired
    private UserRepository userRepository;

    public UserDTO getUserById(Long id) {
        User user = userRepository.findById(id).orElseThrow();
        return new UserDTO(user.getName(), user.getEmail());
    }
}
```

---

### 6. 🌐 Controller Class
- Annotated with `@RestController` or `@Controller`
- Handles HTTP requests and returns responses
- Should contain **no** business logic

```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    @Autowired
    private UserService userService;

    @GetMapping("/{id}")
    public ResponseEntity<UserDTO> getUser(@PathVariable Long id) {
        return ResponseEntity.ok(userService.getUserById(id));
    }
}
```

---

### 7. 🗃️ Repository / DAO Class
- Annotated with `@Repository`
- Handles all database communication (CRUD)
- In Spring Boot, often extends `JpaRepository`

```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    Optional<User> findByEmail(String email);
}
```

---

### 8. 🏛️ Abstract Class
- Declared with `abstract` keyword
- Cannot be instantiated directly
- Can have both abstract (unimplemented) and concrete (implemented) methods

```java
public abstract class BaseEntity {
    private LocalDateTime createdAt;
    private LocalDateTime updatedAt;

    public abstract String getType(); // must be implemented by subclass
}
```

---

### 9. 🚨 Exception Class
- Extends `RuntimeException` (unchecked) or `Exception` (checked)
- Used for custom, domain-specific error handling

```java
public class UserNotFoundException extends RuntimeException {
    public UserNotFoundException(Long id) {
        super("User not found with id: " + id);
    }
}
```

---

### 10. 📬 Record Class *(Java 16+)*
- Immutable data carrier
- Auto-generates constructor, getters, `equals()`, `hashCode()`, `toString()`
- Perfect replacement for simple DTOs

```java
public record UserDTO(String name, String email) {}

// Usage:
UserDTO dto = new UserDTO("Alice", "alice@email.com");
System.out.println(dto.name()); // Alice
```

---

## ⭐ Top 10 for Java Backend Developers

These are the class types you'll encounter every day in Spring Boot projects and in interviews:

```
✅ Utility Class
✅ POJO Class
✅ DTO Class
✅ Entity Class
✅ Service Class
✅ Controller Class
✅ Repository Class
✅ Abstract Class
✅ Concrete Class
✅ Exception Class
```

---

## 🔗 Spring Boot Typical Layer Architecture

```
[ Client / Browser ]
        │  HTTP Request
        ▼
[ Controller Layer ]    ← @RestController
        │
        ▼
[ Service Layer ]       ← @Service  (Business Logic)
        │
        ▼
[ Repository Layer ]    ← @Repository  (Database Access)
        │
        ▼
[ Database ]
```

**Data flows as:**
`Entity` ←→ `Repository` ←→ `Service` ←→ `DTO` ←→ `Controller` ←→ Client

---

## 📚 Resources

- [Official Java Docs](https://docs.oracle.com/en/java/)
- [Spring Boot Docs](https://docs.spring.io/spring-boot/docs/current/reference/html/)
- [Baeldung — Spring Boot](https://www.baeldung.com/spring-boot)
- [JPA / Hibernate Guide](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/)

---

<p align="center">
  Made with ❤️ for Java Backend Developers
</p>
