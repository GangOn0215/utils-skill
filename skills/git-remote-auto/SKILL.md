---
name: git-remote-auto
# prettier-ignore
description: GitHub 저장소 URL 하나만 주면 로컬 디렉토리를 자동으로 원격에 연결·커밋·푸시까지 처리합니다. 사용자가 "깃허브에 올려줘", "자동으로 올려줘", "remote 연결", "github push", "이 폴더 깃허브에", "url 던질테니 알아서"라고 하면 이 스킬을 사용합니다.
---

# Git Remote Auto

GitHub 저장소 URL 하나만 받으면 로컬 디렉토리를 원격에 **자동으로** 연결·푸시합니다.
원격 상태를 먼저 점검한 뒤 상황에 맞는 분기를 따라 무중단으로 처리하며, **데이터 손실 위험이 있을 때만** 사용자에게 확인을 요청합니다.

## 워크플로우

1. URL 유효성 검증 (`https://github.com/...` 또는 `git@github.com:...`)
2. 로컬 git 상태 확인 (`git rev-parse --is-inside-work-tree`)
3. 원격 상태 확인 (`git ls-remote <URL>`)
4. 아래 분기 시나리오 중 하나로 자동 실행
5. 푸시 후 커밋 해시와 저장소 URL 보고

## 분기 시나리오

### A. 로컬 = git 저장소 아님 + 원격 = 비어있음
가장 단순한 경우. 바로 자동 실행:
- `git init`
- `git branch -M main`
- `git remote add origin <URL>`
- 필요 시 기본 `.gitignore` 생성 (node_modules, vendor, __pycache__, .env 등)
- `git add .` → 커밋 → `git push -u origin main`

### B. 로컬 = git 저장소 아님 + 원격 = 커밋 있음 (LICENSE/README 등)
원격 파일 보존하며 진행:
- `git init` → `git branch -M main` → `git remote add origin <URL>`
- `git fetch origin main`
- `git reset --soft FETCH_HEAD`
- 원격에만 있는 파일은 `git checkout origin/main -- <파일>` 로 복원
- 로컬 신규 파일 `git add` → 커밋 → `git push`

### C. 로컬 = 이미 git 저장소 + 원격 미설정
- `git remote add origin <URL>`
- 원격 상태 확인 후 A 또는 B의 후속 단계 진행

### D. 로컬 = 이미 git 저장소 + 다른 원격이 설정됨
**자동 진행 금지.** 기존 remote를 덮어쓰면 사용자의 다른 워크플로우가 깨질 수 있음.
- 현재 remote URL을 보고하고 "교체 / 다른 이름으로 추가 / 중단" 확인

### E. 원격에 동일 경로의 파일이 이미 존재 (충돌 위험)
**자동 진행 금지.** diff를 보여주고 "덮어쓰기 / 병합 / 중단" 확인

## 안전 규칙 (반드시 준수)

- `git push --force`, `git reset --hard`는 절대 자동 실행 금지
- 사용자 커밋이 이미 있는 브랜치에서는 `branch -M` 으로 이름 변경 금지
- `.env`, `*.key`, `*.pem`, `credentials.json`, `id_rsa` 등 민감 파일이 포함되면 푸시 전 경고하고 중단
- `node_modules/`, `vendor/`, `__pycache__/`, `dist/`, `build/` 등은 `.gitignore`에 자동 추가
- 커밋 메시지는 `[[git-commit-helper]]` 규칙과 동일한 형식: `[YY.MM.DD] {이모지} {타입}: {설명}`
- 최초 커밋은 `[YY.MM.DD] 🎉 Initial commit` 형식

## 출력 형식

```
✅ 원격 연결 완료
저장소: https://github.com/user/repo
브랜치: main
커밋: abc1234
시나리오: B (원격 LICENSE 보존 후 신규 파일 추가)
```

## 관련 스킬

- 더 신중하게 단계별 확인을 받고 싶으면 `[[git-remote-guided]]` 사용
- 커밋 메시지만 별도로 생성하려면 `[[git-commit-helper]]` 사용
