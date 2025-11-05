
## Patterns

### Pattern 1: Single Responsibility Principle

**❌ 나쁜 예 - 여러 책임을 가진 클래스**
```java
// 사용자 관리, 이메일 발송, 데이터베이스 접근을 모두 담당
public class User {
    private String name;
    private String email;

    public void save() {
        // 데이터베이스에 저장하는 로직
        Connection conn = DriverManager.getConnection("jdbc:mysql://localhost/db");
        // ... SQL 실행
    }

    public void sendEmail(String message) {
        // 이메일 발송 로직
        // ... SMTP 설정 및 발송
    }

    public void generateReport() {
        // 보고서 생성 로직
        // ... 리포트 생성
    }
}
```

**✅ 좋은 예 - 단일 책임으로 분리**
```java
// 사용자 데이터만 담당
public class User {
    private String name;
    private String email;

    public String getName() { return name; }
    public String getEmail() { return email; }
}

// 데이터베이스 접근만 담당
public class UserRepository {
    public void save(User user) {
        // 데이터베이스 저장 로직
    }

    public User findById(Long id) {
        // 데이터베이스 조회 로직
        return null;
    }
}

// 이메일 발송만 담당
public class EmailService {
    public void sendEmail(User user, String message) {
        // 이메일 발송 로직
    }
}

// 보고서 생성만 담당
public class ReportGenerator {
    public void generateUserReport(User user) {
        // 보고서 생성 로직
    }
}
```

### Pattern 2: Open/Closed Principle

**❌ 나쁜 예 - 새로운 타입 추가 시 기존 코드 수정 필요**
```java
public class AreaCalculator {
    public double calculateArea(Object shape) {
        if (shape instanceof Circle) {
            Circle circle = (Circle) shape;
            return Math.PI * circle.radius * circle.radius;
        } else if (shape instanceof Rectangle) {
            Rectangle rect = (Rectangle) shape;
            return rect.width * rect.height;
        }
        // 새로운 도형을 추가할 때마다 이 메서드를 수정해야 함
        return 0;
    }
}
```

**✅ 좋은 예 - 확장에는 열려있고 수정에는 닫혀있음**
```java
// 추상화를 통한 확장 가능한 설계
public interface Shape {
    double calculateArea();
}

public class Circle implements Shape {
    private double radius;

    public Circle(double radius) {
        this.radius = radius;
    }

    @Override
    public double calculateArea() {
        return Math.PI * radius * radius;
    }
}

public class Rectangle implements Shape {
    private double width;
    private double height;

    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }

    @Override
    public double calculateArea() {
        return width * height;
    }
}

// 새로운 도형을 추가해도 기존 코드를 수정할 필요 없음
public class Triangle implements Shape {
    private double base;
    private double height;

    public Triangle(double base, double height) {
        this.base = base;
        this.height = height;
    }

    @Override
    public double calculateArea() {
        return 0.5 * base * height;
    }
}

// 계산기는 수정할 필요가 없음
public class AreaCalculator {
    public double calculateArea(Shape shape) {
        return shape.calculateArea();
    }
}
```

### Pattern 3: Law of Demeter

**❌ 나쁜 예 - 과도한 메서드 체이닝**
```java
// "낯선 이와 대화하고 있음" - 여러 계층을 통과
public class OrderProcessor {
    public void processOrder(Order order) {
        // 주문 -> 고객 -> 주소 -> 우편번호를 직접 접근
        String zipCode = order.getCustomer().getAddress().getZipCode();

        // Order가 Customer의 구조를 알아야 하고,
        // Customer가 Address의 구조를 알아야 함
        // 높은 결합도!
    }
}
```

**✅ 좋은 예 - 직접 관계만 사용**
```java
public class Order {
    private Customer customer;

    // Order가 필요한 정보를 직접 제공
    public String getShippingZipCode() {
        return customer.getZipCode();
    }
}

public class Customer {
    private Address address;

    // Customer가 필요한 정보를 직접 제공
    public String getZipCode() {
        return address.getZipCode();
    }
}

public class Address {
    private String zipCode;

    public String getZipCode() {
        return zipCode;
    }
}

public class OrderProcessor {
    public void processOrder(Order order) {
        // 간단하고 결합도가 낮음
        String zipCode = order.getShippingZipCode();
    }
}
```

### Pattern 4: Composition Over Inheritance

**❌ 나쁜 예 - 상속 남용**
```java
public class Bird {
    public void fly() {
        System.out.println("Flying...");
    }
}

// 펭귄은 새이지만 날 수 없음 - LSP 위반!
public class Penguin extends Bird {
    @Override
    public void fly() {
        throw new UnsupportedOperationException("Penguins can't fly!");
    }
}
```

**✅ 좋은 예 - 컴포지션 사용**
```java
// 행동을 인터페이스로 분리
public interface FlyBehavior {
    void fly();
}

public class CanFly implements FlyBehavior {
    @Override
    public void fly() {
        System.out.println("Flying...");
    }
}

public class CannotFly implements FlyBehavior {
    @Override
    public void fly() {
        System.out.println("Can't fly");
    }
}

// 컴포지션을 통한 유연한 설계
public class Bird {
    private FlyBehavior flyBehavior;

    public Bird(FlyBehavior flyBehavior) {
        this.flyBehavior = flyBehavior;
    }

    public void performFly() {
        flyBehavior.fly();
    }

    // 런타임에 행동을 변경할 수도 있음
    public void setFlyBehavior(FlyBehavior flyBehavior) {
        this.flyBehavior = flyBehavior;
    }
}

public class Sparrow extends Bird {
    public Sparrow() {
        super(new CanFly());
    }
}

public class Penguin extends Bird {
    public Penguin() {
        super(new CannotFly());
    }
}
```

### Pattern 5: DRY (Don't Repeat Yourself)

**❌ 나쁜 예 - 중복 코드**
```java
public class UserService {
    public void createUser(String name, String email) {
        // 이메일 유효성 검사
        if (email == null || !email.contains("@")) {
            throw new IllegalArgumentException("Invalid email");
        }
        // 사용자 생성 로직
        System.out.println("Creating user: " + name);
    }

    public void updateUserEmail(User user, String newEmail) {
        // 똑같은 이메일 유효성 검사 - 중복!
        if (newEmail == null || !newEmail.contains("@")) {
            throw new IllegalArgumentException("Invalid email");
        }
        // 이메일 업데이트 로직
        user.setEmail(newEmail);
    }
}
```

**✅ 좋은 예 - 중복 제거**
```java
public class UserService {
    // 공통 로직을 추출하여 재사용
    private void validateEmail(String email) {
        if (email == null || !email.contains("@")) {
            throw new IllegalArgumentException("Invalid email");
        }
    }

    public void createUser(String name, String email) {
        validateEmail(email);
        System.out.println("Creating user: " + name);
    }

    public void updateUserEmail(User user, String newEmail) {
        validateEmail(newEmail);
        user.setEmail(newEmail);
    }
}

// 더 나은 방법: 별도 클래스로 분리
public class EmailValidator {
    public static void validate(String email) {
        if (email == null || !email.contains("@")) {
            throw new IllegalArgumentException("Invalid email");
        }
    }
}

public class UserService {
    public void createUser(String name, String email) {
        EmailValidator.validate(email);
        System.out.println("Creating user: " + name);
    }

    public void updateUserEmail(User user, String newEmail) {
        EmailValidator.validate(newEmail);
        user.setEmail(newEmail);
    }
}
```

---

[← 메인으로 돌아가기](../SKILL.md)
