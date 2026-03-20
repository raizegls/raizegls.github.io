# CLAUDE.md — RAIZE Credential Site Build Guide

> **목적:** RAIZE의 전문성과 프로젝트 성과를 아카이빙하여 잠재 고객사·채용 지원자에게 전달하는 기업 웹사이트 제작.
> 브랜드 정체성이 충분히 반영된 세련되고 깔끔한 디자인을 지향한다.

---

## Workflow (4 Phases)

```
Phase 1 → Phase 2 → Phase 3 → Phase 4
Planning   Frontend   Backend    Deploy
```

**RULE: Confirm with user after each Phase before proceeding.**

---

# Phase 1: Planning

## Brand Guide (RAIZE)

- **Target:** 잠재 고객사(B2B), 채용 지원자
- **Colors:**
  - Primary: `#76BA66` (Grass Green)
  - Secondary: `#49A18C` (Sea Green)
  - Text/Base: `#333333` (Dark Gray)
- **Design:** 로고 'A' 화살표 형태를 아이콘/불렛으로 활용. Pretendard 또는 Montserrat 폰트. 넓은 여백 유지.
- **Contact:** DB 연동 없이 Footer에 이메일·연락처 정적 텍스트로 고정.

## Portfolio Categories (filter required)

| Key | 명칭 |
|-----|------|
| `career` | 진로/취업/창업 |
| `social` | 지역사회 |
| `event` | 경진대회 |
| `global` | 글로벌 |
| `ai` | AI활용 |
| `competency` | 역량강화 |
| `consulting` | 컨설팅 |

---

# Phase 2: Frontend

## Requirements

1. **Hero section** — 브랜드 가치 메인 비주얼 + 슬로건
2. **Filter system** — 7개 카테고리 버튼 클릭 시 실시간 정렬
3. **Project card UI** — 썸네일 이미지 / 제목 / 고객사 / 성과 요약
4. **Partners section** — 파트너사 로고 그리드 (채도 낮춤)

## Tech Stack

- Single `index.html` (inline styles)
- Tailwind CSS CDN
- Supabase REST API via native `fetch` (no SDK)
- Placeholder images: `https://placehold.co/WxH`

## Anti-Generic Rules

| Item | ❌ NEVER | ✅ ALWAYS |
|------|---------|----------|
| Colors | Tailwind default blue/indigo | Custom brand colors `#76BA66` / `#49A18C` |
| Shadows | `shadow-md` only | Layered, color-tinted shadows |
| Animation | `transition-all` | `transform`, `opacity` only |
| Buttons | No hover/focus/active states | All states implemented |

## Button States (required for every button)

```css
.btn           { background: #76BA66; }
.btn:hover     { background: #5fa055; transform: scale(1.02); }
.btn:focus     { outline: none; ring: 2px #76BA66; }
.btn:active    { transform: scale(0.98); }
.btn:disabled  { opacity: 0.5; cursor: not-allowed; }
```

---

# Phase 3: Backend (Supabase)

## Storage

- **Bucket:** `project-images` (Public)
- 이미지 업로드 후 Public URL 복사 → DB에 등록

## Database — Table: `projects`

| Column | Type | 설명 |
|--------|------|------|
| `title` | text | 프로그램 명칭 |
| `category` | text | 카테고리 키값 (career / social / event / global / ai / competency / consulting) |
| `image_url` | text | Storage Public URL |
| `client` | text | 발주처 또는 고객사 |
| `outcome` | text | 주요 성과 요약 |

## Supabase Fetch Pattern

```js
const SUPABASE_URL = '...';  // env only
const SUPABASE_KEY = '...';  // env only

const res = await fetch(`${SUPABASE_URL}/rest/v1/projects?select=*&order=created_at.desc`, {
  headers: { 'apikey': SUPABASE_KEY, 'Authorization': 'Bearer ' + SUPABASE_KEY }
});
```

---

# ⚠️ Strict Rules

## Security (MUST follow — no exceptions)

- **NEVER hardcode API keys** — Supabase URL·Anon Key 등 모든 비밀값은 코드에 직접 입력 금지
- **NEVER commit .env files** — `.gitignore`에 `.env` 반드시 포함
- **NEVER expose Service Role key** — 절대 프론트엔드에서 호출 금지

## Quality

- **NO contact form** — DB 연동 문의 폼 대신 Footer 정적 텍스트 유지
- **Dynamic data** — 모든 프로젝트 데이터는 Supabase DB에서 fetch
- **Optimize images** — 업로드 전 리사이징(압축) 권장

---

# Phase 4: Deploy

## GitHub

사용자가 저장소 URL 제공 → Claude Code 실행:

```bash
git add . && git commit -m "message" && git push
```

## GitHub Pages

Settings → Pages → Branch: `main` → Save
→ `https://[username].github.io/[repo-name]`

## Vercel (optional — for env vars)

Vercel → Settings → Environment Variables:
```
NEXT_PUBLIC_SUPABASE_URL
NEXT_PUBLIC_SUPABASE_ANON_KEY
```
저장 후 Redeploy.

## Custom Domain (optional)

```
A record:     @ → 76.76.21.21
CNAME record: www → cname.vercel-dns.com
```
