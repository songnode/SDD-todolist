# Implementation Plan: 할일 목록 웹앱 (로컬 저장/다크모드/반응형)

**Branch**: `001-todo-list-core` | **Date**: 2026-08-20 | **Spec**: [specs/001-todo-list-core/spec.md](spec.md)
**Input**: Feature specification from `/specs/001-todo-list-core/spec.md`

**Note**: This template is filled in by the `/speckit.plan` command. See `.specify/templates/commands/plan.md` for the execution workflow.

## Summary

- 목적: 로컬에서 동작하는 개인용 할일 목록 앱을 제공한다(추가/완료 토글/삭제/필터).
- 상태: 할일/필터/테마를 로컬 스토리지에 자동 저장하고, 재진입 시 복원한다.
- UX: 다크모드는 (1) 최초엔 시스템 선호를 따르고 (2) 사용자가 선택하면 저장값을 우선한다.
- 검증: Vitest + React Testing Library로 주요 로직/컴포넌트를 보호하고, Playwright로 핵심 사용자 여정을 E2E로 보호한다.

## Technical Context

**Language/Version**: TypeScript 5 (strict mode)  
**Primary Dependencies**: Next.js 16.1.4 (App Router), React 19, Tailwind CSS 4  
**Storage**: Browser Local Storage (할일/필터/테마 자동 저장 및 복원)  
**Testing**: Vitest, React Testing Library, Playwright  
**Target Platform**: 모던 브라우저(데스크톱/모바일), 오프라인 동작  
**Project Type**: Web application (Next.js App Router)  
**Performance Goals**: 할일 500개 기준 필터 변경 후 0.2초 이내 화면 반영(SC-003)  
**Constraints**: Client Component 최소화(상호작용 범위만), 로컬 저장 실패/손상에도 앱 중단 금지(FR-008)  
**Scale/Scope**: 단일 사용자(개인), 단일 페이지 중심 UI

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

- [x] 코드 품질: 로직은 `lib/` 순수 함수로 분리하고 UI는 표현/상호작용에 집중한다
- [x] 테스트 표준: 핵심 로직/컴포넌트 단위 테스트 + 핵심 사용자 여정 E2E 1개 이상을 계획한다
- [x] UX 일관성: 빈 상태/오류(로컬 데이터 손상)/입력 검증 안내/키보드 사용성을 명시한다
- [x] 성능 요구사항: 리스트/필터링을 단순한 배열 연산으로 유지하고 불필요 렌더링을 피한다
- [x] 예외 처리: 원칙 위반이 필요해지면 Complexity Tracking에 근거를 남긴다

**Post-Design Re-check (after Phase 1)**: Phase 1 산출물(`research.md`, `data-model.md`, `contracts/`, `quickstart.md`)을 생성했으며, 현재 설계는 헌법 원칙(로직의 `lib/` 분리, 테스트 우선, UX 일관성, Client 최소화)과 충돌하지 않는다.

## Project Structure

### Documentation (this feature)

```text
specs/001-todo-list-core/
├── plan.md              # This file (/speckit.plan command output)
├── research.md          # Phase 0 output (/speckit.plan command)
├── data-model.md        # Phase 1 output (/speckit.plan command)
├── quickstart.md        # Phase 1 output (/speckit.plan command)
├── contracts/           # Phase 1 output (/speckit.plan command)
└── tasks.md             # Phase 2 output (/speckit.tasks command - NOT created by /speckit.plan)
```

### Source Code (repository root)
```text
app/
  layout.tsx
  page.tsx
  globals.css

components/
  # 재사용 UI 컴포넌트(예: todo-form.tsx, todo-list.tsx)

lib/
  # 순수 함수/도메인 로직(예: todo 모델/필터/스토리지 직렬화)

tests/
  unit/
  e2e/
  setup.ts
```

**Structure Decision**: Next.js App Router 기반 단일 웹앱 구조를 사용하며, UI는 `components/`, 도메인 로직은 `lib/`, 테스트는 `tests/`에 배치한다.

## Complexity Tracking

> **Fill ONLY if Constitution Check has violations that must be justified**

| Violation | Why Needed | Simpler Alternative Rejected Because |
|-----------|------------|-------------------------------------|
| [e.g., 4th project] | [current need] | [why 3 projects insufficient] |
| [e.g., Repository pattern] | [specific problem] | [why direct DB access insufficient] |
