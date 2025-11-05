# 서브 에이전트 모델 선택 가이드

서브 에이전트에서 사용할 수 있는 모델과 각 모델의 특성, 장단점, 권장 사용 사례를 정리한 문서입니다.

---

## 🤖 사용 가능한 모델

| 모델 | 성능 | 속도 | 비용 | 권장 타입 |
|------|------|------|------|-----------|
| **inherit** | 변동 | 변동 | 변동 | 모든 타입 (기본) |
| **haiku** | 기본 | ⚡ 빠름 | 💰 저렴 | Specialist |
| **sonnet** | 우수 | 🔄 보통 | 💰💰 보통 | Analyst, Orchestrator |
| **opus** | 최고 | 🐌 느림 | 💰💰💰 비쌈 | 중요한 Orchestrator |

---

## 📚 모델 상세 설명

### inherit (기본값, 추천)

**설명**: 메인 대화와 동일한 모델을 사용합니다.

**장점**:
- ✅ 일관성: 메인 대화와 같은 품질 유지
- ✅ 설정 간편: 별도 모델 선택 불필요
- ✅ 유지보수 용이: 모델 변경 시 자동 반영

**단점**:
- ⚠️ 모델 변경 영향: 메인 모델 변경 시 서브 에이전트도 영향받음
- ⚠️ 세밀한 제어 불가: 에이전트별 최적화 어려움

**사용 사례**:
- 대부분의 서브 에이전트
- 특별한 성능 요구사항이 없는 경우
- 빠른 프로토타이핑

**권장 에이전트 타입**:
- ✅ Specialist (대부분)
- ✅ Analyst (대부분)
- ✅ Orchestrator (일반적인 경우)

**예시**:
```yaml
---
name: code-formatter
description: "코드 포맷팅 도구"
tools: Read,Write,Bash
model: inherit  # 메인 대화와 동일한 모델 사용
---
```

---

### haiku (빠른 응답)

**설명**: Claude Haiku - 가장 빠르고 경제적인 모델

**장점**:
- ⚡ 빠른 응답 속도
- 💰 낮은 비용
- ✅ 단순 작업에 적합

**단점**:
- ⚠️ 낮은 추론 품질
- ⚠️ 복잡한 작업 처리 어려움
- ⚠️ 미묘한 컨텍스트 놓칠 수 있음

**사용 사례**:
- 단순하고 반복적인 작업
- 명확한 규칙 기반 작업
- 빠른 피드백이 중요한 경우

**권장 에이전트 타입**:
- ✅ Specialist (포맷터, 린터 실행기)
- ⚠️ Analyst (간단한 검증만)
- ❌ Orchestrator (복잡도가 높아 부적합)

**적합한 에이전트 예시**:
- `prettier-formatter`: 단순 포맷팅
- `link-checker`: 링크 유효성 검사
- `test-runner`: 테스트 실행 및 보고

**부적합한 에이전트 예시**:
- `code-reviewer`: 복잡한 분석 필요
- `security-auditor`: 미묘한 취약점 탐지 필요
- `release-manager`: 다단계 워크플로우 조율

**예시**:
```yaml
---
name: prettier-formatter
description: "Prettier를 사용하여 코드를 자동으로 포맷팅"
tools: Read,Write,Bash
model: haiku  # 단순한 작업이므로 haiku 충분
---
```

---

### sonnet (균형잡힌 성능)

**설명**: Claude Sonnet 4.5 - 성능과 속도의 균형

**장점**:
- ✅ 우수한 추론 능력
- ✅ 복잡한 작업 처리 가능
- ✅ 합리적인 응답 속도
- ✅ 대부분의 작업에 적합

**단점**:
- ⚠️ haiku보다 느림
- ⚠️ haiku보다 비쌈

**사용 사례**:
- 복잡한 분석 작업
- 다단계 워크플로우
- 높은 품질이 필요한 작업
- 컨텍스트 이해가 중요한 경우

**권장 에이전트 타입**:
- ✅ Analyst (모든 분석 작업)
- ✅ Orchestrator (복잡한 워크플로우)
- ⚠️ Specialist (복잡한 경우만)

**적합한 에이전트 예시**:
- `code-reviewer`: 종합적 코드 리뷰
- `security-auditor`: 보안 취약점 분석
- `performance-analyzer`: 성능 병목 분석
- `deployment-coordinator`: 배포 워크플로우 조율

**예시**:
```yaml
---
name: code-reviewer
description: "TypeScript와 Python 코드의 품질, 보안, 성능을 종합 검증"
tools: Read,Grep,Bash
model: sonnet  # 복잡한 분석이므로 sonnet 권장
---
```

---

### opus (최고 품질)

**설명**: Claude Opus - 최고 성능의 모델

**장점**:
- 🏆 최고 수준의 추론 능력
- ✅ 매우 복잡한 작업 처리
- ✅ 미묘한 컨텍스트 완벽 이해
- ✅ 최고 품질의 출력

**단점**:
- 🐌 가장 느린 응답 속도
- 💰💰💰 높은 비용
- ⚠️ 대부분의 작업에는 과도함

**사용 사례**:
- 매우 중요하고 복잡한 워크플로우
- 실패 시 큰 영향이 있는 작업
- 최고 품질이 필수적인 경우

**권장 에이전트 타입**:
- ⚠️ Analyst (일반적으로 불필요)
- ✅ Orchestrator (매우 중요한 경우만)
- ❌ Specialist (과도함)

**적합한 에이전트 예시**:
- `production-release-manager`: 프로덕션 릴리스 관리
- `critical-security-auditor`: 중요 시스템 보안 감사
- `architecture-reviewer`: 시스템 아키텍처 리뷰

**부적합한 에이전트 예시**:
- 대부분의 일반적인 작업 (비용 대비 효과 낮음)

**예시**:
```yaml
---
name: production-release-manager
description: "프로덕션 환경 릴리스 전체 프로세스 관리"
tools: Read,Write,Bash
model: opus  # 매우 중요한 작업이므로 opus 사용
---
```

---

## 🎯 타입별 권장 모델

### Specialist (단일 작업 전문)

**일반 권장**: `inherit` 또는 `haiku`

| 작업 복잡도 | 권장 모델 | 예시 |
|-------------|-----------|------|
| 단순 | haiku | prettier-formatter, link-checker |
| 보통 | inherit | eslint-enforcer, test-runner |
| 복잡 | sonnet | advanced-refactorer |

```yaml
# 단순한 포맷터
model: haiku

# 일반적인 도구 실행기
model: inherit

# 복잡한 리팩토링 도구
model: sonnet
```

---

### Analyst (분석 및 리뷰)

**일반 권장**: `inherit` 또는 `sonnet`

| 분석 깊이 | 권장 모델 | 예시 |
|-----------|-----------|------|
| 표면적 | inherit | simple-validator |
| 중간 | sonnet | code-reviewer |
| 심층적 | sonnet | security-auditor, architecture-reviewer |

```yaml
# 일반적인 코드 리뷰
model: inherit

# 종합적인 분석
model: sonnet

# 보안 감사 (높은 품질 필요)
model: sonnet
```

**주의**: Analyst는 haiku 사용 권장하지 않음 (분석 품질 저하)

---

### Orchestrator (복잡한 워크플로우)

**일반 권장**: `sonnet` 또는 `opus`

| 워크플로우 중요도 | 권장 모델 | 예시 |
|-------------------|-----------|------|
| 일반 | sonnet | ci-cd-coordinator |
| 중요 | sonnet | release-manager |
| 매우 중요 | opus | production-deployment-manager |

```yaml
# 일반적인 CI/CD
model: sonnet

# 릴리스 관리
model: sonnet

# 프로덕션 배포 (실패 시 큰 영향)
model: opus
```

**주의**: Orchestrator는 haiku 사용 권장하지 않음 (복잡도 처리 어려움)

---

## 🔀 모델 선택 결정 트리

```
┌─────────────────────────┐
│ 에이전트 타입은?       │
└───────┬─────────────────┘
        │
   ┌────┴────┐
   │         │
Specialist  │
   │         │
   ├─ 단순 작업? → haiku
   ├─ 보통 작업? → inherit
   └─ 복잡 작업? → sonnet
             │
        Analyst
             │
             ├─ 표면적 분석? → inherit
             ├─ 중간 분석? → sonnet
             └─ 심층 분석? → sonnet
             │
        Orchestrator
             │
             ├─ 일반 워크플로우? → sonnet
             ├─ 중요 워크플로우? → sonnet
             └─ 매우 중요? → opus
```

---

## 💡 모델 선택 팁

### ✅ Do's

1. **기본값 사용**: 특별한 이유가 없으면 `inherit` 사용
2. **성능 테스트**: 여러 모델로 테스트 후 최적 선택
3. **비용 고려**: haiku → inherit → sonnet → opus 순으로 고려
4. **작업 복잡도 평가**: 작업에 맞는 모델 선택

### ❌ Don'ts

1. **과도한 모델 사용**: 단순 작업에 opus 사용 금지
2. **부족한 모델 사용**: 복잡한 분석에 haiku 사용 금지
3. **무분별한 opus**: 비용 대비 효과 고려 없이 opus 선택
4. **미검증 선택**: 테스트 없이 모델 선택

---

## 📊 모델 비교 매트릭스

| 요구사항 | haiku | inherit | sonnet | opus |
|----------|-------|---------|--------|------|
| 빠른 응답 | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐ | ⭐ |
| 높은 품질 | ⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| 낮은 비용 | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐ | ⭐ |
| 복잡한 추론 | ⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| 단순 작업 | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐ |
| 복잡한 작업 | ⭐ | ⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |

---

## 🔧 실전 예시

### 예시 1: ESLint 포맷터 (Specialist)

```yaml
---
name: eslint-enforcer
description: "ESLint 규칙을 자동으로 적용"
tools: Read,Write,Bash
model: haiku  # ← 단순하고 반복적인 작업이므로 haiku 충분
---
```

**선택 이유**:
- 작업이 명확하고 단순함 (eslint --fix 실행)
- 빠른 피드백이 중요
- 복잡한 추론 불필요

---

### 예시 2: 코드 리뷰어 (Analyst)

```yaml
---
name: code-reviewer
description: "종합적인 코드 품질 검증"
tools: Read,Grep,Bash
model: sonnet  # ← 복잡한 분석이 필요하므로 sonnet
---
```

**선택 이유**:
- 보안, 성능, 품질 등 다각도 분석 필요
- 컨텍스트 이해가 중요
- 미묘한 문제 탐지 필요

---

### 예시 3: 릴리스 관리자 (Orchestrator)

```yaml
---
name: release-manager
description: "전체 릴리스 프로세스 관리"
tools: Read,Write,Bash
model: sonnet  # ← 복잡한 워크플로우이므로 sonnet
---
```

**선택 이유**:
- 다단계 워크플로우 조율
- 에러 처리 및 롤백 필요
- 높은 신뢰성 필요

---

### 예시 4: 프로덕션 배포 (Orchestrator)

```yaml
---
name: production-deployment-manager
description: "프로덕션 환경 배포 전체 프로세스 관리"
tools: Read,Write,Bash
model: opus  # ← 매우 중요하고 실패 시 영향이 크므로 opus
---
```

**선택 이유**:
- 프로덕션 환경의 중요성
- 실패 시 큰 영향
- 최고 품질의 판단 필요

---

## 📖 참고 자료

- **타입 시스템**: `type-system.md`
- **도구 가이드**: `available-tools.md`
- **검증 기준**: `validation-criteria.md`
- **템플릿**: `templates/` 디렉토리
