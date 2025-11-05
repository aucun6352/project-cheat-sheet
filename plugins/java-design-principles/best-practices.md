# Best Practices & Common Pitfalls


## Best Practices

### 설계 시

1. **단순하게 시작하세요** - YAGNI 원칙을 따라 실제로 필요할 때만 복잡성을 추가하세요
2. **추상화에 의존하세요** - 구체적인 구현보다 인터페이스나 추상 클래스에 의존하세요
3. **작은 인터페이스를 만드세요** - ISP를 따라 클라이언트별로 특화된 인터페이스를 제공하세요
4. **컴포지션을 우선 고려하세요** - 상속은 명확한 "is-a" 관계일 때만 사용하세요

### 코딩 시

5. **하나의 책임만 부여하세요** - 각 클래스와 메서드는 하나의 명확한 목적만 가져야 합니다
6. **중복을 제거하세요** - 동일한 로직이 여러 곳에 있다면 추상화하세요
7. **데메테르 법칙을 따르세요** - 직접적인 관계만 사용하고 과도한 메서드 체이닝을 피하세요
8. **변경 가능성을 캡슐화하세요** - 자주 변경되는 부분을 인터페이스 뒤에 숨기세요

### 유지보수 시

9. **보이스카우트 규칙을 실천하세요** - 코드를 발견했을 때보다 더 깨끗하게 만드세요
10. **유지보수자를 위해 코딩하세요** - 6개월 후의 자신이나 다른 개발자가 이해할 수 있게 작성하세요
11. **측정 전에 최적화하지 마세요** - 프로파일링으로 실제 병목을 확인한 후 최적화하세요
12. **리팩토링을 일상화하세요** - 작은 개선을 지속적으로 수행하세요

### 코드 리뷰 시

13. **원칙 위반을 식별하세요** - 각 설계 원칙의 관점에서 코드를 검토하세요
14. **결합도와 응집도를 확인하세요** - 모듈 간 의존성이 적절한지 평가하세요
15. **테스트 가능성을 고려하세요** - 좋은 설계는 테스트하기 쉽습니다

## Common Pitfalls

### 1. 과도한 추상화 (Over-Engineering)
**문제**: 미래를 예측하여 필요 이상으로 복잡한 구조를 만듦
```java
// 단순한 계산기에 Strategy 패턴, Factory 패턴, Visitor 패턴을 모두 적용
// 2+2를 계산하는데 10개의 클래스가 필요한 상황
```
**해결**: YAGNI와 KISS 원칙을 따르세요. 실제로 필요할 때 복잡성을 추가하세요.

### 2. 상속의 과도한 사용
**문제**: "is-a" 관계가 아닌데도 코드 재사용을 위해 상속 사용
```java
// ArrayList를 상속받아 Stack 구현 - 잘못된 설계
public class Stack extends ArrayList {
    public void push(Object item) { add(item); }
    public Object pop() { return remove(size() - 1); }
}
// 문제: Stack에서 add(index, element) 같은 ArrayList 메서드 노출
```
**해결**: Composition Over Inheritance - ArrayList를 멤버 변수로 가지세요.

### 3. 신 객체 (God Object)
**문제**: 하나의 클래스가 너무 많은 책임을 가짐 - SRP 위반
```java
public class Application {
    // 데이터베이스 연결, UI 렌더링, 비즈니스 로직, 로깅 등 모든 것을 담당
    // 5000줄의 코드, 100개의 메서드
}
```
**해결**: 책임별로 클래스를 분리하세요.

### 4. 법칙 위반 (Law of Demeter Violation)
**문제**: 여러 계층을 통과하는 메서드 체이닝
```java
// 과도한 의존성
customer.getOrder().getItems().get(0).getPrice();
```
**해결**: 필요한 정보를 직접 제공하는 메서드를 추가하세요.

### 5. 조기 최적화 (Premature Optimization)
**문제**: 성능 문제가 없는데도 "더 빠르게" 하려다 복잡성 증가
```java
// 명확한 코드 대신
List<String> names = users.stream().map(User::getName).collect(Collectors.toList());

// "최적화"를 위해 복잡한 코드 작성
String[] names = new String[users.size()];
for (int i = 0; i < users.size(); i++) names[i] = users.get(i).getName();
```
**해결**: 먼저 명확하게 작성하고, 프로파일링 후 필요하면 최적화하세요.

### 6. 인터페이스 비대화 (Fat Interface)
**문제**: 너무 많은 메서드를 가진 인터페이스 - ISP 위반
```java
public interface Worker {
    void work();
    void eat();
    void sleep();
    void getPaid();
    void takeBreak();
    void attendMeeting();
    // ... 20개의 메서드
}
// 구현 클래스가 사용하지 않는 메서드도 구현해야 함
```
**해결**: 인터페이스를 역할별로 분리하세요.

### 7. 불필요한 중복 (Code Duplication)
**문제**: 같은 로직을 복사-붙여넣기로 여러 곳에 배치 - DRY 위반
**해결**: 공통 로직을 추출하여 재사용하세요.

### 8. 단단한 결합 (Tight Coupling)
**문제**: 구체적인 구현에 직접 의존 - DIP 위반
```java
public class OrderService {
    private MySQLDatabase database = new MySQLDatabase(); // 구체적 구현에 의존
    // MySQL에서 PostgreSQL로 변경하려면 코드 수정 필요
}
```
**해결**: 인터페이스에 의존하고 의존성 주입을 사용하세요.


---

[← 메인으로 돌아가기](SKILL.md)
