# Command Creator Plugin

Claude Code를 위한 대화형 Slash 커맨드 생성 플러그인입니다.

## 📖 개요

이 플러그인은 Claude Code에서 사용할 수 있는 Slash 커맨드를 **지능형으로** 생성할 수 있도록 도와줍니다.

### ✨ 주요 특징

- 🧠 **대화 내용 분석**: 현재 세션의 대화를 자동으로 분석하여 워크플로우 추출
- 💡 **지능형 추천**: 분석 결과를 바탕으로 커맨드 구조를 자동으로 제안
- ✏️ **선택형 입력**: 추천된 내용 중 선택하거나 직접 수정 가능
- 📝 **표준화된 구조**: 일관된 품질의 커맨드 `.md` 파일 자동 생성

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

### 커맨드 생성하기

```bash
/create-command
```

커맨드를 실행하면 다음과 같이 진행됩니다:

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

#### 2️⃣ 메타데이터 수집

**대화 분석을 완료한 경우 (추천 모드)**:
1. **커맨드 이름**: 워크플로우 기반 2-3개 추천 중 선택
2. **설명**: 자동 생성된 설명 사용하거나 수정

**대화 분석을 건너뛴 경우 (직접 입력 모드)**:
- 모든 항목을 직접 입력
- 예시와 안내가 제공됨

#### 3️⃣ 워크플로우 정의

**추천 모드**:
- **Triggers**: 대화에서 추출한 자동 실행 조건
- **Behavioral Flow**: 대화에서 파악한 실행 단계
- **Boundaries**: 대화에서 식별한 Will/Will Not

**직접 입력 모드**:
- 각 항목 예시와 함께 직접 입력

#### 4️⃣ 도구 설정

- **Tool Coordination**: 사용할 도구 선택

#### 5️⃣ 예제 및 문서화

- **Usage**: 커맨드 사용법
- **Examples**: 구체적인 사용 예제

#### 6️⃣ 커맨드 생성 (자동)
선택/입력한 내용을 바탕으로 표준 커맨드 `.md` 파일을 자동 생성합니다!

### 생성된 커맨드 구조

커맨드는 **plugins/command-creator/commands/** 에 생성됩니다:

```
plugins/command-creator/commands/
└── {커맨드-이름}.md
```

### 커맨드 파일 구조

생성되는 커맨드 파일은 다음과 같은 표준 구조를 따릅니다:

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
3. 저장 후 `.claude/commands/`로 복사/링크

## 📚 섹션 가이드

### Frontmatter 필드

| 필드 | 설명 | 예시 |
|------|------|------|
| `name` | 커맨드 식별자 (kebab-case) | `deploy-production` |
| `description` | 간단한 설명 (1-2문장) | "프로젝트를 배포하는 워크플로우" |

### 주요 섹션

| 섹션 | 목적 | 필수 여부 |
|------|------|-----------|
| **Triggers** | 자동 실행 조건 정의 | ✅ 필수 |
| **Usage** | 사용법 예시 | ✅ 필수 |
| **Behavioral Flow** | 실행 단계 정의 | ✅ 필수 |
| **Tool Coordination** | 사용할 도구 정의 | 🟡 권장 |
| **Key Patterns** | 핵심 실행 패턴 | 🟡 권장 |
| **Examples** | 구체적 사용 예제 | ✅ 필수 |
| **Boundaries** | Will/Will Not 정의 | ✅ 필수 |

## 📂 디렉토리 구조

### 플러그인 구조
```
plugins/command-creator/
├── README.md                 # 이 파일
└── commands/
    ├── create-command.md     # 커맨드 생성 커맨드
    └── (생성된 커맨드들)
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
