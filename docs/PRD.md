# 📝 Team Blog CMS - Product Requirements Document

## 프로젝트 개요

- **프로젝트명**: Team Blog CMS (Notion 기반 팀 블로그/문서 사이트)
- **목적**: Notion을 CMS로 활용하여 팀의 블로그 포스트와 문서를 관리하고 공개하는 웹사이트 구축
- **CMS 선택 이유**: 
  - Notion API를 활용하여 비개발자도 콘텐츠 관리 가능
  - 실시간 데이터 동기화로 최신 콘텐츠 유지
  - 마크다운 형식 지원으로 쉬운 포스트 작성
  - 데이터베이스 구조로 태그, 카테고리, 작성자 등 메타데이터 관리 용이

## 🎯 주요 기능

### 1. 블로그 포스트 목록 페이지
- Notion 데이터베이스에서 전체 포스트 조회 및 목록 표시
- 최신순/인기순 정렬 기능
- 카테고리/태그별 필터링
- 페이지네이션 또는 무한 스크롤

### 2. 포스트 상세 페이지
- Notion 페이지 콘텐츠를 마크다운으로 변환하여 렌더링
- 포스트 메타데이터 표시 (작성자, 작성일, 읽는 시간 등)
- 목차(Table of Contents) 자동 생성
- 관련 포스트 추천

### 3. Claude AI 활용 자동화
- 포스트 요약 자동 생성 (SEO 메타데이터)
- 포스트 설명문(description) 자동 작성
- 관련 키워드 추출

## 🛠️ 기술 스택

| 분야 | 기술 |
|------|------|
| **Frontend** | Next.js 15.5.3 (App Router) |
| **Language** | TypeScript 5 |
| **CMS** | Notion API |
| **Styling** | Tailwind CSS v4 |
| **UI Components** | shadcn/ui (new-york style) |
| **Icons** | Lucide React |
| **AI** | Claude API (자동 요약/메타데이터) |
| **Data Fetching** | `@notionhq/client`, `notion-to-md` |
| **Development** | ESLint, Prettier, TypeScript |

## 📊 Notion 데이터베이스 구조

### Posts 데이터베이스 스키마

| 필드명 | 타입 | 설명 |
|--------|------|------|
| `Title` | Title | 포스트 제목 (필수) |
| `Slug` | Text | URL 슬러그 (필수, 고유값) |
| `Content` | Rich Text | 포스트 본문 콘텐츠 |
| `Category` | Select | 카테고리 (예: React, Next.js, Tips) |
| `Tags` | Multi-select | 태그 (예: performance, tutorial) |
| `Author` | Person | 포스트 작성자 |
| `Published` | Checkbox | 공개 여부 |
| `PublishedDate` | Date | 공개 날짜 |
| `Description` | Text | 포스트 짧은 설명 (SEO용) |
| `Thumbnail` | Files & media | 포스트 썸네일 이미지 |
| `ReadingTime` | Number | 읽는 시간 (분) |
| `Views` | Number | 조회수 |

## 🎨 화면 구성

### 1. 블로그 홈 페이지 (`/blog`)
- 헤더: 사이트 제목, 검색 바
- 필터 섹션: 카테고리, 태그 필터
- 포스트 카드 그리드: 썸네일, 제목, 설명, 작성자, 작성일
- 사이드바: 인기 포스트, 최근 포스트
- 푸터: 사이트 정보

### 2. 포스트 상세 페이지 (`/blog/[slug]`)
- 포스트 헤더: 제목, 작성자, 작성일, 카테고리
- 목차(TOC): 마크다운 헤딩 기반 자동 생성
- 본문: 마크다운으로 렌더링된 포스트 콘텐츠
- 태그: 태그 클릭 시 필터링된 목록 페이지로 이동
- 관련 포스트: 같은 카테고리의 다른 포스트 추천
- 마지막 수정일 표시

### 3. 검색/필터 결과 페이지 (`/blog/category/[category]`, `/blog/tag/[tag]`)
- 필터된 포스트 목록 표시
- 브레드크럼 네비게이션
- 결과 카운트 표시

## 📦 MVP 범위

### 포함할 최소 기능
- ✅ Notion API 연동 및 포스트 데이터 fetch
- ✅ 블로그 홈 페이지: 포스트 목록 표시
- ✅ 포스트 상세 페이지: 마크다운 렌더링
- ✅ 기본 메타데이터 표시 (작성자, 작성일)
- ✅ 카테고리/태그 기반 필터링
- ✅ 반응형 UI (shadcn/ui 기반)
- ✅ Claude API로 포스트 설명 자동 생성 (선택)

### 제외 기능 (향후 추가)
- 사용자 댓글 시스템
- 검색 기능 (Algolia 등)
- 다국어 지원
- 소셜 공유 기능
- 이메일 뉴스레터

## 🚀 구현 단계

### Phase 1: 기초 설정 (5분)
1. Notion API 키 발급 및 데이터베이스 공유
2. 필요한 npm 패키지 설치
   ```bash
   npm install @notionhq/client notion-to-md
   ```
3. 환경 변수 설정 (`.env.local`)
   ```
   NEXT_PUBLIC_NOTION_DATABASE_ID=<database_id>
   NOTION_API_KEY=<api_key>
   NEXT_PUBLIC_CLAUDE_API_KEY=<claude_api_key>
   ```

### Phase 2: Notion 데이터 연동 (8분)
1. Notion API 클라이언트 설정 (`src/lib/notion.ts`)
2. 포스트 데이터 fetch 함수 작성
   - `getPosts()`: 전체 포스트 조회
   - `getPostBySlug()`: 특정 포스트 조회
   - `getPostsByCategory()`: 카테고리별 포스트 조회
3. 마크다운 변환 로직 구현 (`notion-to-md` 라이브러리)

### Phase 3: UI 구현 (12분)
1. 블로그 레이아웃 컴포넌트 (`src/app/blog/layout.tsx`)
2. 블로그 홈 페이지 (`src/app/blog/page.tsx`)
   - 포스트 카드 컴포넌트 (`src/components/blog/post-card.tsx`)
   - 필터 사이드바 컴포넌트 (`src/components/blog/filter-sidebar.tsx`)
3. 포스트 상세 페이지 (`src/app/blog/[slug]/page.tsx`)
   - 마크다운 렌더러 컴포넌트 (`src/components/blog/markdown-renderer.tsx`)
   - 목차 컴포넌트 (`src/components/blog/table-of-contents.tsx`)
   - 관련 포스트 컴포넌트 (`src/components/blog/related-posts.tsx`)
4. 카테고리/태그 페이지 (`src/app/blog/category/[category]/page.tsx`)

### Phase 4: Claude AI 통합 (3분, 선택)
1. Claude API 클라이언트 설정 (`src/lib/claude.ts`)
2. 포스트 설명 자동 생성 함수
3. Notion에 생성된 설명 저장 (또는 캐시)

### Phase 5: 테스트 및 배포 (2분)
1. 로컬 테스트 (`npm run dev`)
2. 빌드 검증 (`npm run build`)
3. Vercel 배포

## 📈 성공 지표

- ✅ Notion 데이터베이스와 완벽하게 동기화
- ✅ 모든 포스트가 마크다운으로 올바르게 렌더링
- ✅ 필터링 및 검색 기능 정상 작동
- ✅ 모바일 반응형 UI
- ✅ 페이지 로드 시간 < 2초 (SSG/ISR 활용)
- ✅ Claude AI 요약이 포스트와 자연스럽게 어울림

## 📝 개발 노트

- Notion 페이지 ID와 데이터베이스 ID는 URL에서 추출 가능
- `notion-to-md` 라이브러리는 Notion 블록을 마크다운으로 변환하는 작은 문제 있을 수 있으므로 커스터마이징 준비
- 이미지는 Notion에 업로드된 URL 또는 외부 링크 지원
- SSG(Static Site Generation)로 빌드 시간에 데이터 미리 fetch 후 ISR(Incremental Static Regeneration)로 주기적 업데이트
