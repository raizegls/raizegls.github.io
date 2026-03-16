# CLAUDE.md — RAIZE 기업 브랜딩 및 크리덴셜 사이트 구축 가이드

> **이 파일의 목적:** "홈페이지 만들어줘"라고 하면 처음부터 끝까지 순서대로 진행한다. RAIZE의 전문성과 프로젝트 성과(Credentials)를 체계적으로 아카이빙하고, 잠재 고객사와 채용 지원자에게 효과적으로 전달하는 고퀄리티 기업 웹사이트를 제작한다.
> 
> Claude Code가 전체 프로세스를 관리하고, 브랜드 정체성이 충분히 반영된 세련되고 깔끔한 디자인을 지향한다.

---

## 🚀 시작하기

사용자가 홈페이지 제작을 요청하면, 아래 **4개 Phase**를 순서대로 진행한다.

```
Phase 1: 기획 (브랜드 및 카테고리 확정)
    ↓
Phase 2: 프론트엔드 개발 (필터링 UI 및 정적 콘텐츠 구현)
    ↓
Phase 3: 백엔드 연동 (Supabase DB 및 Storage 설정)
    ↓
Phase 4: 배포 (GitHub & Vercel 연동)
```

**각 Phase 완료 후 사용자에게 확인받고 다음 Phase로 진행한다.**

---

# Phase 1: 기획 (인터뷰)

## 🎨 RAIZE 브랜드 가이드
- **핵심 타겟:** 잠재 고객사(B2B), 채용 지원자
- **브랜드 컬러:** 로고의 그린 그라데이션을 디자인 포인트로 활용
  - Primary (Top): `#76BA66` (Grass Green)
  - Secondary (Bottom): `#49A18C` (Sea Green)
  - Text/Base: `#333333` (Dark Gray)
- **디자인 컨셉:** - 로고의 'A' 화살표 형태를 시각적 아이콘이나 불렛 포인트로 활용.
  - 넓은 여백(Whitespace)과 세련된 폰트(Pretendard 또는 Montserrat) 레이아웃 유지.
- **문의처(Contact):** 데이터베이스 연동 없이 페이지 하단(Footer)에 이메일, 연락처 등을 텍스트로 고정 안내.

## 📂 포트폴리오 카테고리 (필터 기능 필수)
1. **취업/창업 프로그램** (`career`)
2. **지역사회공헌 프로그램** (`social`)
3. **경진대회 행사 운영** (`event`)
4. **해외 프로그램** (`global`)
5. **AI 역량 강화 프로그램** (`ai`)   
```

**→ 사용자 확인 후 Phase 2로 진행**

---

# Phase 2: 프론트엔드 개발

## 주요 구현 사항
1. **히어로 섹션:** RAIZE의 브랜드 가치를 한눈에 보여주는 메인 비주얼과 슬로건.
2. **필터링 시스템:** 5개 카테고리 버튼 클릭 시 프로젝트 리스트가 실시간으로 정렬되는 기능.
3. **프로젝트 카드 UI:** - 썸네일 이미지 (Storage 연동)
  - 프로젝트 제목 및 수행 고객사
  - 주요 성과(Outcome) 요약 텍스트
4. **협력사 섹션:** 파트너사 로고들을 채도를 낮춘 그리드 형태로 나열.

**→ 사용자 확인 후 Phase 3로 진행**

---

# Phase 3: 백엔드 및 크리덴셜 관리 (Supabase)

## 1. 이미지 저장 (Storage)
- **Bucket 명칭:** `project-images` (Public 설정)
- **관리자 작업:** 프로젝트 사진 업로드 후 발급된 **Public URL**을 복사하여 DB에 등록.

## 2. 데이터베이스 (Table: `projects`)
관리자가 대시보드에서 직접 데이터를 추가/수정할 수 있도록 아래 컬럼을 구성한다.
- `title`: 프로그램 명칭
- `category`: 위 5개 카테고리 키값 중 선택
- `image_url`: Storage에서 복사한 이미지 주소
- `client`: 발주처 또는 고객사 명칭
- `outcome`: 주요 성과 요약 (예: 만족도 4.9, 수료생 100명 등)

---


# ⚠️ 엄격한 규칙 (Security & Quality)

## 🔒 보안 준수 사항 (절대 준수)
- **❌ API 키 하드코딩 금지:** Supabase URL이나 Anon Key 등 모든 비밀값은 절대 코드에 직접 입력하지 않는다.
- **❌ .env 파일 커밋 금지:** `.gitignore`에 `.env`를 반드시 포함하여 GitHub 등 외부 저장소에 유출되지 않도록 한다.
- **❌ 클라이언트 사이드 노출 주의:** 유출될 경우 위험한 서비스 롤(Service Role) 키는 절대 프론트엔드 코드에서 호출하지 않는다.

## 🛠 운영 및 품질 규칙
- **문의 관리 제외:** DB 연동 문의 폼 대신 정적 텍스트 안내를 유지한다.
- **데이터 동적 로딩:** 모든 크리덴셜은 Supabase DB에서 호출하여 관리 편의성을 보장한다.
- **최적화:** 사이트 속도를 위해 프로젝트 이미지는 업로드 전 리사이징(압축)을 권장한다.

**→ 사용자 확인 후 Phase 4로 진행**

---

# Phase 4: 배포 (Vercel)

## 4-1. GitHub 연결

### 👤 사용자가 직접 할 일

```
📋 GitHub 저장소 생성

1. https://github.com → 로그인
2. "+" → "New repository"
3. 생성된 URL 알려주세요:
   ✅ https://github.com/[username]/[repo-name]
```

### 🤖 Claude Code가 할 일

```bash
git init && git add . && git commit -m "Initial commit"
git remote add origin [URL] && git push -u origin main
```

---

## 4-2. Vercel 배포

### 👤 사용자가 직접 할 일

```
📋 Vercel 배포 가이드

1. https://vercel.com → GitHub 로그인
2. "Add New..." → "Project"
3. 저장소 선택 → Import → Deploy
4. 완료 후 URL 알려주세요:
   ✅ https://[프로젝트명].vercel.app
```

---

## 4-3. 환경변수 등록

### 왜 Vercel에도 환경변수를 등록하나요?

```
🤔 .env.local 파일은 내 컴퓨터에만 있음
- Vercel 서버는 이 파일을 모름
- Vercel에 따로 알려줘야 함

✅ Vercel 환경변수 = Vercel 서버에게 API 키 알려주기
```

### 👤 사용자가 직접 할 일

```
Vercel → Settings → Environment Variables

NEXT_PUBLIC_SUPABASE_URL = [값]
NEXT_PUBLIC_SUPABASE_ANON_KEY = [값]
RESEND_API_KEY = [값]

→ 저장 후 Redeploy
```

---

## 4-4. 커스텀 도메인 (선택)

```
Vercel → Settings → Domains → 도메인 입력

DNS 설정:
- A 레코드: @ → 76.76.21.21
- CNAME: www → cname.vercel-dns.com
```

## Phase 4 완료 체크

```
□ GitHub 푸시 완료
□ Vercel 배포 완료
□ 환경변수 등록 및 재배포
□ 배포된 사이트에서 폼 제출 테스트 성공
□ (선택) 커스텀 도메인 연결
```