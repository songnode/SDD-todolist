# Copilot Instructions - todolist-app

## 언어 사용 규칙 (최우선)

**Copilot(AI Coding Agent)의 모든 답변과 설명, 코드 주석,  각종 설정 파일의 주석, Todos는 반드시 한국어로 작성합니다.**

예외 사항 (영어 사용):
- 코드 (변수명, 함수명, 컴포넌트명, 타입명 등)
- 파일명, 폴더명
- 기술 용어의 원어 표기가 더 명확한 경우 괄호 안에 병기 가능 (예: "상태 관리(state management)")

좋은 예시:
```
✅ "이 컴포넌트는 useState 훅을 사용하여 상태를 관리합니다"
✅ "TodoList 컴포넌트를 생성하겠습니다"
❌ "This component uses useState hook to manage state"
❌ "Will create a TodoList component"
```

### 커밋 메시지 형식

```
type(scope): "한국어 설명"
```

- **type**: feat, fix, docs, refactor, test, chore
- **scope**: const, spec, plan, task, impl

예시:
```
feat(impl): "TodoList 컴포넌트 구현"
fix(task): "타입 오류 수정"
test(spec): "E2E 테스트 추가"
docs(const): "README 업데이트"
```

## 프로젝트 개요

Next.js 16 (App Router) 기반의 todolist 애플리케이션입니다.

### 핵심 기술 스택
- **프레임워크**: Next.js 16.1.4 (App Router)
- **언어**: TypeScript 5 (strict mode)
- **UI 라이브러리**: React 19
- **스타일링**: Tailwind CSS 4
- **최적화**: React Compiler 활성화 (`next.config.ts`)
- **폰트**: Geist Sans, Geist Mono (자동 최적화)

### 디렉토리 구조
```
todolist-app/
├── .github/                    # GitHub 설정
│   ├── copilot-instructions.md # Copilot AI 지침서 (이 문서)
│   ├── agents/                 # Spec Kit 에이전트 파일 (9개)
│   │   ├── speckit.analyze.agent.md
│   │   ├── speckit.checklist.agent.md
│   │   ├── speckit.clarify.agent.md
│   │   ├── speckit.constitution.agent.md
│   │   ├── speckit.implement.agent.md
│   │   ├── speckit.plan.agent.md
│   │   ├── speckit.specify.agent.md
│   │   ├── speckit.tasks.agent.md
│   │   └── speckit.taskstoissues.agent.md
│   └── prompts/                # Spec Kit 프롬프트 파일 (9개)
│       ├── speckit.analyze.prompt.md
│       ├── speckit.checklist.prompt.md
│       ├── speckit.clarify.prompt.md
│       ├── speckit.constitution.prompt.md
│       ├── speckit.implement.prompt.md
│       ├── speckit.plan.prompt.md
│       ├── speckit.specify.prompt.md
│       ├── speckit.tasks.prompt.md
│       └── speckit.taskstoissues.prompt.md
│
├── .vscode/                    # VS Code 워크스페이스 설정
│   └── settings.json           # Spec Kit 파일 자동 추천 및 스크립트 자동 승인
│
├── .specify/                   # Spec Kit: AI 주도 개발 워크플로 도구
│   ├── memory/                 # 프로젝트 헌법 및 기억
│   │   └── constitution.md     # 프로젝트 핵심 원칙 및 아키텍처 철학
│   ├── scripts/                # 자동화 스크립트
│   │   └── powershell/         # PowerShell 스크립트
│   │       ├── check-prerequisites.ps1    # 사전 요구사항 검사
│   │       ├── common.ps1                 # 공통 함수 라이브러리
│   │       ├── create-new-feature.ps1     # 새 기능 개발 워크플로
│   │       ├── setup-plan.ps1             # 계획 단계 설정
│   │       └── update-agent-context.ps1   # AI 에이전트 컨텍스트 업데이트
│   └── templates/              # 문서 템플릿
│       ├── agent-file-template.md         # AI 에이전트 파일 템플릿
│       ├── checklist-template.md          # 체크리스트 템플릿
│       ├── plan-template.md               # 계획(Plan) 문서 템플릿
│       ├── spec-template.md               # 명세(Spec) 문서 템플릿
│       └── tasks-template.md              # 작업(Tasks) 문서 템플릿
│
├── app/                        # Next.js App Router (React Server Components)
│   ├── layout.tsx              # 루트 레이아웃 (메타데이터, 폰트, 전역 스타일)
│   ├── page.tsx                # 홈 페이지 (/)
│   ├── globals.css             # Tailwind 전역 스타일
│   └── favicon.ico             # 파비콘
│
├── components/                 # 재사용 가능한 React 컴포넌트
│   └── (컴포넌트 파일들)       # kebab-case.tsx 형식 (예: todo-list.tsx)
│
├── lib/                        # 유틸리티 및 헬퍼 함수
│   └── (유틸리티 파일들)       # 비즈니스 로직, 공통 함수
│
├── tests/                      # 테스트 파일 (Vitest + Playwright)
│   ├── setup.ts                # Vitest 전역 설정 (Testing Library)
│   ├── unit/                   # 단위 테스트 (Vitest + jsdom)
│   │   └── page.test.tsx       # 컴포넌트/함수 단위 테스트
│   ├── integration/            # 통합 테스트
│   └── e2e/                    # E2E 테스트 (Playwright)
│       └── home.spec.ts        # 사용자 시나리오 테스트
│
├── public/                     # 정적 파일 (이미지, 폰트 등)
│   └── (정적 리소스)           # /public URL로 제공 (/logo.png)
│
├── labs/                       # 실습용 디렉토리 (git 무시됨)
│   ├── lab-00/                 # Git 워크플로 실습 (로컬→GitHub→PR→merge)
│   │   └── README.md           # 실습 가이드
│   ├── lab-01/                 # 실습 01
│   │   └── README.md
│   ├── lab-02/                 # 실습 02
│   │   └── README.md
│   └── lab-03/                 # 실습 03
│       └── README.md
│
├── coverage/                   # Vitest 테스트 커버리지 리포트 (자동 생성)
├── playwright-report/          # Playwright E2E 테스트 리포트 (자동 생성)
├── test-results/               # Playwright 테스트 결과 (자동 생성)
│
├── .env.example                # 환경 변수 예시 (복사하여 .env 생성)
├── .gitignore                  # Git 무시 패턴
│
├── eslint.config.mjs           # ESLint 설정 (코드 품질)
├── next.config.ts              # Next.js 설정 (React Compiler 활성화)
├── postcss.config.mjs          # PostCSS 설정 (Tailwind 처리)
├── tailwindcss.config.ts       # Tailwind CSS 설정
├── tsconfig.json               # TypeScript 설정 (strict mode, path alias)
├── vitest.config.ts            # Vitest 설정 (단위 테스트)
├── playwright.config.ts        # Playwright 설정 (E2E 테스트)
│
├── package.json                # 프로젝트 의존성 및 스크립트
└── README.md                   # 프로젝트 문서
```

**핵심 디렉토리 설명**:
- `.github/`: GitHub 및 Copilot 설정
  - `agents/`: Spec Kit 워크플로별 Copilot 에이전트 정의 (analyze, specify, plan, tasks, implement 등)
  - `prompts/`: Spec Kit 워크플로별 Copilot 프롬프트 템플릿
- `.vscode/settings.json`: **Spec Kit 통합** - Copilot Chat에서 Spec Kit 파일 자동 추천 및 스크립트 자동 승인 설정
- `.specify/`: **Spec Kit** - AI 주도 개발을 위한 템플릿, 스크립트, 프로젝트 헌법
  - `memory/constitution.md`: 프로젝트의 불변 원칙 (Library-First, Test-First 등)
  - `scripts/`: 기능 개발 워크플로 자동화 스크립트
  - `templates/`: Spec, Plan, Tasks 문서 표준 템플릿
- `app/`: Next.js 16 App Router 기반 페이지/레이아웃 (기본 Server Component)
- `components/`: 재사용 컴포넌트 (Client Component는 `"use client"` 명시)
- `lib/`: 순수 함수, 헬퍼, 비즈니스 로직 (독립 라이브러리)
- `tests/unit/`: 컴포넌트/함수 단위 테스트 (Vitest + Testing Library)
- `tests/integration/`: 통합 테스트
- `tests/e2e/`: 사용자 시나리오 E2E 테스트 (Playwright)
- `public/`: 정적 파일 (`/public/logo.png` → `/logo.png`로 접근)
- `labs/`: 실습용 임시 파일 (Git, 워크플로, 기술 연습용, `.gitignore`에서 제외됨)

### Path Alias
- `@/*` → 프로젝트 루트 (`tsconfig.json`, `vitest.config.ts`에 설정됨)
- 예: `import Home from '@/app/page'`

## 핵심 규칙

- Next.js 16 App Router, React 19, TypeScript strict mode를 엄격히 준수합니다
- React Compiler 활성화 환경: 불필요한 `useMemo`, `useCallback` 사용 지양
- 라이브러리 공식 문서가 필요한 경우 Context7 도구(`mcp_io_github_ups_*`)를 활용합니다: 프롬프트에 `use Context7` 명령어 포함
- 코드 작성 전 기존 패턴과 설정 파일을 확인하여 일관성을 유지합니다

## 개발 워크플로우

### 개발 서버
```bash
npm run dev        # localhost:3000에서 개발 서버 시작
```

### 테스팅 전략

#### 단위 테스트 (Vitest)
```bash
npm test                # 단위 테스트 실행
npm run test:ui         # Vitest UI 모드
npm run test:coverage   # 커버리지 리포트 생성
```

**설정 포인트**:
- 환경: `jsdom`
- 글로벌 모드 활성화
- Testing Library (`@testing-library/react`, `jest-dom`) 사용
- 테스트 파일: `tests/unit/**/*.{test,spec}.{js,ts,jsx,tsx}`

**테스트 작성 패턴** ([tests/unit/page.test.tsx](../tests/unit/page.test.tsx) 참고):
```typescript
import { describe, it, expect } from 'vitest'
import { render, screen } from '@testing-library/react'

describe('Component Name', () => {
  it('should...', () => {
    render(<Component />)
    const element = screen.getByRole(...)
    expect(element).toBeInTheDocument()
  })
})
```

#### E2E 테스트 (Playwright)
```bash
npm run test:e2e         # E2E 테스트 실행
npm run test:e2e:ui      # Playwright UI 모드
npm run test:e2e:report  # HTML 리포트 보기
```

**설정 포인트**:
- `baseURL`: http://localhost:3000
- 자동 dev 서버 시작 (`webServer` 설정)
- 브라우저: Chromium, Firefox, WebKit
- CI 환경에서 리트라이 2회

**E2E 테스트 작성 패턴** ([tests/e2e/home.spec.ts](../tests/e2e/home.spec.ts) 참고):
```typescript
test.describe('Feature E2E', () => {
  test('should...', async ({ page }) => {
    await page.goto('/')
    await expect(page.getByRole(...)).toBeVisible()
  })
})
```

### 빌드컨벤션

### 파일 및 네이밍
- **파일명**: kebab-case (예: `todo-list.tsx`)
- **컴포넌트명**: PascalCase (예: `TodoList`)
- **함수/변수명**: camelCase (예: `handleSubmit`)

### TypeScript
- Strict 모드 활성화, 모든 컴포넌트에 타입 지정
- JSX: `react-jsx` 변환 사용

### React 컴포넌트
- **Server Components 우선**: `app/` 디렉토리는 기본적으로 Server Component
- Client Component 필요 시 파일 상단에 `"use client"` 추가
- React Compiler 활성화로 불필요한 `useMemo`, `useCallback` 지양

### 스타일링
- Tailwind CSS 유틸리티 클래스 사용
- 다크 모드: `dark:` 접두사, 반응형: `sm:`, `md:` 등
- 예: `className="flex min-h-screen items-center dark:bg-black"`

### Next.js 기능
- 이미지: `next/image` 컴포넌트
- 폰트: `next/font/google` 자동 최적화
- 메타데이터: `layout.tsx`의 `Metadata` 객체

## Spec Kit 워크플로

**Spec Kit**은 AI 주도 개발을 체계화하기 위한 도구 모음으로, `.specify/` 디렉토리에 위치합니다.

### 구조 및 역할

#### `.specify/memory/constitution.md`
프로젝트의 불변 원칙과 아키텍처 철학을 정의합니다.
- Library-First: 모든 기능을 독립 라이브러리로 시작
- Test-First: TDD 필수 (테스트 작성 → 승인 → 구현)
- CLI Interface: 라이브러리는 CLI로 기능 제공
- 관찰 가능성, 버저닝, 단순성 등의 핵심 가치

#### `.specify/templates/`
표준화된 문서 템플릿:
- **spec-template.md**: 기능 명세 작성 템플릿
- **plan-template.md**: 구현 계획 템플릿
- **tasks-template.md**: 작업 목록 템플릿
- **checklist-template.md**: 체크리스트 템플릿
- **agent-file-template.md**: AI 에이전트 컨텍스트 파일 템플릿

#### `.specify/scripts/powershell/`
자동화 스크립트:
- **create-new-feature.ps1**: 새 기능 개발 워크플로 자동화
- **setup-plan.ps1**: 계획 단계 초기화
- **update-agent-context.ps1**: AI 에이전트 컨텍스트 업데이트
- **check-prerequisites.ps1**: 개발 환경 체크
- **common.ps1**: 공통 함수 라이브러리

### 사용 시나리오

**새 기능 개발 시**:
```powershell
# 기능 개발 워크플로 시작
.\.specify\scripts\powershell\create-new-feature.ps1 -FeatureName "todo-crud"
```

이 스크립트는 다음을 자동으로 생성합니다:
1. 기능별 디렉토리 구조
2. Spec, Plan, Tasks 문서 (템플릿 기반)
3. AI 에이전트 컨텍스트 파일

**수동으로 템플릿 사용**:
- `.specify/templates/`에서 필요한 템플릿 복사
- 프로젝트 요구사항에 맞게 커스터마이징
- Copilot에게 템플릿을 참조하도록 지시

**Constitution 참조**:
- 아키텍처 결정 시 `.specify/memory/constitution.md` 참조
- 새 원칙 추가 시 팀 합의 후 업데이트

## 일반적인 작업 패턴

### 새 페이지 추가
1. `app/[route]/page.tsx` 생성 (Server Component 기본)
2. 필요 시 `export const metadata: Metadata = {...}` 추가

### 새 컴포넌트 추가
1. `components/` 디렉토리에 파일 생성
2. 단위 테스트 작성: `tests/unit/[component].test.tsx`
3. E2E 영향 시 `tests/e2e/` 업데이트

### 테스트 우선 개발
1. 기능 구현 전 테스트 작성
2. 단위 테스트로 컴포넌트 로직 검증
3. E2E 테스트로 사용자 흐름 검증
