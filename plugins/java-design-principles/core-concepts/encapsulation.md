
### 5. 캡슐화

#### Encapsulation & Information Hiding (캡슐화와 정보 은닉)

**정의**:
- **캡슐화**: 데이터와 메서드를 하나의 단위로 묶는 것
- **정보 은닉**: 내부 구현 세부사항을 외부로부터 숨기는 것

**적용 방법**:
1. **필드는 private**: 모든 필드는 private으로 선언
2. **Getter/Setter 신중히**: 무분별한 getter/setter는 캡슐화를 깨뜨림
3. **동작 중심**: 데이터를 노출하지 말고 동작을 제공
4. **불변성**: 가능하면 불변 객체로 설계

**실무 예시**:
```java
// ❌ 캡슐화 위반
public class BankAccount {
    public double balance;  // public 필드
}

// 외부에서 직접 조작 가능 - 위험!
account.balance = -1000;

// ✅ 캡슐화 준수
public class BankAccount {
    private double balance;  // private 필드

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

#### Encapsulate What Changes (변화를 캡슐화하라)

**정의**: 변경될 가능성이 있는 부분을 식별하고 캡슐화하여 변경의 영향을 최소화합니다.

**적용 방법**:
1. **변경 지점 식별**: 자주 변경되거나 변경될 가능성이 있는 부분 찾기
2. **인터페이스로 추상화**: 변경 지점을 인터페이스 뒤에 숨기기
3. **전략 패턴 활용**: 알고리즘을 캡슐화
4. **설정 외부화**: 하드코딩 대신 설정 파일 사용

---

---

[← 메인으로 돌아가기](../SKILL.md)
