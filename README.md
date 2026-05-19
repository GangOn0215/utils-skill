# Claude Code Skills

Claude Code CLI에서 사용하는 커스텀 스킬 모음입니다.

## Skills

### 1. 🔖 Git Commit Helper

> `git diff`를 분석해서 **Conventional Commit 메시지**를 자동으로 생성합니다.

**트리거 키워드:** `커밋 메시지 만들어줘`, `커밋`, `commit`, `git commit`, `커밋 좀`

**주요 기능**
- Staged / Unstaged 변경사항 자동 감지
- 성격이 다른 변경사항은 Atomic 커밋으로 분리 제안
- `[YY.MM.DD] {이모지} {타입}: {설명}` 형식으로 제목 작성
- 변경 동기와 부작용을 본문에 포함

**커밋 타입**

| 타입 | 이모지 | 설명 |
|------|--------|------|
| Feat | ✨ | 새로운 기능 추가 |
| Fix | 🐛 | 버그 수정 |
| Refactor | ♻️ | 코드 리팩토링 |
| Chore | 🚚 | 빌드, 패키지, 기타 수정 |
| Docs | 📝 | 문서 수정 |
| Style | 💄 | 포맷팅, UI CSS 변경 |
| Test | ✅ | 테스트 코드 추가/수정 |
| Perf | ⚡️ | 성능 향상 |
| CI | 👷 | CI 설정 수정 |

**예시**
```
[26.05.19] ✨ Feat: 카카오 소셜 로그인 연동

- OAuth 2.0 기반 카카오 로그인 플로우 구현
- 신규 가입 시 기본 프로필 자동 생성
```

---

### 2. 🔍 PHP Code Review (`code-review`)

> 코드를 **체계적인 체크리스트**로 리뷰하고 점수를 매겨줍니다. (PHP 5.6 + PSR-1/PSR-2 기준)

> ℹ️ 폴더명은 `php-code-review`이지만 실제 스킬 이름은 `code-review`입니다.

**트리거 키워드:** `리뷰해줘`, `코드 리뷰`, `PR 리뷰`, `check this code`, `이 코드 좀 봐줘`

**리뷰 항목 (우선순위 순)**
1. **코딩 스타일** — PHP 5.6 + PSR-1/PSR-2 준수 여부 (들여쓰기, 네이밍, DocBlock 등)
2. **보안** — SQL 인젝션, XSS, CSRF, 파일 업로드 취약점
3. **성능** — N+1 쿼리, 불필요한 연산, 메모리 누수
4. **에러 처리** — try-catch, null 체크, 엣지 케이스 대응
5. **테스트 커버리지** — 단위/통합 테스트 누락 여부
6. **브레이킹 체인지** — 기존 코드 영향 범위

**출력 형식**
```
✅ 잘한 점
⚠️ 개선 제안 (코드 패치 포함)
❌ 치명적 문제 (반드시 수정)
   종합 점수: N/10
```

---

### 3. 📋 Work Log Generator

> Git 커밋 내역을 분석해서 **보고용 업무일지**를 자동으로 작성합니다.

**트리거 키워드:** `업무일지`, `일지 써줘`, `work log`, `오늘 뭐했지`, `커밋 정리해줘`, `업무 정리`

**주요 기능**
- 날짜 미지정 시 오늘 날짜 자동 사용
- 커밋 메시지 + `--stat` 기반으로 토큰 최소화하며 분석
- 신규 기능(추가) / 기능 수정으로 자동 분류
- 기술 용어에 괄호 보충 설명 추가 (일반인도 이해 가능한 수준)

**출력 형식**
```
## [ 프로젝트명 ] - 신규 기능
▷ 대분류 제목
  - 소분류 상세 설명

## [ 프로젝트명 ] - 기능 수정
▷ 대분류 제목
  - 소분류 상세 설명

---

## [ 요약 ]
▷ 전체 작업을 한두 문장으로 요약
```

---

## 설치 방법

각 스킬 폴더를 Claude Code의 스킬 디렉토리에 복사하면 됩니다.

### 설치 경로

| 범위 | macOS / Linux | Windows |
|------|---------------|---------|
| **전역** (모든 프로젝트) | `~/.claude/skills/` | `C:\Users\<USER>\.claude\skills\` |
| **프로젝트별** | `<project>/.claude/skills/` | `<project>\.claude\skills\` |

### 설치 예시

```bash
# 저장소 클론
git clone https://github.com/<your-username>/claude.git
cd claude

# 전역으로 설치 (macOS / Linux)
cp -r skills/* ~/.claude/skills/

# 전역으로 설치 (Windows PowerShell)
Copy-Item -Recurse skills\* $env:USERPROFILE\.claude\skills\
```

### 디렉토리 구조

```
skills/
├── git-commit-helper/
│   └── SKILL.md
├── php-code-review/      # 실제 스킬 이름: code-review
│   └── SKILL.md
└── work-log-generator/
    └── SKILL.md
```

설치 후 Claude Code를 재시작하거나 `/skills` 명령으로 스킬이 정상 인식되는지 확인하세요.

## 라이선스

MIT
