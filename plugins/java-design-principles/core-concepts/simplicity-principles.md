# 단순성의 원칙 (Simplicity Principles)

## KISS (Keep It Simple, Stupid)

**정의**: 복잡한 시스템보다 단순한 시스템을 유지하라는 원칙입니다.

**왜 중요한가**:
- 단순한 코드는 작성 시간이 적게 걸립니다
- 버그가 발생할 여지가 적습니다
- 이해하기 쉽고 수정이 간편합니다
- 새로운 팀원이 빠르게 이해할 수 있습니다

**적용 방법**:
- 메서드는 한 가지 일만 하도록 작게 유지하세요
- 중첩된 if문이 3단계 이상이면 리팩토링을 고려하세요
- 복잡한 조건문은 명확한 이름의 메서드로 추출하세요
- 클래스가 100줄을 넘어가면 분리를 고려하세요

**실무 팁**:
```java
// ❌ 복잡함
if ((user.getAge() >= 18 && user.hasLicense()) || user.hasParentalConsent())

// ✅ 단순함
if (canDrive(user))

private boolean canDrive(User user) {
    return isAdult(user) || user.hasParentalConsent();
}

private boolean isAdult(User user) {
    return user.getAge() >= 18 && user.hasLicense();
}
```

## YAGNI (You Aren't Gonna Need It)

**정의**: 실제로 필요할 때만 기능을 구현하고, 미래를 예측한 구현을 피하라는 원칙입니다.

**왜 중요한가**:
- 불필요한 코드 작성에 시간을 낭비하지 않습니다
- 코드베이스를 간결하게 유지합니다
- 실제 요구사항에 집중하게 됩니다
- 변경 비용을 줄입니다

**위반 징후**:
- "나중에 필요할 것 같아서" 추가한 기능
- 사용되지 않는 메서드나 클래스
- 과도하게 일반화된 인터페이스
- 아직 요구되지 않은 확장성 고려

**적용 방법**:
- 현재 스토리/티켓에서 요구하는 기능만 구현하세요
- "나중에 필요할 것 같다"는 생각이 들면 일단 멈추세요
- 필요해질 때까지 기다렸다가 그때 추가하세요

**실무 예시**:
```java
// ❌ YAGNI 위반 - 아직 필요하지 않은 다국어 지원
public class EmailService {
    public void sendEmail(String to, String subject, String body, Locale locale) {
        // 현재는 영어만 지원하지만 "나중을 위해" locale 파라미터 추가
    }
}

// ✅ YAGNI 준수 - 실제 필요한 것만
public class EmailService {
    public void sendEmail(String to, String subject, String body) {
        // 다국어 지원이 실제로 요구될 때 추가
    }
}
```

## Do The Simplest Thing That Could Possibly Work

**정의**: 문제를 해결하는 가장 단순한 방법을 찾아 구현하라는 원칙입니다.

**적용 방법**:
- 항상 질문하세요: "이 문제를 해결하는 가장 단순한 방법은 무엇인가?"
- 첫 번째 해결책을 구현한 후, 더 단순하게 만들 수 있는지 검토하세요
- 복잡한 패턴이 정말 필요한지 자문하세요

**실무 팁**:
- 디자인 패턴을 남용하지 마세요 (단순한 if문으로 충분한데 Strategy 패턴 사용)
- 3개 미만의 구현체를 위해 인터페이스를 만들지 마세요
- 먼저 동작하게 만들고, 그 다음에 개선하세요

---

[← 메인으로 돌아가기](../SKILL.md)
