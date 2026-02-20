# OMC HUD 연동 (선택 사항 -- OMC 사용자 전용)

> **이 섹션은 oh-my-claudecode(OMC) HUD를 사용하는 경우에만 해당됩니다.**
> OMC 없이 standalone 모드로 사용하는 경우 이 섹션을 건너뛰세요.
> 설정 값은 `~/.cliproxy-statusline.json`에서 읽습니다 (SKILL.md Section 0 참조).

OMC HUD(`~/.claude/hud/omc-hud.mjs`)를 사용하는 경우, 별도 statusLine 커맨드 없이 HUD 출력에 쿼터 라인을 삽입할 수 있습니다.

## 설정 소스

OMC HUD 모드에서도 프록시 설정은 `~/.cliproxy-statusline.json`에서 읽습니다 (`loadConfig()` 사용).
별도의 `hud-config.json` 설정은 필요하지 않습니다.

```javascript
// proxy 설정 로드 (standalone config)
const config = loadConfig(); // SKILL.md Section 0-3 참조
if (!config) {
  // 설정 없음 -- 프록시 쿼터 표시 생략
}
```

## stdout 인터셉트 패턴

OMC HUD의 stdout 출력 앞에 쿼터 라인을 삽입합니다.

```javascript
let prefixInjected = false;
const originalWrite = process.stdout.write.bind(process.stdout);

process.stdout.write = function(chunk, encoding, callback) {
  if (!prefixInjected) {
    prefixInjected = true;
    const prefix = buildProxyLines(); // 쿼터 라인 문자열
    if (prefix) return originalWrite(prefix + '\n' + chunk, encoding, callback);
  }
  return originalWrite(chunk, encoding, callback);
};
```

## 동적 rateLimits 토글

`ANTHROPIC_BASE_URL`이 프록시 URL과 일치하면 Claude Code의 rate limit 표시를 비활성화합니다 (프록시가 자체 관리하므로 중복 표시 방지).

```javascript
const config = loadConfig();
const isUsingProxy = config && process.env.ANTHROPIC_BASE_URL?.startsWith(config.proxyUrl);

if (isUsingProxy) {
  // OMC HUD의 rateLimits 섹션 비활성화
  hudConfig.rateLimits = false;
}
```

## 안전 영역 / 위험 영역

| 경로 | 구분 | 설명 |
|------|------|------|
| `~/.cliproxy-statusline.json` | **안전** | 스킬 전용 설정 파일 |
| `~/.claude/hud/` | **안전** | OMC 업데이트 시 덮어쓰지 않음 |
| `~/.claude/plugins/cache/omc/` | **위험** | OMC 업데이트 시 덮어씌워짐. 커스텀 코드 저장 금지 |
