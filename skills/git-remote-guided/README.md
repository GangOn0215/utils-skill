# 🛡️ git-remote-guided

> GitHub 저장소 URL을 받아 **각 단계마다 사용자 확인**을 거치며 원격 연결·푸시를 진행합니다.
> 중요한 저장소를 다룰 때, 흐름을 통제하고 싶을 때 사용.

[← 메인 README로 돌아가기](../../README.md)

---

## 🚀 사용법

```
확인하면서 올려줘
단계별로 깃허브
안전하게 push
step by step github
물어보면서 진행
신중하게 올려줘
```

이어서 GitHub 저장소 URL을 입력하면 됩니다.

```
"https://github.com/회사/중요한레포.git 확인하면서 올려줘"
```

---

## ✨ 주요 기능

- **단계별 사전 보고** — 매 git 명령 **실행 전**에 명령어와 예상 결과를 보여줌
- **시나리오 선택권** — Claude가 판단한 분기를 사용자에게 후보로 제시
- **모든 자동화 가시화** — `.gitignore` 자동 추가, `reset --soft` 등 "조용한" 작업도 보고
- **일괄 승인 옵션** — 사용자가 한 번 "전체 OK" 하면 위험 상황 외엔 일괄 진행

---

## 📋 확인 받는 시점

- `git init` 직전
- `git remote add` 직전
- 스테이징 직전 (포함 파일 목록 표시)
- 커밋 직전 (메시지 검토)
- 푸시 직전 (브랜치/원격 URL 최종 확인)
- `fetch` + `reset --soft`가 필요할 때 (원격 파일 보존 케이스)
- 기존 remote가 다른 URL일 때
- `.gitignore` 자동 생성/수정이 필요할 때

---

## 💡 예시

**입력:**
> "https://github.com/company/secret-tool.git 안전하게 push"

**Claude 출력 (각 단계마다):**

```
📋 1/7 단계: 상태 점검
- 로컬: git 저장소 아님
- 원격: 커밋 3개 (LICENSE, README.md, .gitignore 있음)
- 적용 시나리오: B (원격 파일 보존)
계속 진행할까요? (y/n)
```

```
📋 2/7 단계: git init
명령어: git init && git branch -M main
영향: .git 폴더 생성, 기본 브랜치 main
계속 진행할까요? (y/n)
```

```
📋 5/7 단계: 스테이징 검토
포함 파일:
  + src/main.py
  + src/utils.py
  + tests/test_main.py
제외 (.gitignore에 의해):
  - .env
  - __pycache__/
계속 진행할까요? (y/n)
```

```
📋 7/7 단계: 푸시
원격: https://github.com/company/secret-tool.git
브랜치: main
커밋: 9a1b2c3
이대로 푸시할까요? (y/n)
```

**최종:**
```
✅ 원격 연결 완료
저장소: https://github.com/company/secret-tool
브랜치: main
커밋: 9a1b2c3
적용 시나리오: B (원격 파일 보존)
실행한 단계: 7개 / 사용자 승인: 7개
```

---

## 🆚 git-remote-auto 와의 차이

| | git-remote-guided | git-remote-auto |
|---|---|---|
| 속도 | 🐢 느림 (단계별) | ⚡ 빠름 (한 번에) |
| 확인 빈도 | 매 단계 | 위험 상황만 |
| 권장 상황 | 회사 저장소, 중요한 코드 | 새 프로젝트, 개인 저장소 |

## 🔗 관련 스킬

- [git-remote-auto](../git-remote-auto/README.md) — 빠른 자동 처리가 필요할 때
- [git-commit-helper](../git-commit-helper/README.md) — 커밋 메시지 규칙 공유
