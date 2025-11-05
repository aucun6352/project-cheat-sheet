# Simplicity Principles

## KISS (Keep It Simple, Stupid)

**Definition**: The principle of maintaining simple systems rather than complex ones.

**Why It Matters**:
- Simple code takes less time to write
- Less room for bugs to occur
- Easy to understand and simple to modify
- New team members can understand it quickly

**How to Apply**:
- Keep methods small, doing only one thing
- Consider refactoring when nested if statements exceed 3 levels
- Extract complex conditions into methods with clear names
- Consider splitting classes that exceed 100 lines

**Practical Tips**:
```java
// ❌ Complex
if ((user.getAge() >= 18 && user.hasLicense()) || user.hasParentalConsent())

// ✅ Simple
if (canDrive(user))

private boolean canDrive(User user) {
    return isAdult(user) || user.hasParentalConsent();
}

private boolean isAdult(User user) {
    return user.getAge() >= 18 && user.hasLicense();
}
```

## YAGNI (You Aren't Gonna Need It)

**Definition**: The principle of implementing features only when actually needed, avoiding predictive implementation for the future.

**Why It Matters**:
- Don't waste time writing unnecessary code
- Keeps the codebase concise
- Focuses on actual requirements
- Reduces cost of change

**Violation Signs**:
- Features added "because we might need it later"
- Unused methods or classes
- Overly generalized interfaces
- Extensibility considerations not yet required

**How to Apply**:
- Implement only the features required by the current story/ticket
- Stop when you think "we might need this later"
- Wait until it's actually needed, then add it

**Practical Example**:
```java
// ❌ YAGNI Violation - Unnecessary multi-language support
public class EmailService {
    public void sendEmail(String to, String subject, String body, Locale locale) {
        // Currently only supports English but added locale parameter "for later"
    }
}

// ✅ YAGNI Compliant - Only what's actually needed
public class EmailService {
    public void sendEmail(String to, String subject, String body) {
        // Add multi-language support when actually required
    }
}
```

## Do The Simplest Thing That Could Possibly Work

**Definition**: The principle of finding and implementing the simplest solution to a problem.

**How to Apply**:
- Always ask: "What's the simplest way to solve this problem?"
- After implementing the first solution, review if it can be made simpler
- Question whether complex patterns are really necessary

**Practical Tips**:
- Don't overuse design patterns (using Strategy pattern when a simple if statement suffices)
- Don't create interfaces for fewer than 3 implementations
- Make it work first, then improve

---

[← Back to Main](../SKILL.md)
