# 🧰 Utils Skill

> Claude Code CLI에서 매일 쓰는 잡일을 줄여주는 커스텀 스킬 모음.
> 커밋 메시지, 코드 리뷰, 업무일지, GitHub 연결까지 — 키워드 한 마디로 호출.

---

## 📦 Installation

```bash
# 저장소 클론
git clone https://github.com/GangOn0215/utils-skill.git
cd utils-skill

# 전역 설치 (macOS / Linux)
cp -r skills/* ~/.claude/skills/

# 전역 설치 (Windows PowerShell)
Copy-Item -Recurse skills\* $env:USERPROFILE\.claude\skills\
```

| 범위 | macOS / Linux | Windows |
|------|---------------|---------|
| **전역** | `~/.claude/skills/` | `C:\Users\<USER>\.claude\skills\` |
| **프로젝트별** | `<project>/.claude/skills/` | `<project>\.claude\skills\` |

설치 후 Claude Code를 재시작하거나 `/skills`로 인식 여부를 확인하세요.

---

## 🛠 Skills

| Skill | 한 줄 설명 | 주요 트리거 |
|-------|----------|-------------|
| [**git-commit-helper**](skills/git-commit-helper/README.md) | `git diff` 분석 → Conventional Commit 메시지 자동 작성 | `커밋`, `commit`, `커밋 메시지 만들어줘` |
| [**php-code-review**](skills/php-code-review/README.md) | PHP 5.6 + PSR-1/2 기준 체계적 코드 리뷰 + 점수 | `리뷰해줘`, `코드 리뷰`, `PR 리뷰` |
| [**work-log-generator**](skills/work-log-generator/README.md) | git 커밋 내역 → 보고용 업무일지 자동 생성 | `업무일지`, `오늘 뭐했지`, `커밋 정리해줘` |
| [**git-remote-auto**](skills/git-remote-auto/README.md) | URL 한 줄 → 로컬을 원격에 자동 연결·푸시 | `깃허브에 올려줘`, `자동으로 올려줘` |
| [**git-remote-guided**](skills/git-remote-guided/README.md) | 단계마다 확인받으며 안전하게 GitHub 연결 | `안전하게 push`, `단계별로 깃허브` |

---

## 🤔 Which one should I use?

**커밋 관련**
- 메시지만 자동으로 만들고 싶다 → `git-commit-helper`
- 이미 만든 로컬 저장소를 GitHub에 처음 올리고 싶다 → `git-remote-auto` (URL만 주면 끝)
- 중요한 저장소라서 매 단계 확인하고 싶다 → `git-remote-guided`

**리뷰/문서 관련**
- 작성한 코드를 검수받고 싶다 (php) → `php-code-review`
- 오늘 작업한 내용을 보고용으로 정리하고 싶다 → `work-log-generator`

---

## 🗂 Repository Structure

```
utils-skill/
├── README.md                        ← 이 파일 (인덱스)
└── skills/
    ├── git-commit-helper/
    │   ├── README.md                ← 사람용 상세 문서
    │   └── SKILL.md                 ← Claude Code용 스킬 정의
    ├── php-code-review/
    │   ├── README.md
    │   └── SKILL.md
    ├── work-log-generator/
    │   ├── README.md
    │   └── SKILL.md
    ├── git-remote-auto/
    │   ├── README.md
    │   └── SKILL.md
    └── git-remote-guided/
        ├── README.md
        └── SKILL.md
```

`SKILL.md`는 Claude에게 주는 **지시문**(간결, 규칙 위주), `README.md`는 사람에게 주는 **설명**(예시·동기 위주)입니다.

---

## 🤝 Contributing

새 스킬을 추가하거나 기존 스킬을 개선하는 PR을 환영합니다.
스킬 작성 컨벤션은 [skills/git-commit-helper/SKILL.md](skills/git-commit-helper/SKILL.md)를 참고하세요.

## 📜 License

MIT
