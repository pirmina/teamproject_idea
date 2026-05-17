# Team Blog CMS 개발 로드맵

Notion을 CMS로 활용하여 팀의 블로그 포스트와 문서를 효율적으로 관리하고 공개하는 모던 웹 애플리케이션

## 개요

**Team Blog CMS**는 개발팀과 콘텐츠 크리에이터를 위한 **Notion 기반 블로그 플랫폼**으로 다음 기능을 제공합니다:

- **Notion 기반 콘텐츠 관리**: Notion 데이터베이스를 CMS로 활용하여 비개발자도 손쉽게 포스트 작성 및 관리
- **마크다운 자동 렌더링**: Notion 블록을 마크다운으로 변환하여 일관된 포스트 표시
- **카테고리/태그 필터링**: 카테고리와 태그 기반 콘텐츠 분류 및 검색
- **Claude AI 자동화**: SEO 메타데이터, 포스트 설명, 키워드를 AI로 자동 생성
- **반응형 UI**: shadcn/ui 기반의 모던하고 모바일 친화적인 인터페이스

## 개발 워크플로우

1. **작업 계획**
   - 기존 코드베이스를 학습하고 현재 상태를 파악
   - 새로운 작업을 포함하도록 `ROADMAP.md` 업데이트
   - 우선순위 작업은 마지막 완료된 작업 다음에 삽입

2. **작업 생성**
   - 기존 코드베이스를 학습하고 현재 상태를 파악
   - `/tasks` 디렉토리에 새 작업 파일 생성
   - 명명 형식: `XXX-description.md` (예: `001-setup.md`)
   - 고수준 명세서, 관련 파일, 수락 기준, 구현 단계 포함
   - **API/비즈니스 로직 작업 시 "## 테스트 체크리스트" 섹션 필수 포함 (Playwright MCP 테스트 시나리오 작성)**
   - 예시를 위해 `/tasks` 디렉토리의 마지막 완료된 작업 참조. 예를 들어, 현재 작업이 `012`라면 `011`과 `010`을 예시로 참조.
   - 이러한 예시들은 완료된 작업이므로 내용이 완료된 작업의 최종 상태를 반영함 (체크된 박스와 변경 사항 요약). 새 작업의 경우, 문서에는 빈 박스와 변경 사항 요약이 없어야 함. 초기 상태의 샘플로 `000-sample.md` 참조.

3. **작업 구현**
   - 작업 파일의 명세서를 따름
   - 기능과 기능성 구현
   - **API 연동 및 비즈니스 로직 구현 시 Playwright MCP로 테스트 수행 필수**
   - 각 단계 후 작업 파일 내 단계 진행 상황 업데이트
   - 구현 완료 후 Playwright MCP를 사용한 E2E 테스트 실행
   - 테스트 통과 확인 후 다음 단계로 진행
   - 각 단계 완료 후 중단하고 추가 지시를 기다림

4. **로드맵 업데이트**
   - 로드맵에서 완료된 작업을 ✅로 표시

## 개발 단계

### Phase 1: 애플리케이션 골격 구축

- **Task 001: 프로젝트 라우트 구조 및 환경 설정** - 우선순위
  - Next.js App Router 기반 블로그 라우트 구조 생성 (`/blog`, `/blog/[slug]`, `/blog/category/[category]`, `/blog/tag/[tag]`)
  - 각 페이지의 빈 껍데기 파일 생성 (placeholder content)
  - `.env.local.example` 작성 (Notion API Key, Database ID, Claude API Key)
  - `@notionhq/client`, `notion-to-md`, `@anthropic-ai/sdk` 패키지 설치
  - 블로그 공통 레이아웃 컴포넌트 (`src/app/blog/layout.tsx`) 골격 구현

- **Task 002: 타입 정의 및 데이터 모델 설계**
  - Notion Post 데이터베이스 스키마 TypeScript 인터페이스 정의 (`src/types/post.ts`)
  - 카테고리, 태그, 작성자 등 메타데이터 타입 정의
  - API 응답 타입 및 변환 유틸리티 함수 시그니처 정의
  - Notion → 내부 도메인 모델 매핑 인터페이스 설계
  - Claude AI 응답 타입 정의 (요약, 키워드, 설명)

### Phase 2: UI/UX 완성 (더미 데이터 활용)

- **Task 003: 블로그 공통 컴포넌트 라이브러리 구현**
  - shadcn/ui 추가 컴포넌트 설치 (pagination, breadcrumb, tabs 등)
  - 포스트 카드 컴포넌트 (`src/components/blog/post-card.tsx`) 구현
  - 카테고리/태그 뱃지 컴포넌트 구현
  - 필터 사이드바 컴포넌트 (`src/components/blog/filter-sidebar.tsx`) 구현
  - 더미 데이터 생성 유틸리티 작성 (`src/lib/mock-data.ts`)
  - 블로그 네비게이션 및 헤더 컴포넌트 구현

- **Task 004: 블로그 홈 페이지 UI 구현**
  - 블로그 홈 페이지 (`/blog`) 레이아웃 완성
  - 포스트 카드 그리드 (썸네일, 제목, 설명, 작성자, 작성일)
  - 필터 섹션 (카테고리, 태그) UI 구현
  - 정렬 옵션 (최신순/인기순) UI 구현
  - 페이지네이션 UI 구현
  - 사이드바 (인기 포스트, 최근 포스트) 구현

- **Task 005: 포스트 상세 페이지 UI 구현**
  - 포스트 상세 페이지 (`/blog/[slug]`) 레이아웃 완성
  - 포스트 헤더 (제목, 작성자, 작성일, 카테고리, 읽는 시간)
  - 마크다운 렌더러 컴포넌트 (`src/components/blog/markdown-renderer.tsx`) 구현
  - 목차(TOC) 컴포넌트 (`src/components/blog/table-of-contents.tsx`) 구현
  - 관련 포스트 컴포넌트 (`src/components/blog/related-posts.tsx`) 구현
  - 태그 클릭 시 필터링 페이지로 이동하는 네비게이션 구현

- **Task 006: 카테고리/태그 페이지 UI 구현**
  - 카테고리별 페이지 (`/blog/category/[category]`) UI 구현
  - 태그별 페이지 (`/blog/tag/[tag]`) UI 구현
  - 브레드크럼 네비게이션 구현
  - 결과 카운트 표시
  - 빈 결과 상태 UI 처리
  - 반응형 디자인 및 모바일 최적화

### Phase 3: Notion 데이터 연동 및 핵심 기능 구현

- **Task 007: Notion API 클라이언트 구축** - 우선순위
  - Notion API 클라이언트 설정 (`src/lib/notion.ts`)
  - `getPosts()`: 전체 공개 포스트 조회 함수 구현
  - `getPostBySlug()`: slug 기반 단일 포스트 조회 함수 구현
  - `getPostsByCategory()`: 카테고리별 포스트 조회 함수 구현
  - `getPostsByTag()`: 태그별 포스트 조회 함수 구현
  - Notion 응답 → 내부 도메인 모델 변환 로직 구현
  - 에러 핸들링 및 재시도 로직 구현
  - Playwright MCP로 API 데이터 페칭 검증

- **Task 008: 마크다운 변환 및 렌더링 통합**
  - `notion-to-md` 라이브러리 통합 (`src/lib/markdown.ts`)
  - Notion 블록을 마크다운으로 변환하는 로직 구현
  - 코드 블록 syntax highlighting 설정
  - 이미지 최적화 (Next.js Image 활용)
  - 커스텀 블록 처리 (callout, toggle 등)
  - 마크다운 헤딩 기반 목차 자동 생성 로직
  - Playwright MCP로 렌더링 결과 검증

- **Task 009: 더미 데이터를 실제 API로 교체 및 SSG/ISR 적용**
  - 블로그 홈 페이지에 Notion 실제 데이터 연동
  - 포스트 상세 페이지에 Notion 실제 데이터 연동
  - 카테고리/태그 페이지에 실제 데이터 연동
  - `generateStaticParams`로 SSG 빌드 적용
  - ISR (Incremental Static Regeneration) 설정 (revalidate 주기 설정)
  - 로딩/에러 상태 처리 (`loading.tsx`, `error.tsx`)
  - Playwright MCP로 전체 사용자 플로우 E2E 테스트

- **Task 009-1: 핵심 기능 통합 테스트**
  - Playwright MCP로 블로그 홈 → 상세 페이지 플로우 테스트
  - 카테고리/태그 필터링 동작 검증
  - 페이지네이션 및 정렬 기능 테스트
  - 잘못된 slug, 비공개 포스트 등 엣지 케이스 검증
  - 모바일/태블릿/데스크탑 반응형 테스트

### Phase 4: Claude AI 통합 및 고급 기능

- **Task 010: Claude AI 자동화 기능 구현**
  - Claude API 클라이언트 설정 (`src/lib/claude.ts`)
  - 포스트 요약 자동 생성 함수 구현
  - SEO 메타데이터 자동 생성 (description, keywords)
  - 관련 키워드 추출 기능 구현
  - 생성된 메타데이터 캐싱 전략 (Notion 업데이트 또는 로컬 캐시)
  - 빌드 타임 사전 생성 전략 적용
  - Playwright MCP로 AI 생성 결과 검증

- **Task 011: SEO 및 메타데이터 최적화**
  - 동적 메타데이터 생성 (`generateMetadata`) 구현
  - Open Graph 태그 및 Twitter Card 설정
  - 구조화 데이터 (JSON-LD) 추가 (Article 스키마)
  - sitemap.xml 자동 생성
  - robots.txt 설정
  - 페이지별 canonical URL 설정

- **Task 012: 사용자 경험 향상 기능**
  - 스켈레톤 로딩 UI 적용
  - 다크 모드 지원 (theme-toggle 활용)
  - 스크롤 진행도 표시 (포스트 상세 페이지)
  - 코드 블록 복사 버튼
  - 부드러운 페이지 전환 효과
  - 접근성(a11y) 개선

### Phase 5: 성능 최적화 및 배포

- **Task 013: 성능 최적화**
  - 이미지 최적화 (Next.js Image, blur placeholder)
  - 폰트 최적화 (`next/font` 활용)
  - 번들 크기 분석 및 최적화
  - 캐싱 전략 적용 (fetch cache, ISR)
  - Lighthouse 점수 측정 및 개선 (목표: 90+)
  - 페이지 로드 시간 < 2초 목표 달성 검증

- **Task 014: 빌드 검증 및 Vercel 배포**
  - 프로덕션 빌드 검증 (`npm run build`)
  - 환경 변수 Vercel 설정
  - Vercel 배포 파이프라인 구성
  - 도메인 연결 및 HTTPS 설정
  - 모니터링 및 분석 도구 연동 (Vercel Analytics)
  - 배포 후 Playwright MCP로 프로덕션 환경 E2E 테스트

## 📈 성공 지표 추적

- [ ] Notion 데이터베이스와 실시간 동기화 (ISR)
- [ ] 모든 포스트가 마크다운으로 올바르게 렌더링
- [ ] 카테고리/태그 필터링 정상 작동
- [ ] 모바일/태블릿/데스크탑 반응형 UI 완성
- [ ] 페이지 로드 시간 < 2초 (SSG/ISR 활용)
- [ ] Lighthouse 성능 점수 90+ 달성
- [ ] Claude AI 자동 요약 정확도 검증

## 🔮 향후 확장 계획 (MVP 이후)

- 사용자 댓글 시스템 (Giscus, Disqus 등)
- 풀텍스트 검색 기능 (Algolia, Meilisearch)
- 다국어 지원 (i18n)
- 소셜 공유 기능
- 이메일 뉴스레터 (Resend, Mailchimp)
- RSS 피드 생성
- 작성자 프로필 페이지
- 포스트 조회수 카운팅 시스템
