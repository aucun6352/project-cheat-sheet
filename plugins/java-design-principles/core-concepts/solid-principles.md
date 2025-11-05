### 2. SOLID 원칙

#### Single Responsibility Principle (SRP) - 단일 책임 원칙

**정의**: 클래스는 변경해야 할 이유가 단 하나여야 하며, 하나의 책임만 가져야 합니다.

**핵심 아이디어**:
"책임"은 "변경의 이유"를 의미합니다. 클래스가 여러 이유로 변경된다면, 그것은 여러 책임을 가진 것입니다.

**왜 중요한가**:
- **유지보수성 향상**: 한 기능의 변경이 다른 기능에 영향을 주지 않습니다
- **테스트 용이성**: 단일 책임 클래스는 테스트하기 쉽습니다
- **재사용성 증가**: 특정 책임만 필요할 때 해당 클래스만 재사용할 수 있습니다
- **이해 용이성**: 클래스의 목적이 명확합니다

**위반 징후**:
- 클래스 이름에 "Manager", "Utility", "Helper" 같은 모호한 단어 사용
- 하나의 클래스에서 데이터베이스 접근, 비즈니스 로직, UI 처리를 모두 수행
- 클래스를 설명할 때 "그리고(and)"가 필요한 경우
- 메서드가 10개 이상인 클래스

**적용 방법**:
1. **책임 식별**: 클래스가 무엇을 하는지 한 문장으로 설명해보세요
2. **분리 시점 판단**: "그리고"가 들어가면 분리를 고려하세요
3. **응집도 확인**: 클래스의 모든 메서드가 동일한 데이터를 사용하는가?
4. **변경 이유 분석**: 이 클래스가 변경되는 이유를 나열해보세요

**실무 적용**:
```java
// ❌ SRP 위반 - 여러 책임을 가진 클래스
public class Employee {
    private String name;
    private double salary;

    // 책임 1: 급여 계산
    public double calculatePay() { }

    // 책임 2: 데이터베이스 저장
    public void save() { }

    // 책임 3: 보고서 생성
    public String generateReport() { }
}
// 문제: 급여 정책 변경, 데이터베이스 변경, 보고서 형식 변경 모두 이 클래스를 수정하게 됨

// ✅ SRP 준수 - 각 책임을 별도 클래스로 분리
public class Employee {
    private String name;
    private double salary;

    public String getName() { return name; }
    public double getSalary() { return salary; }
}

public class PayrollCalculator {
    public double calculatePay(Employee employee) {
        // 급여 계산 로직
    }
}

public class EmployeeRepository {
    public void save(Employee employee) {
        // 데이터베이스 저장 로직
    }
}

public class EmployeeReportGenerator {
    public String generateReport(Employee employee) {
        // 보고서 생성 로직
    }
}
```

**검증 질문**:
- 이 클래스를 변경해야 하는 이유가 여러 개인가?
- 다른 액터(사용자, 시스템)가 이 클래스의 다른 메서드를 사용하는가?

---

#### Open/Closed Principle (OCP) - 개방/폐쇄 원칙

**정의**: 소프트웨어 엔티티(클래스, 모듈, 함수)는 확장에는 열려 있고 수정에는 닫혀 있어야 합니다.

**핵심 아이디어**:
기존 코드를 변경하지 않고도(닫혀 있음) 새로운 기능을 추가할 수 있어야(열려 있음) 합니다.

**왜 중요한가**:
- **안정성**: 기존 코드를 건드리지 않아 버그 발생 위험이 적습니다
- **확장성**: 새로운 기능을 쉽게 추가할 수 있습니다
- **유지보수성**: 변경의 영향 범위가 제한적입니다
- **재사용성**: 기존 코드를 다양한 방식으로 확장할 수 있습니다

**적용 방법**:
1. **추상화 사용**: 인터페이스나 추상 클래스로 변화 지점을 추상화
2. **다형성 활용**: 상속이나 인터페이스 구현으로 확장
3. **전략 패턴**: 알고리즘을 인터페이스로 정의하고 구현체를 교체
4. **템플릿 메서드**: 변하지 않는 부분은 고정하고 변하는 부분만 확장

**위반 징후**:
- 새 기능 추가 시 기존 클래스의 if-else나 switch-case가 늘어남
- instanceof로 타입을 체크하는 코드
- 새 타입 추가마다 여러 클래스를 수정해야 함

**실무 적용**:
```java
// ❌ OCP 위반 - 새 도형 추가마다 이 클래스를 수정해야 함
public class AreaCalculator {
    public double calculateArea(Object shape) {
        if (shape instanceof Circle) {
            Circle circle = (Circle) shape;
            return Math.PI * circle.radius * circle.radius;
        } else if (shape instanceof Rectangle) {
            Rectangle rect = (Rectangle) shape;
            return rect.width * rect.height;
        }
        // 새 도형을 추가할 때마다 여기에 else if를 추가해야 함
        return 0;
    }
}

// ✅ OCP 준수 - 확장에는 열려있고 수정에는 닫혀있음
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

// 새 도형 추가 - 기존 코드 수정 없이 확장만
public class Triangle implements Shape {
    private double base;
    private double height;

    @Override
    public double calculateArea() {
        return 0.5 * base * height;
    }
}

// 계산기는 전혀 수정할 필요 없음
public class AreaCalculator {
    public double calculateArea(Shape shape) {
        return shape.calculateArea();
    }
}
```

**검증 질문**:
- 새 기능을 추가할 때 기존 클래스를 수정하고 있는가?
- if-else나 switch-case가 점점 늘어나고 있는가?

---

#### Liskov Substitution Principle (LSP) - 리스코프 치환 원칙

**정의**: 서브타입(자식 클래스)은 언제나 기반타입(부모 클래스)으로 교체할 수 있어야 합니다.

**핵심 아이디어**:
부모 클래스를 사용하는 코드가 자식 클래스로 대체되어도 정상적으로 동작해야 합니다.

**왜 중요한가**:
- **다형성의 올바른 사용**: 상속을 올바르게 사용했는지 검증합니다
- **예측 가능성**: 서브타입이 예상치 못한 동작을 하지 않습니다
- **신뢰성**: 부모 타입의 계약을 자식도 지킵니다
- **유지보수성**: 상속 계층을 안전하게 변경할 수 있습니다

**위반 징후**:
- 자식 클래스에서 부모의 메서드를 오버라이드하여 예외를 던짐
- 자식 클래스가 부모의 메서드를 비워두거나 아무것도 안 함
- 자식 클래스가 부모보다 더 강한 사전조건을 요구
- 자식 클래스가 부모보다 더 약한 사후조건을 보장

**적용 방법**:
1. **계약 준수**: 부모 클래스의 메서드 계약을 자식도 지켜야 함
2. **사전조건 강화 금지**: 자식이 더 엄격한 입력을 요구하면 안 됨
3. **사후조건 약화 금지**: 자식이 더 약한 출력을 보장하면 안 됨
4. **불변조건 유지**: 부모의 불변 규칙을 자식도 유지해야 함

**실무 적용**:
```java
// ❌ LSP 위반 - 직사각형-정사각형 문제
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
        this.height = width;  // 정사각형이므로 높이도 같게
    }

    @Override
    public void setHeight(int height) {
        this.width = height;
        this.height = height;  // 정사각형이므로 너비도 같게
    }
}

// 문제 발생
public void testRectangle(Rectangle rect) {
    rect.setWidth(5);
    rect.setHeight(4);
    assert rect.getArea() == 20;  // Rectangle이면 통과, Square면 실패!
}

// ✅ LSP 준수 - 상속 대신 컴포지션 사용
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

**검증 질문**:
- 부모 타입을 자식 타입으로 바꿨을 때 코드가 여전히 올바르게 동작하는가?
- 자식 클래스에서 부모의 메서드를 오버라이드할 때 UnsupportedOperationException을 던지고 있는가?

---

#### Interface Segregation Principle (ISP) - 인터페이스 분리 원칙

**정의**: 클라이언트는 자신이 사용하지 않는 메서드에 의존하도록 강요받아서는 안 됩니다.

**핵심 아이디어**:
하나의 큰 인터페이스보다 여러 개의 작고 구체적인 인터페이스가 낫습니다.

**왜 중요한가**:
- **불필요한 의존성 제거**: 사용하지 않는 메서드 변경에 영향받지 않습니다
- **유연성 증가**: 필요한 인터페이스만 구현할 수 있습니다
- **이해 용이성**: 작은 인터페이스는 이해하기 쉽습니다
- **테스트 간소화**: 작은 인터페이스는 모킹하기 쉽습니다

**위반 징후**:
- 인터페이스의 일부 메서드만 사용하는 클라이언트
- 빈 메서드나 예외를 던지는 메서드 구현
- 메서드가 10개 이상인 인터페이스
- 서로 관련 없는 기능들이 하나의 인터페이스에 있음

**적용 방법**:
1. **역할별 분리**: 인터페이스를 역할(role)별로 분리하세요
2. **클라이언트 중심**: 클라이언트가 실제로 사용하는 메서드만 포함하세요
3. **작게 유지**: 인터페이스는 작고 응집도 높게 유지하세요
4. **다중 구현**: 필요한 경우 여러 인터페이스를 구현하세요

**실무 적용**:
```java
// ❌ ISP 위반 - 비대한 인터페이스
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

// 문제: 개발자는 manageTeam이 필요 없고, 매니저는 writeCode가 필요 없음
public class Developer implements Worker {
    public void work() { /* 구현 */ }
    public void eat() { /* 구현 */ }
    public void sleep() { /* 구현 */ }
    public void getPaid() { /* 구현 */ }
    public void attendMeeting() { /* 구현 */ }
    public void writeCode() { /* 구현 */ }
    public void reviewCode() { /* 구현 */ }
    public void manageTeam() {
        throw new UnsupportedOperationException("Developer doesn't manage team");
    }
}

// ✅ ISP 준수 - 역할별로 인터페이스 분리
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

// 필요한 인터페이스만 구현
public class Developer implements Workable, Eatable, Sleepable, Payable, Codeable {
    public void work() { /* 구현 */ }
    public void eat() { /* 구현 */ }
    public void sleep() { /* 구현 */ }
    public void getPaid() { /* 구현 */ }
    public void writeCode() { /* 구현 */ }
    public void reviewCode() { /* 구현 */ }
}

public class Manager implements Workable, Eatable, Sleepable, Payable, Manageable {
    public void work() { /* 구현 */ }
    public void eat() { /* 구현 */ }
    public void sleep() { /* 구현 */ }
    public void getPaid() { /* 구현 */ }
    public void manageTeam() { /* 구현 */ }
    public void attendMeeting() { /* 구현 */ }
}
```

**검증 질문**:
- 이 인터페이스를 구현하는 모든 클래스가 모든 메서드를 실제로 사용하는가?
- 빈 메서드나 예외를 던지는 메서드가 있는가?

---

#### Dependency Inversion Principle (DIP) - 의존성 역전 원칙

**정의**:
1. 고수준 모듈은 저수준 모듈에 의존해서는 안 됩니다. 둘 다 추상화에 의존해야 합니다.
2. 추상화는 세부사항에 의존해서는 안 됩니다. 세부사항이 추상화에 의존해야 합니다.

**핵심 아이디어**:
구체적인 구현이 아닌 인터페이스나 추상 클래스에 의존하세요.

**왜 중요한가**:
- **결합도 감소**: 구현 변경이 상위 모듈에 영향을 주지 않습니다
- **테스트 용이성**: Mock 객체로 쉽게 대체할 수 있습니다
- **유연성 증가**: 구현체를 쉽게 교체할 수 있습니다
- **확장성**: 새로운 구현체를 추가하기 쉽습니다

**위반 징후**:
- 클래스 내부에서 `new` 키워드로 구체 클래스를 직접 생성
- 구체 클래스 타입으로 필드나 파라미터를 선언
- static 메서드에 직접 의존
- 데이터베이스, 파일 시스템 등 외부 시스템에 직접 의존

**적용 방법**:
1. **인터페이스 정의**: 의존할 기능을 인터페이스로 정의
2. **의존성 주입**: 생성자, 세터, 또는 메서드로 의존성을 주입
3. **팩토리 사용**: 객체 생성을 팩토리에 위임
4. **추상화 선호**: 구체 클래스보다 인터페이스나 추상 클래스 사용

**실무 적용**:
```java
// ❌ DIP 위반 - 구체 클래스에 직접 의존
public class OrderService {
    // MySQLDatabase라는 구체 클래스에 직접 의존
    private MySQLDatabase database = new MySQLDatabase();

    public void processOrder(Order order) {
        // MySQL에서 PostgreSQL로 변경하려면 이 코드를 수정해야 함
        database.save(order);
    }
}

// ✅ DIP 준수 - 추상화에 의존
public interface Database {
    void save(Order order);
    Order findById(Long id);
}

public class MySQLDatabase implements Database {
    @Override
    public void save(Order order) {
        // MySQL 저장 로직
    }

    @Override
    public Order findById(Long id) {
        // MySQL 조회 로직
        return null;
    }
}

public class PostgreSQLDatabase implements Database {
    @Override
    public void save(Order order) {
        // PostgreSQL 저장 로직
    }

    @Override
    public Order findById(Long id) {
        // PostgreSQL 조회 로직
        return null;
    }
}

// 인터페이스에 의존하고, 의존성 주입 사용
public class OrderService {
    private final Database database;

    // 생성자 주입 - 구체적인 구현은 외부에서 결정
    public OrderService(Database database) {
        this.database = database;
    }

    public void processOrder(Order order) {
        // 어떤 데이터베이스든 상관없이 동작
        database.save(order);
    }
}

// 사용 예시
Database mysql = new MySQLDatabase();
OrderService service1 = new OrderService(mysql);

Database postgres = new PostgreSQLDatabase();
OrderService service2 = new OrderService(postgres);
```

**의존성 주입 방법**:
```java
// 1. 생성자 주입 (권장)
public class Service {
    private final Dependency dependency;

    public Service(Dependency dependency) {
        this.dependency = dependency;
    }
}

// 2. 세터 주입
public class Service {
    private Dependency dependency;

    public void setDependency(Dependency dependency) {
        this.dependency = dependency;
    }
}

// 3. 메서드 주입
public class Service {
    public void doSomething(Dependency dependency) {
        dependency.execute();
    }
}
```

**검증 질문**:
- 이 클래스가 구체 클래스에 직접 의존하고 있는가?
- 구현체를 변경할 때 이 클래스도 수정해야 하는가?

---
---

[← 메인으로 돌아가기](../SKILL.md)
