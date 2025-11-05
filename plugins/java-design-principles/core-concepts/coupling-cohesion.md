### 3. Code Organization Principles

#### DRY (Don't Repeat Yourself)

**Definition**: Every piece of knowledge should have a single, unambiguous, authoritative representation within a system.

**Why It Matters**:
- **Reduced Maintenance Cost**: Only need to modify in one place
- **Ensures Consistency**: Same logic in multiple places can become inconsistent
- **Fewer Bugs**: Duplicate code means duplicate bugs
- **Improved Code Quality**: Leads to better design through abstraction

**How to Apply**:
1. **Extract Method**: Extract duplicate code into methods
2. **Extract Class**: Separate related duplicate code into separate classes
3. **Inheritance/Interface**: Common behavior in parent class or interface
4. **Utility Classes**: Generic functionality as utilities

**Caution**:
- Distinguish between Accidental Duplication and real duplication
- Business logic that looks the same may change for different reasons
- Over-abstraction can increase complexity

**Practical Example**:
```java
// ❌ Duplicate code
public class UserService {
    public void createUser(String email) {
        if (email == null || !email.matches("^[A-Za-z0-9+_.-]+@(.+)$")) {
            throw new IllegalArgumentException("Invalid email");
        }
        // Create user
    }

    public void updateEmail(String email) {
        if (email == null || !email.matches("^[A-Za-z0-9+_.-]+@(.+)$")) {
            throw new IllegalArgumentException("Invalid email");
        }
        // Update email
    }
}

// ✅ DRY applied
public class EmailValidator {
    private static final String EMAIL_PATTERN = "^[A-Za-z0-9+_.-]+@(.+)$";

    public static void validate(String email) {
        if (email == null || !email.matches(EMAIL_PATTERN)) {
            throw new IllegalArgumentException("Invalid email");
        }
    }
}

public class UserService {
    public void createUser(String email) {
        EmailValidator.validate(email);
        // Create user
    }

    public void updateEmail(String email) {
        EmailValidator.validate(email);
        // Update email
    }
}
```

#### Separation of Concerns

**Definition**: A design principle for dividing a program into distinct sections that address separate concerns.

**Core Idea**:
Each module should focus on one specific concern and be independent of other concerns.

**Examples of Concerns**:
- **Presentation Layer**: UI, user input handling
- **Business Logic Layer**: Domain rules, business rules
- **Data Access Layer**: Database, file system

**How to Apply**:
- Use Layered Architecture
- Apply MVC (Model-View-Controller) pattern
- Organize each package by layer, not by feature

**Practical Example**:
```java
// Layer separation
// 1. Presentation Layer
public class OrderController {
    private OrderService orderService;

    public ResponseEntity<Order> createOrder(OrderRequest request) {
        Order order = orderService.createOrder(request);
        return ResponseEntity.ok(order);
    }
}

// 2. Business Logic Layer
public class OrderService {
    private OrderRepository orderRepository;
    private PaymentService paymentService;

    public Order createOrder(OrderRequest request) {
        // Validate business rules
        validateOrder(request);

        // Process payment
        paymentService.processPayment(request.getPayment());

        // Save order
        return orderRepository.save(request.toOrder());
    }
}

// 3. Data Access Layer
public class OrderRepository {
    public Order save(Order order) {
        // Database save logic
        return order;
    }
}
```

#### Maximize Cohesion

**Definition**: A measure of how related the elements within a module are.

**Why It Matters**:
- High cohesion is easier to understand
- Easier to maintain
- Higher reusability
- Smaller scope of change impact

**Cohesion Levels** (Low → High):
1. **Coincidental Cohesion**: Collection of unrelated functions
2. **Logical Cohesion**: Collection of similar types of tasks
3. **Temporal Cohesion**: Tasks executed at the same time
4. **Procedural Cohesion**: Tasks executed in sequence
5. **Communicational Cohesion**: Tasks using the same data
6. **Sequential Cohesion**: Output of one task is input of next
7. **Functional Cohesion**: Tasks for one clear purpose (best)

**How to Apply**:
- Check if all methods in a class use the same instance variables
- Separate unrelated methods into separate classes
- Check if method names include the class name

---

### 4. Coupling Management

#### Minimize Coupling

**Definition**: The principle of minimizing interdependencies between modules.

**Coupling Levels** (Low → High):
1. **Message Coupling**: Passing only parameters
2. **Data Coupling**: Passing data structures
3. **Stamp Coupling**: Passing complex data structures
4. **Control Coupling**: Passing control flags
5. **External Coupling**: Sharing external tools or protocols
6. **Common Coupling**: Sharing global data
7. **Content Coupling**: Modifying another module's internals (worst)

**How to Apply**:
- Dependency injection through interfaces
- Event-driven architecture
- Use message queues
- Minimize shared state

#### Law of Demeter

**Definition**: "Principle of Least Knowledge" - An object should only know what it needs to know directly.

**Rules**: A method can only call methods of:
1. The same object
2. Objects passed as parameters
3. Objects created within the method
4. Instance variables

**Why It Matters**:
- Reduces coupling
- Reduces ripple effects of changes
- Strengthens encapsulation

**Practical Application**:
```java
// ❌ Law of Demeter Violation
public class ShoppingCart {
    public void checkout(Customer customer) {
        // Method chaining through multiple layers
        String street = customer.getAddress().getStreet();
        String city = customer.getAddress().getCity();
        String zipCode = customer.getAddress().getZipCode();

        // ShoppingCart must know Address internal structure - Increased coupling
    }
}

// ✅ Law of Demeter Compliant
public class Customer {
    private Address address;

    // Customer provides needed information directly
    public String getShippingAddress() {
        return address.getFullAddress();
    }
}

public class Address {
    private String street;
    private String city;
    private String zipCode;

    public String getFullAddress() {
        return street + ", " + city + " " + zipCode;
    }
}

public class ShoppingCart {
    public void checkout(Customer customer) {
        // Simple and low coupling
        String address = customer.getShippingAddress();
    }
}
```

#### Composition Over Inheritance

**Definition**: The principle of preferring composition ("has-a") over inheritance ("is-a").

**Why It Matters**:
- **Flexibility**: Can change behavior at runtime
- **Reduced Coupling**: Not affected by parent class changes
- **Reusability**: Can combine only needed functionality
- **Testability**: Each component can be tested independently

**When to Use Inheritance**:
- Clear "is-a" relationship
- Satisfies LSP
- All parent methods are meaningful to child

**When to Use Composition**:
- "has-a" relationship
- Need to change behavior at runtime
- Need to combine functionality from multiple classes


---

[← Back to Main](../SKILL.md)
