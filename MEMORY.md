# taeyoung-linktree 진행 기록

## 프로젝트 개요
- 개인용 linktree (taeyoung5075)
- SPA 형식, 단일 HTML
- 컨셉: 테크 유튜버 / build in public / 인디해커 (터미널 무드)
- 배포: Cloudflare Workers (Static Assets) → parktaeyoung.com 도메인
- 소셜 핸들 통일: X, Instagram, Threads, YouTube, TikTok 모두 `taeyoung5075`

## 디자인 (v4 — v3 + 동작하는 명령어 입력창)
- v3 기반에 nitro 처럼 하단 입력 바 추가 (statusbar / cmdbar 2단)
- Layout: chrome → body(scroll) → statusbar(MEM/CPU/UP · LANG) → cmdbar(`>` + input + 회전 hint + ↵)
- 명령어 시스템 (전부 동작):
  - 소셜: `1`~`5`, `/x|/twitter`, `/instagram|/ig`, `/threads`, `/youtube|/yt`, `/tiktok|/tt`
  - 메타: `/about|/whoami|/me`, `/links|/ls`, `/help|/?|?`, `/clear|/cls`, `/uptime`, `/echo <text>`
- 입력 UX: Tab 자동완성, `/` 입력 시 팔레트 드롭업 (↑↓ 이동, Enter 실행, Esc 닫기), ↑↓ 히스토리(빈 입력일 때), 1~5 단축키(빈 입력일 때만)
- 글로벌: 시퀀스 끝나면 input 자동 포커스, 패널 빈 곳 클릭 = focus, 시퀀스 중 키 입력 = skip(스킵 후엔 input 으로 전달)
- 회전 힌트: 입력이 비었을 때 2.8s 마다 다음 콘텐츠 안내 / 팁 토글
- 미동작 항목: Language KO/EN 토글 (시각만, 차후)

## 디자인 (v3 — nitro.hashed.com 무드 + Claude Code 팔레트 — 폐기)
- 출처: https://nitro.hashed.com/ 레이아웃 + 기존 토큰
- 컨테이너: 페이지 `#0d0d0d` + 10px padding → 둥근 단일 패널 `#1a1a1a` (`#333` border)
- 헤더: `#252525` 바, traffic lights + `taeyoung@parktaeyoung — bash` + 우측 `● online`
- 푸터: `MEM / CPU / UP` 라이브 + `> next: /x` 회전 + 깜빡 caret + `LANG: KO·EN`
- 본문 max-width 900px, JetBrains Mono only
- 인터랙션: 페이지 로드 시 `whoami → cat about.md → ls socials/ → uptime` 순서로 한 줄씩 타이핑(촤라락), 5개 소셜 링크는 행 단위로 `→ /x  @taeyoung5075  x.com/...` 형태로 펼쳐짐
- Skip: 아무 키 또는 빈 곳 클릭 시 즉시 완성, `1~5` 키로 새 탭에서 소셜 열기
- 감속/접근성: `prefers-reduced-motion` 시 모든 애니메이션 정지

## 디자인 (v2 — Claude Code Terminal Theme — 폐기)
- 출처: `claude-code-template-new.zip` (사용자 제공)
- 토큰 스냅샷: `brand-tokens.json` (저장됨)
- 핵심 토큰
  - Primary: Terra Cotta `#E07A5F` (+ light `#E89A84`, glow `rgba(224,122,95,.2)`)
  - BG: `#0F0F0F` / `#1A1A1A` / `#252525`
  - Text: `#D8D8D8` / `#888888` / `#555555`
  - Terminal: green `#27C93F`, red `#FF5F56`, yellow `#FFBD2E`, cyan `#00ffff`, blue `#60A5FA`
  - Border `#333333`
- 폰트: JetBrains Mono (메인) + Press Start 2P (옵션)
- 레이아웃
  - 상단: traffic lights + `taeyoung@parktaeyoung — bash` + Status Online
  - 본문: prompt → badge → ASCII `TAEYOUNG` → subtitle → nav (5 social) → whoami 결과 블록 → system → CTA
  - 하단: UPTIME / FPS / next: cmds / LANG / 깜빡이는 caret
- 효과: 사이드 글로우 (`inset 0 0 200px rgba(224,122,95,.07)`) + 미세 스캔라인
- 인터랙션: 키보드 1~5로 각 소셜 새 탭 오픈, 우측 화살표 호버

## 결정사항
- 단일 `public/index.html` + 임베드 CSS/JS (의존성 zero)
- 배포: `wrangler` + Workers Static Assets (`assets` 바인딩)
- 라우트: `parktaeyoung.com/*` (사용자가 CF Dashboard에서 도메인 연결 필요)

## 작업 단계
1. ~~디자인 분석~~
2. ~~`index.html` 작성 (디자인 매치)~~
3. ~~`wrangler.jsonc` + 워커 엔트리 (Static Assets만 사용)~~
4. ~~로컬 미리보기~~
5. ~~배포 (API token 방식, `.env` 사용)~~
6. parktaeyoung.com Custom Domain 연결 (사용자 작업)

## 배포 정보
- 계정: `Mu07010@naver.com's Account` (`5e2069ee6becb1625d819edf10c1427a`)
- 인증: API Token via `.env` (CLOUDFLARE_API_TOKEN, CLOUDFLARE_ACCOUNT_ID)
- 워커: `taeyoung-linktree`
- URL: https://taeyoung-linktree.mu07010-5e2.workers.dev/
- Version: `e58ea17b-5e08-4dad-bf31-69a5401a8c06`
- Wrangler: 3.114.17 (v4 업그레이드 권장)
- `.env` 는 .gitignore 에 추가됨

## 이슈 / 메모
- wrangler v3 → v4 마이그레이션 권장 (작동은 정상)
