# Best Practices & Common Pitfalls


## Best Practices

### During Design

1. **Start simple** - Follow YAGNI principle and add complexity only when actually needed
2. **Depend on abstractions** - Depend on interfaces or abstract classes rather than concrete implementations
3. **Create small interfaces** - Follow ISP and provide client-specific interfaces
4. **Favor composition** - Use inheritance only for clear "is-a" relationships

### During Coding

5. **Assign a single responsibility** - Each class and method should have one clear purpose
6. **Eliminate duplication** - Abstract when the same logic exists in multiple places
7. **Follow the Law of Demeter** - Use only direct relationships and avoid excessive method chaining
8. **Encapsulate variability** - Hide frequently changing parts behind interfaces

### During Maintenance

9. **Practice the Boy Scout Rule** - Leave the code cleaner than you found it
10. **Code for maintainers** - Write so that yourself in 6 months or other developers can understand
11. **Don't optimize before measuring** - Optimize after profiling identifies actual bottlenecks
12. **Make refactoring routine** - Continuously perform small improvements

### During Code Review

13. **Identify principle violations** - Review code from each design principle's perspective
14. **Check coupling and cohesion** - Evaluate whether dependencies between modules are appropriate
15. **Consider testability** - Good design is easy to test

## Common Pitfalls

### 1. Over-Engineering (Excessive Abstraction)
**Problem**: Creating unnecessarily complex structures by trying to predict the future
```java
// Applying Strategy pattern, Factory pattern, and Visitor pattern all to a simple calculator
// Situation where 10 classes are needed to calculate 2+2
```
**Solution**: Follow YAGNI and KISS principles. Add complexity when actually needed.

### 2. Excessive Use of Inheritance
**Problem**: Using inheritance for code reuse even when there's no "is-a" relationship
```java
// Implementing Stack by inheriting ArrayList - incorrect design
public class Stack extends ArrayList {
    public void push(Object item) { add(item); }
    public Object pop() { return remove(size() - 1); }
}
// Problem: Exposes ArrayList methods like add(index, element) from Stack
```
**Solution**: Composition Over Inheritance - Have ArrayList as a member variable.

### 3. God Object
**Problem**: A single class has too many responsibilities - violates SRP
```java
public class Application {
    // Handles everything: database connections, UI rendering, business logic, logging, etc.
    // 5000 lines of code, 100 methods
}
```
**Solution**: Separate classes by responsibility.

### 4. Law of Demeter Violation
**Problem**: Method chaining that passes through multiple layers
```java
// Excessive dependencies
customer.getOrder().getItems().get(0).getPrice();
```
**Solution**: Add methods that directly provide the needed information.

### 5. Premature Optimization
**Problem**: Increasing complexity by trying to make things "faster" even without performance issues
```java
// Instead of clear code
List<String> names = users.stream().map(User::getName).collect(Collectors.toList());

// Writing complex code for "optimization"
String[] names = new String[users.size()];
for (int i = 0; i < users.size(); i++) names[i] = users.get(i).getName();
```
**Solution**: Write clearly first, then optimize if needed after profiling.

### 6. Fat Interface
**Problem**: Interface with too many methods - violates ISP
```java
public interface Worker {
    void work();
    void eat();
    void sleep();
    void getPaid();
    void takeBreak();
    void attendMeeting();
    // ... 20 methods
}
// Implementation classes must implement even unused methods
```
**Solution**: Separate interfaces by role.

### 7. Code Duplication
**Problem**: Placing the same logic in multiple places through copy-paste - violates DRY
**Solution**: Extract and reuse common logic.

### 8. Tight Coupling
**Problem**: Directly depending on concrete implementations - violates DIP
```java
public class OrderService {
    private MySQLDatabase database = new MySQLDatabase(); // Depends on concrete implementation
    // Code modification required to change from MySQL to PostgreSQL
}
```
**Solution**: Depend on interfaces and use dependency injection.


---

[← Back to Main](SKILL.md)
