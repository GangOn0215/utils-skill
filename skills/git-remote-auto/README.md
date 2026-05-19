# 🚀 git-remote-auto

> GitHub 저장소 URL **하나만** 던지면 로컬 디렉토리를 자동으로 원격에 연결·커밋·푸시까지 처리합니다.
> 데이터 손실 위험이 있을 때만 확인을 요청.

[← 메인 README로 돌아가기](../../README.md)

---

## 🚀 사용법

```
깃허브에 올려줘
자동으로 올려줘
remote 연결
github push
이 폴더 깃허브에
url 던질테니 알아서
```

이어서 GitHub 저장소 URL을 입력하면 됩니다.

```
"https://github.com/user/my-repo.git 여기에 자동으로 올려줘"
```

---

## ✨ 주요 기능

- **상태 자동 점검** — 로컬이 git 저장소인지, 원격에 커밋이 있는지 먼저 확인
- **5가지 분기 시나리오** — 상황에 맞춰 자동 분기
- **원격 파일 보존** — 원격에 LICENSE/README가 있어도 덮어쓰지 않음
- **민감 파일 차단** — `.env`, `*.key` 등이 포함되면 푸시 전 경고
- **자동 `.gitignore`** — `node_modules`, `vendor`, `__pycache__` 등 추가

---

## 🔀 분기 시나리오

| 상황 | 자동 처리 | 사용자 확인 |
|------|-----------|------------|
| **A.** 로컬 비-git + 원격 비어있음 | ✅ 전 단계 자동 | — |
| **B.** 로컬 비-git + 원격에 커밋 있음 | ✅ `fetch` + `reset --soft`로 원격 파일 보존 | — |
| **C.** 이미 git 저장소 + 원격 미설정 | ✅ remote 추가 후 A/B 진행 | — |
| **D.** 다른 원격이 이미 설정됨 | ❌ | "교체 / 다른 이름 추가 / 중단" |
| **E.** 원격에 같은 경로 파일이 있음 | ❌ | "덮어쓰기 / 병합 / 중단" |

---

## 💡 예시

**입력:**
> "https://github.com/user/my-tools.git 여기에 자동으로 올려줘"

**Claude 실행 (시나리오 B 자동 감지):**
```bash
git init
git branch -M main
git remote add origin https://github.com/user/my-tools.git
git fetch origin main
git reset --soft FETCH_HEAD
git checkout origin/main -- LICENSE
git add .
git commit -m "[26.05.19] 🎉 Initial commit"
git push -u origin main
```

**출력:**
```
✅ 원격 연결 완료
저장소: https://github.com/user/my-tools
브랜치: main
커밋: 7c9443d
시나리오: B (원격 LICENSE 보존 후 신규 파일 추가)
```

---

## 🛡️ 안전 규칙

- `git push --force` / `git reset --hard` **절대 자동 실행 금지**
- `.env`, `*.key`, `*.pem`, `credentials.json`, `id_rsa` 감지 시 **푸시 차단**
- 이미 커밋이 있는 브랜치에서 `branch -M` 으로 이름 변경 금지
- 커밋 메시지는 `git-commit-helper`와 동일한 형식 (`[YY.MM.DD] {이모지} {타입}: {설명}`)

---

## 🆚 git-remote-guided 와의 차이

| | git-remote-auto | git-remote-guided |
|---|---|---|
| 속도 | ⚡ 빠름 (한 번에) | 🐢 느림 (단계별) |
| 확인 빈도 | 위험 상황(D/E)만 | 매 단계 |
| 권장 상황 | 새 프로젝트, 개인 저장소 | 회사 저장소, 중요한 코드 |

## 🔗 관련 스킬

- [git-remote-guided](../git-remote-guided/README.md) — 단계별 확인이 필요할 때
- [git-commit-helper](../git-commit-helper/README.md) — 커밋 메시지 규칙 공유
