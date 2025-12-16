# PWD Week 2 – SvelteKit Mini Portfolio

수업 과제로 만든 SvelteKit 기반 미니 포트폴리오입니다. Svelte 5 러닝 포인트(라우팅, rune 상태, API 라우트, 동적 페이지, 로컬스토리지 활용)를 한 번에 연습할 수 있게 구성했습니다.

## 주요 기능
- 홈: 이름 입력 시 상태/파생값을 통해 미리보기 업데이트, 랜덤 메시지 버튼 제공.
- 공통 레이아웃: 심플한 네비게이션(Home / About / Projects)과 페이지 메타 설정.
- About: 과제에서 연습한 항목(시맨틱 HTML, 라우팅, 상태 등) 소개.
- Projects 목록: `/api/projects`에서 JSON을 받아 카드 형태로 렌더링.
- 프로젝트 상세: `[slug]` 동적 라우트, 없는 슬러그는 404 처리.  
  - `memo`: 로컬스토리지에 메모 저장/복원.  
  - `timetable`: 교시별 시간 자동 계산, 입력값 로컬스토리지 저장.

## 기술 스택
- SvelteKit 2 / Svelte 5 runes (`$state`, `$derived`, `$effect`)
- Vite 빌드/개발 서버
- 스타일: 기본 CSS(`src/app.css`) 기반의 간단한 카드/레이아웃
- 품질 도구: ESLint + Prettier, svelte-check, Vitest(단위) + Playwright(E2E)

## 시작하기
```sh
npm install
npm run dev -- --open   # 개발 서버 실행(브라우저 자동 오픈)
```

## 유용한 스크립트
- `npm run build` : 프로덕션 빌드
- `npm run preview` : 빌드 결과 로컬 미리보기
- `npm run check` : svelte-check로 타입/문법 점검
- `npm run lint` : Prettier + ESLint 검사
- `npm run format` : Prettier로 포맷팅
- `npm test` : Vitest 단위 테스트 후 Playwright E2E 실행

## 구조 메모
- `src/routes/+layout.svelte` : 전역 레이아웃/네비게이션
- `src/routes/+page.svelte` : 홈, 상태/파생값/이펙트 기본 연습
- `src/routes/projects/+page.js` : 프로젝트 목록 로딩(fetch `/api/projects`)
- `src/routes/projects/[slug]/+page.server.js` : 슬러그별 데이터 조회, 404 처리
- `src/routes/api/projects/+server.js` : 예제 프로젝트 목록 API
