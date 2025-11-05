### 3. 코드 조직 원칙

#### DRY (Don't Repeat Yourself)

**정의**: 모든 지식은 시스템 내에서 단 한 번만, 명확하게, 권위 있게 표현되어야 합니다.

**왜 중요한가**:
- **유지보수 비용 감소**: 한 곳만 수정하면 됩니다
- **일관성 보장**: 같은 로직이 여러 곳에 있으면 불일치 발생 가능
- **버그 감소**: 중복 코드는 버그도 중복됩니다
- **코드 품질 향상**: 추상화를 통해 더 나은 설계로 이어집니다

**적용 방법**:
1. **메서드 추출**: 중복 코드를 메서드로 추출
2. **클래스 추출**: 관련 중복 코드를 별도 클래스로 분리
3. **상속/인터페이스**: 공통 동작을 부모 클래스나 인터페이스로
4. **유틸리티 클래스**: 범용적인 기능은 유틸리티로

**주의사항**:
- 우연한 중복(Accidental Duplication)과 진짜 중복을 구분하세요
- 비즈니스 로직이 같아 보여도 다른 이유로 변경될 수 있습니다
- 과도한 추상화는 오히려 복잡도를 높입니다

**실무 예시**:
```java
// ❌ 중복 코드
public class UserService {
    public void createUser(String email) {
        if (email == null || !email.matches("^[A-Za-z0-9+_.-]+@(.+)$")) {
            throw new IllegalArgumentException("Invalid email");
        }
        // 사용자 생성
    }

    public void updateEmail(String email) {
        if (email == null || !email.matches("^[A-Za-z0-9+_.-]+@(.+)$")) {
            throw new IllegalArgumentException("Invalid email");
        }
        // 이메일 업데이트
    }
}

// ✅ DRY 적용
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
        // 사용자 생성
    }

    public void updateEmail(String email) {
        EmailValidator.validate(email);
        // 이메일 업데이트
    }
}
```

#### Separation of Concerns (관심사의 분리)

**정의**: 프로그램을 서로 다른 관심사를 다루는 별개의 섹션으로 나누는 설계 원칙입니다.

**핵심 아이디어**:
각 모듈은 하나의 특정 관심사에만 집중하고, 다른 관심사와는 독립적이어야 합니다.

**관심사의 예**:
- **프레젠테이션 계층**: UI, 사용자 입력 처리
- **비즈니스 로직 계층**: 도메인 규칙, 비즈니스 규칙
- **데이터 접근 계층**: 데이터베이스, 파일 시스템

**적용 방법**:
- 계층형 아키텍처(Layered Architecture) 사용
- MVC(Model-View-Controller) 패턴 적용
- 각 패키지를 기능이 아닌 계층으로 구성

**실무 예시**:
```java
// 계층 분리
// 1. 프레젠테이션 계층
public class OrderController {
    private OrderService orderService;

    public ResponseEntity<Order> createOrder(OrderRequest request) {
        Order order = orderService.createOrder(request);
        return ResponseEntity.ok(order);
    }
}

// 2. 비즈니스 로직 계층
public class OrderService {
    private OrderRepository orderRepository;
    private PaymentService paymentService;

    public Order createOrder(OrderRequest request) {
        // 비즈니스 규칙 검증
        validateOrder(request);

        // 결제 처리
        paymentService.processPayment(request.getPayment());

        // 주문 저장
        return orderRepository.save(request.toOrder());
    }
}

// 3. 데이터 접근 계층
public class OrderRepository {
    public Order save(Order order) {
        // 데이터베이스 저장 로직
        return order;
    }
}
```

#### Maximize Cohesion (응집도 최대화)

**정의**: 모듈 내부의 요소들이 얼마나 관련되어 있는지를 나타내는 척도입니다.

**왜 중요한가**:
- 높은 응집도는 이해하기 쉽습니다
- 유지보수가 쉽습니다
- 재사용성이 높습니다
- 변경의 영향 범위가 작습니다

**응집도 레벨** (낮음 → 높음):
1. **우연적 응집**: 아무 관계 없는 기능들의 집합
2. **논리적 응집**: 비슷한 종류의 작업들의 집합
3. **시간적 응집**: 같은 시간에 실행되는 작업들
4. **절차적 응집**: 순서대로 실행되는 작업들
5. **통신적 응집**: 같은 데이터를 사용하는 작업들
6. **순차적 응집**: 한 작업의 출력이 다음 작업의 입력
7. **기능적 응집**: 하나의 명확한 목적을 위한 작업들 (최고)

**적용 방법**:
- 클래스의 모든 메서드가 같은 인스턴스 변수를 사용하는지 확인
- 서로 관련 없는 메서드는 별도 클래스로 분리
- 메서드 이름에 클래스 이름이 포함되는지 확인

---

### 4. 결합도 관리

#### Minimize Coupling (결합도 최소화)

**정의**: 모듈 간의 상호 의존성을 최소화하는 원칙입니다.

**결합도 레벨** (낮음 → 높음):
1. **메시지 결합**: 파라미터만 전달
2. **데이터 결합**: 데이터 구조 전달
3. **스탬프 결합**: 복잡한 데이터 구조 전달
4. **제어 결합**: 제어 플래그 전달
5. **외부 결합**: 외부 도구나 프로토콜 공유
6. **공통 결합**: 전역 데이터 공유
7. **내용 결합**: 다른 모듈의 내부 수정 (최악)

**적용 방법**:
- 인터페이스를 통한 의존성 주입
- 이벤트 기반 아키텍처
- 메시지 큐 사용
- 공유 상태 최소화

#### Law of Demeter (데메테르 법칙)

**정의**: "최소 지식의 원칙" - 객체는 자신이 직접 알아야 할 것만 알아야 합니다.

**규칙**: 메서드는 다음의 메서드만 호출할 수 있습니다:
1. 같은 객체의 메서드
2. 파라미터로 전달된 객체의 메서드
3. 메서드 내에서 생성한 객체의 메서드
4. 인스턴스 변수의 메서드

**왜 중요한가**:
- 결합도를 낮춥니다
- 변경의 파급 효과를 줄입니다
- 캡슐화를 강화합니다

**실무 적용**:
```java
// ❌ Law of Demeter 위반
public class ShoppingCart {
    public void checkout(Customer customer) {
        // 여러 계층을 통과하는 메서드 체이닝
        String street = customer.getAddress().getStreet();
        String city = customer.getAddress().getCity();
        String zipCode = customer.getAddress().getZipCode();

        // ShoppingCart가 Address의 내부 구조를 알아야 함 - 결합도 증가
    }
}

// ✅ Law of Demeter 준수
public class Customer {
    private Address address;

    // Customer가 필요한 정보를 직접 제공
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
        // 간단하고 결합도가 낮음
        String address = customer.getShippingAddress();
    }
}
```

#### Composition Over Inheritance (상속보다 컴포지션)

**정의**: 상속("is-a")보다 컴포지션("has-a")을 선호하라는 원칙입니다.

**왜 중요한가**:
- **유연성**: 런타임에 동작을 변경할 수 있습니다
- **결합도 감소**: 부모 클래스 변경에 영향받지 않습니다
- **재사용성**: 필요한 기능만 조합할 수 있습니다
- **테스트 용이성**: 각 구성 요소를 독립적으로 테스트 가능

**상속을 사용해야 하는 경우**:
- 명확한 "is-a" 관계일 때
- LSP를 만족할 때
- 부모의 모든 메서드가 자식에게 의미있을 때

**컴포지션을 사용해야 하는 경우**:
- "has-a" 관계일 때
- 런타임에 동작을 변경해야 할 때
- 여러 클래스의 기능을 조합해야 할 때


---

[← 메인으로 돌아가기](../SKILL.md)
