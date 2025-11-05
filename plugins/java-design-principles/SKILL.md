---
name: java-design-principles
description: Java 개발에서 필수적인 설계 원칙과 모범 사례를 제공하는 종합 가이드
license: MIT
---

# Java Design Principles

Java 개발에서 필수적인 설계 원칙과 모범 사례를 제공하는 종합 가이드입니다. 초보자부터 고급 개발자까지, 단계별로 학습할 수 있도록 구성되어 있습니다.

## When to Use This Skill

이 스킬은 다음과 같은 상황에서 활용하세요:

- 🏗️ **새로운 Java 프로젝트를 설계할 때** - 아키텍처 결정 시 올바른 원칙을 적용
- 🔧 **기존 코드를 리팩토링할 때** - 코드 품질 개선을 위한 지침 확인
- 👀 **코드 리뷰를 수행할 때** - 설계 원칙 위반 여부를 체계적으로 검토
- 🎯 **클래스 설계 결정을 내릴 때** - 상속 vs 컴포지션, 인터페이스 분리 등
- 🧩 **모듈 간 의존성을 관리할 때** - 결합도를 낮추고 응집도를 높이는 방법
- 📚 **신입 개발자를 교육할 때** - 핵심 설계 원칙을 체계적으로 전달
- 🐛 **기술 부채를 해결할 때** - 어떤 원칙이 위반되었는지 파악하고 개선

## 📚 Learning Path

### 🌱 Beginner - 단순성부터 시작하기

설계 원칙을 처음 배우는 분들을 위한 입문 과정입니다.

1. **[단순성의 원칙](./core-concepts/simplicity-principles.md)**
   - KISS (Keep It Simple, Stupid)
   - YAGNI (You Aren't Gonna Need It)
   - Do The Simplest Thing That Could Possibly Work

   💡 *왜 먼저 배워야 하나요?* 복잡한 원칙을 배우기 전에, 단순하게 코드를 작성하는 습관을 들이는 것이 가장 중요합니다.

### 🚀 Intermediate - 핵심 설계 원칙

객체지향 설계의 핵심 원칙들을 학습합니다.

2. **[SOLID 원칙](./core-concepts/solid-principles.md)**
   - Single Responsibility Principle (단일 책임 원칙)
   - Open/Closed Principle (개방/폐쇄 원칙)
   - Liskov Substitution Principle (리스코프 치환 원칙)
   - Interface Segregation Principle (인터페이스 분리 원칙)
   - Dependency Inversion Principle (의존성 역전 원칙)

   💡 *왜 중요한가요?* SOLID는 유지보수 가능하고 확장 가능한 코드를 작성하기 위한 필수 원칙입니다.

3. **[결합도와 응집도](./core-concepts/coupling-cohesion.md)**
   - DRY (Don't Repeat Yourself)
   - Separation of Concerns
   - Minimize Coupling / Maximize Cohesion
   - Law of Demeter
   - Composition Over Inheritance

   💡 *실무 적용:* 모듈 간의 관계를 올바르게 설계하는 방법을 배웁니다.

### 🎯 Advanced - 심화 원칙과 패턴

더 깊이 있는 설계 원칙과 실전 패턴을 학습합니다.

4. **[캡슐화](./core-concepts/encapsulation.md)**
   - Encapsulation & Information Hiding
   - Encapsulate What Changes

5. **[고급 원칙](./core-concepts/advanced-principles.md)**
   - Code For The Maintainer
   - Boy-Scout Rule
   - Avoid Premature Optimization
   - Inversion of Control
   - Command Query Separation
   - Robustness Principle (Postel's Law)

6. **[실전 패턴](./patterns/common-patterns.md)**
   - 5가지 핵심 패턴의 좋은 예 vs 나쁜 예
   - Single Responsibility
   - Open/Closed
   - Law of Demeter
   - Composition Over Inheritance
   - DRY

## 📖 추가 자료

- **[Best Practices & Common Pitfalls](./best-practices.md)**
  - 설계/코딩/유지보수 시 체크리스트
  - 피해야 할 8가지 흔한 실수

- **[Resources](./resources.md)**
  - 필수 서적
  - 온라인 자료
  - 실습 자료

## 🎓 학습 팁

### 처음 시작하는 분들을 위한 추천 순서

1. **Week 1**: 단순성의 원칙 읽고 실습
2. **Week 2-3**: SOLID 원칙 하나씩 학습 (하루에 하나씩)
3. **Week 4**: 결합도/응집도 개념 이해
4. **Week 5**: 실전 패턴으로 연습
5. **Week 6+**: Best Practices 체크리스트로 코드 리뷰

### 이미 경험이 있는 분들

- 관심 있는 원칙부터 선택적으로 학습하세요
- Common Pitfalls를 먼저 읽고 현재 코드에 적용해보세요
- 팀 코드 리뷰 시 이 가이드를 참고 문서로 활용하세요

## 📂 프로젝트 구조

```
java-design-principles/
├── SKILL.md (이 파일)
├── core-concepts/
│   ├── simplicity-principles.md
│   ├── solid-principles.md
│   ├── coupling-cohesion.md
│   ├── encapsulation.md
│   └── advanced-principles.md
├── patterns/
│   └── common-patterns.md
├── best-practices.md
└── resources.md
```

## 🤝 Contributing

이 가이드에 기여하고 싶으시다면:
- 오타나 개선사항을 발견하면 이슈를 열어주세요
- 더 나은 예제가 있다면 Pull Request를 보내주세요
- 실무 사례를 공유해주세요

---

**라이센스**: MIT
**최종 업데이트**: 2025년 11월

Happy Coding! 🚀
