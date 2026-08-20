# GitHub Spec Kit으로 배우는 Spec Driven Development (SDD)

---

## 📖 왜 Spec Driven Development인가?

전통적인 "코드부터 짜고 나중에 문서화" 방식은 요구사항이 모호한 채로 구현이 진행되어 재작업이 잦습니다. **Spec Driven Development(SDD)**는 반대로 접근합니다.

```
문제 정의 → 스펙 작성 → 명확화 → 설계/계획 → 작업 분해 → TDD 구현 → 정합성 검증
(Specify)   (Specify)   (Clarify)  (Plan)      (Tasks)     (Implement) (Analyze)
```

핵심은 **"코드를 쓰기 전에 무엇을 만들지 합의부터 한다"**는 것이며, GitHub Spec Kit은 이 과정을 슬래시 커맨드(`/speckit.*`)로 표준화한 도구입니다. AI 코딩 어시스턴트(Copilot Chat 등)와 함께 사용하면 스펙 → 계획 → 작업 → 코드까지 일관된 흐름으로 이어집니다.

### Spec Kit 6단계 핵심 커맨드 요약

| 단계 | 커맨드 | 산출물 | 한 줄 설명 |
|---|---|---|---|
| 0 | `speckit.constitute` | `constitution.md` | 프로젝트 원칙(품질/테스트/UX/성능) 수립 |
| 1 | `/speckit.specify` | `spec.md` | 무엇을(What) 만들지 사용자 스토리로 정의 (구현 방법은 배제) |
| 2 | `/speckit.clarify` | `spec.md`(갱신) | 최대 5개 질문으로 모호한 부분 확정 |
| 3 | `/speckit.plan` | `plan.md`, `research.md`, `data-model.md`, `contracts/` | 어떻게(How) 만들지 기술 스택/구조 결정 |
| 4 | `/speckit.tasks` | `tasks.md` | 실행 가능한 작업 단위로 분해 (TDD 순서 포함) |
| 5 | `speckit.analyze` | 분석 리포트 | 스펙-계획-작업 간 누락/불일치 점검 |
| 6 | `/speckit.implement` | 코드 + 테스트 | Red(실패 테스트) → Green(구현) 반복으로 완성 |

이 자료는 **Part 1**에서 이미 완성된 프로젝트(할일 목록 웹앱)를 통해 이 6단계 산출물이 실제로 어떻게 생기는지 "읽고 확인"하고, **Part 2**에서는 신규 기능(편집 기능)을 학생이 **직접 처음부터 끝까지** 이 사이클로 만들어보는 구조입니다.

---

## 🎯 통합 학습 목표

- Spec Kit의 6단계 워크플로우(헌법 → 스펙 → 명확화 → 계획 → 작업 → 분석/구현)를 이해한다
- 스펙 문서에 **"무엇을"**만 담고 **"어떻게"**는 배제하는 습관을 기른다
- 명확화(Clarify) 단계를 통해 요구사항의 모호성을 제거하는 경험을 한다
- 계획(Plan)과 작업(Tasks) 산출물이 어떻게 서로 연결되는지 파악한다
- **TDD(Red → Green) 사이클**을 실제 작업 단위로 체득한다
- 스펙 ↔ 계획 ↔ 작업 ↔ 구현이 서로 정합성을 유지해야 하는 이유를 이해한다

---

## 📋 사전 준비사항

- Node.js 20 이상
- Git 설치 및 이 저장소 클론
- VS Code + GitHub Copilot Chat 확장
- (Part 2 진행 전) Part 1 실습 완료 및 기본 애플리케이션(`001-todo-webapp`) 정상 실행 확인

```bash
# 저장소 클론 및 의존성 설치
git clone <repo-url>
cd <repo-name>
npm install

# 정상 동작 확인
npm run dev
# → http://localhost:3000 접속되면 준비 완료
```

---

# Part 1. Spec Kit 워크플로우 이해하기

## 1단계: `speckit.constitute` — 프로젝트 헌법 수립

**목표**: 코드 품질, 테스트 기준, UX 일관성, 성능 원칙을 수립하고 프로젝트 전반의 거버넌스를 정의합니다. 이는 이후 모든 스펙/계획/작업이 지켜야 할 "상위 규칙"입니다.

**확인 파일**
- 헌법 파일: `/.specify/memory/constitution.md`
- 템플릿 동기화: `/.specify/templates/plan-template.md`, `/.specify/templates/spec-template.md`, `/.specify/templates/tasks-template.md`

**핵심 확인 포인트**
- [ ] 헌법 원칙(코드 품질/테스트/UX/성능)이 명시되어 있는가
- [ ] Governance 규칙과 버전 정책이 정의되어 있는가
- [ ] Plan/Spec/Tasks 템플릿에 헌법 체크가 반영되어 있는가

---

## 2단계: `/speckit.specify` — 기능 스펙 작성

**목표**: "할일 목록 웹앱" 기능을 스펙으로 문서화합니다.

**확인 파일**
- 스펙: `/specs/001-todo-webapp/spec.md`
- 요구사항 체크리스트: `/specs/001-todo-webapp/checklists/requirements.md`

**핵심 확인 포인트**
- [ ] 사용자 스토리(P1~P3)가 독립적으로 테스트 가능한가
- [ ] 기능 요구사항, 엣지 케이스, 성공 기준이 명확한가
- [ ] 구현 세부사항이 과도하게 들어가지 않았는가 (예: "React useState로..." 같은 표현이 없어야 함)

> 💡 **강의 포인트**: 스펙은 "무엇을(What)"과 "왜(Why)"에 집중합니다. "어떻게(How)"가 섞이면 나중에 계획(Plan) 단계에서 기술적 선택의 자유가 줄어듭니다.

---

## 3단계: `speckit.clarify` — 핵심 질문으로 명확화

**목표**: 스펙의 모호성을 제거하기 위해 최대 5개의 질문을 통해 결정사항을 확정합니다.

**이번 실습에서 반영된 결정**

| 항목 | 결정 |
|---|---|
| 기본 정렬 | 미완료 우선, 완료는 하단 분리 |
| 다크모드 기본값 | 시스템 설정 → 사용자 전환 시 저장 |
| 완료 토글 시 이동 | 상태 변경 시 섹션 이동 |
| 삭제 정책 | 삭제 전 확인 모달 |
| 스토리지 실패 시 | 경고 표시 후 세션 내 임시 상태로 동작 |

**확인 파일**
- 명확화 기록: `/specs/001-todo-webapp/spec.md` (Clarifications 섹션)

**핵심 확인 포인트**
- [ ] `spec.md`의 Clarifications 섹션에 결정사항이 기록되었는가
- [ ] 모호했던 표현이 구체적인 규칙으로 바뀌었는가

---

## 4단계: `/speckit.plan` — 구현 계획 및 설계 산출물 생성

**목표**: 기술 스택, 프로젝트 구조, 설계 산출물을 정리하고 구현 계획을 수립합니다.

**확인 파일**
- 구현 계획: `/specs/001-todo-webapp/plan.md`
- 리서치: `/specs/001-todo-webapp/research.md`
- 데이터 모델: `/specs/001-todo-webapp/data-model.md`
- 계약(로컬 스토리지/동작): `/specs/001-todo-webapp/contracts/local-storage.schema.json`, `/specs/001-todo-webapp/contracts/ui-actions.md`
- 퀵스타트: `/specs/001-todo-webapp/quickstart.md`
- 에이전트 컨텍스트: `.github/agents/copilot-instructions.md`

**핵심 확인 포인트**
- [ ] 기술 스택이 요구사항과 일치하는가 (Next.js, React, TS, Tailwind 등)
- [ ] 성능 예산/제약 사항이 계획에 반영되었는가
- [ ] 프로젝트 구조가 실제 코드베이스와 일치하는가

---

## 5단계: `/speckit.tasks` — 실행 가능한 작업 목록 생성

**목표**: 사용자 스토리별로 독립 구현이 가능한 작업 리스트를 작성합니다.

**확인 파일**
- 작업 목록: `/specs/001-todo-webapp/tasks.md`

**핵심 확인 포인트**
- [ ] 작업이 사용자 스토리별로 분리되어 있는가
- [ ] 테스트 작업이 먼저 정의되어 있는가 (TDD)
- [ ] 파일 경로가 명시되어 있는가

---

## 6단계: `speckit.analyze` — 스펙/계획/작업 정합성 분석

**목표**: 스펙, 계획, 작업 목록 간의 누락/모호성/불일치를 분석하고 수정 후보를 도출합니다.

**확인 파일**
- 분석 결과는 Copilot Chat 응답으로 제공되며, 필요 시 문서를 보정합니다.

**핵심 확인 포인트**
- [ ] 요구사항 대비 작업 누락이 없는가 (커버리지 100%)
- [ ] 모호한 문구/표현이 남아 있지 않은가
- [ ] 헌법 원칙(품질/테스트/UX/성능)이 작업에 반영되었는가
- [ ] typecheck/lint 기준이 명확하게 고정되어 있는가

---

## 7단계: `/speckit.implement` — 작업 구현 및 검증

**목표**: Tasks 문서에 정의된 작업을 실제 코드/테스트로 구현하고, lint/typecheck/test 기준으로 품질을 검증합니다.

**이번 실습에서 구현된 핵심 산출물(예시)**

```
app/
  page.tsx                 # 앱 조립(메인 화면)
  layout.tsx                # 레이아웃/메타데이터
lib/
  types/todo.ts, preferences.ts
  utils/validation.ts       # 유효성 검증
  utils/storage.ts          # 로컬 스토리지
  utils/filter.ts           # 필터 로직
  utils/theme.ts            # 다크모드
  hooks/useTodos.ts, usePreferences.ts
components/
  TodoForm.tsx / TodoList.tsx / TodoItem.tsx
  FilterTabs.tsx / ThemeToggle.tsx
  ConfirmModal.tsx / EmptyState.tsx / StorageErrorBanner.tsx
```

**중간 버그 수정 사례 (Copilot Chat 활용) — Spec-Kit 정석 패턴**

실습 도중 발생한 버그는 "Task Update → TDD" 패턴으로 해결합니다.

1. **상황**: 다크모드 수동 전환 시 HTML 클래스는 변하지만 실제 배경색이 변경되지 않음 (테스트 누락)
2. **절차**: `tasks.md`에 `[Bug] Fix: 다크모드...` 항목 추가 → Copilot Chat에 "TDD 절차(Test 강화 → Verify(Red 확인) → Fix → Update)"로 수정 요청
3. **결과**:
   - 테스트 강화: `/tests/e2e/theme-responsive.spec.ts`에 `toHaveCSS` 검증 추가
   - 구현 수정: `/app/globals.css`에 `@custom-variant dark` 추가 (Tailwind v4 대응)
   - 작업 완료: `tasks.md` 업데이트 완료

> 💡 **강의 포인트**: 버그가 발견되면 바로 코드를 고치지 않고, **테스트를 먼저 강화해서 실패(Red)를 재현**한 뒤 고치는 것이 SDD/TDD의 핵심 습관입니다.

**테스트 산출물(예시)**
- Unit: `validation.test.ts`, `storage.test.ts`, `filter.test.ts`
- Hook Unit: `useTodos.test.tsx`
- Component: `TodoForm.test.tsx`, `FilterTabs.test.tsx`, `ThemeToggle.test.tsx`
- Integration: `todo-flow.test.tsx`, `filtering.test.tsx`, `theme.test.tsx`
- E2E: `todo-core.spec.ts`, `filtering.spec.ts`, `theme-responsive.spec.ts` (버그 수정 포함)

**실행/검증 명령**

```bash
npm run dev              # 개발 서버
npm run lint              # 정적 검증
npx tsc --noEmit          # 타입 체크
npm run test               # 테스트 (또는 npx vitest run)
npm run test:e2e           # E2E
```

> 참고: 테스트 실행 시 출력이 부족하게 보이면 `npx vitest run --reporter verbose`로 더 자세한 로그를 확인할 수 있습니다.

**핵심 확인 포인트 (스펙 ↔ 구현 매핑)**
- [ ] CreateTodo/Toggle/Delete 흐름이 정상 동작하고 새로고침 후 복원되는가
- [ ] 필터(all/active/completed) 전환 및 저장/복원이 동작하는가
- [ ] 테마(system/user) 전환, 저장/복원, 시스템 변경 동기화가 동작하는가
- [ ] 로컬 스토리지 실패 시 경고 표시 후 세션 내 상태로 동작하는가
- [ ] 정렬 규칙(미완료 우선)이 일관되게 적용되는가

## ✅ Part 1 체크리스트

- [ ] 헌법과 템플릿 동기화 파일을 확인했다
- [ ] 스펙 문서가 사용자 스토리/요구사항/성공 기준을 포함한다
- [ ] Clarify 결정이 스펙에 반영되었다
- [ ] Plan 산출물(리서치/데이터 모델/계약/퀵스타트)을 확인했다
- [ ] Tasks 문서가 사용자 스토리별 작업과 테스트를 포함한다
- [ ] Analyze 결과에서 누락/모호성/불일치를 확인하고 필요 시 보정했다
- [ ] Implement 산출물(코드/테스트)이 Tasks와 일치하는지 확인했다
- [ ] lint/typecheck/test 실행으로 품질을 검증했다

---

# Part 2. 신규 기능을 위한 완전한 SDD 사이클 직접 실습


## 🎬 실습 시나리오

**상황**: 사용자가 "할일 목록 앱에 편집 기능을 추가해주세요"라고 요청했습니다.
**목표**: Spec Kit 워크플로우를 따라 편집 기능을 완전히 구현하고 배포 가능한 상태로 만듭니다.

## 0단계: 기준선 확인

**목표**: 기존 애플리케이션이 정상 동작하는지 확인합니다.

```bash
npm run dev
# 브라우저에서 http://localhost:3000 접속
```

**예상 실행 결과**
```
▲ Next.js 16.x
- Local:        http://localhost:3000
- ready in 1.2s
```

**확인 포인트**
- [ ] 할일 추가 기능 동작
- [ ] 완료 토글 기능 동작
- [ ] 삭제 기능 동작
- [ ] 새로고침 후 데이터 유지

---

## 1단계: `/speckit.specify` — 신규 기능 스펙 작성

**목표**: "할일 편집 기능"을 명확한 스펙 문서로 작성합니다.

**실행 방법**
```
Copilot Chat에서:
/speckit.specify 사용자가 기존 할일의 제목과 설명을 편집할 수 있는 기능 추가
```

**생성되는 파일**
- 스펙: `/specs/002-edit-todo/spec.md`
- 요구사항 체크리스트: `/specs/002-edit-todo/checklists/requirements.md`

**핵심 확인 포인트**
- [ ] User Story가 우선순위별(P1~P3)로 정의되어 있는가
  - **US1 (P1)**: 기존 할일 편집 (인라인 편집, 저장/취소)
  - **US2 (P2)**: 편집 중 유효성 검증 (빈 제목, 길이 초과 차단)
  - **US3 (P3)**: 편집 중 다른 작업 방지 (버튼 비활성화)
- [ ] Functional Requirements가 테스트 가능한가 (예: FR-001 편집 버튼 표시, FR-005 제목 1~100자/설명 0~500자 검증, FR-011 편집 중 다른 버튼 비활성화)
- [ ] Edge Cases가 명시되어 있는가 (ESC 취소, Enter 저장, textarea Enter=줄바꿈, 완료 항목도 편집 가능, 필터 상태 유지)
- [ ] 구현 세부사항이 배제되어 있는가 (React, hooks, state 등 미언급)

**실습 활동**
```bash
code specs/002-edit-todo/spec.md
code specs/002-edit-todo/checklists/requirements.md
```

---

## 2단계: `/speckit.clarify` — 핵심 질문으로 명확화

**목표**: 스펙의 모호한 부분을 질문을 통해 명확히 합니다.

**실행 방법**
```
Copilot Chat에서:
/speckit.clarify
```

**이번 실습에서 반영된 결정사항**

| 질문 | 결정 |
|---|---|
| 편집 모드 진입 방식은? | 각 항목에 별도 "편집" 버튼 추가 (명시적 액션) |
| 다중 줄 설명 편집 시 Enter 키 동작은? | 설명은 textarea, Enter는 줄바꿈, 저장은 버튼만 |
| 편집 중 변경사항 유실 방지 정책은? | ESC/취소만 변경 무시, 다른 동작은 자유롭게 허용 |

**확인 포인트**
- [ ] `spec.md`의 Clarifications 섹션에 결정사항이 기록되었는가
- [ ] 모든 `[NEEDS CLARIFICATION]` 마커가 제거되었는가
- [ ] 결정사항이 User Story와 Requirements에 반영되었는가

---

## 3단계: `/speckit.plan` — 구현 계획 및 설계

**목표**: 기술 스택, 아키텍처, 설계 산출물을 작성합니다.

**실행 방법**
```
Copilot Chat에서:
/speckit.plan
```

**생성되는 파일**
- 계획: `/specs/002-edit-todo/plan.md`
- 리서치: `/specs/002-edit-todo/research.md`
- 데이터 모델: `/specs/002-edit-todo/data-model.md`
- 계약: `/specs/002-edit-todo/contracts/ui-actions.md`
- 퀵스타트: `/specs/002-edit-todo/quickstart.md`

**핵심 확인 포인트**

`plan.md`
- [ ] 기술 스택이 기존 애플리케이션과 일치하는가 (Next.js 16, React 19, TypeScript 5, Tailwind CSS 4)
- [ ] 구현 전략이 명확한가 (인라인 편집, 기존 `validation.ts` 재사용, `editingId` 상태로 동시 편집 방지)
- [ ] 성능 예산이 정의되어 있는가 (편집 모드 전환 100ms 이내, 저장 후 UI 업데이트 50ms 이내)

`data-model.md`
- [ ] 기존 `TodoItem` 타입에 `updatedAt` 필드가 추가되었는가
- [ ] `EditingState` 관리 방식이 명시되어 있는가

`contracts/ui-actions.md`
- [ ] `updateTodo` 동작이 명확히 정의되어 있는가 (입력: id, title, description / 출력: updatedAt 갱신, 로컬 스토리지 저장)

`quickstart.md`
- [ ] 개발 환경 설정 방법이 명시되어 있는가
- [ ] TDD 구현 순서가 제시되어 있는가
- [ ] 수동 테스트 체크리스트가 포함되어 있는가

---

## 4단계: `/speckit.tasks` — 실행 가능한 작업 목록 생성

**목표**: 구현 가능한 단위 작업으로 분해하고 TDD 순서를 정의합니다.

**실행 방법**
```
Copilot Chat에서:
/speckit.tasks
```

**생성되는 파일**: `/specs/002-edit-todo/tasks.md`

**Phase 구조**
- [ ] Phase 1-2: Setup & Foundational
- [ ] Phase 3: User Story 1 (P1 - MVP)
- [ ] Phase 4: User Story 2 (P2)
- [ ] Phase 5: User Story 3 (P3)
- [ ] Phase 6: Polish & Cross-Cutting

**TDD 패턴**: 각 Phase마다 "테스트 작성(Red) → 구현(Green)" 순서 (예: T007-T009 테스트 → T010-T017 구현)

**작업 세부사항**
- [ ] 모든 작업에 정확한 파일 경로가 명시되어 있는가 (예: `T010 [US1] updateTodo 함수 구현 in lib/hooks/useTodos.ts`)
- [ ] 검증 방법이 명시되어 있는가 (예: `npm run test:unit -- useTodos.test.tsx`)
- [ ] 병렬 실행 가능한 작업에 `[P]` 마킹이 있는가

**Phase 3 (US1 - MVP) 상세**

```
테스트:
- T007: useTodos 훅 테스트 (updateTodo 함수)
- T008: TodoItem 컴포넌트 테스트 (편집 모드)
- T009: 편집 흐름 통합 테스트

구현:
- T010: updateTodo 함수 구현
- T011: TodoItem 편집 모드 상태 추가
- T012: TodoItem 편집 모드 UI 구현
- T013: 편집 버튼 추가
- T014: 저장 로직 구현
- T015: 취소 로직 구현
- T016: TodoList editingId 상태 관리
- T017: E2E 테스트
```

---

## 5단계: `/speckit.implement` — TDD 기반 구현

**목표**: `tasks.md`의 작업 순서대로 TDD를 따라 구현합니다.

```
Copilot Chat에서:
/speckit.implement 진행
```

### Phase 3: User Story 1 — 기본 편집 기능 (MVP)

**Step 1: 테스트 작성 (Red)**

```bash
# T007: useTodos 훅 테스트
T007 작업 진행해줘
npm run test:unit -- useTodos.test.tsx
```
**예상 실행 결과 (Red)**
```
FAIL  tests/unit/hooks/useTodos.test.tsx
  ✗ updateTodo() should update title and description
  ✗ updateTodo() should refresh updatedAt timestamp
  Tests: 2 failed, 2 total
```

```bash
# T008: TodoItem 컴포넌트 테스트
T008 작업 진행해줘
npm run test:component -- TodoItem.test.tsx

# T009: 통합 테스트
T009 작업 진행해줘
npm run test:integration -- todo-flow.test.tsx
```

**확인 포인트 (T007-T009)**
- [ ] `updateTodo` 함수 테스트, 제목/설명 업데이트, `updatedAt` 갱신, 로컬 스토리지 저장 검증이 추가되었는가
- [ ] 편집 버튼 클릭 → 편집 모드 전환 / 저장 → `onEdit` 호출 / 취소 → `onCancelEdit` 호출 / Enter·ESC 키 동작 테스트가 있는가
- [ ] 편집→저장→목록 반영, 편집→취소→원래 내용 유지, 편집→저장→스토리지 지속성 검증이 있는가
- [ ] **모든 테스트가 실패(Red)하는가** — 이것이 정상입니다. 아직 구현이 없기 때문입니다.

**Step 2: 구현 (Green)**

```bash
# T010: updateTodo 함수 구현
T010 작업 진행해줘
code lib/hooks/useTodos.ts
npm run test:unit -- useTodos.test.tsx
```
**예상 실행 결과 (Green)**
```
PASS  tests/unit/hooks/useTodos.test.tsx
  ✓ updateTodo() should update title and description
  ✓ updateTodo() should refresh updatedAt timestamp
  Tests: 2 passed, 2 total
```

**확인 포인트 (T010)**
- [ ] `updateTodo` 함수 추가, `id`로 항목 찾아 업데이트, `updatedAt`을 ISO-8601로 갱신, `saveTodos()` 호출, `useTodos` 반환값에 `updateTodo` 포함
- [ ] **T007 테스트가 통과(Green)하는가**

```bash
# T011-T015: TodoItem 컴포넌트 구현 (순차 진행)
T011 작업 진행해줘   # 편집 모드 상태 추가
T012 작업 진행해줘   # 편집 모드 UI 구현
T013 작업 진행해줘   # 편집 버튼 추가
T014 작업 진행해줘   # 저장 로직
T015 작업 진행해줘   # 취소 로직
npm run test:component -- TodoItem.test.tsx
```

**확인 포인트 (T011-T015)**
- [ ] `isEditing`, `editTitle`, `editDescription` 상태 추가
- [ ] 편집 모드일 때 input/textarea 렌더링, 표시 모드일 때 편집 버튼 표시
- [ ] `handleSave`: `onEdit` 호출 후 편집 모드 종료 / `handleCancel`: 원래 값 복원 후 편집 모드 종료
- [ ] Enter/ESC 키 핸들러 구현
- [ ] **T008 테스트가 통과(Green)하는가**

```bash
# T016: TodoList editingId 상태 관리
T016 작업 진행해줘
code components/TodoList.tsx
npm run test:integration -- todo-flow.test.tsx
```

**확인 포인트 (T016)**
- [ ] `editingId` 상태 추가 (`useState<string | null>`)
- [ ] `handleStartEdit`, `handleCancelEdit`, `handleEdit` 구현
- [ ] `TodoItem`에 `isEditing`, `disabled`, `onStartEdit`, `onCancelEdit`, `onEdit` props 전달
- [ ] **T009 통합 테스트가 통과(Green)하는가**

```bash
# T017: E2E 테스트
T017 작업 진행해줘
npm run test:e2e -- todo-core.spec.ts
```

**✅ Checkpoint: User Story 1 완료**
```bash
npm run test -- TodoItem useTodos todo-flow
# 결과: 모두 통과해야 함
```

---

### Phase 4: User Story 2 — 유효성 검증

**Step 1: 테스트 작성 (Red)**
```bash
T018, T019 작업을 순차적으로 진행해줘
npm run test:component -- TodoItem.test.tsx
npm run test:integration -- todo-flow.test.tsx
```
**확인 포인트**: 빈 제목/100자 초과 제목/500자 초과 설명 저장 시도 테스트, 오류 시 `onEdit` 미호출 검증, 테스트가 실패(Red)하는가

**Step 2: 구현 (Green)**
```bash
T020, T021, T022 작업을 순차적으로 진행해줘
npm run test:component -- TodoItem.test.tsx
npm run test:integration -- todo-flow.test.tsx
```
**확인 포인트**: `titleError`/`descriptionError` 상태 추가, `handleSave`에 유효성 검증 함수 연결, 오류 시 저장 차단(return), 오류 메시지 UI 및 필드 테두리(`text-red-500`) 표시, 테스트 통과(Green)

```bash
T023 작업 진행해줘   # E2E 테스트
npm run test:e2e -- todo-core.spec.ts
```

**✅ Checkpoint: User Story 1 AND 2 완료**

---

### Phase 5: User Story 3 — 동시 편집 방지

**Step 1: 테스트 작성 (Red)**
```bash
T024, T025 작업을 순차적으로 진행해줘
npm run test:component -- TodoItem.test.tsx
npm run test:integration -- todo-flow.test.tsx
```
**확인 포인트**: disabled prop 전달 시 버튼 비활성화, 편집 중 다른 항목 버튼 비활성화, 편집 완료 후 재활성화 테스트가 실패(Red)하는가

**Step 2: 구현 (Green)**
```bash
T026, T027, T028 작업을 순차적으로 진행해줘
npm run test:component -- TodoItem.test.tsx
npm run test:integration -- todo-flow.test.tsx
```
**확인 포인트**
- [ ] `TodoItem`에 `disabled` prop 및 편집/삭제 버튼 disabled 속성 추가
- [ ] disabled 시 시각 피드백 (`opacity-50`, `cursor-not-allowed`)
- [ ] ARIA 라벨 추가 (`Edit {todo.title}`, `Save changes`, `Cancel editing`, `Edit title`, `Edit description`)
- [ ] 편집 모드 진입 시 제목 필드 자동 포커스 (`autoFocus`)
- [ ] **모든 테스트가 통과(Green)하는가**

**✅ Checkpoint: 모든 User Story 완료**

---

### Phase 6: Polish & Quality

```bash
npm run lint
npx tsc --noEmit
```
**확인 포인트**: lint 오류 없음, TypeScript 타입 오류 없음

```bash
npm run test -- TodoItem TodoForm useTodos todo-flow filtering --run
```
**예상 실행 결과**
```
✅ TodoItem: 18/18 passed
✅ TodoForm: 3/3 passed
✅ useTodos: 8/8 passed
✅ todo-flow: 6/6 passed
✅ filtering: 1/1 passed
━━━━━━━━━━━━━━━━━━━━━━
✅ Total: 39/39 passed
```

**수동 테스트 체크리스트** (quickstart.md 참조)
- [ ] 편집 버튼 클릭 → 편집 모드 전환
- [ ] 제목/설명 수정 → 저장 → 반영 확인
- [ ] Enter 키로 저장 (제목 필드) / ESC 키로 취소
- [ ] 빈 제목 저장 시도 → 오류 메시지
- [ ] 편집 중 다른 항목 버튼 비활성화 → 편집 완료 후 재활성화

**성능 확인**
- [ ] 편집 모드 전환이 즉시 반응하는가
- [ ] 저장 후 UI 업데이트가 부드러운가

**✅ 최종 확인: 모든 Phase 완료**

---

## 6단계: 실습 종합 확인

```bash
npm run dev
```

**수동 테스트 시나리오**
1. 할일 추가 → 편집 버튼 클릭 → 제목 변경 → 저장 → 확인
2. 편집 → ESC 키 → 변경사항 취소 확인
3. 편집 → 제목을 빈 값으로 → 저장 시도 → 오류 메시지 확인
4. 한 항목 편집 중 → 다른 항목의 편집/삭제 버튼 비활성화 확인
5. 편집 완료 → 새로고침 → 변경사항 유지 확인

**산출물 확인**
```bash
code specs/002-edit-todo/spec.md specs/002-edit-todo/plan.md specs/002-edit-todo/tasks.md
code lib/hooks/useTodos.ts components/TodoItem.tsx components/TodoList.tsx
code tests/unit/hooks/useTodos.test.tsx tests/component/TodoItem.test.tsx tests/integration/todo-flow.test.tsx tests/e2e/todo-core.spec.ts
```

## ✅ Part 2 완료 체크리스트

**Phase 1-2: Setup**
- [ ] 기존 validation 유틸/useTodos 훅/TodoItem 컴포넌트 구조 분석 완료

**Phase 3: US1 (MVP)**
- [ ] T007-T009 테스트 작성 완료 (Red 확인) → T010-T016 구현 완료 → T017 E2E 통과
- [ ] **US1 단독 테스트 가능 확인**

**Phase 4: US2**
- [ ] T018-T019 테스트 작성 완료 (Red 확인) → T020-T022 구현 완료 → T023 E2E 통과
- [ ] **US1 AND US2 동시 동작 확인**

**Phase 5: US3**
- [ ] T024-T025 테스트 작성 완료 (Red 확인) → T026-T028 구현 및 접근성 개선 완료
- [ ] **모든 US 동시 동작 확인**

**Phase 6: Polish**
- [ ] Lint/Typecheck 통과, 전체 테스트 스위트 통과 (39/39), 수동 테스트·성능·문서 완료

### 구현된 기능 요약

1. **인라인 편집**: 편집 버튼 클릭으로 즉시 편집 모드 전환
2. **키보드 단축키**: Enter(제목)=저장, ESC=취소, Enter(설명)=줄바꿈
3. **실시간 유효성 검증**: 빈 제목 차단, 제목 100자/설명 500자 제한
4. **동시 편집 방지**: 한 항목 편집 중 다른 항목 버튼 비활성화
5. **접근성**: ARIA 라벨, 자동 포커스, 키보드 네비게이션
6. **데이터 지속성**: 로컬 스토리지 자동 저장

---

## 🎓 학습 포인트

### Spec Driven Development의 핵심

1. **명확성 우선**: 스펙 작성 시 구현 세부사항 배제, 명확화 단계에서 모호성 제거
2. **독립성 보장**: 각 User Story가 독립적으로 테스트 가능, 우선순위별 단계적 구현 가능
3. **TDD 패턴**: Red(테스트 실패) → Green(구현) 반복, 테스트가 구현의 가이드 역할
4. **산출물 연결**: Spec → Plan → Tasks → Implementation, 각 단계의 산출물이 다음 단계의 입력
5. **품질 검증**: Lint/Typecheck로 코드 품질, 테스트로 기능 정확성, 수동 테스트로 UX 확인

### TDD 실천 패턴

1. **테스트 먼저**: 구현 전에 테스트를 작성하여 요구사항을 명확히 함
2. **작은 단위**: 각 작업을 작은 단위로 분해하여 진행
3. **빠른 피드백**: 각 단계마다 테스트 실행으로 즉시 확인
4. **리팩토링 안전망**: 테스트가 있어 리팩토링 시 회귀 방지

---

## 💡 Copilot Chat 활용 팁

1. **단계별 진행**
   ```
   /speckit.specify → /speckit.clarify → /speckit.plan → /speckit.tasks → /speckit.implement
   ```
2. **작업 단위 요청** (한 번에 1-2개씩)
   ```
   T007 작업 진행해줘
   T010 작업 진행해줘
   ```
3. **TDD 주기 명시**
   ```
   T007 테스트 작성 후 Red 확인해줘
   T010 구현 후 Green 확인해줘
   ```
4. **중간 검증**: 각 단계마다 테스트 실행으로 확인
5. **스펙 수정 시**: `clarify → plan → tasks` 순서로 다시 동기화

## 🔧 문제 해결

**"테스트가 통과하지 않아요"**
1. 테스트 파일과 구현 파일을 모두 확인
2. 오류 메시지를 Copilot Chat에 복사하여 해결 요청
3. 필요시 캐시 클리어: `npm run test -- --no-cache`

**"Lint/Typecheck 오류가 발생해요"**
1. 오류 메시지를 확인하고 Copilot Chat에 해결 요청
2. import 누락 확인 / 타입 정의 확인

**"개발 서버가 실행되지 않아요"**
1. 포트 충돌 확인 (3000 포트)
2. 의존성 재설치: `rm -rf node_modules && npm install`

---

## 🔍 심화 학습

### 추가 실습 아이디어
1. **우선순위 변경 기능**: 할일 순서를 드래그앤드롭으로 변경
2. **카테고리 기능**: 할일에 카테고리 태그 추가
3. **검색 기능**: 제목/설명으로 할일 검색
4. **마감일 기능**: 할일에 마감일 추가 및 정렬

### Spec Kit 고급 활용
1. **성능 최적화**: `React.memo`, `useMemo`/`useCallback`, Virtual scrolling
2. **접근성 강화**: 스크린 리더 테스트, 키보드 네비게이션 개선, WCAG 2.1 AA 준수
3. **에러 처리**: ErrorBoundary 추가, Toast 알림 시스템, 오류 로깅

---

## 📚 참고 자료

- [GitHub Spec Kit 문서](https://github.com/github/spec-kit)
- [React 19 Documentation](https://react.dev/)
- [Next.js 16 Documentation](https://nextjs.org/docs)
- [Vitest Documentation](https://vitest.dev/)
- [Playwright Documentation](https://playwright.dev/)
- [WAI-ARIA Authoring Practices](https://www.w3.org/WAI/ARIA/apg/)

---

## ✅ 최종 확인

- [ ] Part 1: 기존 프로젝트의 헌법 → 스펙 → 명확화 → 계획 → 작업 → 분석 → 구현 산출물을 모두 확인했다
- [ ] Part 2: 편집 기능을 스펙부터 구현까지 직접 완성했다
- [ ] 편집 기능이 정상 동작한다
- [ ] 모든 테스트가 통과한다 (39/39)
- [ ] Lint/Typecheck 오류가 없다
- [ ] 수동 테스트 체크리스트를 완료했다
- [ ] TDD Red-Green 사이클을 직접 경험했다

**축하합니다! 🎉 GitHub Spec Kit을 활용한 완전한 Spec Driven Development 사이클을 이해하고 실습했습니다!**
