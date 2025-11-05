# 스킬 생성 커맨드 (Create Skill)

이 커맨드는 Claude Code에서 사용할 수 있는 새로운 스킬(Skill)을 대화형으로 생성합니다.

## 목적

사용자와 대화형으로 상호작용하며 표준화된 구조의 스킬 문서를 자동으로 생성합니다. 복잡한 파일 구조를 수동으로 만들지 않고도 일관된 품질의 스킬을 빠르게 작성할 수 있습니다.

**💡 참고:** 기존 SKILL.md를 평가하고 싶다면 `/evaluate-skill` 커맨드를 사용하세요.

## Extended Thinking

당신은 스킬 생성 전문가로서 다음과 같이 동작합니다:

**📋 진행 상황 체크리스트**:
- [ ] Phase 0: 대화 분석 및 추천 생성 (조건부)
- [ ] Phase 1: 스킬 타입 선택 + 기본 정보 수집 (타입, 이름, 설명, 사용 시나리오, License)
- [ ] Phase 2: 내용 작성 (핵심 개념, 패턴/예제, 모범 사례, 주의사항, 포맷팅)
- [ ] Phase 3: 파일 생성 (Progressive Disclosure 적용 + 타입별 템플릿 생성)
- [ ] Phase 4: 검증 및 완료 (@shared/skill/validation-criteria.md 기준 검증 + 사용자 안내)

**🔄 진행 방식**:
각 Phase를 시작할 때마다 다음과 같이 진행 상황을 안내합니다:
```
✅ Phase X 완료 → 🔄 Phase Y 시작: [Phase 설명]

📊 전체 진행률: X/4 Phase 완료 (YY%)
```

**Phase 내 진행률 표시:**
Phase 1, 2에서는 질문 진행 상황도 표시합니다:
```
🔄 Phase 2: 내용 작성
📊 Phase 진행률: 3/5 질문 완료 (60%)
📊 전체 진행률: 2/4 Phase 완료 (50%)
```

**핵심 동작**:
1. **대화 분석 및 추천**: 세션 대화 분석 → 스킬 메타데이터 자동 제안
2. **타입 및 정보 수집**: 스킬 타입 선택 → 최적화된 질문으로 정보 수집
3. **템플릿 기반 생성**: 타입별 템플릿 + Progressive Disclosure 적용
4. **품질 검증 및 안내**: @shared/skill/validation-criteria.md 기준으로 검증 후 사용 방법 안내

## 실행 단계

### Phase 0: 대화 분석 및 추천 생성 (조건부)

**⚠️ 중요**: 현재 세션의 대화 내용이 충분하지 않은 경우 이 단계를 건너뛰고 **Phase 1**로 직접 이동합니다.

**대화 내용 충분성 판단 기준**:
- 대화 메시지 수가 5개 미만인 경우 → Phase 0 건너뛰기
- 기술적 내용이나 코드 예제가 전혀 없는 경우 → Phase 0 건너뛰기
- 단순 인사나 질문만 있는 경우 → Phase 0 건너뛰기

**대화 내용이 충분한 경우에만 다음을 수행합니다:**

1. **대화 히스토리 분석 및 자동 생성**
   - 주요 주제/키워드/코드 예제 식별 및 추출
   - 스킬 이름 2-3개 제안 (kebab-case, Gerund form)
   - 스킬 설명, 사용 시나리오, 핵심 개념 자동 생성
   - 모범 사례와 주의사항 수집

2. **분석 결과 요약**
   ```
   📊 대화 분석 완료!

   🔍 발견된 내용:
   - 주요 주제: {식별된_주제}
   - 코드 예제: {발견된_예제_수}개
   - 논의된 패턴: {패턴_목록}
   - 핵심 개념: {개념_수}개

   💡 이제 추천 내용을 바탕으로 스킬을 생성합니다.
   ```

**대화 내용이 불충분한 경우 안내**:
   ```
   📊 대화 내용 분석 결과

   ℹ️ 현재 세션의 대화 내용이 부족하여 자동 추천이 어렵습니다.

   💡 직접 입력 모드로 진행합니다.
      - 모든 정보를 수동으로 입력해주세요
      - 또는 스킬 주제에 대해 먼저 대화를 나눈 후 다시 시도해보세요
   ```

### Phase 1: 스킬 타입 선택 + 기본 정보 수집

**1단계: 스킬 타입 선택**

**목적**: 스킬 특성에 맞는 최적화된 템플릿과 가이드 제공

**4가지 스킬 타입**:

1. **Quick Workflow** (200-400 단어) - 단일 목적 순차 프로세스
   - 적합: 간단한 도구, 빠른 자동화

2. **Comprehensive Guide** (600-1,500 단어) - 다기능 + 예제
   - 적합: 표준 개발 워크플로우

3. **Technical Reference** (1,500-5,000+ 단어) - 깊이 있는 API 문서
   - 적합: 복잡한 도구킷, 라이브러리

4. **Philosophy-Driven** (1,000-3,000 단어) - 개념 프레임워크 + 구현
   - 적합: 창의적 프로세스, 판단 기반 작업

**실행**: AskUserQuestion으로 타입 선택 제공 (기본값: Comprehensive Guide)

**2-5단계: 기본 정보 수집**

**질문 패턴** (공통):
- **추천 모드** (Phase 0 완료): 추출 내용 제시 → "그대로 사용/수정/추가" 선택
- **직접 입력 모드** (Phase 0 건너뜀): 형식 안내 → 예시 제공 → 입력 요청

**수집 정보**:

2. **스킬 이름**: kebab-case, 64자 이내, Gerund form 권장 (예: processing-pdfs)
   - 추천 모드: 2-3개 이름 제시 + 직접 입력 옵션
   - 검증: 형식 오류 시 재질문

3. **스킬 설명**: 1024자 이내, 1-2문장, 3인칭, "무엇을+언제" 명시
   - 추천 모드: 자동 생성 설명 제시 + 수정 옵션
   - 검증: 필수 입력

4. **사용 시나리오**: 여러 줄 목록 형태
   - 추천 모드: 추출 시나리오 제시 + 추가/수정 옵션
   - 예: "새로운 API 설계 시", "코드 리뷰 시" 등

5. **License** (선택): Frontmatter에 license 필드 포함 여부
   - 추천: "Complete terms in LICENSE.txt" (표준 형식)
   - 미포함 선택 가능

### Phase 2: 내용 작성

**질문 패턴**: Phase 1과 동일 (추천 모드/직접 입력 모드)

⚠️ **작성 시 유의사항**: Phase 4 품질 검증 기준 참조

**수집 정보**:

6. **핵심 개념**: 자유 형식 텍스트, 여러 단락 가능
   - 주요 개념, 원칙, 이론 등 포함
   - 추천 모드: 자동 정리 제시 + 편집 옵션

7. **패턴/예제** (선택):
   - 코드 블록: 패턴 이름 + 언어 + 코드
   - 추천 모드: 발견된 예제 제시 + 선택/추가/제외 옵션
   - 직접 입력 모드: 포함 여부 선택 → 예시 입력
   - **예제 품질 레벨 선택**:
     - Basic: 문법 시연만 (최소)
     - Standard: 실제 값 포함 작동 코드 (권장)
     - Production: 에러 핸들링 + edge cases (고급)

8. **모범 사례**: 목록 형태
   - 예: "명확한 네이밍 사용", "일관된 에러 처리"
   - 추천 모드: 수집된 사례 제시 + 추가 옵션

9. **주의사항**: 목록 형태
   - 예: "과도한 추상화 지양", "성능 고려"
   - 추천 모드: 식별된 주의사항 제시 + 추가 옵션

10. **포맷팅 스타일** (선택):
    - **이모지 마커**: 섹션 구분용 (🚀, 📋, ⚠️ 등)
    - **시각적 마커**: 베스트 프랙티스 표시 (✅/❌)
    - **테이블 사용**: 빠른 참조용
    - 기본값: 최소 사용 (필요시만)

### Phase 3: 파일 생성

수집한 정보를 바탕으로 프로젝트 루트에 다음 파일 구조를 생성합니다:

**Progressive Disclosure 기준** (단어 수 기반):

**Level 1** (< 400 단어):
```
{스킬-이름}/
└── SKILL.md          # 모든 내용 포함
```

**Level 2** (400-1,500 단어):
```
{스킬-이름}/
├── SKILL.md          # 개요 + 핵심 가이드
└── examples/         # 상세 예제 디렉토리
```

**Level 3** (1,500-3,500 단어):
```
{스킬-이름}/
├── SKILL.md          # 개요 + When to Use + 간단 예제
├── REFERENCE.md      # 상세 API 문서
└── examples/         # 예제 및 템플릿
```

**Level 4** (> 3,500 단어):
```
{스킬-이름}/
├── SKILL.md          # 개요 (참조 링크 포함)
├── EXAMPLES.md       # 상세 예제
├── REFERENCE.md      # API/기술 문서
└── FORMS.md          # 템플릿/체크리스트
```

**중요**: 스킬은 현재 프로젝트의 루트 디렉토리에 생성됩니다.

**공통 섹션 구성요소**:

**Frontmatter** (필수):
```yaml
---
name: {스킬-이름}           # kebab-case, 64자 이내
description: {스킬-설명}     # 1024자 이내, "무엇을+언제"
license: Complete terms in LICENSE.txt  # 선택, 85% 스킬이 사용
---
```

**필수 본문 섹션**:
- `# {스킬 제목}` (H1, 1개만)
- `## When to Use This Skill` - 사용 시나리오, 트리거
- `## Core Concepts` - 핵심 개념, 원칙

**권장 본문 섹션** (스킬 타입에 따라 선택):
- `## Quick Start` / `## Overview` / `## Philosophy` - 시작점 (타입별 선택)
- `## Patterns` / `## Examples` - 패턴 및 예제
- `## Best Practices` - 모범 사례
- `## Common Pitfalls` - 주의사항, 안티패턴
- `## Additional Resources` - 추가 리소스, 참조 링크

**스킬 타입별 템플릿 구조**:

**1. Quick Workflow 타입** (200-400 단어):
```markdown
---
name: quick-tool-name
description: Brief description of what and when
---

# Quick Tool Name

## Quick Start
[즉시 사용 가능한 3-5단계]

## When to Use This Skill
- Trigger 1
- Trigger 2

## Detailed Workflow
[상세 단계별 가이드]

## Common Pitfalls
- 주의사항 목록
```

**2. Comprehensive Guide 타입** (600-1,500 단어):
```markdown
---
name: comprehensive-skill-name
description: Detailed description of purpose and context
license: Complete terms in LICENSE.txt
---

# Comprehensive Skill Name

## Overview
[1-2 문단 소개]

## When to Use This Skill
- Use case 1
- Use case 2
- Use case 3

## Core Concepts
[핵심 개념 설명]

## How to Use
[단계별 워크플로우 또는 패턴]

## Patterns / Examples
[코드 예제]

## Best Practices
- Best practice 1
- Best practice 2

## Common Pitfalls
- 주의사항

## Additional Resources
- 관련 리소스 링크
```

**3. Technical Reference 타입** (1,500-5,000+ 단어):
```markdown
---
name: technical-toolkit-name
description: Comprehensive toolkit for specific technical domain
license: Complete terms in LICENSE.txt
---

# Technical Toolkit Name

## Overview
[기술 스택 및 목적]

## When to Use This Skill
- Complex scenario 1
- Complex scenario 2

## Quick Reference
| Task | Tool/Method | Example |
|------|------------|---------|
| ... | ... | ... |

## Core Concepts
[이론적 배경]

## Detailed API Reference
[상세 API 문서]

## Examples
### Example 1: [Use Case]
[완전한 코드 예제]

### Example 2: [Use Case]
[완전한 코드 예제]

## Best Practices
- Practice 1
- Practice 2

## Additional Resources
- [@{skill-name}/REFERENCE.md](./REFERENCE.md)
- [@{skill-name}/EXAMPLES.md](./EXAMPLES.md)
```

**4. Philosophy-Driven 타입** (1,000-3,000 단어):
```markdown
---
name: creative-approach-name
description: Conceptual framework for creative/judgment tasks
license: Complete terms in LICENSE.txt
---

# Creative Approach Name

## Philosophy / Approach
[개념적 프레임워크, 사고방식]

## When to Use This Skill
- Creative scenario 1
- Judgment-based scenario 2

## Core Concepts
[핵심 원칙 및 이론]

## Implementation Workflow
1. [개념 적용 단계]
2. [실행 단계]
3. [검증 단계]

## Patterns / Examples
[개념 시연 예제]

## Best Practices
- 창의적 접근법 가이드

## Common Pitfalls
- 피해야 할 함정

## Additional Resources
- 관련 철학/이론 링크
```

**참고**:
- Level 3-4 스킬은 위 템플릿의 Additional Resources 섹션에 참조 링크 포함
- Progressive Disclosure 파일 생성 시 `examples/`, `REFERENCE.md`, `EXAMPLES.md`, `FORMS.md` 활용

### Phase 4: 검증 및 완료

**검증 실행**:

@shared/skill/validation-criteria.md 의 검증 절차를 따라 실행합니다.

**1. 기본 검증 수행**:
   - 구조 검증 (파일 존재, Progressive Disclosure 적합성)
   - Frontmatter 검증 (name, description 형식)
   - 필수 섹션 검증 (When to Use, Core Concepts, Best Practices)
   - 콘텐츠 품질 검증 (코드 블록, 섹션 계층, 링크)

**2. 간단한 요약 리포트 생성**:

```markdown
📊 생성 완료 - 간단한 검증 결과

## 기본 품질 확인
✅ 파일 구조: 적절함
✅ Frontmatter: 올바른 형식
✅ 필수 섹션: 모두 포함
⚠️ 개선 가능 항목: {있다면 1-2개만 간략히}

💡 더 자세한 평가가 필요하다면 `/evaluate-skill` 커맨드를 사용하세요.
```

**사용자 안내**:
   ```
   ✅ 스킬이 성공적으로 생성되었습니다!

   📂 위치: {스킬-이름}/SKILL.md (프로젝트 루트)

   📖 다음 단계:
   1. Claude Code와의 대화에서 @{스킬-이름}/SKILL.md 로 참조하여 사용
   2. 필요시 SKILL.md 파일을 직접 수정하여 내용 보완
   3. {스킬-이름}/assets/ 또는 {스킬-이름}/references/ 디렉토리를 추가하여 추가 리소스 포함

   💡 팁:
   - 스킬은 언제든지 수정 가능합니다
   - 코드 템플릿이나 체크리스트를 assets/에 추가하면 더욱 유용합니다
   - 다른 사람과 공유하여 팀의 지식을 축적하세요
   ```

## 에러 처리

### 생성 모드 에러
- **잘못된 스킬 이름**: kebab-case 형식 재입력 요청
- **이미 존재하는 스킬**: 다른 이름 사용 / 덮어쓰기 / 취소 선택지 제공
- **필수 정보 누락**: 누락된 필드 재입력 요청

## 주의사항

1. **진행 상황 추적**:
   - 각 Phase 시작 시 "✅ Phase X 완료 → 🔄 Phase Y 시작: [Phase 설명]" 형식으로 진행 상황 안내
   - 전체 프로세스 중 현재 위치를 명확히 표시: "📊 진행 상황: X/4 Phase 완료"

2. **대화 분석 우선** (Phase 0 실행 시):
   - 질문하기 전에 반드시 현재 세션의 대화 내용을 분석하여 추천 정보 생성
   - 분석 결과가 없거나 부족하면 사용자에게 안내 후 직접 입력 모드로 전환

3. **모드별 질문 방식**:
   - **추천 모드**: 추출된 내용을 선택지로 제시하고 "직접 입력" 옵션 포함
   - **직접 입력 모드**: 예시와 함께 사용자에게 직접 입력 요청

4. **AskUserQuestion 사용**: 모든 정보 수집은 AskUserQuestion 도구를 통해 단계별로 진행

5. **Write 도구 사용**: SKILL.md 파일 생성 시 Write 도구 사용

6. **Bash 도구 사용**: 디렉토리 생성 시 `mkdir -p` 명령 사용

7. **검증 필수**: 파일 생성 후 반드시 내용 검증

8. **친절한 피드백**: 각 단계마다 사용자에게 진행 상황 안내

9. **분석 결과 공유**: Phase 0 완료 시 발견된 내용을 요약하여 사용자에게 제시

---

**이제 스킬 생성을 시작합니다. 먼저 현재 세션의 대화를 분석하고, 추천 정보를 생성한 후, 사용자와 대화하며 스킬을 완성합니다.**
