# 서브 에이전트 사용 가능 도구 목록

서브 에이전트에서 사용할 수 있는 모든 도구와 각 도구의 설명, 용도, 보안 고려사항을 정리한 문서입니다.

---

## 📚 전체 도구 목록

| 도구 | 설명 | 주요 용도 | 보안 등급 |
|------|------|-----------|-----------|
| **Read** | 파일 읽기 | 코드 분석, 검증, 리뷰 | 🟢 안전 |
| **Write** | 파일 쓰기/생성 | 파일 생성, 내용 작성 | 🟡 주의 |
| **Edit** | 파일 편집 | 기존 파일 수정 | 🟡 주의 |
| **Bash** | 명령어 실행 | 도구 실행, 빌드, 테스트 | 🔴 위험 |
| **Grep** | 패턴 검색 | 코드 검색, 패턴 찾기 | 🟢 안전 |
| **Glob** | 파일 검색 | 파일 패턴 매칭 | 🟢 안전 |
| **WebFetch** | 웹 페이지 가져오기 | 문서 조회, 정보 수집 | 🟢 안전 |
| **WebSearch** | 웹 검색 | 정보 검색, 문서 찾기 | 🟢 안전 |

---

## 🔧 도구 상세 설명

### Read
**설명**: 파일의 내용을 읽습니다.

**사용 사례**:
- 코드 분석 및 리뷰
- 설정 파일 검증
- 문서 읽기
- 테스트 파일 확인

**보안 고려사항**:
- ✅ 읽기 전용, 안전함
- ✅ 파일 시스템 변경 없음

**권장 에이전트 타입**: 모든 타입

---

### Write
**설명**: 새로운 파일을 생성하거나 기존 파일을 완전히 덮어씁니다.

**사용 사례**:
- 새 파일 생성
- 보고서 작성
- 설정 파일 생성
- 템플릿 파일 작성

**보안 고려사항**:
- ⚠️ 기존 파일을 덮어쓸 수 있음
- ⚠️ 의도하지 않은 파일 손실 가능
- 💡 사용 전 Read로 기존 파일 확인 권장

**권장 에이전트 타입**: Specialist (포맷터), Orchestrator (워크플로우 관리)

---

### Edit
**설명**: 기존 파일의 특정 부분을 수정합니다.

**사용 사례**:
- 코드 수정
- 버전 번호 업데이트
- 설정 값 변경
- 리팩토링

**보안 고려사항**:
- ⚠️ 파일 내용 변경
- 💡 정확한 패턴 매칭 필요
- 💡 변경 전 백업 권장

**권장 에이전트 타입**: Specialist (코드 포맷터), Orchestrator (버전 관리)

---

### Bash
**설명**: 쉘 명령어를 실행합니다.

**사용 사례**:
- 테스트 실행 (npm test, pytest)
- 빌드 명령 (npm run build)
- 린터 실행 (eslint, black)
- Git 명령 (git status, git add)
- 패키지 관리 (npm install, pip install)

**보안 고려사항**:
- 🔴 임의 명령 실행 가능
- 🔴 파일 시스템 변경 가능
- 🔴 네트워크 요청 가능
- 💡 명령어 범위를 명확히 제한해야 함
- 💡 위험한 명령 (rm -rf, sudo) 실행 금지

**권장 에이전트 타입**: Specialist (도구 실행), Analyst (검증), Orchestrator (빌드/배포)

---

### Grep
**설명**: 파일 내용에서 패턴을 검색합니다.

**사용 사례**:
- 특정 코드 패턴 찾기
- 보안 취약점 검색
- TODO/FIXME 찾기
- 의존성 검색

**보안 고려사항**:
- ✅ 읽기 전용, 안전함
- ✅ 파일 시스템 변경 없음

**권장 에이전트 타입**: Analyst (코드 리뷰, 보안 감사)

---

### Glob
**설명**: 파일 경로 패턴으로 파일을 검색합니다.

**사용 사례**:
- 특정 확장자 파일 찾기 (*.ts, *.py)
- 디렉토리 구조 탐색
- 파일 목록 생성

**보안 고려사항**:
- ✅ 읽기 전용, 안전함
- ✅ 파일 시스템 변경 없음

**권장 에이전트 타입**: Analyst, Orchestrator

---

### WebFetch
**설명**: 웹 페이지의 내용을 가져옵니다.

**사용 사례**:
- 공식 문서 조회
- API 문서 확인
- 외부 정보 수집

**보안 고려사항**:
- ✅ 읽기 전용
- ⚠️ 외부 네트워크 요청
- 💡 신뢰할 수 있는 도메인만 사용 권장

**권장 에이전트 타입**: Analyst (정보 수집), Orchestrator (문서 확인)

---

### WebSearch
**설명**: 웹에서 정보를 검색합니다.

**사용 사례**:
- 최신 기술 정보 검색
- 에러 메시지 해결 방법 찾기
- 베스트 프랙티스 조회

**보안 고려사항**:
- ✅ 읽기 전용
- ⚠️ 외부 네트워크 요청
- 💡 검색 결과 검증 필요

**권장 에이전트 타입**: Analyst, Orchestrator

---

## 🔐 최소 권한 원칙

에이전트의 역할에 따라 필요한 최소한의 도구만 허용하는 것이 권장됩니다.

### 역할별 권장 도구 조합

#### 읽기 전용 분석 (코드 리뷰, 보안 감사)
```yaml
tools: Read,Grep
```
**이유**: 파일을 읽고 검색만 하므로 안전함

#### 분석 + 도구 실행 (테스트 실행, 린터)
```yaml
tools: Read,Grep,Bash
```
**이유**: 분석 후 도구를 실행하여 검증

#### 파일 수정 (포맷팅, 코드 정리)
```yaml
tools: Read,Write,Edit
```
**이유**: 파일을 읽고 수정해야 함

#### 복잡한 워크플로우 (빌드, 배포, 릴리스)
```yaml
tools: Read,Write,Bash
```
**이유**: 파일 읽기/쓰기 + 명령 실행 필요

#### 포괄적 분석 (종합 리뷰)
```yaml
tools: Read,Grep,Bash
```
**이유**: 코드 분석 + 테스트/린터 실행

---

## ⚠️ 타입별 도구 적합성 검증

### Specialist (단일 작업 전문)

**권장 도구**:
- Formatting: `Read,Write,Bash`
- Validation: `Read,Bash`
- Link Checking: `Read,Bash,WebFetch`

**경고 발생 시나리오**:
```yaml
# ❌ 포맷터에 Grep은 불필요
specialist_formatter:
  tools: Read,Write,Bash,Grep  # Grep 경고
  reason: "포맷팅에는 검색 불필요"

# ✅ 적절한 도구 조합
specialist_formatter:
  tools: Read,Write,Bash
```

---

### Analyst (분석 및 리뷰)

**권장 도구**:
- Code Review: `Read,Grep,Bash`
- Security Audit: `Read,Grep`
- Performance Analysis: `Read,Grep,Bash`

**경고 발생 시나리오**:
```yaml
# ❌ 분석 작업에 Write는 부적절 (읽기 전용이어야 함)
analyst_reviewer:
  tools: Read,Grep,Write  # Write 경고
  reason: "분석은 읽기 전용이어야 함"

# ✅ 적절한 도구 조합
analyst_reviewer:
  tools: Read,Grep,Bash
```

---

### Orchestrator (복잡한 워크플로우)

**권장 도구**:
- Deployment: `Read,Write,Bash`
- Release Management: `Read,Write,Edit,Bash`
- CI/CD Pipeline: `Read,Write,Bash,Grep`

**경고 발생 시나리오**:
```yaml
# ⚠️ 모든 도구 허용은 권장하지 않음
orchestrator_release:
  tools: "*"  # 모든 도구 - 과도한 권한
  reason: "필요한 도구만 명시해야 함"

# ✅ 적절한 도구 조합
orchestrator_release:
  tools: Read,Write,Bash
```

---

## 🛡️ 보안 체크리스트

에이전트 생성 시 다음 사항을 확인하세요:

- [ ] **최소 권한**: 역할에 필요한 최소한의 도구만 허용했는가?
- [ ] **읽기 전용**: 분석 작업은 Read, Grep만 사용하는가?
- [ ] **Write/Edit 정당성**: 파일 수정이 정말 필요한가?
- [ ] **Bash 명령 범위**: 실행할 명령어가 명확히 정의되어 있는가?
- [ ] **위험 명령 금지**: rm -rf, sudo 등 위험한 명령을 사용하지 않는가?
- [ ] **모든 도구 허용 회피**: tools: "*" 사용을 피했는가?

---

## 📖 참고 자료

- **검증 기준**: `validation-criteria.md`
- **타입 시스템**: `type-system.md`
- **베스트 프랙티스**: `best-practices.md`
- **템플릿**: `templates/` 디렉토리
