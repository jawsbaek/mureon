# v0 프롬프트: Mureon AI FinOps Frontend

아래 지시를 정확히 따르세요. 변경 범위는 명시된 파일에 한정합니다.

## 컨텍스트
- 기술 스택: Next.js(App Router, TypeScript), Tailwind CSS, 서버 컴포넌트 우선, 클라이언트 최소화(React Query/서스펜스)
- 문서: `docs/prd.md`, `docs/front-end-spec.md`
- 라우트 섹션: `/finops`, `/platform`, `/product`
- NFR: 대시보드 p95<500ms(캐시 적중<200ms), WCAG AA, LCP p75<2.5s(데스크탑)

## 목표(High-Level)
MVP 화면 골격과 핵심 컴포넌트 셋을 생성합니다. 서버 컴포넌트 기반 SSR을 우선하며, 모바일 퍼스트로 반응형을 설계합니다.

## 작업 지시(Step-by-step)
1) 레이아웃/네비게이션
- `src/app/(mureon)/layout.tsx`: 공통 레이아웃(헤더/사이드/메인)과 전역 검색 입력을 포함
- `src/app/page.tsx`: 역할 기반 메인 개요(카드 4개 + 트렌드 placeholder)
- 접근성: SkipLink, Landmark(role) 지정, 키보드 포커스 이동 지원

2) 라우트 생성(SSR 우선)
- `/finops/page.tsx`: 단위경제 탐색 골격(좌측 필터 패널, 중앙 차트/표 placeholder)
- `/platform/page.tsx`: 이상탐지/라우팅/GPU 권장 섹션 링크 카드
- `/product/page.tsx`: 정책 승인/변경 이력/설정 링크 카드

3) 핵심 컴포넌트
- `src/components/ui/KpiCard.tsx`: 타이틀/값/증감/툴팁, a11y 레이블 포함
- `src/components/ui/FilterPanel.tsx`: 기간/팀/모델 필터(클라이언트 컴포넌트)
- `src/components/ui/DataTable.tsx`: 가상 스크롤 가능한 테이블 스텁(접근성 고려)
- `src/components/ui/Skeleton.tsx`: 카드/차트/테이블 스켈레톤 변형
- Tailwind 토큰 우선. 색/간격은 `docs/front-end-spec.md` 기준

4) 상태/로딩/오류
- 서버 컴포넌트에서 데이터 fetch 시 로딩 스켈레톤 우선 노출
- 오류 시 알림(라이브 영역) 및 재시도 버튼 제공

5) 성능/접근성 가드
- 이미지/SVG 최적화, 불필요한 클라 JS 배제, 인터랙션 <100ms
- 대비/포커스/레이블 검증, 폼 에러/도움말 연결

6) 테스트/예시 데이터
- 목 데이터 유틸 추가(`src/lib/mock.ts`)하여 카드/표 채우기 예시

## API/데이터 계약(임시)
- 서버 컴포넌트는 `getKpis()`/`getUnitEconomics()` 모킹 함수 사용. 실제 API 연결은 추후

## 산출물(파일 제한)
- 생성/수정 허용:
  - `src/app/(mureon)/layout.tsx`, `src/app/page.tsx`
  - `src/app/finops/page.tsx`, `src/app/platform/page.tsx`, `src/app/product/page.tsx`
  - `src/components/ui/{KpiCard,FilterPanel,DataTable,Skeleton}.tsx`
  - `src/lib/mock.ts`
- 그 외 파일 수정 금지(스타일은 Tailwind 유틸로 해결)

## 금지 사항
- 전역 상태 도입 금지, 무분별한 클아이드 컴포넌트 금지
- 디자인 시스템 무단 추가 금지(토큰 기반 Tailwind만)

## 완료 기준
- 페이지가 빌드/렌더되고 a11y 기본 가드 준수
- 모바일→데스크탑 반응형 확인
- KPI 카드 4개와 단위경제 기본 골격 확인
