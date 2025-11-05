### 6. 유지보수 원칙

#### Code For The Maintainer (유지보수자를 위한 코딩)

**정의**: 6개월 후의 자신이나 다른 개발자가 코드를 이해할 수 있도록 작성하세요.

**실천 방법**:
1. **명확한 네이밍**: 변수, 메서드, 클래스 이름을 의미있게
2. **주석의 적절한 사용**: "왜"를 설명하는 주석
3. **일관성 유지**: 코딩 스타일과 패턴을 일관되게
4. **복잡도 관리**: 복잡한 로직은 여러 메서드로 분리

**Principle of Least Astonishment (최소 놀람의 원칙)**:
코드가 예상한 대로 동작해야 하며, 놀라운 동작을 해서는 안 됩니다.

#### Boy-Scout Rule (보이스카우트 규칙)

**정의**: "캠핑장을 발견했을 때보다 더 깨끗하게 남겨두세요"

**적용 방법**:
- 코드를 수정할 때마다 주변 코드도 약간 개선
- 변수명 개선, 작은 리팩토링, 주석 추가 등
- 큰 리팩토링이 아닌 작은 개선을 지속적으로

#### Avoid Premature Optimization (조기 최적화 피하기)

**정의**: "조기 최적화는 만악의 근원이다" - Donald Knuth

**적용 방법**:
1. **먼저 동작하게 만들기**: 올바른 코드를 먼저 작성
2. **프로파일링**: 실제 병목 지점을 측정
3. **필요한 곳만 최적화**: 측정 결과를 바탕으로
4. **가독성 우선**: 명확한 코드가 빠른 코드보다 중요

---

### 7. 고급 원칙

#### Inversion of Control (제어의 역전)

**정의**: 프레임워크가 애플리케이션의 흐름을 제어하는 설계 원칙입니다.

**종류**:
1. **의존성 주입(DI)**: 의존성을 외부에서 주입
2. **이벤트/콜백**: 프레임워크가 코드를 호출
3. **템플릿 메서드**: 프레임워크가 알고리즘 구조를 제어

#### Command Query Separation (명령-쿼리 분리)

**정의**:
- **Command (명령)**: 상태를 변경하지만 값을 반환하지 않음
- **Query (쿼리)**: 값을 반환하지만 상태를 변경하지 않음

**적용 예**:
```java
// ✅ CQS 준수
public class Stack<T> {
    private List<T> items = new ArrayList<>();

    // Command - 상태 변경, 반환 없음
    public void push(T item) {
        items.add(item);
    }

    // Command - 상태 변경, 반환 없음
    public void pop() {
        if (!items.isEmpty()) {
            items.remove(items.size() - 1);
        }
    }

    // Query - 상태 변경 없음, 값 반환
    public T peek() {
        if (items.isEmpty()) {
            return null;
        }
        return items.get(items.size() - 1);
    }

    // Query - 상태 변경 없음, 값 반환
    public boolean isEmpty() {
        return items.isEmpty();
    }
}
```

#### Robustness Principle (견고성 원칙) - Postel's Law

**정의**: "보내는 것에는 보수적으로, 받는 것에는 자유롭게"

**의미**:
- **출력**: 표준을 엄격히 준수하여 출력
- **입력**: 다양한 형식의 입력을 관대하게 수용

**적용 예**:
```java
public class DateParser {
    // 다양한 날짜 형식을 받아들임 (자유롭게)
    public LocalDate parse(String dateString) {
        // "2024-01-15", "01/15/2024", "15-Jan-2024" 등 다양한 형식 허용
        try {
            return LocalDate.parse(dateString, DateTimeFormatter.ISO_DATE);
        } catch (DateTimeParseException e) {
            try {
                return LocalDate.parse(dateString, DateTimeFormatter.ofPattern("MM/dd/yyyy"));
            } catch (DateTimeParseException ex) {
                // 다른 형식 시도...
            }
        }
        throw new IllegalArgumentException("Invalid date format");
    }

    // 출력은 표준 형식으로 (보수적으로)
    public String format(LocalDate date) {
        return date.format(DateTimeFormatter.ISO_DATE);  // 항상 "2024-01-15" 형식
    }
}
```

---

[← 메인으로 돌아가기](../SKILL.md)
