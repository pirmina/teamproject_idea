# Task 001: 프로젝트 라우트 구조 및 환경 설정

**상태**: pending
**우선순위**: 높음
**생성 날짜**: 2026-05-17

## 개요

Next.js App Router 기반 블로그 라우트 구조 생성 및 필수 패키지 설치

## 수락 기준

- [ ] `/blog`, `/blog/[slug]`, `/blog/category/[category]`, `/blog/tag/[tag]` 라우트 생성
- [ ] 각 페이지에 placeholder content가 있는 빈 껍데기 파일 생성
- [ ] `.env.local.example` 파일 작성 (Notion API Key, Database ID, Claude API Key)
- [ ] 필수 패키지 설치: `@notionhq/client`, `notion-to-md`, `@anthropic-ai/sdk`
- [ ] 블로그 공통 레이아웃 컴포넌트 (`src/app/blog/layout.tsx`) 골격 구현

## 구현 단계

1. 라우트 구조 생성
2. 각 페이지 placeholder 파일 생성
3. 패키지 설치
4. `.env.local.example` 작성
5. 블로그 공통 레이아웃 컴포넌트 구현

## 참고사항

- Next.js 15.5.3 App Router 사용
- TypeScript strict 모드 활용
- shadcn/ui 컴포넌트 활용
