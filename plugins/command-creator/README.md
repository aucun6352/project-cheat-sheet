# Command Creator Plugin

Claude Code를 위한 대화형 Slash 커맨드 생성 플러그인입니다.

## 📖 개요

이 플러그인은 Claude Code에서 사용할 수 있는 Slash 커맨드를 **지능형으로** 생성할 수 있도록 도와줍니다.

### ✨ 주요 특징

- 🧠 **대화 내용 분석**: 현재 세션의 대화를 자동으로 분석하여 워크플로우 추출
- 💡 **지능형 추천**: 분석 결과를 바탕으로 커맨드 구조를 자동으로 제안
- ✏️ **선택형 입력**: 추천된 내용 중 선택하거나 직접 수정 가능
- 📝 **표준화된 구조**: 일관된 품질의 커맨드 `.md` 파일 자동 생성
- 🎯 **타입 시스템**: Simple Task, Workflow Pipeline, Complex Orchestration 3가지 타입
- ✅ **품질 검증**: 생성 후 자동 검증 및 품질 평가

대화만 하면 자동으로 Slash 커맨드가 생성됩니다!

## 🆚 Skill vs Command

### Skill (스킬)
- **목적**: 지식과 패턴 문서화
- **내용**: 개념, 모범 사례, 주의사항
- **사용**: `@{스킬-이름}/SKILL.md`로 참조
- **위치**: `{프로젝트-루트}/{스킬-이름}/`

### Command (커맨드)
- **목적**: 실행 가능한 워크플로우 정의
- **내용**: 트리거, 실행 단계, 도구, 경계
- **사용**: `/{커맨드-이름}`으로 실행
- **위치**: `plugins/command-creator/commands/`

## 🚀 사용법

### 1. 커맨드 생성하기

```bash
/create-command
```

커맨드를 실행하면 **5단계**로 진행됩니다:

#### 1️⃣ 대화 분석 (자동, 조건부)
Claude가 현재 세션의 대화 내용을 자동으로 분석합니다:
- 📊 워크플로우와 자동화 프로세스 식별
- 🎯 트리거 조건 추출
- 📋 실행 단계 파악
- 🔧 사용 도구 식별
- ⚖️ 경계 조건 (Will/Will Not) 파악

**⚠️ 대화 내용이 부족하면 이 단계는 자동으로 건너뜁니다!**
- 메시지 5개 미만 → 직접 입력 모드
- 기술적 내용 없음 → 직접 입력 모드

#### 2️⃣ 기본 정보 수집 (Phase 1)

**커맨드 타입 선택** (새로운 기능! ✨):
- **Simple Task** (50-150 단어): 단일 작업, 간단한 자동화
- **Workflow Pipeline** (150-400 단어): 다단계 워크플로우
- **Complex Orchestration** (400-800 단어): 복잡한 조건 분기

**대화 분석을 완료한 경우 (추천 모드)**:
1. **커맨드 이름**: 워크플로우 기반 2-3개 추천 중 선택
2. **설명**: 자동 생성된 설명 사용하거나 수정

**대화 분석을 건너뛴 경우 (직접 입력 모드)**:
- 모든 항목을 직접 입력
- 예시와 안내가 제공됨

#### 3️⃣ 워크플로우 및 도구 정의 (Phase 2)

**추천 모드**:
- **Triggers**: 대화에서 추출한 자동 실행 조건
- **Behavioral Flow**: 대화에서 파악한 실행 단계
- **Tool Coordination**: 대화에서 식별한 사용 도구
- **Boundaries**: 대화에서 식별한 Will/Will Not

**직접 입력 모드**:
- 각 항목 예시와 함께 직접 입력

#### 4️⃣ 예제 문서화 (Phase 3)

- **Usage**: 커맨드 사용법
- **Examples**: 구체적인 사용 예제

#### 5️⃣ 파일 생성 및 검증 (Phase 4)

선택/입력한 내용을 바탕으로 표준 커맨드 `.md` 파일을 자동 생성합니다!

**자동 검증**:
- 선택한 타입에 맞는 템플릿 사용
- 생성 후 자동으로 품질 검증 (100점 만점)
- 검증 결과 및 개선 제안 제공

### 2. 커맨드 평가하기 (새로운 기능! ✨)

```bash
/evaluate-command
```

생성된 커맨드의 품질을 평가하고 개선 제안을 받을 수 있습니다:

- ✅ 6가지 항목 자동 검증 (구조, Frontmatter, 필수 섹션, 콘텐츠 품질, 타입 적합성, 완성도)
- 📊 100점 만점으로 점수 산출 및 A/B/C/D/F 등급 부여
- 💡 자동 수정 가능 항목은 즉시 개선 적용
- 📋 상세한 리포트 및 개선 가이드 제공

### 생성된 커맨드 구조

커맨드는 **plugins/command-creator/commands/** 에 생성됩니다:

```
plugins/command-creator/
├── README.md
├── shared/
│   └── validation-criteria.md  # 검증 기준 (100점 만점)
└── commands/
    ├── create-command.md
    ├── evaluate-command.md
    └── {생성된-커맨드}.md
```

### 커맨드 파일 구조

생성되는 커맨드 파일은 **선택한 타입**에 따라 최적화된 구조를 따릅니다:

```markdown
---
name: command-name
description: "커맨드 설명"
---

# /command-name - Command Title

## Triggers
- 자동 실행 조건들

## Usage
\```
/command-name [arguments] [--flags]
\```

## Behavioral Flow
1. **단계 1**: 단계 설명
2. **단계 2**: 단계 설명

## Tool Coordination
- **Tool Name**: 사용 목적

## Key Patterns
- **Pattern Name**: 패턴 설명

## Examples
### Example 1
\```
/command-name example-usage
\```

## Boundaries
**Will:**
- 수행할 작업들

**Will Not:**
- 수행하지 않을 작업들
```

## 💡 예제

### 예제 1: 대화 기반 자동 생성

**시나리오**: 배포 워크플로우에 대해 Claude와 대화한 후

```bash
/create-command
```

**Claude의 분석 결과**:
```
📊 대화 분석 완료!

🔍 발견된 워크플로우:
- 주요 작업: 프로젝트 빌드 및 배포
- 트리거: 배포 준비 완료, production 브랜치 머지
- 실행 단계: 4단계 (검증 → 빌드 → 테스트 → 배포)
- 사용 도구: Bash, Read (설정 파일)

💡 이제 추천 내용을 바탕으로 커맨드를 생성합니다.
```

**추천된 커맨드 이름**:
1. `deploy-production` ✅ (선택)
2. `build-and-deploy`
3. `production-release`

**자동 생성된 메타데이터**:
- 설명: "프로젝트를 빌드하고 production 환경에 배포하는 워크플로우"

→ 몇 번의 클릭만으로 커맨드 생성 완료!

### 예제 2: 빈 대화에서 수동 생성

**시나리오**: 새로운 세션에서 처음부터 커맨드 생성

```bash
/create-command
```

**Claude의 분석 결과**:
```
📊 대화 내용 분석 결과

ℹ️ 현재 세션의 대화 내용이 부족하여 자동 추천이 어렵습니다.

💡 직접 입력 모드로 진행합니다.
```

**진행 방식**:
- Phase 0 (대화 분석) 자동으로 건너뜀 ⏭️
- Phase 1부터 직접 입력 모드로 진행
- 모든 질문에 수동으로 답변 입력

## 🎯 커맨드 활용 방법

생성된 커맨드는 다음과 같이 사용할 수 있습니다:

### 방법 1: 직접 복사
```bash
# 생성된 파일을 .claude/commands/로 복사
cp plugins/command-creator/commands/deploy-production.md .claude/commands/
```

### 방법 2: 심볼릭 링크
```bash
# 심볼릭 링크 생성
ln -s "$(pwd)/plugins/command-creator/commands/deploy-production.md" .claude/commands/deploy-production.md
```

### 방법 3: 플러그인에서 직접 실행
설정 파일에서 플러그인 경로를 인식하도록 설정

## 🔧 커스터마이징

생성된 커맨드는 언제든지 수정할 수 있습니다:

1. `plugins/command-creator/commands/{커맨드-이름}.md` 파일을 열기
2. 필요한 섹션 추가 또는 수정
3. `/evaluate-command`로 품질 재확인 (선택)
4. 저장 후 `.claude/commands/`로 복사/링크

## 📚 섹션 가이드

### 커맨드 타입별 특징

| 타입 | 단어 수 | 복잡도 | 주요 사용 사례 |
|------|---------|--------|----------------|
| **Simple Task** | 50-150 | 낮음 | 파일 포맷팅, 코드 정리, 단일 명령 실행 |
| **Workflow Pipeline** | 150-400 | 중간 | 빌드→테스트→배포, CI/CD 파이프라인 |
| **Complex Orchestration** | 400-800 | 높음 | 다중 환경 배포, 복잡한 릴리스 프로세스 |

### Frontmatter 필드

| 필드 | 설명 | 예시 | 검증 기준 |
|------|------|------|----------|
| `name` | 커맨드 식별자 (kebab-case) | `deploy-production` | 3-64자, 소문자+하이픈 |
| `description` | 간단한 설명 (1-2문장) | "프로젝트를 배포하는 워크플로우" | 10-500자 |

### 필수 섹션

| 섹션 | 목적 | 필수 여부 | 검증 배점 |
|------|------|-----------|----------|
| **Triggers** | 자동 실행 조건 정의 | ✅ 필수 | 7점 |
| **Usage/Behavioral Flow** | 사용법 또는 실행 흐름 | ✅ 필수 | 8점 |
| **Boundaries** | Will/Will Not 정의 | ✅ 필수 | 5점 |
| **Tool Coordination** | 사용할 도구 정의 | 🟡 권장 | - |
| **Key Patterns** | 핵심 실행 패턴 | 🟡 권장 | - |
| **Examples** | 구체적 사용 예제 | ✅ 필수 | 10점 (품질 검증) |

### 검증 기준 (100점 만점)

생성된 커맨드는 다음 기준으로 자동 평가됩니다:

1. **구조 검증** (20점): 파일 위치, 명명 규칙
2. **Frontmatter 검증** (15점): name, description 형식
3. **필수 섹션 검증** (25점): Triggers, Usage/Flow, Boundaries
4. **콘텐츠 품질 검증** (25점): 코드 블록 언어 명시, 섹션 계층, 예제 품질
5. **커맨드 타입 적합성** (10점): 단어 수, 복잡도 일치
6. **일관성 및 완성도** (5점): 오타, 완성도

**등급 기준**: A (90-100), B (80-89), C (70-79), D (60-69), F (< 60)

## 📂 디렉토리 구조

### 플러그인 구조
```
plugins/command-creator/
├── README.md                       # 이 파일
├── shared/
│   └── validation-criteria.md     # 검증 기준 문서 (공통)
└── commands/
    ├── create-command.md           # 커맨드 생성 (Phase 5개)
    ├── evaluate-command.md         # 커맨드 평가 (새로운 기능!)
    └── {생성된-커맨드}.md          # 사용자가 생성한 커맨드들
```

### 사용을 위한 구조
```
{프로젝트-루트}/
├── .claude/
│   └── commands/             # 실제 사용되는 커맨드 위치
│       └── {커맨드-이름}.md  # 여기로 복사/링크
└── plugins/
    └── command-creator/
        └── commands/
            └── {커맨드-이름}.md  # 생성된 원본
```

## 🤝 기여

새로운 커맨드 패턴이나 개선 사항이 있다면:

1. 커맨드를 생성하고 테스트
2. 유용한 워크플로우나 패턴 문서화
3. 팀과 공유

---

**버전**: 1.0.0
**마지막 업데이트**: 2025-11-04
**관련 플러그인**: [Skill Creator](../skill-creator/README.md)
