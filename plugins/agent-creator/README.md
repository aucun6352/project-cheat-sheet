# Agent Creator Plugin

프로젝트를 분석하여 맞춤형 서브 에이전트를 자동으로 생성하는 Claude Code 플러그인입니다.

## 개요

이 플러그인은 `/create-agent` 커맨드를 제공하여 Claude Code의 서브 에이전트를 대화형으로 쉽게 만들 수 있습니다. 프로젝트의 언어, 프레임워크, 도구를 자동으로 감지하고, 대화 패턴에서 반복 작업을 식별하여 최적의 에이전트를 추천합니다.

## 주요 기능

### ✨ 지능형 프로젝트 분석
- **언어/프레임워크 자동 감지**: package.json, requirements.txt 등을 분석하여 프로젝트 환경 파악
- **대화 패턴 식별**: 현재 세션의 대화에서 반복 작업 패턴 추출
- **맞춤형 추천**: 프로젝트와 작업 패턴에 최적화된 에이전트 제안

### 🎯 3단계 전문성 시스템

3가지 타입의 에이전트를 제공합니다. 상세한 비교와 선택 가이드는 **[타입 시스템 가이드](shared/type-system.md)**를 참고하세요.

| 타입 | 복잡도 | 단어 수 | 용도 |
|------|--------|---------|------|
| **Specialist** | 낮음 | 100-300 | 단일 작업 전문 |
| **Analyst** | 중간 | 300-800 | 분석/리뷰 |
| **Orchestrator** | 높음 | 800-2000+ | 복잡한 워크플로우 |

### 🔒 보안 중심 설계
- **최소 권한 원칙**: 역할에 필요한 최소한의 도구만 허용
- **권한 검증**: 과도한 도구 권한 사용 시 경고
- **명시적 경계**: Will/Will Not 조건으로 에이전트 행동 명확히 제한

### ✅ 품질 보증
- **100점 만점 평가 시스템**: 6가지 카테고리로 품질 측정
- **자동 검증**: 생성 후 즉시 품질 검사 및 피드백 제공
- **A/B/C/D/F 등급**: 객관적인 품질 평가

## 설치 및 사용

### 설치

1. **플러그인 디렉토리 복사**
   ```bash
   # 전체 플러그인을 프로젝트에 복사
   cp -r plugins/agent-creator /path/to/your/project/plugins/
   ```

2. **커맨드 활성화**
   ```bash
   # .claude/commands/ 디렉토리에 심볼릭 링크 생성
   ln -s "$(pwd)/plugins/agent-creator/commands/create-agent.md" .claude/commands/create-agent.md
   ```

### 기본 사용법

```bash
/create-agent
```

커맨드를 실행하면 대화형 프로세스가 시작됩니다:

1. **프로젝트 분석** (자동)
   - 언어/프레임워크 감지
   - 대화 패턴 식별
   - 에이전트 추천

2. **타입 및 기본 정보** (질문)
   - 에이전트 타입 선택
   - 이름 입력 (kebab-case)
   - 역할 설명 작성

3. **도구 및 권한 설정** (질문)
   - 허용 도구 선택
   - 모델 선택

4. **시스템 프롬프트 작성** (질문)
   - 트리거 조건 정의
   - 행동 지침/워크플로우 작성
   - 경계 조건 설정

5. **생성 및 검증** (자동)
   - 파일 생성 (.claude/agents/)
   - 품질 검증 및 피드백
   - 사용 안내 제공

## 사용 예시

### 예시 1: 코드 리뷰 에이전트 (Analyst)

```bash
# 커맨드 실행
/create-agent

# Phase 0: 자동 분석
📊 프로젝트 분석 완료!
감지된 환경: TypeScript, React
추천: code-reviewer (Analyst)

# Phase 1: 기본 정보
타입: Analyst
이름: code-reviewer
설명: TypeScript 코드의 품질, 보안, 성능을 검증하는 리뷰어

# Phase 2: 도구 설정
도구: Read, Grep, Bash
모델: sonnet

# Phase 3: 시스템 프롬프트
트리거:
  - PR creation or update
  - User requests "review this code"

분석 프로세스: (4단계 정의)
경계 조건: (Will/Will Not)

# Phase 4: 생성 완료
✅ .claude/agents/code-reviewer.md 생성됨
📊 품질 점수: 92/100 (A)
```

**생성된 에이전트 사용**:
```
"Use the code-reviewer subagent to analyze this TypeScript file"
```

### 예시 2: ESLint 실행 에이전트 (Specialist)

```bash
/create-agent

# 직접 입력 모드 (프로젝트 분석 불충분)
타입: Specialist
이름: eslint-enforcer
설명: ESLint 규칙을 자동으로 적용하는 포맷터
도구: Read, Write, Bash
모델: haiku

트리거:
  - User mentions "lint" or "eslint"
  - .ts or .js files modified

행동 지침: (3단계 정의)
출력 형식: Structured Report

✅ .claude/agents/eslint-enforcer.md 생성됨
📊 품질 점수: 88/100 (B)
```

### 예시 3: 릴리스 관리 에이전트 (Orchestrator)

```bash
/create-agent

타입: Orchestrator
이름: release-manager
설명: 버전 관리부터 배포까지 전체 릴리스 프로세스 조율
도구: Read, Write, Bash, Grep
모델: sonnet

워크플로우 Phase: (5단계)
  - Phase 1: Pre-Release Validation
  - Phase 2: Version Management
  - Phase 3: Build & Test
  - Phase 4: Deployment
  - Phase 5: Post-Release

✅ .claude/agents/release-manager.md 생성됨
📊 품질 점수: 95/100 (A)
```

## 디렉토리 구조

```
plugins/agent-creator/
├── README.md                           # 이 파일
├── commands/
│   └── create-agent.md                 # 메인 커맨드
└── shared/
    ├── validation-criteria.md          # 검증 기준 (100점 만점)
    ├── type-system.md                  # 타입 시스템 가이드
    ├── model-selection-guide.md        # 모델 선택 가이드
    ├── available-tools.md              # 도구 목록 및 가이드
    ├── best-practices.md               # 베스트 프랙티스
    ├── examples/
    │   └── frontmatter-examples.md     # Frontmatter 예시
    └── templates/
        ├── specialist-template.md      # Specialist 타입 템플릿
        ├── analyst-template.md         # Analyst 타입 템플릿
        └── orchestrator-template.md    # Orchestrator 타입 템플릿
```

## 에이전트 타입

서브 에이전트는 3가지 타입으로 구분됩니다:

| 타입 | 복잡도 | 단어 수 | 용도 |
|------|--------|---------|------|
| **Specialist** | 낮음 | 100-300 | 단일 작업 전문 (포맷팅, 테스트 실행) |
| **Analyst** | 중간 | 300-800 | 분석/리뷰 (코드 리뷰, 보안 감사) |
| **Orchestrator** | 높음 | 800-2000+ | 복잡한 워크플로우 (릴리스, CI/CD) |

**상세 가이드**: 타입별 특징, 사용 사례, 선택 가이드, 의사결정 트리는 **[타입 시스템 가이드](shared/type-system.md)**를 참고하세요.

## 품질 검증

생성된 에이전트는 100점 만점 시스템으로 자동 검증됩니다.

6가지 카테고리로 평가: 구조(20점), Frontmatter(20점), 프롬프트 품질(30점), 도구 적합성(15점), 전문성 일치(10점), 완성도(5점)

| 점수 | 등급 | 평가 |
|------|------|------|
| 90-100 | A | 탁월함 |
| 80-89 | B | 우수함 |
| 70-79 | C | 양호함 |
| 60-69 | D | 미흡함 |
| 0-59 | F | 불합격 |

**상세 기준**: **[검증 기준 문서](shared/validation-criteria.md)**를 참고하세요.

## 베스트 프랙티스

**핵심 원칙**:
- ✅ 명확한 역할 정의, 구체적인 트리거, 최소 권한 원칙
- ❌ 모호한 역할, 과도한 권한, 불명확한 트리거

상세한 작성 가이드, 타입별 팁, 보안 체크리스트는 **[베스트 프랙티스](shared/best-practices.md)**를 참고하세요.

## 고급 기능

**배포 레벨**:
- 프로젝트 레벨 (`.claude/agents/`): 팀 공유 가능
- 사용자 레벨 (`~/.claude/agents/`): 모든 프로젝트에서 사용

**에이전트 조합**: 여러 에이전트를 순차적으로 연결하여 파이프라인 구성 가능

상세한 고급 활용법은 **[베스트 프랙티스](shared/best-practices.md)**의 "고급 활용" 섹션을 참고하세요.

## 문제 해결

가장 흔한 문제와 해결 방법:

**에이전트가 실행되지 않음**:
- 트리거 조건 확인 (더 구체적으로 수정)
- 파일 위치 확인 (`.claude/agents/`에 위치)
- Frontmatter 형식 확인 (`---`로 시작/끝)

**품질 점수 개선**:
- D/F 등급: 필수 섹션 누락 → validation-criteria.md 참고
- C 등급: 내용 부족 → 타입별 권장 단어 수 충족
- B 등급: 개선 가능 → 검증 리포트의 제안 사항 적용

추가 문제 해결은 **[베스트 프랙티스](shared/best-practices.md)**의 "흔한 실수" 섹션을 참고하세요.

## 기여 및 피드백

### 템플릿 개선

새로운 템플릿이나 개선 사항이 있다면:

1. `shared/templates/`에 추가
2. `validation-criteria.md`에 기준 추가
3. `README.md` 업데이트

### 버그 리포트

문제 발견 시:
1. 재현 단계 작성
2. 생성된 에이전트 파일 첨부
3. 검증 리포트 포함

## 참고 자료

### 공식 문서
- [Claude Code 서브 에이전트 가이드](https://docs.claude.com/en/docs/claude-code/sub-agents)
- [Claude Code 커맨드 문서](https://docs.claude.com/en/docs/claude-code/commands)

### 플러그인 내부

**가이드 문서**:
- [타입 시스템 가이드](shared/type-system.md) - 타입 선택 및 비교
- [모델 선택 가이드](shared/model-selection-guide.md) - 모델 선택 전략
- [도구 가이드](shared/available-tools.md) - 도구 목록 및 사용법
- [베스트 프랙티스](shared/best-practices.md) - 작성 팁 및 원칙
- [검증 기준](shared/validation-criteria.md) - 품질 평가 기준

**템플릿**:
- [Specialist 템플릿](shared/templates/specialist-template.md)
- [Analyst 템플릿](shared/templates/analyst-template.md)
- [Orchestrator 템플릿](shared/templates/orchestrator-template.md)

**예시**:
- [Frontmatter 예시](shared/examples/frontmatter-examples.md)

### 관련 플러그인
- `command-creator`: 슬래시 커맨드 생성
- `skill-creator`: Claude Code 스킬 생성

## 라이선스

이 플러그인은 프로젝트의 라이선스를 따릅니다.

## 변경 이력

### v1.0.0 (2025-01-05)
- ✨ 초기 릴리스
- ✅ `/create-agent` 커맨드
- ✅ 3가지 타입 시스템 (Specialist/Analyst/Orchestrator)
- ✅ 프로젝트 컨텍스트 분석
- ✅ 100점 만점 검증 시스템
- ✅ 타입별 템플릿 제공

---

**만든 이**: Agent Creator Plugin Team
**버전**: 1.0.0
**최종 업데이트**: 2025-01-05
