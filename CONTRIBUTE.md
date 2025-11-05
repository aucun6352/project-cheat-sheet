# 기여 가이드

project-cheat-sheet 프로젝트에 기여해주셔서 감사합니다!

## 🚀 시작하기

```bash
# Fork 후 Clone
git clone https://github.com/YOUR_USERNAME/project-cheat-sheet.git
cd project-cheat-sheet

# 브랜치 생성
git checkout -b feature/your-feature-name
```

## 🤝 기여 방법

### 플러그인 추가/수정

`plugins/` 디렉토리의 플러그인을 추가하거나 수정할 수 있습니다.

**Creator Suite 사용 권장 (선택사항)**

강제는 아니지만, 플러그인에 agent/command/skill을 추가할 때 **Creator Suite를 사용하는 것을 권장합니다**:

```bash
/create-agent    # 플러그인에 Agent 추가
/create-command  # 플러그인에 Command 추가
/create-skill    # 플러그인에 Skill 추가
```

Creator Suite를 사용하면:
- ✅ 자동 검증으로 품질 보장
- ✅ 일관된 구조 유지
- ✅ 빠르고 정확한 생성

**Creator Suite 개선 지향**

Creator Suite를 사용하면서 발견한 개선점이 있다면:
- Creator Suite 자체를 개선하는 PR 환영
- 버그 수정이나 기능 개선 제안
- 더 나은 자동화 아이디어 공유

직접 작성도 가능하지만, Creator Suite를 통한 기여가 프로젝트의 품질과 일관성 유지에 도움이 됩니다.


## 💡 도움말

- 📖 [Creator Suite README](plugins/creator-suite/README.md)
- 🐛 [이슈 제출](https://github.com/aucun6352/project-cheat-sheet/issues)

---

**모든 기여에 감사드립니다! 🚀**