# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

> **목적:** RAIZE의 전문성과 프로젝트 성과(Credentials)를 아카이빙하여 잠재 고객사·채용 지원자에게 전달하는 기업 웹사이트.

---

## Project Status

- **Phase 1 (Planning)** ✅ 완료
- **Phase 2 (Frontend)** ✅ 완료
- **Phase 3 (Supabase)** ✅ 완료 — 데이터 fetch 및 카드 렌더링 동작 중
- **Phase 4 (Deploy)** ✅ GitHub Pages 배포 완료

---

## Local Preview

```bash
npx serve .
# 또는
python -m http.server 3000
```

index.html을 브라우저에서 직접 열면 Supabase fetch가 CORS로 동작하지 않을 수 있음.

## Deploy

```bash
git add . && git commit -m "message" && git push
```

GitHub Pages가 `main` 브랜치를 자동 감지하여 재배포.

---

## Code Architecture

단일 `index.html`에 모든 HTML/CSS/JS 포함. 외부 파일 없음.

```
<head>  Pretendard + Montserrat 폰트 / Tailwind CDN / <style> 커스텀 CSS
<body>  Nav → Hero → About → Keywords → Credentials → Partners → Footer
<script> Supabase fetch → 카드 렌더링 → 필터(카테고리+특성화분야) → Stats 카운터 → Scroll Reveal → 마인드맵 → 파트너 마퀴
```

### Tailwind 커스텀 컬러

```js
colors: { grass: '#76BA66', sea: '#49A18C', dark: '#333333' }
```

### JS 시스템

**Supabase fetch** — SDK 없이 native `fetch` 사용
```js
const SUPABASE_URL = 'https://ftchvdqpaghtanoytqtm.supabase.co';
const SUPABASE_KEY = '...';  // anon key (RLS로 보호, 공개 설계)
// GET /rest/v1/projects?select=*&order=created_at.desc
```

**카드 렌더링 & 필터** — 두 개의 독립 필터 시스템
- **카테고리 필터** (`.filter-btn`, `data-filter`): `career/social/event/global/ai/competency/consulting`
- **특성화분야 필터** (`.specialty-btn`, `data-specialty`): `반도체/에너지/AI/ESG`
- 두 필터는 서로 클릭 시 상대방을 "전체"로 리셋
- `all` 탭에서는 최대 `MAX_ALL = 12`개만 표시, 초과 시 블로그(`BLOG_URL`) 링크 버튼 노출
- `data-category`는 콤마 구분 복수 카테고리 지원 (예: `"career,ai"`)

**Stats 카운터** — `.hero-rise-5` 뷰포트 진입 시 `.stat-num[data-target]` 요소 애니메이션

**Scroll Reveal** — `IntersectionObserver`, threshold 0.12
- `.reveal` / `.reveal-l` / `.reveal-r` / `.reveal-s` → 진입 시 `.in-view` 추가
- `.d1/.d2/.d3` 클래스로 순차 딜레이

**마인드맵** — 데스크탑 전용 (`window.innerWidth < 768`이면 skip)
- `[data-ring="outer"]` / `[data-ring="inner"]` 요소를 타원형으로 배치
- `outerRX = W*0.42`, `outerRY = H*0.40`, `innerRX = W*0.22`, `innerRY = H*0.26`
- `ResizeObserver`로 창 크기 변화 시 재계산

**파트너 마퀴** — `PARTNER_LOGOS` 배열 기반 동적 렌더링
- `./partners/` 폴더 이미지 사용
- `onerror` 시 텍스트 fallback 표시
- 렌더 후 `revealObserver`에 동적 등록

### 파트너 로고 추가

1. `partners/` 폴더에 이미지 추가
2. `PARTNER_LOGOS` 배열에 `{ src, alt }` 항목 추가

---

## Brand Guide

- **Colors:** `#76BA66` (Grass Green) / `#49A18C` (Sea Green) / `#333333` (Dark)
- **Fonts:** Pretendard (한글) + Montserrat (영문/숫자)
- **Contact:** Footer 정적 텍스트 — DB 연동 문의 폼 없음 (`qna@raizegls.com`)

## Portfolio Categories

| Key | 명칭 | 필터 색상 |
|-----|------|----------|
| `career` | 진로/취업/창업 | `#76BA66` |
| `social` | 지역사회 | `#49A18C` |
| `event` | 경진대회 | `#8B5CF6` |
| `global` | 글로벌 | `#3B82F6` |
| `ai` | AI활용 | `#F59E0B` |
| `competency` | 역량강화 | `#06B6D4` |
| `consulting` | 컨설팅 | `#F43F5E` |

## Database — Table: `projects`

| Column | Type | 설명 |
|--------|------|------|
| `title` | text | 프로그램 명칭 |
| `category` | text | 카테고리 키값 (콤마 구분 복수 가능) |
| `specialty` | text | 특성화분야 (반도체/에너지/AI/ESG, 콤마 구분) |
| `image_url` | text | Supabase Storage Public URL |
| `client` | text | 발주처 또는 고객사 |
| `outcome` | text | 주요 성과 요약 |

**Storage Bucket:** `project-images` (Public)

---

## Strict Rules

- **NEVER `transition-all`** — `opacity`, `transform`만 허용
- **NEVER Tailwind 기본 blue/indigo** — `#76BA66` / `#49A18C` 사용
- **NEVER Service Role key** — anon key만 사용
- **NO contact form** — Footer 정적 이메일 텍스트 유지
- **모든 버튼** hover/focus/active/disabled 상태 필수 구현
