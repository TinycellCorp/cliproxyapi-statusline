# cliproxy-statusline

Display cliproxyapi proxy multi-account quota usage in Claude Code statusline.

cliproxyapi 프록시 서버의 다중 계정 쿼터 사용량을 Claude Code 상태라인에 표시하는 스킬입니다.

## Installation

### Project scope

```bash
npx skills add TinycellCorp/cliproxyapi-statusline
```

### Global scope

```bash
npx skills add TinycellCorp/cliproxyapi-statusline -g
```

## Setup

설치 후 스킬을 호출하면 설정 흐름이 시작됩니다:

1. cliproxyapi 프록시 서버 URL 입력
2. 관리 API 키(management key) 입력

설정은 `~/.cliproxy-statusline.json`에 저장됩니다.

수동 설정:

```bash
echo '{"proxyUrl":"http://localhost:3000","managementKey":"YOUR_KEY"}' > ~/.cliproxy-statusline.json
```

## Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code)
- Node.js 18+
- cliproxyapi 프록시 서버 접근 권한

## How it works

이 스킬은 Claude Code의 `statusLine.command` 스펙을 활용하여 프록시 쿼터 사용량을 표시합니다.

```
Q1 5h:[████░░░░]52%(1h17m) wk:[█░░░░░░░]7%(6d20h)
Q2 5h:[██████░░]78%(0h43m) wk:[████░░░░]52%(3d12h)
```

oh-my-claudecode(OMC) HUD를 사용하는 경우, HUD 출력에 쿼터 라인을 통합할 수도 있습니다 (선택 사항).

## Repository Structure

```
cliproxyapi-statusline/    ← repo root = skill directory
├── SKILL.md               ← name: cliproxyapi-statusline
├── references/omc-hud.md
├── README.md
├── LICENSE
└── .gitignore
```

Built on the [Agent Skills](https://agentskills.io/specification) open standard.

## License

MIT
