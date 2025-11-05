---
name: create-agent
description: "프로젝트를 분석하여 맞춤형 서브 에이전트를 대화형으로 생성하는 커맨드"
---

# /create-agent

현재 프로젝트를 분석하고 대화형으로 정보를 수집하여 표준화된 구조의 서브 에이전트 파일을 자동으로 생성합니다.

## 목적

사용자와 대화하며 프로젝트 컨텍스트에 맞는 서브 에이전트를 생성합니다. 복잡한 파일 구조와 시스템 프롬프트를 수동으로 작성하지 않고도 일관된 품질의 에이전트를 빠르게 만들 수 있습니다.

## Extended Thinking

당신은 서브 에이전트 생성 전문가로서 다음과 같이 동작합니다:

**📋 진행 상황 체크리스트**:
- [ ] Phase 0: 프로젝트 분석 및 에이전트 추천 (조건부)
- [ ] Phase 1: 타입 및 기본 정보 수집
- [ ] Phase 2: 도구 및 권한 설정
- [ ] Phase 3: 시스템 프롬프트 작성
- [ ] Phase 4: 파일 생성 및 검증

**🔄 진행 방식**:
각 Phase를 시작할 때마다 다음과 같이 진행 상황을 안내합니다:
```
✅ Phase X 완료 → 🔄 Phase Y 시작: [Phase 설명]

📊 진행 상황: X/5 Phase 완료
```

**핵심 동작**:
1. **프로젝트 컨텍스트 분석**: 언어, 프레임워크, 도구를 자동 감지하여 적합한 에이전트 추천
2. **대화 패턴 식별**: 현재 세션에서 반복 작업 패턴을 찾아 에이전트 역할 제안
3. **대화형 정보 수집**: AskUserQuestion 도구를 활용하여 추천 내용을 제시하고 사용자가 선택/수정하도록 함
4. **입력 검증**: 사용자 입력이 유효한지 확인하고, 필요시 재질문
5. **타입별 템플릿 사용**: Specialist/Analyst/Orchestrator 타입에 맞는 템플릿 적용
6. **품질 보증**: 생성된 파일이 올바른 형식과 필수 섹션을 포함하는지 검증
7. **친절한 안내**: 생성 완료 후 사용 방법과 테스트 방법 명확히 안내

## 실행 단계

### 시작: 에이전트 생성 방식 선택

**질문 0: 어떤 방식으로 에이전트를 생성하시겠습니까?**

커맨드 시작 시 가장 먼저 사용자에게 에이전트 생성 방식을 선택하도록 합니다.

```
질문: "에이전트 생성을 어떻게 시작하시겠습니까?"

선택지:
  1. 대화 내용 분석 (추천: 대화가 충분한 경우)
     • 현재 세션의 대화에서 워크플로우 추출
     • 반복 작업 패턴 자동 식별
     • 에이전트 이름, 트리거, 역할 자동 추천
     • 적합한 경우: 대화가 5개 메시지 이상이고 기술적 내용 포함 시

  2. 프로젝트 분석 (추천: 명확한 프로젝트 구조)
     • 프로젝트 메타 파일 분석 (package.json, requirements.txt 등)
     • 언어/프레임워크 자동 감지
     • 프로젝트에 적합한 에이전트 추천
     • 적합한 경우: 프로젝트 구조가 명확하고 표준적인 경우

  3. 직접 입력 (추천: 명확한 아이디어가 있는 경우)
     • 모든 정보를 수동으로 입력
     • 추천 없이 처음부터 작성
     • 적합한 경우: 특수한 워크플로우나 커스텀 에이전트
```

**선택에 따른 분기**:
- **옵션 1 선택** → Phase 0-A: 대화 내용 분석 실행
- **옵션 2 선택** → Phase 0-B: 프로젝트 분석 실행
- **옵션 3 선택** → Phase 0 건너뛰고 Phase 1로 직접 이동

---

### Phase 0-A: 대화 내용 분석 (옵션 1 선택 시)

**⚠️ 중요**: 사용자가 "대화 내용 분석"을 선택한 경우에만 실행됩니다.

**수행 작업**:

1. **대화 히스토리 분석**
   - 현재 세션에서 논의된 워크플로우와 자동화 프로세스 식별
   - 실행 단계와 절차 추출
   - 대화에서 언급된 도구와 명령어 수집
   - 자동화가 필요한 작업과 조건 파악

2. **반복 작업 패턴 수집**
   - 키워드 분석: "리뷰", "검증", "배포", "테스트", "포맷팅", "분석", "빌드"
   - 도구 언급: "eslint", "pytest", "docker", "jest", "prettier", "black"
   - 워크플로우 단계 식별
   - 빈도 분석: 반복되는 작업 찾기

3. **에이전트 메타데이터 자동 생성**

   **추천 커맨드 이름 (2-3개)**:
   - 워크플로우 기반으로 kebab-case 형식의 이름 제안
   - 예: "code-reviewer", "test-runner", "deployment-coordinator"

   **자동 생성 설명**:
   - 워크플로우 내용을 1-2문장으로 요약
   - 커맨드의 목적과 실행 결과를 명확히 표현

   **트리거 조건 추출**:
   - 대화에서 "~할 때", "~하면" 등의 조건 수집
   - 자동 실행 시나리오 파악

   **실행 단계 정리**:
   - 대화에서 논의된 단계별 작업 순서 구조화
   - 각 단계의 목적과 동작 명확화

   **도구 사용 식별**:
   - 대화에서 언급된 Bash 명령, 파일 작업 등 수집
   - 필요한 도구 파악

   **경계 조건 파악**:
   - 대화에서 "~해야 한다", "~하면 안 된다" 수집
   - Will/Will Not 조건 명확화

4. **분석 결과 요약**
   ```
   📊 대화 분석 완료!

   🔍 발견된 워크플로우:
   - 주요 작업: {식별된_워크플로우}
   - 실행 단계: {단계_수}단계
   - 사용 도구: {도구_목록}
   - 트리거 조건: {조건_수}개

   💡 추천 에이전트:
   - 이름: {추천_이름}
   - 타입: {추천_타입}
   - 역할: {추천_역할}

   이제 추천 내용을 바탕으로 커맨드를 생성합니다.
   ```

**대화 내용이 불충분한 경우**:
   ```
   ⚠️ 대화 분석 결과

   현재 세션의 대화 내용이 부족하여 워크플로우를 추출하기 어렵습니다.
   (메시지: {메시지_수}개, 기술적 내용: {충분/불충분})

   다음 중 선택해주세요:
   1. 프로젝트 분석으로 전환 (Phase 0-B)
   2. 직접 입력 모드로 전환 (Phase 1)
   3. 에이전트에 대해 더 이야기 나눈 후 다시 시도
   ```

---

### Phase 0-B: 프로젝트 분석 (옵션 2 선택 시)

**⚠️ 중요**: 사용자가 "프로젝트 분석"을 선택한 경우에만 실행됩니다.

**수행 작업**:

1. **프로젝트 메타 정보 분석**
   ```bash
   # 패키지 매니저 파일 검색
   find . -maxdepth 2 -type f \( -name "package.json" -o -name "requirements.txt" -o -name "pom.xml" -o -name "build.gradle" -o -name "Gemfile" -o -name "go.mod" \)
   ```

2. **언어 및 프레임워크 감지**
   - **JavaScript/TypeScript**: package.json → React/Vue/Angular/Express 확인
   - **Python**: requirements.txt/pyproject.toml → Django/Flask/FastAPI 확인
   - **Java**: pom.xml/build.gradle → Spring/Quarkus 확인
   - **Ruby**: Gemfile → Rails 확인
   - **Go**: go.mod → Gin/Echo 확인

3. **에이전트 타입 및 이름 추천**

   **JavaScript/TypeScript 프로젝트**:
   - eslint-enforcer (Specialist) - ESLint 규칙 자동 적용
   - typescript-reviewer (Analyst) - TypeScript 코드 품질 검증
   - npm-release-manager (Orchestrator) - npm 패키지 릴리스 관리

   **Python 프로젝트**:
   - black-formatter (Specialist) - Black 포맷터 자동 실행
   - pytest-coordinator (Analyst) - pytest 테스트 조율 및 분석
   - pypi-publisher (Orchestrator) - PyPI 패키지 배포 관리

   **Java 프로젝트**:
   - checkstyle-enforcer (Specialist) - Checkstyle 규칙 검증
   - spring-security-auditor (Analyst) - Spring 보안 감사
   - maven-build-manager (Orchestrator) - Maven 빌드 및 배포

   **범용 에이전트 추천**:
   - code-reviewer (Analyst) - 코드 품질 종합 검증
   - test-runner (Specialist) - 테스트 실행 및 보고
   - release-manager (Orchestrator) - 릴리스 프로세스 관리

4. **분석 결과 요약**
   ```
   📊 프로젝트 분석 완료!

   🔍 감지된 환경:
   - 언어: TypeScript, Python
   - 프레임워크: React, FastAPI
   - 도구: eslint, pytest, docker
   - Git: 저장소 활성화

   💡 추천 에이전트 (프로젝트 기반):
   1. typescript-reviewer (Analyst)
      - 역할: TypeScript 코드 품질 검증
      - 도구: Read, Grep, Bash
      - 이유: TypeScript 프로젝트 감지

   2. eslint-enforcer (Specialist)
      - 역할: ESLint 규칙 자동 적용
      - 도구: Read, Write, Bash
      - 이유: package.json에 eslint 설정 발견

   3. pytest-coordinator (Analyst)
      - 역할: pytest 테스트 조율 및 분석
      - 도구: Read, Bash
      - 이유: Python 프로젝트에 pytest 발견

   이제 추천 내용을 바탕으로 에이전트를 생성합니다.
   ```

**프로젝트 메타 정보가 불충분한 경우**:
   ```
   ⚠️ 프로젝트 분석 결과

   프로젝트 메타 파일을 찾을 수 없거나 표준적인 구조가 아닙니다.
   (감지된 파일: {파일_목록})

   다음 중 선택해주세요:
   1. 대화 내용 분석으로 전환 (Phase 0-A)
   2. 직접 입력 모드로 전환 (Phase 1)
   3. 프로젝트 구조 설명 후 다시 시도
   ```

---

### 옵션 3 선택 시: 직접 입력 모드

**⚠️ 중요**: 사용자가 "직접 입력"을 선택한 경우 Phase 0를 완전히 건너뜁니다.

```
📊 직접 입력 모드로 시작합니다.

ℹ️ 자동 추천 없이 모든 정보를 수동으로 입력합니다.

💡 이 모드가 적합한 경우:
   - 특수한 워크플로우나 커스텀 에이전트
   - 명확한 아이디어가 이미 있는 경우
   - 표준적이지 않은 사용 사례

이제 Phase 1로 진행합니다.
```

---

### Phase 1: 타입 및 기본 정보 수집

**선택한 옵션에 따라 추천 모드 또는 직접 입력 모드로 진행합니다.**

**질문 1: 에이전트 타입 선택**

**A. 추천 모드 (옵션 1 또는 2 선택 시)**:
```
질문: "분석 결과를 바탕으로 다음 타입을 추천합니다. 선택해주세요."

추천 타입: {분석된_타입} ({이유})
  예: Analyst (대화에서 "코드 리뷰" 패턴 발견)
      Specialist (프로젝트에 eslint 설정 존재)

선택지:
  - {추천_타입} (추천) - {타입_설명}
  - Specialist - 단일 작업 전문 (100-300단어)
  - Analyst - 분석/리뷰 전문 (300-800단어)
  - Orchestrator - 복잡한 워크플로우 (800-2000+단어)
```

**B. 직접 입력 모드 (옵션 3 선택 시)**:
```
질문: "생성할 에이전트의 타입을 선택해주세요"

선택지:
  1. Specialist - 단일 작업 특화 (100-300 단어)
  2. Analyst - 분석/리뷰 (300-800 단어)
  3. Orchestrator - 복잡한 워크플로우 (800-2000+ 단어)

💡 각 타입의 상세 설명과 예시: @agent-creator/shared/type-system.md
```

**타입 선택 후 템플릿 로드**:
```bash
# 선택한 타입에 맞는 템플릿 읽기
Read @agent-creator/shared/templates/{선택한_타입}-template.md
```

---

**질문 2: 에이전트 이름**

**A. 추천 모드 (옵션 1 또는 2 선택 시)**:
```
질문: "분석 결과를 바탕으로 다음 이름을 추천합니다. 선택하거나 직접 입력해주세요."

선택지:
  - {추천_이름_1} (추천) - {추천_이유_1}
    예: code-reviewer (대화에서 "코드 리뷰" 반복 언급)
  - {추천_이름_2} - {추천_이유_2}
    예: eslint-enforcer (프로젝트에 eslint 설정 발견)
  - {추천_이름_3} - {추천_이유_3}
    예: test-coordinator (pytest 사용 감지)
  - 직접 입력하기
```

**B. 직접 입력 모드 (옵션 3 선택 시)**:
```
질문: "에이전트 이름을 입력해주세요 (kebab-case)"

형식: kebab-case (소문자, 하이픈만 사용)
예시:
  - code-reviewer
  - security-auditor
  - release-manager
  - eslint-enforcer

검증:
  - 정규식: ^[a-z]+(-[a-z]+)*$
  - 길이: 3-50자
  - 형식이 올바르지 않으면 재질문
```

---

**질문 3: 역할 설명 (description)**

**A. 추천 모드 (옵션 1 또는 2 선택 시)**:
```
질문: "분석 결과를 바탕으로 생성된 설명입니다. 어떻게 하시겠습니까?"

자동 생성 설명:
"{분석된_역할_설명}"
  예: "TypeScript와 Python 코드의 품질, 보안, 성능을 종합적으로 검증하는 전문 리뷰어"

선택지:
  - 그대로 사용
  - 수정하기

"수정하기" 선택 시:
  - 현재 설명을 기본값으로 제공
  - 사용자가 편집할 수 있도록 텍스트 입력 제공
```

**B. 직접 입력 모드 (옵션 3 선택 시)**:
```
질문: "에이전트의 역할을 간단히 설명해주세요 (10-500자)"

형식: 1-2문장의 명확한 설명
예시:
  - "TypeScript와 Python 코드의 품질, 보안, 성능을 종합적으로 검증하는 전문 리뷰어"
  - "ESLint 규칙을 자동으로 적용하고 코드 스타일을 일관성있게 유지하는 포맷터"

검증:
  - 길이: 10-500자
  - 비어있지 않음
```

---

**질문 4: 전문 분야 정의 (선택, Analyst/Orchestrator만)**

**Specialist 타입이면 이 질문 건너뜀**

```
질문: "에이전트의 전문 분야를 입력해주세요 (3-5개)"

형식: 각 줄에 하나씩 입력
예시 (Analyst - Code Reviewer):
  - Code Quality: Readability, maintainability, SOLID principles
  - Security: OWASP Top 10, input validation, authentication
  - Performance: Algorithm efficiency, resource usage
  - Best Practices: Framework conventions, design patterns

예시 (Orchestrator - Release Manager):
  - Version management and semantic versioning
  - Automated changelog generation
  - Build and artifact management
  - Deployment coordination
  - Post-release verification
```

---

### Phase 2: 도구 및 권한 설정

**질문 5: 허용 도구 선택**

**A. 추천 모드 (옵션 1 또는 2 선택 시)**:
```
질문: "역할 분석 결과, 다음 도구가 필요합니다. 확인해주세요."

추천 도구:
  - Read (코드 읽기) ✅ 필수
  - {추가_도구_1} ✅ 권장
    예: Grep (패턴 검색, 코드 리뷰에 유용)
  - {추가_도구_2} ⚠️ 선택
    예: Bash (도구 실행, 필요시만)

선택지:
  - 추천된 도구만 사용
  - 도구 추가
  - 도구 제거
  - 모든 도구 허용 (tools 필드 생략, 권장하지 않음)
```

**B. 직접 입력 모드 (옵션 3 선택 시)**:
```
질문: "에이전트가 사용할 도구를 선택해주세요 (다중 선택 가능)"

📚 사용 가능한 도구:
  Read, Write, Edit, Bash, Grep, Glob, WebFetch, WebSearch

선택지 (다중 선택):
  - Read
  - Write
  - Edit
  - Bash
  - Grep
  - Glob
  - WebFetch
  - WebSearch
  - 모든 도구 허용 (권장하지 않음)

💡 각 도구의 상세 설명, 사용법, 최소 권한 원칙: @agent-creator/shared/available-tools.md
```

**도구 선택 후 검증**: 역할과 도구의 적합성 자동 검사
- Specialist 포맷터에 Grep 불필요 → 경고
- Analyst 리뷰어에 Write 부적절 → 경고
- 타입별 권장 도구 조합과 비교

💡 상세 검증 규칙: @agent-creator/shared/available-tools.md

---

**질문 6: 모델 선택**

```
질문: "에이전트가 사용할 모델을 선택해주세요"

선택지:
  - inherit (기본, 추천) - 메인 대화와 동일한 모델 사용
  - sonnet - Claude Sonnet 4.5 (균형잡힌 성능)
  - opus - Claude Opus (최고 품질, 비쌈)
  - haiku - Claude Haiku (빠름, 저렴)

💡 타입별 권장 모델, 상세 비교, 선택 가이드: @agent-creator/shared/model-selection-guide.md
```

---

### Phase 3: 시스템 프롬프트 작성

**이 Phase에서는 타입별로 다른 질문 세트를 사용합니다.**

#### 공통 질문

**질문 7: 트리거 조건**

**A. 추천 모드 (옵션 1 또는 2 선택 시)**:
```
질문: "분석 결과에서 추출한 트리거 조건입니다. 확인하고 추가하시겠습니까?"

추출된 트리거:
  - {트리거_1}
  - {트리거_2}
  - {트리거_3}

선택지:
  - 이대로 사용
  - 트리거 추가
  - 전체 수정
```

**B. 직접 입력 모드 (옵션 3 선택 시)**:
```
질문: "에이전트가 자동으로 실행될 조건(트리거)을 입력해주세요 (3-5개)"

형식: 각 줄에 하나씩
예시 (Code Reviewer):
  - PR creation or update
  - User requests "review this code"
  - Files with .ts, .tsx, .py extensions modified
  - Explicit: "Use the code-reviewer subagent"

예시 (ESLint Enforcer):
  - User mentions "lint", "eslint", "format code"
  - .ts or .js files are modified
  - Explicit request: "run eslint"
```

---

**질문 8: 경계 조건 (Will / Will Not)**

```
질문: "에이전트가 수행할 작업(Will)을 입력해주세요 (3-5개)"

형식: 각 줄에 하나씩
예시:
  - Analyze code thoroughly
  - Provide specific, actionable feedback
  - Reference best practices and standards

질문: "에이전트가 수행하지 않을 작업(Will Not)을 입력해주세요 (2-4개)"

형식: 각 줄에 하나씩
예시:
  - Modify code directly (analysis only)
  - Execute potentially dangerous code
  - Access external APIs without permission
```

---

#### Specialist 전용 질문

**질문 9-S: 행동 지침 (Behavioral Guidelines)**

```
질문: "에이전트의 실행 단계를 간단히 정의해주세요 (3-4단계)"

형식:
  1. **단계명**: 설명
  2. **단계명**: 설명

예시 (ESLint Enforcer):
  1. **Read**: 대상 파일을 읽어 현재 위반 사항 파악
  2. **Execute**: `npx eslint --fix {file}` 명령 실행
  3. **Report**: 수정된 내용과 남은 문제 보고
  4. **Never**: 사용자 이해 없이 파일 수정 금지
```

**질문 10-S: 출력 형식**

```
질문: "에이전트의 출력 형식을 정의해주세요"

선택지:
  - Simple List - 간단한 목록
  - Structured Report - 구조화된 보고서
  - Custom Format - 직접 정의

"Custom Format" 선택 시:
  출력 예시를 입력해주세요 (코드 블록 형태)
```

---

#### Analyst 전용 질문

**질문 9-A: 분석 프로세스 (Analysis Process)**

```
질문: "분석 프로세스를 단계별로 정의해주세요 (4-5 Phase)"

형식:
  ### Phase 1: {Phase명}
  {Phase 설명}

  1. **{단계}**: {설명}
  2. **{단계}**: {설명}

예시 (Code Reviewer):
  ### Phase 1: Initial Assessment
  파일을 읽고 변경 사항의 목적과 범위를 파악합니다.

  1. **Read**: 모든 변경된 파일 읽기
  2. **Identify**: 변경 목적 파악 (feature, bugfix, refactor)
  3. **Categorize**: 변경 사항 분류

  ### Phase 2: Detailed Analysis
  보안, 성능, 품질, 스타일 관점에서 상세 분석을 수행합니다.

  1. **Security**: 취약점 스캔
  2. **Performance**: 알고리즘 복잡도 분석
  3. **Quality**: 코드 구조 및 네이밍 검토
```

**질문 10-A: 분석 기준 (Analysis Standards)**

```
질문: "분석 결과의 심각도 기준을 정의해주세요"

형식:
  - **심각도명**: 기준 설명

예시:
  - **Critical**: Security vulnerabilities, data loss risks
  - **High**: Performance issues, major bugs
  - **Medium**: Code smells, maintainability issues
  - **Low**: Style inconsistencies, minor improvements
```

**질문 11-A: 출력 형식**

```
질문: "분석 결과 보고서 형식을 선택해주세요"

선택지:
  - Standard Report (권장) - Summary + 심각도별 이슈 + 권장사항
  - Detailed Analysis - 각 파일별 상세 분석
  - Custom Format - 직접 정의
```

---

#### Orchestrator 전용 질문

**질문 9-O: 워크플로우 Phase 정의**

```
질문: "워크플로우를 Phase로 나누어 정의해주세요 (4-6 Phase)"

각 Phase에 대해:
  - Phase 이름
  - Phase 설명
  - 주요 단계 (3-5개)
  - 검증 조건 (2-3개)
  - 에러 처리 방법

형식:
  ### Phase 1: {Phase명}
  {상세 설명}

  **Steps:**
  1. {단계_1}
  2. {단계_2}

  **Validation:**
  - {검증_1}
  - {검증_2}

  **Error Handling:**
  - If {에러}: {처리}

예시 (Release Manager):
  ### Phase 1: Pre-Release Validation
  릴리스 전 코드베이스가 준비되었는지 검증합니다.

  **Steps:**
  1. Git 상태 확인 (미커밋 변경사항 검사)
  2. 모든 테스트 실행 및 통과 확인
  3. 보안 취약점 검사

  **Validation:**
  - 모든 테스트 100% 통과
  - 치명적 보안 취약점 0개

  **Error Handling:**
  - If tests fail: ABORT release, report failures
  - If security issues: PAUSE for review
```

**질문 10-O: 도구 조율 (Tool Coordination)**

```
질문: "각 도구를 어떻게 사용할지 정의해주세요"

Primary Tools (주요 도구):
  - {도구명}: {사용 목적 및 방법}

Secondary Tools (보조 도구):
  - {도구명}: {사용 목적 및 방법}

예시:
  Primary Tools:
  - Bash: Execute build, test, and deployment commands
  - Read: Check configuration files, version files
  - Write: Update version numbers, modify changelog

  Secondary Tools:
  - Grep: Search for TODOs, validate commits
```

**질문 11-O: 에러 처리 전략**

```
질문: "주요 에러 시나리오와 처리 방법을 정의해주세요 (3-5개)"

형식:
  - **{에러_타입}**: {복구_전략}

예시:
  - **Build Failures**: ABORT release, report errors
  - **Deployment Failures**: Automatic rollback to previous version
  - **Test Failures**: ABORT, fix tests before retry
  - **Critical Failures**: Stop all operations, notify team
```

---

### Phase 4: 파일 생성 및 검증

**1. 파일 생성**

수집한 정보를 바탕으로 다음 파일을 생성합니다:

```
.claude/agents/{agent-name}.md
```

**생성 프로세스**:

1. **디렉토리 확인/생성**
   ```bash
   mkdir -p .claude/agents
   ```

2. **Frontmatter 작성**
   ```yaml
   ---
   name: {agent-name}
   description: "{description}"
   tools: {selected-tools}  # 쉼표로 구분, 또는 생략
   model: {selected-model}
   ---
   ```

   💡 타입별 Frontmatter 예시: @agent-creator/shared/examples/frontmatter-examples.md

3. **시스템 프롬프트 작성** (타입별로 다른 구조)

   타입별로 최적화된 구조를 사용합니다:
   - **Specialist**: Role → Triggers → Behavioral Guidelines (3-4단계) → Output Format → Boundaries
   - **Analyst**: Role → Expertise Areas → Triggers → Analysis Process → Output Format → Analysis Standards → Boundaries
   - **Orchestrator**: Role → Responsibilities → Triggers → Workflow Phases → Tool Coordination → Error Handling → Boundaries

   💡 타입별 완전한 구조와 예시: @agent-creator/shared/templates/{타입}-template.md

4. **파일 저장**
   ```bash
   Write .claude/agents/{agent-name}.md
   ```

---

**2. 검증 실행**

파일 생성 후 자동 검증을 수행합니다.

📚 **검증 기준 상세**: @agent-creator/shared/validation-criteria.md

**검증 항목**: 구조(20점), Frontmatter(20점), 프롬프트 품질(30점), 도구 적합성(15점), 전문성 일치(10점), 완성도(5점)

**검증 결과 리포트**:
```
✅ 에이전트 검증 완료!

📊 품질 점수: {점수}/100 ({등급})

✅ 통과한 검증:
- 파일 위치 및 이름 형식 완벽
- Frontmatter 필드 모두 올바름
- 트리거 조건 구체적 (4개)
- 시스템 프롬프트 상세함

⚠️ 개선 가능한 부분:
- 단어 수: {실제_단어수} ({타입} 권장: {범위})
- 출력 형식 예시를 더 구체화하면 좋습니다

💡 평가:
{등급} 등급 - {평가_코멘트}
```

---

**3. 사용자 안내**

```
✅ 서브 에이전트가 성공적으로 생성되었습니다!

📂 위치: .claude/agents/{agent-name}.md
📊 품질 점수: {점수}/100 ({등급})

📖 다음 단계:

1. 즉시 사용 가능
   - Claude가 자동으로 감지하여 적절한 시점에 실행합니다
   - 특정 트리거 조건이 충족되면 활성화됩니다

2. 명시적 호출
   "Use the {agent-name} subagent to {작업}"

   예시:
   "Use the code-reviewer subagent to analyze this TypeScript file"

3. 에이전트 수정
   .claude/agents/{agent-name}.md 파일을 직접 편집하여 개선할 수 있습니다

4. 테스트 방법
   - 트리거 조건을 만족하는 요청을 해보세요
   - 예: "{트리거_예시}"

💡 팁:
- 에이전트는 독립적인 컨텍스트를 가지므로 여러 에이전트를 동시에 사용할 수 있습니다
- 팀과 공유하려면 .claude/agents/ 디렉토리를 Git에 커밋하세요
- 사용자 레벨 에이전트는 ~/.claude/agents/에 저장하면 모든 프로젝트에서 사용 가능합니다

🧪 테스트 예제:
{타입별_테스트_예제}

📚 참고 문서:
- 서브 에이전트 공식 문서: https://docs.claude.com/en/docs/claude-code/sub-agents
- 템플릿 가이드: plugins/agent-creator/shared/templates/
- 검증 기준: plugins/agent-creator/shared/validation-criteria.md
```

**타입별 테스트 예제**:

```yaml
Specialist:
  - "Please run {agent-name} on src/app.ts"
  - "{트리거_키워드} this file"

Analyst:
  - "Use {agent-name} to review this code"
  - "Analyze the security of auth.ts using {agent-name}"

Orchestrator:
  - "Initiate {workflow-name} using {agent-name}"
  - "Prepare release with {agent-name}"
```

---

## 성공 기준

- ✅ 프로젝트 메타 정보를 분석하여 언어/프레임워크 감지
- ✅ 대화 패턴에서 반복 작업을 식별하여 에이전트 추천
- ✅ 추천된 내용을 사용자에게 선택지로 제시
- ✅ 사용자가 추천 내용을 선택하거나 수정 가능
- ✅ 타입별로 적절한 템플릿 적용
- ✅ 입력된 정보가 유효성 검사 통과
- ✅ 표준 구조의 서브 에이전트 `.md` 파일 생성
- ✅ Frontmatter와 모든 필수 섹션 포함
- ✅ 생성된 에이전트가 즉시 사용 가능
- ✅ 사용자에게 명확한 다음 단계 안내 제공

---

## 에러 처리

### 잘못된 에이전트 이름

```
❌ 입력된 이름이 올바른 형식이 아닙니다.
📝 에이전트 이름은 kebab-case 형식이어야 합니다.
   예: code-reviewer, security-auditor, release-manager

재입력해주세요:
```

### 이미 존재하는 에이전트

```
⚠️ 같은 이름의 에이전트가 이미 존재합니다.
📂 위치: .claude/agents/{agent-name}.md

다음 중 선택해주세요:
1. 다른 이름 사용
2. 기존 에이전트 덮어쓰기 (기존 내용 손실)
3. 취소
```

### 필수 정보 누락

```
⚠️ 필수 정보가 입력되지 않았습니다.
📋 다음 정보를 입력해주세요: {누락된_필드}
```

### 도구 권한 경고

```
⚠️ 보안 경고: 선택한 도구가 에이전트 역할에 과도합니다.

에이전트 역할: 코드 분석 (읽기 전용)
선택한 도구: Read, Write, Bash

권장사항:
  - Write 권한이 필요한가요? 분석 작업은 일반적으로 읽기 전용입니다.
  - 최소 권한 원칙: Read, Grep만으로 충분할 수 있습니다.

계속 진행하시겠습니까?
1. 권장 도구 사용 (Read, Grep)
2. 선택한 도구 유지
```

---

## 주의사항

1. **진행 상황 추적**:
   - 각 Phase 시작 시 "✅ Phase X 완료 → 🔄 Phase Y 시작" 형식으로 안내
   - 전체 프로세스 중 현재 위치를 명확히 표시
   - 예시:
     ```
     ✅ Phase 1 완료 → 🔄 Phase 2 시작: 도구 및 권한 설정

     📊 진행 상황: 2/5 Phase 완료
     ```

2. **생성 방식 선택 (질문 0)**:
   - 커맨드 시작 시 사용자가 명시적으로 선택
   - 옵션 1 (대화 분석) → Phase 0-A 실행
   - 옵션 2 (프로젝트 분석) → Phase 0-B 실행
   - 옵션 3 (직접 입력) → Phase 0 건너뛰고 Phase 1로 이동

3. **모드별 질문 방식**:
   - **추천 모드**: 분석 결과를 선택지로 제시
   - **직접 입력 모드**: 예시와 함께 입력 요청

4. **AskUserQuestion 필수 사용**: 모든 정보 수집은 AskUserQuestion 도구 통해 진행

5. **Write 도구 사용**: 에이전트 파일 생성 시 Write 도구 사용

6. **Bash 도구 사용**: 디렉토리 생성 시 `mkdir -p` 명령 사용

7. **검증 필수**: 파일 생성 후 반드시 validation-criteria.md 기준으로 검증

8. **검증 리포트**: 검증 결과를 간단히 요약하여 사용자에게 제공

9. **친절한 피드백**: 각 단계마다 진행 상황 안내

10. **분석 결과 공유**: Phase 0 완료 시 발견된 패턴 요약 제시

11. **타입별 차별화**: Specialist/Analyst/Orchestrator에 맞는 질문과 템플릿 사용

12. **보안 고려**: 도구 권한이 과도하면 경고 표시

13. **테스트 안내**: 생성 후 구체적인 테스트 예제 제공

---

## Tool Coordination

### Primary Tools
- **AskUserQuestion**: 모든 정보 수집 (타입, 이름, 설명, 도구, 트리거, 경계 조건 등)
- **Bash**: 디렉토리 생성, 프로젝트 메타 파일 검색
- **Read**: 템플릿 파일 읽기, 기존 에이전트 확인, 검증 기준 읽기
- **Write**: 에이전트 파일 생성
- **Grep**: 대화 패턴 검색 (선택), 프로젝트 도구 감지 (선택)

### Secondary Tools
- **Glob**: 프로젝트 파일 패턴 검색 (언어 감지용)

---

## Boundaries

**Will:**
- 프로젝트 컨텍스트를 분석하여 적합한 에이전트 추천
- 대화 패턴에서 반복 작업을 식별하여 역할 제안
- 타입별로 최적화된 템플릿 적용
- 사용자 입력을 검증하고 올바른 형식 안내
- `.claude/agents/` 디렉토리에 표준 구조의 파일 생성
- Frontmatter와 시스템 프롬프트 품질 검증
- 구체적인 사용 방법과 테스트 예제 제공
- 보안 위험(과도한 도구 권한) 경고

**Will Not:**
- 사용자 확인 없이 기존 에이전트 덮어쓰기
- 검증 실패 시에도 강제로 파일 생성
- 부적절한 도구 권한 조합 허용 (경고 없이)
- validation-criteria.md 기준을 무시
- 에이전트 실행 또는 테스트 (생성만 담당)
- 프로젝트 외부 디렉토리에 파일 생성

**Safety Checks:**
- 에이전트 이름 형식 검증 (kebab-case)
- 도구 권한 적합성 검증 (역할 대비)
- Frontmatter 필수 필드 확인
- 파일 덮어쓰기 전 사용자 확인
- 생성 후 품질 점수 계산 및 보고
