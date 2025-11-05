# 서브 에이전트 베스트 프랙티스

효과적이고 안전한 서브 에이전트를 작성하기 위한 베스트 프랙티스와 작성 팁입니다.

---

## ✅ Do's (권장 사항)

### 1. 명확한 역할 정의

**원칙**: 에이전트가 무엇을 하는지 한 문장으로 설명할 수 있어야 합니다.

**좋은 예시**:
```markdown
"TypeScript와 Python 코드의 품질, 보안, 성능을 종합적으로 검증하는 전문 리뷰어"
```

**나쁜 예시**:
```markdown
"코드를 개선합니다"  # 너무 모호함
"여러 가지 작업을 수행합니다"  # 역할이 불명확
```

**체크리스트**:
- [ ] 역할이 한 문장으로 명확히 설명되는가?
- [ ] 전문 분야가 구체적으로 명시되었는가?
- [ ] 입력과 출력이 명확한가?

---

### 2. 구체적인 트리거 조건

**원칙**: 에이전트가 언제 실행될지 명확하고 구체적으로 정의합니다.

**좋은 예시**:
```markdown
## Triggers
- PR creation or update
- User requests "review this code" or "code review"
- Files with .ts, .tsx, .py extensions are modified
- Commits to main/develop branches
- Explicit: "Use the code-reviewer subagent"
```

**나쁜 예시**:
```markdown
## Triggers
- 코드 관련 작업
- 필요할 때
- 사용자가 요청하면
```

**체크리스트**:
- [ ] 3-5개의 구체적 시나리오가 있는가?
- [ ] 파일 확장자나 브랜치명이 명시되어 있는가?
- [ ] 사용자 명령어 패턴이 포함되어 있는가?

---

### 3. 최소 권한 원칙 (Principle of Least Privilege)

**원칙**: 역할 수행에 필요한 최소한의 도구만 허용합니다.

**좋은 예시**:
```yaml
# 분석 에이전트 (읽기 전용)
tools: Read,Grep

# 분석 + 검증 도구 실행
tools: Read,Grep,Bash

# 포맷팅 (파일 수정 필요)
tools: Read,Write,Bash
```

**나쁜 예시**:
```yaml
# 분석 에이전트에 Write 권한
name: code-reviewer
tools: Read,Write,Bash  # Write 불필요!

# 모든 도구 허용
# tools 필드 생략 (모든 도구 허용됨)
```

**체크리스트**:
- [ ] 분석 작업은 Read, Grep만 사용하는가?
- [ ] Write/Edit는 정말 필요한 경우만 허용했는가?
- [ ] Bash는 어떤 명령을 실행할지 명확한가?

---

### 4. 명확한 경계 설정 (Boundaries)

**원칙**: Will/Will Not을 명확히 정의하여 에이전트 행동을 제한합니다.

**좋은 예시**:
```markdown
## Boundaries

**Will:**
- Analyze code thoroughly across all quality dimensions
- Provide specific, actionable feedback with code examples
- Reference official documentation and best practices
- Suggest improvements with technical rationale
- Identify security vulnerabilities and performance issues

**Will Not:**
- Modify code directly (analysis only, no auto-fix)
- Execute potentially dangerous code
- Access external APIs without permission
- Review binary files or compiled output
- Make changes without explicit user approval
```

**나쁜 예시**:
```markdown
## Boundaries

**Will:**
- 코드 분석

**Will Not:**
- 나쁜 일
```

**체크리스트**:
- [ ] Will 항목이 3-5개인가?
- [ ] Will Not 항목이 2-4개인가?
- [ ] 보안 고려사항이 포함되어 있는가?

---

### 5. 구조화된 출력 형식

**원칙**: 일관된 출력 형식을 제공하여 사용자 경험을 향상시킵니다.

**좋은 예시**:
```markdown
## Output Format
\`\`\`
📋 Code Review Report

## Summary
- Files Reviewed: {count}
- Total Issues: {count}
- Critical: {count} | High: {count} | Medium: {count} | Low: {count}

## Critical Issues 🚨

### [Line 45-52] SQL Injection Risk
**Issue**: User input directly concatenated
**Severity**: Critical
**Fix**: Use parameterized queries

## Recommendations
1. Address critical security issues immediately
2. Consider performance optimizations
3. Add comprehensive error handling
\`\`\`
```

**나쁜 예시**:
```markdown
## Output Format
결과를 출력합니다
```

**체크리스트**:
- [ ] 출력 예시가 구체적으로 제공되었는가?
- [ ] 구조가 명확하고 읽기 쉬운가?
- [ ] 이모지나 마크다운을 적절히 사용했는가?

---

### 6. 타입별 맞춤 구조

**원칙**: Specialist/Analyst/Orchestrator에 맞는 구조를 사용합니다.

**Specialist**:
```markdown
- 100-300 단어
- 3-4단계 Behavioral Guidelines
- 간결한 Output Format
- 빠른 실행 (<30초)
```

**Analyst**:
```markdown
- 300-800 단어
- 4-5 Phase Analysis Process
- 심각도 분류 시스템
- 구조화된 보고서
```

**Orchestrator**:
```markdown
- 800-2000+ 단어
- 4-6 Phase Workflow
- 상세한 Error Handling
- 롤백 메커니즘
```

---

### 7. 에러 처리 및 검증

**원칙**: 특히 Orchestrator는 상세한 에러 처리가 필수입니다.

**좋은 예시** (Orchestrator):
```markdown
## Error Handling

### Build Failures
- **Action**: ABORT release immediately
- **Recovery**: Report errors with stack trace
- **Notification**: Alert team via configured channel

### Deployment Failures
- **Action**: Automatic rollback to previous version
- **Recovery**: Restore from backup
- **Notification**: Critical alert to on-call team

### Test Failures
- **Action**: ABORT, fix tests before retry
- **Recovery**: Provide detailed test report
- **Notification**: Standard notification
```

**체크리스트**:
- [ ] 주요 에러 시나리오가 3-5개 정의되었는가?
- [ ] 각 에러에 대한 복구 전략이 있는가?
- [ ] 롤백 메커니즘이 포함되어 있는가? (Orchestrator)

---

### 8. 타입과 모델 일치

**원칙**: 작업 복잡도에 맞는 모델을 선택합니다.

| 타입 | 권장 모델 | 이유 |
|------|-----------|------|
| Specialist (단순) | haiku | 빠르고 저렴 |
| Specialist (복잡) | inherit | 적절한 성능 |
| Analyst | sonnet | 정밀한 분석 필요 |
| Orchestrator | sonnet/opus | 복잡한 조율 필요 |

**체크리스트**:
- [ ] 단순 작업에 opus 사용하지 않았는가?
- [ ] 복잡한 분석에 haiku 사용하지 않았는가?

---

## ❌ Don'ts (피해야 할 사항)

### 1. 모호한 역할

**❌ 나쁜 예**:
```markdown
description: "코드를 개선합니다"
```

**✅ 좋은 예**:
```markdown
description: "TypeScript 코드의 성능과 보안을 분석하고 구체적인 개선 방안을 제시하는 분석가"
```

---

### 2. 과도한 권한

**❌ 나쁜 예**:
```yaml
# 분석 에이전트에 Write 권한
name: security-auditor
tools: Read,Write,Bash
```

**✅ 좋은 예**:
```yaml
# 분석은 읽기 전용
name: security-auditor
tools: Read,Grep,Bash
```

---

### 3. 불명확한 트리거

**❌ 나쁜 예**:
```markdown
## Triggers
- 코드 관련 작업
- 필요할 때
```

**✅ 좋은 예**:
```markdown
## Triggers
- PR creation or update
- Files with .ts, .py extensions modified
- User requests "review this code"
```

---

### 4. 에러 처리 누락

**❌ 나쁜 예** (Orchestrator):
```markdown
# 에러 처리 섹션 없음
```

**✅ 좋은 예**:
```markdown
## Error Handling
- Build Failures: ABORT release
- Deployment Failures: Auto rollback
- Test Failures: ABORT, fix tests
```

---

### 5. 테스트 없이 배포

**❌ 나쁜 예**:
- 생성 후 바로 프로덕션 사용
- 트리거 조건 검증 안 함

**✅ 좋은 예**:
- 생성 후 반드시 테스트
- 트리거 조건이 올바르게 동작하는지 확인
- 여러 시나리오에서 검증

---

### 6. 모든 도구 허용

**❌ 나쁜 예**:
```yaml
# tools 필드 생략 (모든 도구 허용)
---
name: example
description: "설명"
---
```

**✅ 좋은 예**:
```yaml
# 필요한 도구만 명시
---
name: example
description: "설명"
tools: Read,Grep,Bash
---
```

---

## 💡 타입별 작성 팁

### Specialist 작성 팁

**핵심 원칙**: 단순함, 빠름, 명확함

✅ **Do's**:
- 한 가지 작업에만 집중
- 3-4단계의 간단한 프로세스
- 빠른 실행 (< 30초)
- 최소 도구만 사용

❌ **Don'ts**:
- 복잡한 워크플로우 (→ Orchestrator)
- 다중 작업 처리
- 과도한 분석 (→ Analyst)

**예시 체크리스트**:
- [ ] 100-300 단어 범위인가?
- [ ] 단일 명확한 목적이 있는가?
- [ ] 3-4단계의 간단한 프로세스인가?

---

### Analyst 작성 팁

**핵심 원칙**: 종합적 분석, 건설적 피드백, 읽기 전용

✅ **Do's**:
- 여러 관점에서 종합 분석
- 구체적인 예제와 함께 피드백
- 심각도 분류 시스템 사용
- 참조 문서 제공

❌ **Don'ts**:
- 코드 직접 수정 (읽기 전용!)
- 추상적이거나 모호한 피드백
- 실행 또는 테스트 수행

**예시 체크리스트**:
- [ ] 300-800 단어 범위인가?
- [ ] 4-5단계의 분석 프로세스가 있는가?
- [ ] 심각도 분류 시스템이 있는가?
- [ ] Write/Edit 도구를 사용하지 않는가?

---

### Orchestrator 작성 팁

**핵심 원칙**: 조율, 에러 처리, 안전성

✅ **Do's**:
- 다단계 워크플로우 명확히 정의
- 상세한 에러 처리 및 롤백
- 각 Phase별 검증 조건
- 진행 상황 실시간 보고

❌ **Don'ts**:
- 에러 처리 누락
- 롤백 메커니즘 없음
- 검증 없이 다음 단계 진행

**예시 체크리스트**:
- [ ] 800-2000+ 단어 범위인가?
- [ ] 4-6단계의 워크플로우가 있는가?
- [ ] 에러 처리 전략이 정의되었는가?
- [ ] 롤백 메커니즘이 포함되었는가?

---

## 🔒 보안 베스트 프랙티스

### 1. 최소 권한 원칙

```yaml
# ✅ 좋은 예
tools: Read,Grep  # 분석만 필요

# ❌ 나쁜 예
# tools 필드 생략 (모든 도구 허용)
```

---

### 2. 위험한 명령 금지

```markdown
## Boundaries

**Will Not:**
- Execute `rm -rf` or destructive commands
- Run commands with `sudo` or elevated privileges
- Access environment variables containing secrets
- Modify critical system files
```

---

### 3. 입력 검증

```markdown
## Behavioral Guidelines
1. **Validate**: Check file paths before operations
2. **Sanitize**: Escape special characters in user input
3. **Verify**: Confirm critical operations before execution
```

---

### 4. 안전 검사 (Orchestrator)

```markdown
## Safety Checks
- Verify git status before release
- Check for uncommitted changes
- Validate environment before deployment
- Confirm backup exists before migration
```

---

## 📊 품질 향상 팁

### 1. 검증 점수 90점 이상 목표

**높은 점수를 위한 팁**:
- 모든 필수 섹션 포함
- 구체적이고 상세한 설명
- 타입에 맞는 단어 수
- 트리거 조건 3개 이상
- 구조화된 출력 형식

---

### 2. 실전 테스트

**테스트 시나리오**:
1. 트리거 조건 검증
2. 다양한 입력으로 테스트
3. 에러 시나리오 확인
4. 출력 형식 검증

---

### 3. 반복적 개선

**개선 프로세스**:
1. 생성 후 검증 리포트 확인
2. "개선 가능한 부분" 적용
3. 실제 사용하며 피드백 수집
4. 지속적으로 업데이트

---

## 🎯 실전 예시

### 예시 1: 좋은 Specialist

```yaml
---
name: eslint-enforcer
description: "JavaScript/TypeScript 파일에 ESLint 규칙을 자동으로 적용하고 코드 스타일을 일관성있게 유지하는 전문 포맷터"
tools: Read,Write,Bash
model: haiku
---

# ESLint Enforcer

단순하고 명확한 작업에 집중합니다.

## Role
ESLint를 실행하여 코드 스타일을 자동으로 수정합니다.

## Triggers
- User mentions "lint", "eslint", "format"
- .ts, .tsx, .js, .jsx files modified
- Explicit: "run eslint"

## Behavioral Guidelines
1. **Read**: 파일을 읽어 현재 상태 파악
2. **Execute**: `npx eslint --fix {file}` 실행
3. **Report**: 수정 내용과 남은 문제 보고

## Output Format
✅ ESLint Results
- Auto-Fixed: 12 issues
- Remaining: 1 warning

## Boundaries
**Will:** Run eslint --fix, Report results
**Will Not:** Modify .eslintrc, Ignore errors
```

**왜 좋은가**:
- 단일 명확한 목적
- 3단계 프로세스
- 최소 필요 도구
- 빠른 모델 (haiku)

---

### 예시 2: 좋은 Analyst

```yaml
---
name: security-auditor
description: "애플리케이션의 보안 취약점을 식별하고 OWASP 기준으로 평가하는 전문 보안 분석가"
tools: Read,Grep,Bash
model: sonnet
---

# Security Auditor

보안 취약점을 종합적으로 분석합니다.

## Role
OWASP Top 10 기준으로 보안 감사를 수행합니다.

## Expertise Areas
- OWASP Top 10 vulnerabilities
- Authentication and authorization
- Data encryption and protection
- Input validation and sanitization

## Triggers
- Security review requests
- Pre-deployment checks
- Code changes in auth/security modules

## Analysis Process
1. **Scan**: SQL injection, XSS, CSRF 검색
2. **Analyze**: 인증/권한 로직 검토
3. **Review**: 데이터 처리 및 암호화 확인
4. **Report**: 위험도별 분류 보고

## Output Format
🔒 Security Audit Report
## Risk Summary
Critical: {count} | High: {count}

## Critical Vulnerabilities
[상세 내용]

## Recommendations
[우선순위별 개선 사항]

## Analysis Standards
- **Critical**: 즉시 조치 필요
- **High**: 배포 전 수정 권장
- **Medium**: 계획적 개선
- **Low**: 참고 사항

## Boundaries
**Will:** 취약점 식별, 위험 평가, 개선 제안
**Will Not:** 코드 수정, 프로덕션 데이터 접근
```

**왜 좋은가**:
- 종합적 분석
- 심각도 분류
- 읽기 전용 (Write 없음)
- 적절한 모델 (sonnet)

---

## 📖 참고 자료

- **타입 시스템**: `type-system.md`
- **모델 선택 가이드**: `model-selection-guide.md`
- **도구 가이드**: `available-tools.md`
- **검증 기준**: `validation-criteria.md`
- **Frontmatter 예시**: `examples/frontmatter-examples.md`
