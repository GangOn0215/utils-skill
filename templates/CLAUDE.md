# Claude Code 공통 지침 (Project-Agnostic)

> 이 파일은 모든 프로젝트에 공통 적용되는 Claude Code 지침입니다.
> 프로젝트별 특수 규칙은 **§9 프로젝트별 커스터마이즈** 섹션에 추가하세요.

Claude는 모든 작업 시 이 파일의 내용을 최우선으로 준수해야 합니다.

---

## 0. 문서 디렉터리 구조 (필수 숙지)

업무 관련 문서는 모두 `docs/work/` 아래의 3개 폴더로 관리합니다.

```
docs/work/
├── 📓 daily_record/         ← 활동 일지: 그날 무엇을 "했는가"
│   └── YYYY_MM_DD.md
├── 📅 daily_todo_record/    ← 날짜별 TODO 스냅샷: 그날 무엇을 "할 것인가" + 진행률
│   └── YYYY_MM_DD.md
└── 🎯 todo/                 ← 주제별 장기 TODO (날짜 무관, 주제 종료 시까지 유지)
    └── {topic}.md
```

### 폴더별 역할 분담

| 폴더 | 시점 | 단위 | 수명 |
|---|---|---|---|
| `daily_record` | 작업 후 | 날짜 | 영구 (활동 기록) |
| `daily_todo_record` | 작업 전·중 | 날짜 | 영구 (체크박스 완료 상태 그대로 보존) |
| `todo` | 수시 | 주제 | 주제 종료 시까지 |

### `todo/{topic}.md` 본문 포맷

한 파일에 한 주제. 주제 안에서 여러 하위 항목을 `##` 으로 나누고, 각 항목은 `### 📐 계획` / `### 💻 코드` / `### 🧪 테스트` 3섹션으로 구성합니다. 이모지로 꾸미면 가독성↑.

```markdown
# 🎯 {주제명}

## {하위 항목 1}

### 📐 계획
- [ ] ...

### 💻 코드
- [ ] ...

### 🧪 테스트
- [ ] ...
```

### `daily_todo_record/YYYY_MM_DD.md` 본문 포맷

`todo/{topic}.md` 와 동일한 3섹션 구조(계획/코드/테스트). 단, **그날 처리할 단발성 작업** 단위로 묶음. 완료 항목은 `- [x] ... (YYYY-MM-DD HH:MM)` 형식으로 시각 기록.

---

## 1. 시작 루틴 (필수)

새로운 세션을 시작할 때, 다음 파일들을 순서대로 확인하고 **사용자에게 한 줄로 브리핑**하십시오.

1. **오늘의 TODO 스냅샷 (`docs/work/daily_todo_record/YYYY_MM_DD.md`)**
   - 오늘 날짜 파일이 있으면 진행 중/대기 항목을 파악
   - 없으면 `docs/work/daily_todo_record/` 내 가장 최근 파일을 참고해 미완료 인계 여부 결정
2. **주제별 장기 TODO (`docs/work/todo/*.md`)**
   - 진행 중인 주제 파일을 훑고, 오늘 작업과 연관된 항목이 있는지 확인
3. **오늘의 활동 일지 (`docs/work/daily_record/YYYY_MM_DD.md`)**
   - 있으면 현재 진행 상황 파악, 없으면 새로 생성할 준비
4. **`memory/MEMORY.md`** (있는 경우)
   - 이전 세션의 피드백, 사용자 선호, 프로젝트 컨텍스트 숙지
5. **`CLAUDE.md`** (이 파일)
   - 본 규칙 + 프로젝트별 오버라이드 재확인

> **`docs/work/` 구조가 없는 프로젝트라면** 위 1~3번은 건너뛰고 사용자에게 "해당 구조를 사용하시는지" 한 번 확인 후 진행하십시오.

### 파일 컨벤션
- 단발성 신규 TODO 항목은 항상 `docs/work/daily_todo_record/YYYY_MM_DD.md` (오늘 날짜 파일)에 기록
- 여러 날에 걸치는 주제성 작업은 `docs/work/todo/{topic}.md` 에 누적하고, 그날 다룬 항목은 `daily_todo_record` 에서 `> 관련: docs/work/todo/{topic}.md` 로 cross-reference
- 루트의 `TODO.md` 가 있는 경우 → **legacy 백로그(아카이브)** 로 간주, 신규 항목 추가 금지

---

## 2. 작업 완료 후 기록 및 알림 (필수)

모든 주요 작업이 완료되면 다음 절차를 순서대로 수행하십시오.

1. **로그 업데이트** — `docs/work/daily_record/YYYY_MM_DD.md` 에 다음을 누적 기록:
   - 사용자가 지시한 작업 내용
   - TODO 진행 상황(Done)
   - 변경된 파일 목록
   - 다음 계획(Plan)
   - 각 Task 제목 옆에 완료 시각을 반드시 기록 → 형식: `### ✅ [Task N] - 제목 (HH:MM)`
   - "업데이트 해줘" 요청을 기다리지 말고, 주요 작업이 끝나는 즉시 자동 기록
2. **현재 시각 확인** (필요 시):
   ```powershell
   Get-Date -Format 'HH:mm'
   ```
3. **Sound Notification** (Windows 환경 한정 — Mac/Linux 사용자는 비활성화):
   ```powershell
   (New-Object Media.SoundPlayer 'C:\Windows\Media\tada.wav').PlaySync()
   ```

---

## 3. 코딩 컨벤션 (Project-Driven)

> **기본 원칙: 프로젝트의 기존 코드 컨벤션을 따른다.**

새로운 파일/함수를 작성하기 전에 **이웃 파일을 먼저 읽고** 동일한 패턴을 적용하십시오.

- 들여쓰기 (스페이스 vs 탭, 개수)
- 네이밍 (camelCase / snake_case / PascalCase)
- 따옴표 (single vs double)
- 세미콜론, 트레일링 콤마, import 정렬
- 폴더 구조 및 파일명 컨벤션

**프로젝트가 명시적 컨벤션을 가진 경우** (예: `.editorconfig`, `.prettierrc`, `.eslintrc`, `phpcs.xml`)는 무조건 그 설정을 따르십시오.

**컨벤션이 불분명할 때만** 아래 일반 가이드를 폴백으로 사용:
- 들여쓰기 4 spaces, LF, UTF-8 (BOM 없음)
- 함수/메서드는 단일 책임
- 의미 없는 주석은 작성 금지 (이름이 설명하지 못하는 "왜"만 주석으로)

---

## 4. 함수 설계 원칙 (언어 무관)

- **1 함수 = 1 기능** — 한 함수가 여러 역할을 하지 않도록 엄수
- 인자에 `$params` / `options` 배열로 여러 책임을 욱여넣지 말 것
- 함수 반환은 명시적으로 (체이닝 직접 반환 금지, 변수에 담아 반환 권장)
- 페이지 번호/범위 처리는 명시적인 조건문으로 작성 (`max(1, ...)` 같은 트릭 지양)

### 트랜잭션이 필요한 경우 (DB/파일/외부 API 다중 작업)
- 여러 자원을 한 번에 변경하는 함수는 반드시 트랜잭션 또는 동등한 롤백 메커니즘으로 묶을 것
- 프레임워크가 제공하는 고수준 API (`trans_start`/`trans_complete`, `with transaction:`, `db.transaction(...)`)를 우선 사용
- 수동 commit/rollback은 가능한 한 피하고, 불가피한 경우에만 사용

---

## 5. Git 워크플로우

- 커밋 메시지: [utils-skill](https://github.com/GangOn0215/utils-skill) 의 **git-commit-helper** 스킬 형식을 따른다
  - `[YY.MM.DD] {이모지} {타입}: {설명}`
  - 성격이 다른 변경은 반드시 Atomic 커밋으로 분리
- **사용자 명시 요청 없이 commit/push 금지**
- `git push --force`, `git reset --hard`, `git clean -fd` 같은 파괴적 명령은 **반드시 사용자 확인**
- `.env`, `*.key`, `*.pem`, `credentials.json` 같은 민감 파일은 절대 스테이징/커밋 금지

---

## 6. 안전 규칙

- 의문이 있을 때는 실행하지 말고 **사용자에게 묻는다**
- 외부에 영향이 있는 작업(push, deploy, 외부 API, 메시지 전송)은 사전 확인 필수
- 대용량 파일 / 생성 파일(`*.sql`, 빌드 산출물, 로그) 은 diff/read 대신 `--stat` / `head -N` 으로 제한적으로 확인
- 사용자 데이터 디렉토리(예: `uploads/`, `backups/`) 의 파일은 임의 삭제/이동 금지

---

## 7. 업무 일지 형식 (옵션)

프로젝트가 `docs/work-business-journal/` 구조를 사용하는 경우, 두 가지 버전으로 작성한다.

- `YYYY_MM_DD_dev.md` — 개발자용 (기술 용어, 변경 파일 목록 포함)
- `YYYY_MM_DD_general.md` — 일반용 (비개발자도 이해할 수 있는 간결한 설명)

### general 버전 섹션 형식

```
## [ {프로젝트명} ] - 신규 기능
▷ 항목

## [ {프로젝트명} ] - 기능 수정
▷ 항목

---

## [ 요약 ]
▷ 한두 문장으로 전체 요약
```

- 섹션 제목의 `{프로젝트명}` 은 §9 프로젝트별 커스터마이즈에 정의된 이름으로 교체
- 신규 기능이 없으면 해당 섹션 생략, 수정만 있으면 수정 섹션만 작성
- 마지막에 `[ 요약 ]` 섹션으로 한두 문장 마무리

> 자동화: [utils-skill](https://github.com/GangOn0215/utils-skill) 의 **work-log-generator** 스킬로 자동 생성 가능

---

## 8. 추천 스킬 (utils-skill)

이 지침과 잘 맞물려 동작하는 외부 스킬:

| 스킬 | 용도 |
|------|------|
| `git-commit-helper` | §5 형식의 커밋 메시지 자동 생성 |
| `work-log-generator` | §7 형식의 업무일지 자동 생성 |
| `php-code-review` | PHP 프로젝트 코드 리뷰 (해당 시) |
| `git-remote-auto` / `git-remote-guided` | GitHub 원격 자동 연결·푸시 |

설치: https://github.com/GangOn0215/utils-skill

---

## 9. 프로젝트별 커스터마이즈

> **이 섹션은 프로젝트마다 다릅니다.** 새 프로젝트에 이 CLAUDE.md를 복사한 뒤 아래 항목을 채워 넣으십시오.

### 프로젝트 정보
- **프로젝트명**: (예: BM 마켓 / 인테리어 플랫폼 / ...)
- **기술 스택**: (예: PHP 5.6 + CodeIgniter 3 / Next.js 14 + Supabase / ...)
- **주요 디렉토리**:
  - 컨트롤러: `...`
  - 모델/도메인: `...`
  - 뷰/컴포넌트: `...`
  - 정적 자원: `...`

### 프로젝트 특수 규칙
<!-- 예시:
- 파일 업로드 경로: `./uploads/estimate/request/`
- 모든 업로드 이미지는 `thumb/` 하위에 동일 이름으로 썸네일 자동 생성
- DB 작업은 반드시 Model 내부에서만 수행 (Strict MVC)
- 컨트롤러 파일명은 소문자, 클래스명은 PascalCase
- 뷰 파일명: {폴더명}_list.php / {폴더명}_view.php / {폴더명}_form.php
- JS: var 금지, let/const 만 사용, 템플릿 리터럴 우선
-->

### 도메인 상태 코드 / 용어 정의
<!-- 예시:
- 주문 상태: 0=접수 / 1=검토 / 2=진행 / 3=완료대기 / 4=완료 / -1=취소
- 사용자 권한: customer / partner / admin
-->

### 현재 진행 상황 (선택)
<!-- 큰 마일스톤만 짧게. 상세 TODO는 docs/todo/ 일자별 파일을 참조 -->

### 외부 참조
- 비즈니스 로직: `memory/*.md` (있는 경우)
- API 명세: `...`
- 디자인 시스템: `...`

---

*새로운 규칙이나 작업 계획이 추가될 경우 §9에 즉시 업데이트한다. §1~§8은 모든 프로젝트 공통이므로 함부로 수정하지 않는다.*
