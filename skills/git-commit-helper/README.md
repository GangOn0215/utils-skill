# 🔖 git-commit-helper

> `git diff`를 분석해서 **이모지 + Conventional Commit** 형식의 커밋 메시지를 자동 생성합니다.
> 성격이 다른 변경은 **Atomic 커밋**으로 분리 제안.

[← 메인 README로 돌아가기](../../README.md)

---

## 🚀 사용법

Claude Code에서 다음 키워드 중 하나로 호출:

```
커밋 메시지 만들어줘
커밋
commit
git commit
커밋 좀
```

---

## ✨ 주요 기능

- **Staged / Unstaged 자동 감지** — `git diff --cached` 우선, 없으면 `git diff`
- **Atomic 커밋 분리** — 성격이 다른 변경을 한 커밋에 묶지 않고 단위로 쪼개서 제안
- **날짜 자동 삽입** — 제목 앞에 `[YY.MM.DD]` 형식의 오늘 날짜
- **이모지 + 타입** — 9가지 타입(Feat/Fix/Refactor/Chore/Docs/Style/Test/Perf/CI)별 지정 이모지

---

## 📐 커밋 메시지 형식

```
[YY.MM.DD] {이모지} {타입}: {설명}

{본문: 왜 바꿨는지, 어떤 영향이 있는지}

{footer: Closes #123 등 — 있으면}
```

### 타입 일람

| 타입 | 이모지 | 용도 | 예시 |
|------|--------|------|------|
| `Feat` | ✨ | 새 기능 | `[26.05.19] ✨ Feat: 카카오 로그인 연동` |
| `Fix` | 🐛 | 버그 수정 | `[26.05.19] 🐛 Fix: 파트너 정보 컨트롤러 버그 수정` |
| `Refactor` | ♻️ | 리팩토링 | `[26.05.19] ♻️ Refactor: 회원가입 로직 함수 분리` |
| `Chore` | 🚚 | 빌드/패키지 | `[26.05.19] 🚚 Chore: 웹팩 설정 오타 수정` |
| `Docs` | 📝 | 문서 | `[26.05.19] 📝 Docs: README 사용법 추가` |
| `Style` | 💄 | 포맷팅/UI | `[26.05.19] 💄 Style: 모달 버튼 중앙 정렬` |
| `Test` | ✅ | 테스트 | `[26.05.19] ✅ Test: 로그인 실패 케이스 추가` |
| `Perf` | ⚡️ | 성능 | `[26.05.19] ⚡️ Perf: 쿼리 인덱스 추가` |
| `CI` | 👷 | CI/CD | `[26.05.19] 👷 CI: 깃헙 액션 워크플로우 추가` |

---

## 💡 예시

**입력 (사용자):**
> "커밋 메시지 만들어줘"

**Claude가 분석한 diff:**
- `src/auth/kakao.php` (신규 파일, 카카오 OAuth 핸들러)
- `src/auth/social.php` (수정, 공통 소셜 로그인 인터페이스 추출)
- `README.md` (수정, 카카오 로그인 사용법 추가)

**출력 (Atomic 커밋 2개로 분리 제안):**

```
[26.05.19] ✨ Feat: 카카오 소셜 로그인 연동

- OAuth 2.0 기반 카카오 로그인 플로우 구현
- 공통 소셜 로그인 인터페이스 추출 (Naver/Google 확장 대비)
- 신규 가입 시 기본 프로필 자동 생성
```

```
[26.05.19] 📝 Docs: README 카카오 로그인 사용법 추가
```

---

## ⚠️ 규칙 (반드시 준수)

- 제목 끝에 마침표 금지
- 타입 첫 글자는 대문자 (`Feat`, `Fix`, `Refactor`)
- 제목 최대 72자
- 성격이 다른 변경은 반드시 분리

## 🔗 관련 스킬

- [git-remote-auto](../git-remote-auto/README.md) — 커밋 후 GitHub에 자동 푸시
- [work-log-generator](../work-log-generator/README.md) — 이 형식의 커밋을 모아 업무일지 작성
