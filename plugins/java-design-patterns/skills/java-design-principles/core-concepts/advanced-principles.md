### 6. Maintenance Principles

#### Code For The Maintainer

**Definition**: Write code so that yourself in 6 months or another developer can understand it.

**Best Practices**:
1. **Clear Naming**: Meaningful variable, method, and class names
2. **Appropriate Comments**: Comments that explain "why"
3. **Maintain Consistency**: Consistent coding style and patterns
4. **Manage Complexity**: Split complex logic into multiple methods

**Principle of Least Astonishment**:
Code should behave as expected, not surprisingly.

#### Boy-Scout Rule

**Definition**: "Leave the campground cleaner than you found it"

**How to Apply**:
- Slightly improve surrounding code whenever modifying code
- Improve variable names, small refactoring, add comments, etc.
- Continuous small improvements rather than large refactoring

#### Avoid Premature Optimization

**Definition**: "Premature optimization is the root of all evil" - Donald Knuth

**How to Apply**:
1. **Make it work first**: Write correct code first
2. **Profiling**: Measure actual bottlenecks
3. **Optimize only where needed**: Based on measurement results
4. **Readability first**: Clear code is more important than fast code

---

### 7. Advanced Principles

#### Inversion of Control

**Definition**: A design principle where the framework controls the application flow.

**Types**:
1. **Dependency Injection (DI)**: Inject dependencies from outside
2. **Events/Callbacks**: Framework calls your code
3. **Template Method**: Framework controls algorithm structure

#### Command Query Separation

**Definition**:
- **Command**: Changes state but doesn't return a value
- **Query**: Returns a value but doesn't change state

**Application Example**:
```java
// ✅ CQS Compliant
public class Stack<T> {
    private List<T> items = new ArrayList<>();

    // Command - Changes state, no return
    public void push(T item) {
        items.add(item);
    }

    // Command - Changes state, no return
    public void pop() {
        if (!items.isEmpty()) {
            items.remove(items.size() - 1);
        }
    }

    // Query - No state change, returns value
    public T peek() {
        if (items.isEmpty()) {
            return null;
        }
        return items.get(items.size() - 1);
    }

    // Query - No state change, returns value
    public boolean isEmpty() {
        return items.isEmpty();
    }
}
```

#### Robustness Principle - Postel's Law

**Definition**: "Be conservative in what you send, be liberal in what you accept"

**Meaning**:
- **Output**: Strictly adhere to standards for output
- **Input**: Liberally accept various formats for input

**Application Example**:
```java
public class DateParser {
    // Accept various date formats (liberally)
    public LocalDate parse(String dateString) {
        // Allow various formats like "2024-01-15", "01/15/2024", "15-Jan-2024"
        try {
            return LocalDate.parse(dateString, DateTimeFormatter.ISO_DATE);
        } catch (DateTimeParseException e) {
            try {
                return LocalDate.parse(dateString, DateTimeFormatter.ofPattern("MM/dd/yyyy"));
            } catch (DateTimeParseException ex) {
                // Try other formats...
            }
        }
        throw new IllegalArgumentException("Invalid date format");
    }

    // Output in standard format (conservatively)
    public String format(LocalDate date) {
        return date.format(DateTimeFormatter.ISO_DATE);  // Always "2024-01-15" format
    }
}
```

---

[← Back to Main](../SKILL.md)
