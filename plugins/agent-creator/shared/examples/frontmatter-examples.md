# Frontmatter 예시 모음

서브 에이전트의 Frontmatter 작성 예시를 타입별로 정리한 문서입니다.

---

## 📝 Frontmatter 구조

```yaml
---
name: agent-name              # 필수: kebab-case 형식
description: "간단한 설명"    # 필수: 10-500자
tools: Read,Grep,Bash         # 선택: 쉼표로 구분 또는 생략 (모든 도구 허용)
model: inherit                # 선택: inherit/sonnet/opus/haiku (기본값: inherit)
---
```

---

## 🔧 Specialist 타입 예시

### 예시 1: ESLint Enforcer (포맷터)

```yaml
---
name: eslint-enforcer
description: "JavaScript/TypeScript 파일에 ESLint 규칙을 자동으로 적용하고 코드 스타일을 일관성있게 유지하는 전문 포맷터"
tools: Read,Write,Bash
model: haiku
---
```

**설명**:
- `name`: 명확하고 설명적인 이름
- `description`: 역할과 목적을 명확히 표현
- `tools`: 파일 읽기 + 쓰기 + 도구 실행 (ESLint)
- `model`: 단순한 작업이므로 haiku

---

### 예시 2: Prettier Formatter

```yaml
---
name: prettier-formatter
description: "Prettier를 사용하여 코드 형식을 자동으로 정리하는 포맷터"
tools: Read,Write,Bash
model: haiku
---
```

**설명**:
- 간단하고 반복적인 작업
- 빠른 실행을 위해 haiku 모델

---

### 예시 3: Link Checker

```yaml
---
name: link-checker
description: "Markdown 파일의 모든 링크를 검증하고 깨진 링크를 보고하는 도구"
tools: Read,Bash,Grep
model: inherit
---
```

**설명**:
- `tools`: 파일 읽기 + 검색 + 외부 도구 실행
- `model`: inherit (기본 모델 사용)

---

### 예시 4: Test Runner

```yaml
---
name: test-runner
description: "프로젝트 테스트를 실행하고 결과를 간결하게 보고하는 테스트 실행기"
tools: Read,Bash
model: haiku
---
```

**설명**:
- 간단한 테스트 실행 및 보고
- Read는 설정 파일 확인용

---

## 📊 Analyst 타입 예시

### 예시 1: Code Reviewer (종합 리뷰)

```yaml
---
name: code-reviewer
description: "TypeScript와 Python 코드의 품질, 보안, 성능, 유지보수성을 종합적으로 검증하는 전문 코드 리뷰어"
tools: Read,Grep,Bash
model: sonnet
---
```

**설명**:
- `description`: 분석 범위를 명확히 (품질, 보안, 성능, 유지보수성)
- `tools`: 읽기 + 검색 + 도구 실행 (분석 전용, Write 없음!)
- `model`: 복잡한 분석이므로 sonnet

---

### 예시 2: Security Auditor (보안 감사)

```yaml
---
name: security-auditor
description: "애플리케이션의 보안 취약점을 식별하고 OWASP 기준으로 평가하는 전문 보안 분석가"
tools: Read,Grep,Bash
model: sonnet
---
```

**설명**:
- 보안에 특화된 분석
- OWASP 기준 명시
- sonnet 모델로 정밀한 분석

---

### 예시 3: Performance Analyzer

```yaml
---
name: performance-analyzer
description: "코드의 성능 병목을 식별하고 최적화 방안을 제시하는 성능 분석 전문가"
tools: Read,Grep,Bash
model: sonnet
---
```

**설명**:
- 성능 분석에 특화
- 최적화 방안 제시까지 포함

---

### 예시 4: TypeScript Reviewer (언어 특화)

```yaml
---
name: typescript-reviewer
description: "TypeScript 코드의 타입 안정성, 패턴 사용, 베스트 프랙티스를 검증하는 TypeScript 전문 리뷰어"
tools: Read,Grep,Bash
model: inherit
---
```

**설명**:
- 특정 언어에 특화된 리뷰어
- inherit 모델 사용 (일반적인 경우)

---

## 🎯 Orchestrator 타입 예시

### 예시 1: Release Manager (릴리스 관리)

```yaml
---
name: release-manager
description: "npm 패키지의 전체 릴리스 프로세스를 관리하고 버전 업데이트부터 배포까지 자동화하는 릴리스 매니저"
tools: Read,Write,Bash
model: sonnet
---
```

**설명**:
- `description`: 관리 범위를 명확히 (버전 업데이트 → 배포)
- `tools`: 파일 읽기/쓰기 + 명령 실행
- `model`: 복잡한 워크플로우이므로 sonnet

---

### 예시 2: CI/CD Manager

```yaml
---
name: ci-cd-manager
description: "CI/CD 파이프라인을 조율하고 빌드, 테스트, 배포 전 과정을 관리하는 자동화 매니저"
tools: Read,Write,Bash,Grep
model: sonnet
---
```

**설명**:
- 전체 파이프라인 관리
- Grep 추가 (로그 검색 등)

---

### 예시 3: Production Deployment Manager (프로덕션 배포)

```yaml
---
name: production-deployment-manager
description: "프로덕션 환경 배포 전체 프로세스를 관리하고 안전 검사, 롤백 전략을 수행하는 배포 매니저"
tools: Read,Write,Bash
model: opus
---
```

**설명**:
- 프로덕션 배포의 중요성
- opus 모델 사용 (최고 품질)
- 안전 검사와 롤백 언급

---

### 예시 4: Database Migration Coordinator

```yaml
---
name: db-migration-coordinator
description: "데이터베이스 마이그레이션을 조율하고 스키마 변경, 데이터 이전, 롤백을 관리하는 마이그레이션 코디네이터"
tools: Read,Write,Bash
model: sonnet
---
```

**설명**:
- 데이터베이스 특화 워크플로우
- 스키마 변경 + 데이터 이전 + 롤백

---

## 🔍 Model 선택 패턴

### haiku 사용 예시

```yaml
# 단순한 작업
model: haiku

# 적합한 경우:
# - 포맷팅 (prettier, eslint)
# - 링크 검증
# - 간단한 테스트 실행
# - 빠른 피드백이 중요한 경우
```

---

### inherit 사용 예시 (기본값)

```yaml
# 일반적인 작업
model: inherit

# 적합한 경우:
# - 대부분의 에이전트
# - 특별한 요구사항 없는 경우
# - 메인 대화와 일관성 유지
```

---

### sonnet 사용 예시

```yaml
# 복잡한 분석/워크플로우
model: sonnet

# 적합한 경우:
# - 코드 리뷰
# - 보안 감사
# - 복잡한 워크플로우
# - 높은 품질이 필요한 경우
```

---

### opus 사용 예시

```yaml
# 매우 중요한 작업
model: opus

# 적합한 경우:
# - 프로덕션 배포
# - 중요 시스템 감사
# - 실패 시 큰 영향
# - 최고 품질 필수
```

---

## 🔒 Tools 선택 패턴

### 읽기 전용 (분석 작업)

```yaml
tools: Read,Grep

# 적합한 경우:
# - 순수 분석 작업
# - 코드 리뷰
# - 보안 감사 (읽기만)
```

---

### 읽기 + 도구 실행

```yaml
tools: Read,Grep,Bash

# 적합한 경우:
# - 분석 + 검증 도구 실행
# - 코드 리뷰 + 테스트 실행
# - 보안 감사 + 취약점 스캔
```

---

### 파일 수정 포함

```yaml
tools: Read,Write,Bash

# 적합한 경우:
# - 포맷팅
# - 코드 자동 수정
# - 파일 생성/업데이트
```

---

### 종합 도구 (워크플로우)

```yaml
tools: Read,Write,Bash,Grep

# 적합한 경우:
# - 복잡한 워크플로우
# - 릴리스 관리
# - CI/CD 파이프라인
```

---

### 모든 도구 허용 (권장하지 않음)

```yaml
# tools 필드 생략 시 모든 도구 허용
---
name: example
description: "설명"
# tools 필드 없음 = 모든 도구 허용
---

# 이유:
# - 보안 위험
# - 과도한 권한
# - 최소 권한 원칙 위배
```

---

## ✅ Frontmatter 작성 체크리스트

생성 전 확인 사항:

- [ ] `name`: kebab-case 형식인가? (예: `code-reviewer`)
- [ ] `name`: 3-50자 범위인가?
- [ ] `description`: 10-500자 범위인가?
- [ ] `description`: 역할과 목적이 명확한가?
- [ ] `tools`: 역할에 필요한 최소한의 도구만 포함했는가?
- [ ] `tools`: 분석 작업에 Write/Edit 포함하지 않았는가?
- [ ] `model`: 타입과 복잡도에 적합한 모델인가?
- [ ] `model`: 비용 대비 효과를 고려했는가?

---

## ❌ 흔한 실수

### 실수 1: 모호한 description

```yaml
# ❌ 나쁜 예
description: "코드를 개선합니다"

# ✅ 좋은 예
description: "TypeScript 코드의 성능과 보안을 분석하고 구체적인 개선 방안을 제시하는 분석가"
```

---

### 실수 2: 과도한 도구 권한

```yaml
# ❌ 나쁜 예 (분석 작업에 Write 권한)
name: code-reviewer
tools: Read,Write,Bash

# ✅ 좋은 예 (읽기 전용)
name: code-reviewer
tools: Read,Grep,Bash
```

---

### 실수 3: 부적절한 모델 선택

```yaml
# ❌ 나쁜 예 (단순 작업에 opus)
name: prettier-formatter
model: opus

# ✅ 좋은 예 (단순 작업에 haiku)
name: prettier-formatter
model: haiku
```

---

### 실수 4: 모든 도구 허용

```yaml
# ❌ 나쁜 예 (tools 필드 생략)
---
name: code-reviewer
description: "코드 리뷰어"
# tools 필드 없음 = 모든 도구 허용
---

# ✅ 좋은 예 (필요한 도구만 명시)
---
name: code-reviewer
description: "코드 리뷰어"
tools: Read,Grep,Bash
---
```

---

## 📖 참고 자료

- **타입 시스템**: `../type-system.md`
- **모델 선택 가이드**: `../model-selection-guide.md`
- **도구 가이드**: `../available-tools.md`
- **검증 기준**: `../validation-criteria.md`
- **템플릿**: `../templates/` 디렉토리
