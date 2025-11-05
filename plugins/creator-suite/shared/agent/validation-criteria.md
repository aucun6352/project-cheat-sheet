# 서브 에이전트 검증 기준

이 문서는 `/create-agent` 커맨드로 생성된 서브 에이전트 파일의 품질을 평가하는 기준을 정의합니다.

## 평가 시스템

**총점: 100점**

| 점수 범위 | 등급 | 평가 |
|-----------|------|------|
| 90-100 | A | 탁월함 (Excellent) |
| 80-89 | B | 우수함 (Good) |
| 70-79 | C | 양호함 (Fair) |
| 60-69 | D | 미흡함 (Poor) |
| 0-59 | F | 불합격 (Fail) |

---

## 1. 구조 검증 (20점)

### 1.1 파일 위치 (10점)
- ✅ **10점**: `.claude/agents/` 디렉토리에 위치
- ⚠️ **5점**: 다른 위치이지만 agents 디렉토리 존재
- ❌ **0점**: 잘못된 위치

### 1.2 파일명 규칙 (10점)
- ✅ **10점**: kebab-case 형식 (예: `code-reviewer.md`)
- ⚠️ **5점**: 소문자이지만 하이픈 미사용
- ❌ **0점**: camelCase, PascalCase, snake_case 사용

**검증 방법**:
```bash
# 올바른 예
.claude/agents/code-reviewer.md
.claude/agents/test-coordinator.md

# 잘못된 예
.claude/agents/CodeReviewer.md     # PascalCase
.claude/agents/code_reviewer.md    # snake_case
agents/code-reviewer.md             # 잘못된 위치
```

---

## 2. Frontmatter 검증 (20점)

### 2.1 name 필드 (5점)
- ✅ **5점**: 필수 필드 존재, kebab-case 형식
- ⚠️ **3점**: 존재하지만 형식 오류
- ❌ **0점**: 필드 없음

### 2.2 description 필드 (5점)
- ✅ **5점**: 10-500자의 명확한 설명
- ⚠️ **3점**: 너무 짧거나 김 (10자 미만 또는 500자 초과)
- ❌ **0점**: 필드 없음

### 2.3 tools 필드 (5점)
- ✅ **5점**: 올바른 형식 (쉼표로 구분, 유효한 도구명)
- ⚠️ **3점**: 형식은 맞지만 불필요한 도구 포함
- ✅ **5점**: 생략 (모든 도구 허용)
- ❌ **0점**: 잘못된 형식 또는 존재하지 않는 도구

**유효한 도구**: Read, Write, Edit, Bash, Grep, Glob, WebFetch, WebSearch 등

### 2.4 model 필드 (5점)
- ✅ **5점**: inherit, sonnet, opus, haiku 중 하나
- ⚠️ **3점**: 생략 (기본값 inherit 사용)
- ❌ **0점**: 잘못된 모델명

**올바른 Frontmatter 예시**:
```yaml
---
name: code-reviewer
description: "TypeScript와 Python 코드의 품질, 보안, 성능을 체계적으로 검증하는 전문 리뷰어"
tools: Read,Grep,Bash
model: sonnet
---
```

---

## 3. 시스템 프롬프트 품질 (30점)

### 3.1 역할 정의 명확성 (8점)
- ✅ **8점**: 에이전트의 역할과 책임이 명확히 정의됨
- ⚠️ **4점**: 역할은 있지만 모호함
- ❌ **0점**: 역할 정의 없음

**평가 기준**:
- 에이전트가 무엇을 하는지 명확한가?
- 전문 분야가 구체적으로 명시되었는가?
- 사용자가 언제 이 에이전트를 사용해야 하는지 알 수 있는가?

### 3.2 트리거 조건 구체성 (7점)
- ✅ **7점**: 구체적인 트리거 조건 3개 이상
- ⚠️ **4점**: 트리거는 있지만 모호함 (1-2개)
- ❌ **0점**: 트리거 조건 없음

**좋은 예**:
```markdown
## Triggers
- User mentions "lint", "eslint", or "format code"
- .ts or .js files are modified
- Explicit request: "run eslint"
```

### 3.3 행동 지침 상세성 (8점)
- ✅ **8점**: 단계별 행동 지침이 명확함 (3단계 이상)
- ⚠️ **4점**: 행동 지침이 있지만 추상적
- ❌ **0점**: 행동 지침 없음

**평가 요소**:
- 작업 프로세스가 단계별로 설명되었는가?
- 각 단계에서 무엇을 해야 하는지 명확한가?
- 에러 처리 방법이 언급되었는가?

### 3.4 출력 형식 명시 (7점)
- ✅ **7점**: 출력 형식이 예시와 함께 명확히 정의됨
- ⚠️ **4점**: 출력 형식은 있지만 예시 없음
- ❌ **0점**: 출력 형식 언급 없음

**좋은 예**:
````markdown
## Output Format
```
✅ ESLint Results for {file}

Fixed Issues:
- [Line 23] semi: Missing semicolon
- [Line 45] no-unused-vars: 'data' is defined but never used

Remaining Issues:
- [Line 67] complexity: Function too complex (12 > 10)
```
````

---

## 4. 도구 설정 적합성 (15점)

### 4.1 최소 권한 원칙 (8점)
- ✅ **8점**: 역할에 필요한 최소한의 도구만 허용
- ⚠️ **4점**: 불필요한 도구 일부 포함
- ❌ **0점**: 모든 도구 무분별하게 허용

**평가 기준**:
- 읽기 전용 작업인데 Write 권한이 있는가? (감점)
- 파일 검색이 필요 없는데 Grep, Glob이 있는가? (감점)
- Bash 실행이 필요 없는데 Bash 권한이 있는가? (감점)

### 4.2 보안 고려사항 (7점)
- ✅ **7점**: 보안 위험이 명시되고 경계 조건이 명확함
- ⚠️ **4점**: 경계 조건은 있지만 보안 고려 부족
- ❌ **0점**: 보안 고려사항 없음

**확인 사항**:
```markdown
## Boundaries
**Will Not:**
- Execute potentially dangerous code
- Modify production database directly
- Access external APIs without permission
```

---

## 5. 전문성 수준 일치 (10점)

### 5.1 타입과 복잡도 일치 (5점)
- ✅ **5점**: 선택한 타입과 실제 복잡도가 일치
- ⚠️ **3점**: 약간의 불일치
- ❌ **0점**: 명백한 불일치

| 타입 | 예상 복잡도 | 단어 수 범위 |
|------|-------------|--------------|
| Specialist | 낮음 | 100-300 |
| Analyst | 중간 | 300-800 |
| Orchestrator | 높음 | 800-2000+ |

### 5.2 단어 수 적정성 (5점)
- ✅ **5점**: 타입별 권장 범위 내
- ⚠️ **3점**: 범위를 ±20% 벗어남
- ❌ **0점**: 범위를 크게 벗어남

**검증 방법**:
```bash
# Markdown 본문 단어 수 계산 (Frontmatter 제외)
wc -w .claude/agents/code-reviewer.md
```

---

## 6. 일관성 및 완성도 (5점)

### 6.1 문법 및 오타 (3점)
- ✅ **3점**: 오타 없음, 문법 정확
- ⚠️ **2점**: 경미한 오타 1-2개
- ❌ **0점**: 다수의 오타 또는 문법 오류

### 6.2 완성도 (2점)
- ✅ **2점**: 모든 섹션 완성, TODO 없음
- ⚠️ **1점**: 일부 섹션 미완성 또는 TODO 존재
- ❌ **0점**: 대부분 미완성

---

## 검증 체크리스트

생성된 서브 에이전트 파일이 다음을 모두 만족하는지 확인하세요:

### 필수 요소 (반드시 포함)
- [ ] Frontmatter (`---`로 구분)
- [ ] `name` 필드 (kebab-case)
- [ ] `description` 필드 (10-500자)
- [ ] 에이전트 역할 정의
- [ ] 트리거 조건 (최소 1개)
- [ ] 행동 지침 또는 워크플로우
- [ ] 경계 조건 (Will/Will Not)

### 권장 요소 (포함 시 가산점)
- [ ] `tools` 필드 (명시적 권한 제어)
- [ ] `model` 필드 (성능 최적화)
- [ ] 출력 형식 예시
- [ ] 에러 처리 방법
- [ ] 사용 예제
- [ ] 보안 고려사항

### 품질 지표
- [ ] 단어 수가 타입에 적합
- [ ] 역할과 도구가 일치
- [ ] 오타 및 문법 오류 없음
- [ ] TODO 또는 미완성 섹션 없음

---

## 자동 검증 스크립트 (참고)

```bash
#!/bin/bash
# validate-agent.sh - 서브 에이전트 파일 검증

AGENT_FILE="$1"

if [ ! -f "$AGENT_FILE" ]; then
  echo "❌ 파일을 찾을 수 없습니다: $AGENT_FILE"
  exit 1
fi

score=0

# 1. 파일 위치 검증
if [[ "$AGENT_FILE" == *".claude/agents/"* ]]; then
  echo "✅ 파일 위치 올바름 (+10점)"
  score=$((score + 10))
else
  echo "❌ 파일 위치 잘못됨 (0점)"
fi

# 2. 파일명 검증 (kebab-case)
filename=$(basename "$AGENT_FILE" .md)
if [[ "$filename" =~ ^[a-z]+(-[a-z]+)*$ ]]; then
  echo "✅ 파일명 형식 올바름 (+10점)"
  score=$((score + 10))
else
  echo "❌ 파일명 형식 오류: $filename (0점)"
fi

# 3. Frontmatter 검증
if grep -q "^---$" "$AGENT_FILE"; then
  echo "✅ Frontmatter 존재 (+5점)"
  score=$((score + 5))

  # name 필드
  if grep -q "^name:" "$AGENT_FILE"; then
    echo "✅ name 필드 존재 (+5점)"
    score=$((score + 5))
  fi

  # description 필드
  if grep -q "^description:" "$AGENT_FILE"; then
    echo "✅ description 필드 존재 (+5점)"
    score=$((score + 5))
  fi
else
  echo "❌ Frontmatter 없음 (0점)"
fi

# 최종 점수
echo ""
echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
echo "📊 검증 점수: $score/100"

if [ $score -ge 90 ]; then
  echo "🏆 등급: A (탁월함)"
elif [ $score -ge 80 ]; then
  echo "🥈 등급: B (우수함)"
elif [ $score -ge 70 ]; then
  echo "🥉 등급: C (양호함)"
elif [ $score -ge 60 ]; then
  echo "⚠️ 등급: D (미흡함)"
else
  echo "❌ 등급: F (불합격)"
fi
```

---

## 평가 예시

### 예시 1: 탁월한 에이전트 (A 등급, 95점)

```markdown
---
name: code-reviewer
description: "TypeScript와 Python 코드의 품질, 보안, 성능을 체계적으로 검증하는 전문 리뷰어"
tools: Read,Grep,Bash
model: sonnet
---

# Code Reviewer

You are an expert code reviewer specializing in TypeScript and Python.

## Role
Perform thorough code reviews focusing on quality, security, performance, and maintainability.

## Triggers
- PR creation or update
- User requests "review this code"
- Files with `.ts`, `.tsx`, `.py` extensions modified

## Review Process
1. **Read** the code files
2. **Analyze** for issues (security, performance, maintainability, style)
3. **Grep** for common anti-patterns
4. **Report** findings with severity levels

## Output Format
```
📋 Code Review Report
...
```

## Boundaries
**Will:**
- Analyze code thoroughly
- Provide specific, actionable feedback

**Will Not:**
- Modify code directly (analysis only)
- Execute potentially dangerous code
```

**평가**:
- 구조: 20/20 ✅
- Frontmatter: 20/20 ✅
- 시스템 프롬프트: 28/30 ✅
- 도구 설정: 15/15 ✅
- 전문성 수준: 10/10 ✅
- 완성도: 2/5 ⚠️ (출력 예시 미완성)
- **총점: 95/100 (A)**

### 예시 2: 개선 필요 (C 등급, 72점)

```markdown
---
name: myAgent
description: "코드 검토"
---

# My Agent

이 에이전트는 코드를 검토합니다.

## 작업
코드를 읽고 문제를 찾습니다.
```

**평가**:
- 구조: 10/20 ⚠️ (파일명 오류: camelCase)
- Frontmatter: 8/20 ❌ (name 형식 오류, description 너무 짧음)
- 시스템 프롬프트: 12/30 ❌ (역할 모호, 트리거 없음, 출력 형식 없음)
- 도구 설정: 15/15 ✅ (생략 = 모든 도구)
- 전문성 수준: 5/10 ⚠️ (단어 수 부족)
- 완성도: 2/5 ⚠️
- **총점: 52/100 (F)**

**개선 필요 사항**:
1. 파일명을 `my-agent.md`로 변경
2. description 구체화 (10자 이상)
3. 트리거 조건 추가
4. 행동 지침 상세화
5. 출력 형식 정의
6. 경계 조건 추가

---

## 결론

이 검증 기준을 활용하여:
- ✅ 생성된 서브 에이전트의 품질을 객관적으로 평가
- ✅ 개선이 필요한 부분을 명확히 식별
- ✅ 일관된 품질의 에이전트 유지
- ✅ 팀 전체의 에이전트 품질 향상
