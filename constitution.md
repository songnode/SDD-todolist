<!--
Sync Impact Report:
Version: 1.0.0 → 1.1.0
Rationale: 코드 품질, 테스트 표준, 사용자 경험(UX) 일관성, 성능 요구사항을 중심으로 원칙과 거버넌스를 강화
Modified Principles:
  - I. Component-First Architecture → V. Component-First Architecture (우선순위 조정)
  - II. Test-First Development (필수 원칙) → II. Test Standards & Test-First (필수 원칙) (표준 구체화)
  - III. TypeScript Strict Mode → VI. TypeScript Strict Mode (우선순위 조정)
  - IV. React Compiler 최적화 원칙 → IV. Performance Budgets & Measurement (성능 요구사항으로 확장)
  - V. 한국어 우선 원칙 → VII. 한국어 우선 원칙 (우선순위 조정)
Added Sections:
  - Core Principles 내 신규 원칙: I. Code Quality & Maintainability, III. UX Consistency & Accessibility
Removed Sections: None
Templates Status:
  ✅ .specify/templates/plan-template.md (Constitution Check 구체화)
  ✅ .specify/templates/spec-template.md (기존 구조 유지, 헌법과 정렬)
  ✅ .specify/templates/tasks-template.md ("테스트 optional" 문구 제거 및 Test-First 정렬)
  ⚠ .specify/templates/commands/*.md (폴더/파일 미존재로 검증 불가)
Follow-up TODOs: None
-->

# todolist-app Constitution

## Core Principles

### I. Code Quality & Maintainability

코드 품질은 기능 구현과 동등한 우선순위를 가집니다. “작동한다”는 이유로 품질을 양보하지 않습니다.

- 모든 PR은 `lint` 및 TypeScript 컴파일 오류 0건을 만족해야 합니다
- 변경 범위를 최소화하고, 중복/우회 구현보다 단순하고 일관된 설계를 선택합니다
- 비즈니스 로직은 가능하면 `lib/`의 순수 함수로 분리하고, UI 컴포넌트는 표현과 사용자 상호작용에 집중합니다
- 에러/로딩/빈 상태는 구현의 일부로 취급하며, 누락 시 결함으로 간주합니다
- 리팩터링은 “동작 보존 + 테스트 통과”를 전제로 하며, 테스트 없이 구조를 크게 흔들지 않습니다

**근거**: 유지보수 비용을 낮추고, 변경을 안전하고 빠르게 만들기 위해서입니다.

### II. Test Standards & Test-First (필수 원칙)

테스트는 선택이 아니라 품질 게이트입니다. 모든 기능은 테스트로 보호되어야 합니다.

- 기본 순서: 테스트 작성 → 승인/합의 → 실패 확인(Red) → 구현(Green) → 리팩터링(Refactor)
- 단위 테스트(Vitest + Testing Library)로 로직/컴포넌트 행위를 우선 검증합니다
- E2E 테스트(Playwright)는 “핵심 사용자 여정”에 대해 최소 1개 이상 유지합니다
- 접근성/UX 관련 변경은 역할(role) 기반 쿼리(Testing Library) 및 키보드 내비게이션 시나리오를 포함해야 합니다
- 스냅샷(snapshot) 테스트는 회귀 가치를 증명할 수 있는 경우에만 사용하며 기본값이 아닙니다

**근거**: 회귀 방지와 자신 있는 리팩터링을 가능하게 합니다.

### III. UX Consistency & Accessibility

사용자 경험은 기능의 일부이며, 화면 간 일관성과 접근성을 비기능 요구사항이 아닌 “필수 요구사항”으로 취급합니다.

- UI는 Tailwind CSS 유틸리티를 기본으로 하고, 스타일은 일관된 패턴(간격/타이포/색상)을 따릅니다
- 입력/버튼/링크는 일관된 인터랙션(hover/focus/disabled/loading)을 제공해야 합니다
- 키보드만으로 주요 흐름을 완료할 수 있어야 하며, 포커스 상태는 항상 시각적으로 드러나야 합니다
- 의미 있는 시맨틱 요소와 접근성 속성(필요 시 ARIA)을 사용합니다
- 사용자 메시지(오류/성공/가이드)는 구체적이고 행동 가능해야 합니다

**근거**: 예측 가능한 UI는 학습 비용을 줄이고, 접근성은 모든 사용자에게 기본 품질을 보장합니다.

### IV. Performance Budgets & Measurement

성능은 “나중에”가 아니라 “처음부터” 관리합니다. 특히 Client JS 증가는 신중하게 다룹니다.

- Server Component를 기본으로 하며, Client Component는 상호작용이 필요한 범위로 최소화합니다
- 불필요한 클라이언트 상태/이펙트 사용을 피하고, 데이터 흐름은 단순하게 유지합니다
- 성능 관련 변경(렌더링 경로, 대형 컴포넌트, 리스트/필터링)은 측정 근거를 남깁니다
- 성능 목표(Web Vitals)는 LCP/CLS/INP 개선 또는 최소한 “악화 없음”을 원칙으로 합니다
- React Compiler 활성화 환경에서 `useMemo`/`useCallback`은 측정된 병목이 있을 때만 사용합니다

**근거**: 성능 문제는 늦게 발견할수록 비용이 폭증하며, 사용자 이탈로 직결됩니다.

### V. Component-First Architecture

모든 UI 기능은 독립적이고 재사용 가능한 컴포넌트로 시작합니다.

- 컴포넌트는 자체적으로 완결되어야 하며, 독립적으로 테스트 가능하고 문서화되어야 합니다
- 각 컴포넌트는 명확한 단일 책임(Single Responsibility)을 가져야 합니다
- Server Component를 기본으로 하며, 클라이언트 상호작용이 필요한 경우에만 Client Component(`"use client"`)를 사용합니다
- 컴포넌트 파일명은 kebab-case(예: `todo-list.tsx`), 컴포넌트명은 PascalCase(예: `TodoList`)를 사용합니다

**근거**: React Server Components의 장점을 살리고, 컴포넌트 경계로 복잡도를 제어합니다.

### VI. TypeScript Strict Mode

TypeScript strict mode를 활성화하여 타입 안정성을 보장합니다.

- 모든 컴포넌트, 함수, 변수에 명시적 타입을 지정합니다
- `any` 타입 사용을 금지하며, 불가피한 경우 명확한 근거(이슈/PR 설명)와 함께 정당화가 필요합니다
- Props, State, 함수 반환값 모두 타입을 선언합니다
- 타입 정의는 인터페이스(Interface) 또는 타입(Type)으로 명확히 분리합니다

**근거**: 타입은 문서이자 품질 게이트이며, 변경 비용을 낮춥니다.

### VII. 한국어 우선 원칙

개발자 커뮤니케이션 및 문서화는 한국어를 우선으로 합니다.

- AI 에이전트(Copilot)의 모든 답변, 설명, 코드 주석은 한국어로 작성합니다
- 커밋 메시지는 `type(scope): "한국어 설명"` 형식을 따릅니다 (예: `feat(impl): "TodoList 컴포넌트 구현"`)
- 코드 자체(변수명, 함수명, 컴포넌트명)는 영어를 사용합니다
- 기술 용어는 원어 표기가 더 명확한 경우 괄호 안에 병기합니다 (예: "상태 관리(state management)")

**근거**: 팀 내 명확한 의사소통과 빠른 컨텍스트 파악을 위해서입니다.

## Technology Stack Requirements

### Mandatory Stack

- **Framework**: Next.js 16.1.4 (App Router) - 이전 버전 또는 Pages Router 사용 금지
- **Language**: TypeScript 5 strict mode - JavaScript 파일 작성 금지
- **UI Library**: React 19 - 호환되지 않는 라이브러리 사용 시 마이그레이션 필요
- **Styling**: Tailwind CSS 4 - 인라인 스타일 또는 CSS-in-JS 지양
- **Testing**: Vitest (단위 테스트), Playwright (E2E 테스트) - Jest 사용 금지

### Recommended Practices

- 폰트 최적화: `next/font` 사용 (Geist Sans, Geist Mono)
- 이미지 최적화: `next/image` 컴포넌트 사용
- Path Alias: `@/*` 사용 (예: `import Home from '@/app/page'`)
- 환경 변수: `.env.local` 파일 사용, `.env.example`에 템플릿 작성

## Development Workflow & Quality Gates

### Pre-Implementation Gate

새 기능 개발 전 필수 체크리스트:

1. **명세 확인**: Spec 문서가 작성되고 사용자 시나리오가 명확히 정의되었는가?
2. **테스트 계획**: 단위 테스트 및 E2E 테스트 케이스가 포함되고, “먼저 작성”에 합의했는가?
3. **UX 체크**: 로딩/에러/빈 상태, 키보드 포커스, 반응형 동작이 정의되었는가?
4. **성능 체크**: Client Component 도입이 최소화되었고, 악화 가능 지점(리스트/필터/렌더링 경로)을 식별했는가?
5. **아키텍처 검토**: Server Component vs Client Component 선택이 적절한가?
6. **타입 설계**: Props, State, API 응답 타입이 사전에 정의되었는가?

### Implementation Workflow

1. **테스트 작성**: 기능 구현 전 테스트 코드를 먼저 작성합니다 (Red)
2. **최소 구현**: 테스트를 통과하는 최소한의 코드를 작성합니다 (Green)
3. **리팩토링**: 코드 품질 개선 및 최적화를 수행합니다 (Refactor)
4. **문서화**: 복잡한 로직은 주석 또는 별도 문서로 설명합니다
5. **검증**: ESLint, TypeScript 컴파일, 테스트가 모두 통과하는지 확인합니다

### Code Review Standards

모든 Pull Request는 다음 기준을 충족해야 합니다:

- [ ] TypeScript strict mode 오류 없음
- [ ] ESLint 경고/오류 없음
- [ ] 단위 테스트 작성 및 통과 (새 기능/변경사항)
- [ ] E2E 테스트 통과 (핵심 사용자 여정/UX 변경 시)
- [ ] 로딩/에러/빈 상태 처리 포함
- [ ] 키보드 포커스/시맨틱 마크업 등 접근성 고려됨
- [ ] 성능 악화 요인(클라이언트 JS 증가, 불필요 렌더링) 점검 및 근거 기록
- [ ] 한국어 커밋 메시지 및 주석
- [ ] 불필요한 `useMemo`/`useCallback` 제거됨

## Governance

### Constitution Authority

이 헌법은 모든 개발 관행과 코딩 표준보다 우선합니다.

- 헌법과 상충하는 코드는 리뷰에서 반려됩니다
- 예외 사항은 명확한 근거 문서화와 팀 합의가 필요합니다
- AI 에이전트(Copilot)는 이 헌법을 최우선 지침으로 따릅니다

### Technical Decision Governance

이 헌법의 원칙은 “기술적 결정”과 “구현 선택”의 기본 판단 기준입니다.

- PR은 최소한 아래 항목에 대해 근거를 남겨야 합니다:
  - **코드 품질**: 구조가 단순한가? 중복/우회가 없는가?
  - **테스트**: 어떤 테스트가 추가/수정되었는가? 핵심 여정은 E2E로 보호되는가?
  - **UX**: 로딩/에러/빈 상태와 키보드 포커스가 일관적인가?
  - **성능**: Client Component/클라이언트 JS 증가가 정당화되는가? 악화 없음이 확인되는가?
- 원칙을 예외 처리해야 하는 경우, PR 설명에 다음을 반드시 포함합니다:
  - 예외 대상 원칙
  - 예외 사유(대안 비교 포함)
  - 영향 범위(UX/성능/테스트)
  - 추후 해소 계획(가능하면 이슈 링크)

### Amendment Process

헌법 수정은 다음 절차를 따릅니다:

1. **제안**: 변경 사항에 대한 명확한 근거와 영향 범위 분석
2. **검토**: 기존 템플릿, 문서, 코드베이스와의 일관성 확인
3. **승인**: 팀 합의 (또는 프로젝트 소유자 승인)
4. **마이그레이션**: 영향받는 파일 업데이트 계획 수행
5. **버전 업데이트**: Semantic Versioning에 따른 헌법 버전 증가

### Versioning Policy

헌법 버전은 **MAJOR.MINOR.PATCH** 형식을 따릅니다:

- **MAJOR**: 기존 원칙 제거 또는 호환되지 않는 변경 (예: 필수 기술 스택 변경)
- **MINOR**: 새로운 원칙 추가 또는 섹션 확장 (예: 새로운 품질 게이트 추가)
- **PATCH**: 문구 명확화, 오타 수정, 예시 개선 등 비의미론적 변경

### Compliance Review

준수 여부는 다음 시점에 검증됩니다:

- Pull Request 검토 시 필수 체크리스트 항목으로 확인
- 분기별 코드베이스 감사 (자동화 린트 규칙 + 수동 샘플링)
- 주요 기능 출시 전 아키텍처 검토


---

**Version**: 1.1.0 | **Ratified**: 2026-08-20 | **Last Amended**: 2026-08-20
