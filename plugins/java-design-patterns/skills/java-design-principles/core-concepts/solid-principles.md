### 2. SOLID Principles

#### Single Responsibility Principle (SRP)

**Definition**: A class should have only one reason to change and only one responsibility.

**Core Idea**:
"Responsibility" means "reason to change". If a class changes for multiple reasons, it has multiple responsibilities.

**Why It Matters**:
- **Improved Maintainability**: Changes to one feature don't affect other features
- **Testability**: Single responsibility classes are easier to test
- **Increased Reusability**: You can reuse only the class you need
- **Ease of Understanding**: The purpose of the class is clear

**Violation Signs**:
- Using vague words like "Manager", "Utility", "Helper" in class names
- A single class performing database access, business logic, and UI processing
- Needing "and" when describing a class
- Classes with more than 10 methods

**How to Apply**:
1. **Identify Responsibility**: Describe what the class does in one sentence
2. **Determine Separation Point**: Consider splitting if "and" is needed
3. **Check Cohesion**: Do all methods in the class use the same data?
4. **Analyze Reasons for Change**: List the reasons why this class would change

**Practical Application**:
```java
// ❌ SRP Violation - Class with multiple responsibilities
public class Employee {
    private String name;
    private double salary;

    // Responsibility 1: Calculate salary
    public double calculatePay() { }

    // Responsibility 2: Save to database
    public void save() { }

    // Responsibility 3: Generate report
    public String generateReport() { }
}
// Problem: Changes to salary policy, database, or report format all modify this class

// ✅ SRP Compliant - Each responsibility in separate classes
public class Employee {
    private String name;
    private double salary;

    public String getName() { return name; }
    public double getSalary() { return salary; }
}

public class PayrollCalculator {
    public double calculatePay(Employee employee) {
        // Salary calculation logic
    }
}

public class EmployeeRepository {
    public void save(Employee employee) {
        // Database save logic
    }
}

public class EmployeeReportGenerator {
    public String generateReport(Employee employee) {
        // Report generation logic
    }
}
```

**Validation Questions**:
- Are there multiple reasons to change this class?
- Do different actors (users, systems) use different methods of this class?

---

#### Open/Closed Principle (OCP)

**Definition**: Software entities (classes, modules, functions) should be open for extension but closed for modification.

**Core Idea**:
You should be able to add new functionality (open) without modifying existing code (closed).

**Why It Matters**:
- **Stability**: Lower risk of bugs since existing code isn't touched
- **Extensibility**: Easy to add new features
- **Maintainability**: Limited scope of change impact
- **Reusability**: Existing code can be extended in various ways

**How to Apply**:
1. **Use Abstraction**: Abstract variation points with interfaces or abstract classes
2. **Leverage Polymorphism**: Extend through inheritance or interface implementation
3. **Strategy Pattern**: Define algorithms as interfaces and swap implementations
4. **Template Method**: Fix unchanging parts and extend only varying parts

**Violation Signs**:
- if-else or switch-case statements growing when adding new features
- Code checking types with instanceof
- Need to modify multiple classes when adding new types

**Practical Application**:
```java
// ❌ OCP Violation - Must modify this class when adding new shapes
public class AreaCalculator {
    public double calculateArea(Object shape) {
        if (shape instanceof Circle) {
            Circle circle = (Circle) shape;
            return Math.PI * circle.radius * circle.radius;
        } else if (shape instanceof Rectangle) {
            Rectangle rect = (Rectangle) shape;
            return rect.width * rect.height;
        }
        // Must add else if here when adding new shapes
        return 0;
    }
}

// ✅ OCP Compliant - Open for extension, closed for modification
public interface Shape {
    double calculateArea();
}

public class Circle implements Shape {
    private double radius;

    @Override
    public double calculateArea() {
        return Math.PI * radius * radius;
    }
}

public class Rectangle implements Shape {
    private double width;
    private double height;

    @Override
    public double calculateArea() {
        return width * height;
    }
}

// Adding new shape - Extension only, no modification to existing code
public class Triangle implements Shape {
    private double base;
    private double height;

    @Override
    public double calculateArea() {
        return 0.5 * base * height;
    }
}

// Calculator needs no modification
public class AreaCalculator {
    public double calculateArea(Shape shape) {
        return shape.calculateArea();
    }
}
```

**Validation Questions**:
- Are you modifying existing classes when adding new features?
- Are if-else or switch-case statements growing?

---

#### Liskov Substitution Principle (LSP)

**Definition**: Subtypes (child classes) must be substitutable for their base types (parent classes).

**Core Idea**:
Code using a parent class should work correctly when replaced with a child class.

**Why It Matters**:
- **Correct Use of Polymorphism**: Validates proper use of inheritance
- **Predictability**: Subtypes don't behave unexpectedly
- **Reliability**: Children honor the parent type's contract
- **Maintainability**: Inheritance hierarchies can be safely modified

**Violation Signs**:
- Child class overriding parent's method to throw exceptions
- Child class leaving parent's method empty or doing nothing
- Child class requiring stronger preconditions than parent
- Child class ensuring weaker postconditions than parent

**How to Apply**:
1. **Honor Contract**: Child must follow parent's method contract
2. **No Strengthening Preconditions**: Child can't require stricter inputs
3. **No Weakening Postconditions**: Child can't guarantee weaker outputs
4. **Maintain Invariants**: Child must maintain parent's invariant rules

**Practical Application**:
```java
// ❌ LSP Violation - Rectangle-Square problem
public class Rectangle {
    protected int width;
    protected int height;

    public void setWidth(int width) {
        this.width = width;
    }

    public void setHeight(int height) {
        this.height = height;
    }

    public int getArea() {
        return width * height;
    }
}

public class Square extends Rectangle {
    @Override
    public void setWidth(int width) {
        this.width = width;
        this.height = width;  // Square so height must match
    }

    @Override
    public void setHeight(int height) {
        this.width = height;
        this.height = height;  // Square so width must match
    }
}

// Problem occurs
public void testRectangle(Rectangle rect) {
    rect.setWidth(5);
    rect.setHeight(4);
    assert rect.getArea() == 20;  // Passes for Rectangle, fails for Square!
}

// ✅ LSP Compliant - Use composition instead of inheritance
public interface Shape {
    int getArea();
}

public class Rectangle implements Shape {
    private int width;
    private int height;

    public Rectangle(int width, int height) {
        this.width = width;
        this.height = height;
    }

    public void setWidth(int width) { this.width = width; }
    public void setHeight(int height) { this.height = height; }

    @Override
    public int getArea() {
        return width * height;
    }
}

public class Square implements Shape {
    private int side;

    public Square(int side) {
        this.side = side;
    }

    public void setSide(int side) { this.side = side; }

    @Override
    public int getArea() {
        return side * side;
    }
}
```

**Validation Questions**:
- Does the code still work correctly when replacing parent type with child type?
- Are child classes throwing UnsupportedOperationException when overriding parent methods?

---

#### Interface Segregation Principle (ISP)

**Definition**: Clients should not be forced to depend on methods they do not use.

**Core Idea**:
Multiple small, specific interfaces are better than one large interface.

**Why It Matters**:
- **Eliminates Unnecessary Dependencies**: Not affected by changes to unused methods
- **Increased Flexibility**: Can implement only needed interfaces
- **Ease of Understanding**: Small interfaces are easier to understand
- **Simplified Testing**: Small interfaces are easier to mock

**Violation Signs**:
- Clients using only some methods of an interface
- Implementing empty methods or methods that throw exceptions
- Interfaces with more than 10 methods
- Unrelated functionalities in one interface

**How to Apply**:
1. **Separate by Role**: Separate interfaces by role
2. **Client-Centric**: Include only methods clients actually use
3. **Keep Small**: Keep interfaces small and highly cohesive
4. **Multiple Implementation**: Implement multiple interfaces when needed

**Practical Application**:
```java
// ❌ ISP Violation - Bloated interface
public interface Worker {
    void work();
    void eat();
    void sleep();
    void getPaid();
    void attendMeeting();
    void writeCode();
    void reviewCode();
    void manageTeam();
}

// Problem: Developer doesn't need manageTeam, Manager doesn't need writeCode
public class Developer implements Worker {
    public void work() { /* implementation */ }
    public void eat() { /* implementation */ }
    public void sleep() { /* implementation */ }
    public void getPaid() { /* implementation */ }
    public void attendMeeting() { /* implementation */ }
    public void writeCode() { /* implementation */ }
    public void reviewCode() { /* implementation */ }
    public void manageTeam() {
        throw new UnsupportedOperationException("Developer doesn't manage team");
    }
}

// ✅ ISP Compliant - Interfaces separated by role
public interface Workable {
    void work();
}

public interface Eatable {
    void eat();
}

public interface Sleepable {
    void sleep();
}

public interface Payable {
    void getPaid();
}

public interface Codeable {
    void writeCode();
    void reviewCode();
}

public interface Manageable {
    void manageTeam();
    void attendMeeting();
}

// Implement only needed interfaces
public class Developer implements Workable, Eatable, Sleepable, Payable, Codeable {
    public void work() { /* implementation */ }
    public void eat() { /* implementation */ }
    public void sleep() { /* implementation */ }
    public void getPaid() { /* implementation */ }
    public void writeCode() { /* implementation */ }
    public void reviewCode() { /* implementation */ }
}

public class Manager implements Workable, Eatable, Sleepable, Payable, Manageable {
    public void work() { /* implementation */ }
    public void eat() { /* implementation */ }
    public void sleep() { /* implementation */ }
    public void getPaid() { /* implementation */ }
    public void manageTeam() { /* implementation */ }
    public void attendMeeting() { /* implementation */ }
}
```

**Validation Questions**:
- Do all classes implementing this interface actually use all methods?
- Are there empty methods or methods throwing exceptions?

---

#### Dependency Inversion Principle (DIP)

**Definition**:
1. High-level modules should not depend on low-level modules. Both should depend on abstractions.
2. Abstractions should not depend on details. Details should depend on abstractions.

**Core Idea**:
Depend on interfaces or abstract classes, not concrete implementations.

**Why It Matters**:
- **Reduced Coupling**: Implementation changes don't affect higher modules
- **Testability**: Easy to replace with mock objects
- **Increased Flexibility**: Easy to swap implementations
- **Extensibility**: Easy to add new implementations

**Violation Signs**:
- Directly creating concrete classes with `new` keyword inside classes
- Declaring fields or parameters as concrete class types
- Direct dependency on static methods
- Direct dependency on external systems like databases or file systems

**How to Apply**:
1. **Define Interfaces**: Define dependencies as interfaces
2. **Dependency Injection**: Inject dependencies through constructor, setter, or method
3. **Use Factories**: Delegate object creation to factories
4. **Prefer Abstraction**: Use interfaces or abstract classes over concrete classes

**Practical Application**:
```java
// ❌ DIP Violation - Direct dependency on concrete class
public class OrderService {
    // Direct dependency on MySQLDatabase concrete class
    private MySQLDatabase database = new MySQLDatabase();

    public void processOrder(Order order) {
        // Must modify this code to change from MySQL to PostgreSQL
        database.save(order);
    }
}

// ✅ DIP Compliant - Depend on abstraction
public interface Database {
    void save(Order order);
    Order findById(Long id);
}

public class MySQLDatabase implements Database {
    @Override
    public void save(Order order) {
        // MySQL save logic
    }

    @Override
    public Order findById(Long id) {
        // MySQL query logic
        return null;
    }
}

public class PostgreSQLDatabase implements Database {
    @Override
    public void save(Order order) {
        // PostgreSQL save logic
    }

    @Override
    public Order findById(Long id) {
        // PostgreSQL query logic
        return null;
    }
}

// Depend on interface, use dependency injection
public class OrderService {
    private final Database database;

    // Constructor injection - Concrete implementation decided externally
    public OrderService(Database database) {
        this.database = database;
    }

    public void processOrder(Order order) {
        // Works with any database
        database.save(order);
    }
}

// Usage example
Database mysql = new MySQLDatabase();
OrderService service1 = new OrderService(mysql);

Database postgres = new PostgreSQLDatabase();
OrderService service2 = new OrderService(postgres);
```

**Dependency Injection Methods**:
```java
// 1. Constructor injection (recommended)
public class Service {
    private final Dependency dependency;

    public Service(Dependency dependency) {
        this.dependency = dependency;
    }
}

// 2. Setter injection
public class Service {
    private Dependency dependency;

    public void setDependency(Dependency dependency) {
        this.dependency = dependency;
    }
}

// 3. Method injection
public class Service {
    public void doSomething(Dependency dependency) {
        dependency.execute();
    }
}
```

**Validation Questions**:
- Is this class directly depending on concrete classes?
- Do you need to modify this class when changing implementations?

---
---

[← Back to Main](../SKILL.md)
