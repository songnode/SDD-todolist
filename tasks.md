# Tasks: 할일 목록 웹앱 (로컬 저장/다크모드/반응형)

**Input**: 설계 문서 from `/specs/001-todo-list-core/`
- plan.md (필수)
- spec.md (필수)
- research.md
- data-model.md
- contracts/
- quickstart.md

**Tests**: 테스트는 필수입니다. 새 기능/행동 변경을 보호하는 단위 테스트(Vitest + React Testing Library)와 핵심 여정 E2E(Playwright)를 포함합니다.

**Organization**: 작업은 사용자 스토리(US1~US3) 단위로 그룹화하여 각 스토리가 독립적으로 구현/테스트/데모 가능하도록 구성합니다.

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: 프로젝트 공통 설정 및 작업 기반 마련

- [x] T001 `components/` 및 `lib/` 기본 폴더/인덱스 구성(필요 파일 생성 포함): `components/README.md`, `lib/README.md`
- [x] T002 Prettier 도입 및 스크립트 추가: `package.json`, `.prettierrc.json`, `.prettierignore`
- [x] T003 [P] ESLint와 Prettier 충돌 최소화 설정 정리: `eslint.config.mjs`
- [x] T004 [P] 테스트 실행 안정화를 위해 기존 테스트 스크립트/디렉토리 구조 점검(필요 시 문서화): `README.md`

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: 모든 사용자 스토리가 공유하는 코어 기반(타입/로직/스토리지/테마) 정의

**⚠️ CRITICAL**: 이 단계 완료 전에는 사용자 스토리 구현을 시작하지 않습니다.

- [x] T005 공통 타입 정의: `lib/todo/types.ts` (TodoItem, UserPreferences, Filter, StorageEnvelope, storageVersion)
- [x] T006 [P] 순수 도메인 로직 정의: `lib/todo/operations.ts` (add/toggle/delete/filter/sort: 최신 우선)
- [x] T007 [P] 입력 검증/정규화 로직 정의: `lib/todo/validation.ts` (title 공백 금지, description 처리)
- [x] T008 [P] 로컬 스토리지 어댑터 정의: `lib/todo/storage.ts` (safe load/save, 버전 체크, 손상 처리)
- [x] T009 [P] 테마 유틸 정의: `lib/theme/theme.ts` (시스템 선호 감지, `dark` 클래스 적용)
- [x] T010 [P] 접근성/일관된 UI를 위한 최소 UI 프리미티브 구성: `components/ui/button.tsx`, `components/ui/text-input.tsx`
- [x] T011 [P] 공통 상수 정의(스토리지 키, 기본 필터/테마): `lib/todo/constants.ts`

**Checkpoint**: Foundation ready - User Story 구현 시작 가능

---

## Phase 3: User Story 1 - 할일을 관리한다 (추가/완료/삭제/필터) (Priority: P1) 🎯 MVP

**Goal**: 사용자가 할일을 추가하고(제목/설명), 완료 체크/해제, 삭제, 필터(전체/진행중/완료)를 사용할 수 있다.

**Independent Test**: 할일 2개 추가 → 1개 완료 토글 → 필터 전환(진행중/완료) → 삭제가 정상 동작하는지 확인한다.

### Tests for User Story 1 (필수) ⚠️

> 테스트를 먼저 작성하고 실패(Red) 확인 후 구현합니다.

- [x] T012 [P] [US1] 도메인 로직 단위 테스트 작성(추가/토글/삭제/필터/정렬): `tests/unit/todo-operations.test.ts`
- [x] T013 [P] [US1] UI 단위 테스트 작성(추가 폼/버튼 disabled/필터/리스트 렌더): `tests/unit/todo-app.test.tsx`
- [x] T014 [P] [US1] E2E 테스트 작성(추가→토글→필터→삭제): `tests/e2e/todo-core.spec.ts`

### Implementation for User Story 1

- [x] T015 [P] [US1] 할일 입력 폼 컴포넌트 구현(제목/설명, Enter 지원): `components/todo-form.tsx`
	- 구현 체크리스트: `label`(또는 `aria-label`)로 "제목"/"설명" 이름 제공, Enter 제출 시도 경로 제공(버튼 클릭 외)
- [x] T016 [P] [US1] 필터 컨트롤 컴포넌트 구현(전체/진행중/완료): `components/todo-filters.tsx`
	- 구현 체크리스트: 버튼 이름(name)을 "전체"/"진행중"/"완료"로 안정화(또는 "필터: 전체" 등 접두어 포함 시 일관 유지)
- [x] T017 [P] [US1] 할일 아이템 컴포넌트 구현(체크/삭제 버튼, 접근성 라벨): `components/todo-item.tsx`
	- 구현 체크리스트: 체크박스 `aria-label`을 "완료: {title}", 삭제 버튼을 "삭제: {title}" 형태로 제공(항목 구분 가능)
- [x] T018 [P] [US1] 할일 리스트 컴포넌트 구현(빈 상태 포함): `components/todo-list.tsx`
- [x] T019 [US1] 상태/핸들러 결합(최신 우선, 즉시 삭제, 입력 검증 안내): `components/todo-app.tsx`
	- 구현 체크리스트: 안내 문구는 테스트에서 잡을 수 있도록 일관된 텍스트 또는 `role=alert` 제공
- [x] T020 [US1] 홈 페이지를 Todo 앱으로 교체(기존 GPT-5 데모 제거): `app/page.tsx`
- [x] T021 [US1] 기존 홈 단위 테스트를 Todo 앱 기준으로 갱신: `tests/unit/page.test.tsx`
- [x] T022 [US1] 기존 홈 E2E 테스트를 Todo 앱 기준으로 갱신 또는 대체: `tests/e2e/home.spec.ts`

**Checkpoint**: US1 단독으로 기능 시연/테스트 가능(MVP)

---

## Phase 4: User Story 2 - 새로고침해도 할일이 유지된다 (자동 저장/복원) (Priority: P2)

**Goal**: 할일/완료/필터/테마 상태가 로컬 스토리지에 자동 저장되고, 재진입/새로고침 시 복원된다.

**Independent Test**: 할일 1개 이상 추가 후 토글/필터/테마 변경 → 새로고침 → 동일 상태 복원을 확인한다.

### Tests for User Story 2 (필수) ⚠️

- [x] T023 [P] [US2] 스토리지 로직 단위 테스트 작성(정상 저장/복원, 손상 JSON, 버전 불일치): `tests/unit/todo-storage.test.ts`
- [x] T024 [P] [US2] E2E 테스트 작성(새로고침 후 복원): `tests/e2e/todo-persistence.spec.ts`

### Implementation for User Story 2

- [x] T025 [P] [US2] 안전 파싱/버전 체크/기본값 적용 로직 보강: `lib/todo/storage.ts`
- [x] T026 [US2] 앱 초기 로딩 시 로컬 저장값을 하이드레이션: `components/todo-app.tsx`
- [x] T027 [US2] 상태 변경 시 자동 저장 연결(예외 처리 포함): `components/todo-app.tsx`
- [x] T028 [US2] 로컬 데이터 손상/읽기 실패 시 사용자 안내 UI 추가: `components/storage-warning.tsx`

**Checkpoint**: US2 포함 시 “재방문 신뢰성”을 독립적으로 시연/테스트 가능

---

## Phase 5: User Story 3 - 환경에 맞는 화면을 제공한다 (다크모드/반응형) (Priority: P3)

**Goal**: 다크모드 토글과 반응형 레이아웃으로 모바일/데스크톱에서 쾌적하게 사용할 수 있다.

**Independent Test**: (1) 저장된 테마 없음 → 시스템 선호를 따르는지 확인, (2) 사용자 토글 후 재진입 시 저장값 우선 적용, (3) 모바일 뷰포트에서 핵심 작업 수행 가능.

### Tests for User Story 3 (필수) ⚠️

- [x] T029 [P] [US3] 테마 유틸 단위 테스트 작성(시스템 기본값, 저장값 우선 규칙): `tests/unit/theme.test.ts`
- [x] T030 [P] [US3] E2E 테스트 작성(테마 토글 + 모바일 뷰포트 핵심 여정): `tests/e2e/todo-ui.spec.ts`

### Implementation for User Story 3

- [x] T031 [P] [US3] 테마 토글 컴포넌트 추가(라이트/다크): `components/theme-toggle.tsx`
  [ ] [Bug] Fix: 다크모드 수동 전환 시 배경색 미적용 버그 수정
- [x] T032 [US3] 테마 적용 로직 구현(`document.documentElement`에 `dark` 클래스 적용, 시스템 기본값): `components/todo-app.tsx`
- [x] T033 [US3] 테마 저장/복원 규칙을 스토리지 엔벨로프에 반영(저장값 우선): `lib/todo/types.ts`, `lib/todo/storage.ts`
- [x] T034 [US3] 반응형 레이아웃 보완(v0 스타일 카드형, 모바일/데스크톱): `components/todo-app.tsx`, `components/todo-list.tsx`
- [x] T035 [US3] 전역 레이아웃/배경 대비 점검 및 보정: `app/layout.tsx`, `app/globals.css`

**Checkpoint**: 모든 스토리(US1~US3) 통합 동작 + 모바일/다크모드 검증 가능

---

## Phase 6: Polish & Cross-Cutting Concerns

**Purpose**: 다중 스토리에 걸친 정리/품질/문서

- [x] T036 [P] Out of Scope 중복 항목 정리(명세 문서 일관성): `specs/001-todo-list-core/spec.md`
- [x] T037 린트/타입 오류 0건 달성: `eslint.config.mjs`, `app/`, `components/`, `lib/`
- [x] T038 단위 테스트 전체 통과 및 필요 시 보강: `tests/unit/`
- [x] T039 E2E 테스트 전체 통과 및 안정화(Playwright web-first assertions 사용): `tests/e2e/`
- [x] T040 README 업데이트(앱 기능/스크립트/테스트 방법): `README.md`
- [x] T041 quickstart 검증 및 실제 명령과 동기화: `specs/001-todo-list-core/quickstart.md`

- [x] T042 [US1] E2E 키보드 내비게이션 시나리오 추가(핵심 플로우를 "키보드만"으로 완료 + 포커스 가시성 확인): `tests/e2e/todo-core.spec.ts`
	- 목표: 마우스 없이 Tab/Shift+Tab/Enter/Space만으로 추가→필터→완료 토글→삭제가 가능해야 함
	- 전제: 모든 컨트롤은 역할(role)과 이름(name)으로 선택 가능해야 함(getByRole 기반)
	- 권장 접근성 라벨(예시):
	  - 제목 입력: role=textbox, name="제목" (또는 label "제목")
	  - 설명 입력: role=textbox, name="설명" (멀티라인이면 textarea여도 name 동일)
	  - 추가 버튼: role=button, name="추가"
	  - 필터 버튼: role=button, name="전체" / "진행중" / "완료" (또는 "필터: 전체" 형태로 통일)
	  - 완료 체크박스: role=checkbox, name="완료: {title}" (항목별 구분 가능해야 함)
	  - 삭제 버튼: role=button, name="삭제: {title}" (항목별 구분 가능해야 함)
	- 시나리오(예시, 최소 요구):
	  1) 페이지 진입 후 Tab으로 “제목 입력”에 포커스가 이동하고, 텍스트 입력 가능
	  2) Enter로 “추가”가 동작(또는 Add 버튼까지 Tab 이동 후 Enter)하여 새 할일 1개 생성
	  3) Tab 이동으로 “필터: 완료” 버튼에 도달 → Enter로 완료 필터 적용(빈 상태도 허용)
	  4) “필터: 전체”로 돌아온 뒤, 첫 할일의 체크박스(완료 토글)에 Space로 토글
	  5) 같은 할일의 “삭제” 버튼까지 Tab 이동 → Enter로 삭제 → 목록에서 제거 확인
	- 포커스 가시성: Tab 이동 시 포커스가 시각적으로 식별되어야 함(예: outline/ring; 최소 1개 요소에서 확인)

- [x] T043 [US1] UI 단위 테스트에 키보드 상호작용 최소 1개 추가(Enter 제출/disabled 처리/안내 노출을 "테스트로 고정"): `tests/unit/todo-app.test.tsx`
	- 케이스 A(권장): 제목이 비어 있을 때 “추가”가 disabled이고, 제목 입력에 포커스 후 Enter를 눌러도 항목이 추가되지 않으며 안내 문구(예: "제목을 입력하세요")가 표시됨
	- 케이스 B(대안): 유효한 제목 입력 후 Enter로 제출 시 항목이 추가되고, 새 항목이 목록 상단에 렌더됨(최신 우선)
	- 선택자 원칙: Testing Library의 role/name 우선(버튼: role=button, 입력: role=textbox, 안내: role=alert 또는 텍스트)

- [x] T044 [Perf] SC-003 "측정/기록" 작업 추가(500개 seed 후 필터 전환 시간을 측정하고 로그/리포트로 남김; 초기엔 hard fail 금지): `tests/e2e/todo-performance.spec.ts`
- [x] T045 [Perf] 측정 방법/환경/기준값을 문서화(SC-003 근거 남김, 재현 방법 포함): `specs/001-todo-list-core/research.md`
- [x] T046 [Perf] (안정화 후) 성능 게이트 강화: CI에서 "악화 없음(회귀 방지)" 또는 환경 보정된 임계값으로 단계적 전환(플레이키 원인 분석 포함): `tests/e2e/todo-performance.spec.ts`

---

## Dependencies & Execution Order

### Phase Dependencies

- Phase 1(Setup) → Phase 2(Foundational) → Phase 3(US1) → Phase 4(US2) → Phase 5(US3) → Phase 6(Polish)

### User Story Dependencies

- US1(P1): Foundational 완료 후 시작 가능 (MVP)
- US2(P2): US1 위에 자연스럽게 얹히는 확장(저장/복원)으로, UI는 US1을 재사용한다
- US3(P3): 테마는 저장/복원 규칙과 결합하므로 US2 이후가 안전하지만, 반응형 스타일은 US1 이후 병행 가능

---

## Parallel Example (Story-based)

### US1 병렬 예시

- [P] 테스트 작성: `tests/unit/todo-operations.test.ts`, `tests/unit/todo-app.test.tsx`
- [P] 컴포넌트 작성: `components/todo-form.tsx`, `components/todo-list.tsx`, `components/todo-item.tsx`

### US2 병렬 예시

- [P] 스토리지 단위 테스트: `tests/unit/todo-storage.test.ts`
- [P] 저장/복원 유틸 구현: `lib/todo/storage.ts`

### US3 병렬 예시

- [P] 테마 단위 테스트: `tests/unit/theme.test.ts`
- [P] 테마 토글 UI: `components/theme-toggle.tsx`

---

## Implementation Strategy

### MVP First (User Story 1 Only)

1. Phase 1 + Phase 2 완료
2. US1 테스트 작성 → 실패 확인 → 구현
3. US1 체크포인트에서 데모/검증 후 다음 스토리 진행

### Incremental Delivery

- US1(핵심 기능) → US2(저장/복원) → US3(다크모드/반응형)

---

## Notes

- [P] 작업은 파일 충돌이 없고 의존성이 없는 경우에만 부여했습니다.
- Testing Library는 역할(role)/접근성 기반 쿼리를 우선합니다.
- Playwright는 web-first assertions(예: `toBeVisible`, `toBeChecked`, `toHaveText`)를 우선합니다.
