
### 5. Encapsulation

#### Encapsulation & Information Hiding

**Definition**:
- **Encapsulation**: Bundling data and methods into a single unit
- **Information Hiding**: Hiding internal implementation details from the outside

**How to Apply**:
1. **Fields are private**: Declare all fields as private
2. **Getter/Setter Carefully**: Indiscriminate getter/setter breaks encapsulation
3. **Behavior-Centric**: Provide behavior, not data exposure
4. **Immutability**: Design as immutable objects when possible

**Practical Example**:
```java
// ❌ Encapsulation Violation
public class BankAccount {
    public double balance;  // public field
}

// Can be manipulated directly from outside - Dangerous!
account.balance = -1000;

// ✅ Encapsulation Compliant
public class BankAccount {
    private double balance;  // private field

    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        }
    }

    public void withdraw(double amount) {
        if (amount > 0 && balance >= amount) {
            balance -= amount;
        } else {
            throw new IllegalArgumentException("Insufficient balance");
        }
    }

    public double getBalance() {
        return balance;
    }
}
```

#### Encapsulate What Changes

**Definition**: Identify parts that are likely to change and encapsulate them to minimize impact of changes.

**How to Apply**:
1. **Identify Change Points**: Find parts that change frequently or are likely to change
2. **Abstract with Interface**: Hide change points behind interfaces
3. **Use Strategy Pattern**: Encapsulate algorithms
4. **Externalize Configuration**: Use configuration files instead of hardcoding

---

---

[← Back to Main](../SKILL.md)
