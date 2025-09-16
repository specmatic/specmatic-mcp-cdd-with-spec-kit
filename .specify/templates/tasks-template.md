# Tasks: [FEATURE NAME]

**Input**: Design documents from `/specs/[###-feature-name]/`
**Prerequisites**: plan.md (required), research.md, data-model.md, contracts/

## Execution Flow (main)
```
1. Load plan.md from feature directory
   → If not found: ERROR "No implementation plan found"
   → Extract: tech stack, libraries, structure
2. Load optional design documents:
   → data-model.md: Extract entities → model tasks
   → contracts/: Each file → Specmatic MCP contract test task
   → research.md: Extract decisions → setup tasks
3. Generate tasks by THREE-PHASE approach:
   🚨 **CRITICAL: BACKEND AND FRONTEND PHASES MUST EXECUTE IN PARALLEL** 🚨
   → BACKEND PHASE: Check existing structure, setup/update, contract tests (RED), implementation (GREEN), contract pass verification, resiliency tests (RED), validation (GREEN), resiliency pass verification
   → FRONTEND PHASE: Check existing structure, mock server setup, UI development, component tests, shutdown mocks
   → **PARALLEL EXECUTION REQUIREMENT**: Launch BOTH backend-api-engineer AND frontend-react-engineer agents simultaneously using single message with multiple Task tool calls
   → INTEGRATION PHASE: Real backend startup, frontend reconfiguration, integration tests, shutdown all (ONLY after both Backend and Frontend phases complete)
4. Apply task rules:
   → Different files = mark [P] for parallel
   → Same file = sequential (no [P])
   → Backend TDD: Tests before implementation (RED-GREEN-REFACTOR)
   → Frontend: Build against mocks, component tests (no RED-GREEN needed)
   → Always shutdown mocks when their use is over
5. Number tasks sequentially (T001, T002...) within each phase
6. Generate dependency graph with phase barriers
7. Create parallel execution examples per phase
8. Validate task completeness:
   → Backend completion gates: All contract + resiliency tests pass?
   → All entities have models?
   → All endpoints implemented and tested?
   → Mock lifecycle properly managed?
9. **CRITICAL VERIFICATION**: Ensure parallel execution setup is correctly configured
   → Verify backend-api-engineer tasks and frontend-react-engineer tasks can launch simultaneously
   → Confirm no sequential dependencies between Backend and Frontend phases
   → Validate both phases can complete independently before Integration phase
10. Return: SUCCESS (three-phase tasks ready for PARALLEL execution)
```

## Format: `[ID] [P?] Description`
- **[P]**: Can run in parallel (different files, no dependencies)
- Include exact file paths in descriptions

## Path Conventions
- **Single project**: `src/`, `tests/` at repository root
- **Web app**: `backend/src/`, `frontend/src/`
- **Mobile**: `api/src/`, `ios/src/` or `android/src/`
- Paths shown below assume single project - adjust based on plan.md structure

## 🚨 BACKEND PHASE - MUST RUN IN PARALLEL WITH FRONTEND 🚨
**CRITICAL: Use backend-api-engineer agent - LAUNCH SIMULTANEOUSLY with frontend-react-engineer**

### Phase 3.1: Backend Setup
**Prerequisites: Verify port 3000 is available (use `lsof -ti:3000` to check, `kill -9 PID` to cleanup if needed)**
- [ ] T001 Check for existing backend directory, create or update project structure per implementation plan
- [ ] T002 Initialize or update Node.js backend project with Express framework dependencies
- [ ] T003 [P] Configure or update backend linting and formatting tools

### Phase 3.2: Backend Contract Tests First (TDD) ⚠️ MUST COMPLETE BEFORE 3.3
**CRITICAL: Contract RED-GREEN-REFACTOR Cycle - Contract tests MUST be written and MUST FAIL before ANY implementation**
**NO MANUAL CURL TESTING - Use only Specialized MCP Agents**
**ALL BACKEND DECISIONS DRIVEN BY JUNIT TEST REPORTS from Specmatic MCP**
**VERIFY: Contract tests fail because NO routes/endpoints exist yet**
- [ ] T004 backend-api-engineer: Setup Specmatic MCP contract tests (must fail initially) - analyze JUnit test reports for implementation guidance

### Phase 3.3: Backend Implementation (ONLY after contract tests are failing)
**GREEN Phase: Implement JUST ENOUGH to make failing contract tests pass**
- [ ] T005 [P] User model in backend/src/models/user.js (Node.js module)
- [ ] T006 [P] UserService CRUD in backend/src/services/user_service.js (Express service)
- [ ] T007 [P] CLI --create-user in backend/src/cli/user_commands.js (Node.js CLI)
- [ ] T008 POST /api/users endpoint (make contract tests pass)
- [ ] T009 GET /api/users/{id} endpoint (make contract tests pass)
- [ ] T010 backend-api-engineer: Verify contract tests now pass (GREEN) - analyze JUnit test reports to confirm all endpoints implemented correctly
**VERIFY: All contract tests now pass before proceeding to resiliency phase**

### Phase 3.4: Backend Resiliency Tests (ONLY after contract tests pass)
**CRITICAL: Start ONLY after contract tests are GREEN**
**PRAGMATIC APPROACH: Tests may pass or fail - fix any failures found and proceed**
- [ ] T011 backend-api-engineer: Run Specmatic MCP boundary condition tests and assess current state - analyze JUnit test reports to identify validation gaps
- [ ] T012 Input validation (fix any resiliency test failures found in JUnit reports from T011)
- [ ] T013 Error handling and logging (fix any resiliency test failures found in JUnit reports from T011)
- [ ] T014 backend-api-engineer: Final resiliency test verification - ensure all pass - analyze JUnit test reports for complete validation coverage
**VERIFY: All resiliency tests now pass**

### Phase 3.5: Backend Integration & Polish
**REFACTOR Phase: Clean up while keeping tests green**
- [ ] T015 Connect UserService to DB
- [ ] T016 Auth middleware
- [ ] T017 Request/response logging
- [ ] T018 CORS and security headers
- [ ] T020 backend-api-engineer: Final backend verification - ALL contract tests MUST pass - analyze JUnit test reports for comprehensive contract compliance
- [ ] T021 backend-api-engineer: Final backend verification - ALL resiliency tests MUST pass - analyze JUnit test reports for complete resiliency validation
- [ ] T022 Remove duplication (REFACTOR while keeping tests green)
**BACKEND COMPLETION GATE: All contract and resiliency tests must pass before integration phase**

## 🚨 FRONTEND PHASE - MUST RUN IN PARALLEL WITH BACKEND 🚨
**CRITICAL: Use frontend-react-engineer agent - LAUNCH SIMULTANEOUSLY with backend-api-engineer**

### Phase 4.1: Frontend Setup with Mocks
**Prerequisites: Verify ports 9001 and 4000 are available (use `lsof -ti:PORT` to check, `kill -9 PID` to cleanup if needed)**
- [ ] T023 frontend-react-engineer: Start Specmatic mock server on port 9001
- [ ] T024 Check for existing frontend directory, create or update project structure
- [ ] T025 Initialize or update React frontend project with Create React App or Vite
- [ ] T026 Configure React frontend environment: REACT_APP_API_BASE_URL=http://localhost:9001 for mock server development
- [ ] T027 [P] Configure or update frontend linting and formatting tools

### Phase 4.2: Frontend Development & Component Testing
**Build against mock API - No RED-GREEN cycle needed for UI**
- [ ] T028 [P] User registration component in frontend/src/components/UserRegistration.js
- [ ] T029 [P] User profile component in frontend/src/components/UserProfile.js
- [ ] T030 [P] frontend-react-engineer: Component test for user registration form
- [ ] T031 [P] frontend-react-engineer: Component test for user profile display
- [ ] T032 [P] frontend-react-engineer: Component test auth flow with mock server
- [ ] T033 frontend-react-engineer: Shutdown Specmatic mock servers (port 9001)

## INTEGRATION PHASE - Final Integration
**⚠️ ONLY START AFTER BOTH BACKEND AND FRONTEND PHASES COMPLETE ⚠️**

### Phase 5.1: Real Backend Integration
**Prerequisites: Verify all ports (3000, 4000) are available - kill any conflicting processes from previous phases**
- [ ] T034 Start real backend on port 3000
- [ ] T035 Reconfigure React frontend: REACT_APP_API_BASE_URL=http://localhost:3000 for real backend integration
- [ ] T036 integration-tester: Integration tests with real backend
- [ ] T037 [P] End-to-end workflow validation
- [ ] T039 [P] Update documentation
- [ ] T040 Shutdown all services after testing complete

## Dependencies
**Backend Phase Dependencies (Sequential Contract → Resiliency):**
- Contract tests setup (T004) before backend implementation (T005-T009)
- Backend models (T005) block backend services (T006)
- Backend implementation complete (T010) before resiliency phase starts
- **CRITICAL GATE**: Contract tests pass (T010) before resiliency tests begin (T011)
- Resiliency tests setup (T011) before validation implementation (T012-T013)
- Resiliency tests pass (T014) before integration (T015-T018)
- Backend completion gate: T020-T021 must pass before Integration Phase

**Frontend Phase Dependencies:**
- Mock server (T023) before frontend development (T024-T032)
- Frontend components before component tests (T028-T029 before T030-T032)
- Mock shutdown (T033) before Integration Phase

**Integration Phase Dependencies:**
- Backend must be running (T034) before frontend reconfiguration (T035)
- Integration tests (T036) before final validation (T037-T039)

## 🚨 CRITICAL: Parallel Agent Launch Examples 🚨

### **CORRECT: Simultaneous Backend and Frontend Phase Launch**
```
# CRITICAL: Use single message with multiple Task tool calls
# Launch BOTH agents simultaneously for parallel execution

Task: "backend-api-engineer: Complete Backend Phase (T001-T022)"
Description: "Complete all backend tasks from setup through final verification"
Subagent_type: "backend-api-engineer"

Task: "frontend-react-engineer: Complete Frontend Phase (T023-T033)"
Description: "Complete all frontend tasks from mock setup through shutdown"
Subagent_type: "frontend-react-engineer"

# RESULT: Both phases execute in parallel, dramatically reducing total time
```

### **INCORRECT: Sequential Execution (DO NOT DO THIS)**
```
# WRONG: This causes sequential execution and doubles development time
Task: "backend-api-engineer: Complete Backend Phase"
# Wait for backend completion...
Task: "frontend-react-engineer: Complete Frontend Phase"
```

### Backend Phase Internal Parallel Tasks:
```
# Within Backend Phase - Launch T005-T007 together (different files):
Task: "User model in backend/src/models/user.py"
Task: "UserService CRUD in backend/src/services/user_service.py"
Task: "CLI --create-user in backend/src/cli/user_commands.py"

# NOTE: Contract and resiliency tests are SEQUENTIAL (no [P] markers)
# Backend Track: T004 @backend-api-engineer → T005-T009 → T010 @backend-api-engineer → T011 @backend-api-engineer → T012-T013 → T014 @backend-api-engineer
```

### Frontend Phase Internal Parallel Tasks:
```
# Within Frontend Phase - Launch T030-T032 together (UI component tests can be parallel):
Task: "frontend-react-engineer: Component test for user registration form" [P]
Task: "frontend-react-engineer: Component test for user profile display" [P]
Task: "frontend-react-engineer: Component test auth flow with mock server" [P]
```

## Notes
- [P] tasks = different files, no dependencies within same phase
- **Backend Phase**: Verify tests fail before implementing (RED-GREEN-REFACTOR)
- **Frontend Phase**: Build against mocks, no RED-GREEN needed for UI components
- **Integration Phase**: Real backend + frontend integration
- Always shutdown mocks when their use is over
- Commit after each task
- Avoid: vague tasks, same file conflicts
- **🚨 CRITICAL PHASE BARRIERS**: Backend and frontend MUST develop in parallel - launch both agents simultaneously using single message with multiple Task tool calls, both must complete before integration
- **INCORRECT**: Running backend then frontend sequentially
- **CORRECT**: Launch backend-api-engineer and frontend-react-engineer simultaneously

## Task Generation Rules
*Applied during main() execution*

1. **From Contracts**:
   - Each contract file → backend-api-engineer: Specmatic MCP contract test task (sequential, no [P]) with JUnit report analysis
   - Each contract file → backend-api-engineer: Specmatic MCP resiliency test task (sequential, AFTER contract tests pass) with JUnit report analysis
   - Each endpoint → implementation task

2. **From Data Model**:
   - Each entity → model creation task [P]
   - Relationships → service layer tasks

3. **From User Stories**:
   - Each story → frontend-react-engineer: Component test [P] (if frontend)
   - Each story → contract test (via backend-api-engineer, sequential) with JUnit report analysis for implementation guidance
   - Quickstart scenarios → validation tasks
   - Environment switching → frontend-react-engineer tasks (dev/prod mode)

4. **Ordering**:
   - **Backend Phase**: Setup → Contract Tests (RED) → Models → Services → Endpoints (GREEN) → Contract Pass Verification → Resiliency Tests Assessment → Fix Any Issues Found → Final Resiliency Verification → Integration & Polish (REFACTOR)
   - **Frontend Phase**: Mock setup → Components → Component tests → Mock shutdown (runs in parallel with Backend Phase)
   - **Integration Phase**: Real backend → Frontend reconfiguration → Integration tests → Shutdown all
   - **CRITICAL**: Contract tests must fully pass before resiliency tests begin
   - **PRAGMATIC**: Resiliency tests may pass or fail initially - fix issues found and proceed
   - **🚨 CRITICAL**: Backend and Frontend phases execute in parallel after OpenAPI contract is defined - MUST launch both agents simultaneously
   - Dependencies block parallel execution within phases
   - Both Backend and Frontend phase completion gates block progression to Integration phase
   - **PARALLEL LAUNCH REQUIREMENT**: Use single message with multiple Task tool calls to launch backend-api-engineer and frontend-react-engineer agents simultaneously

## Validation Checklist
*GATE: Checked by main() before returning*

- [ ] All contracts have corresponding sequential Specmatic MCP tests with JUnit report analysis handled by backend-api-engineer in Backend Phase
- [ ] All entities have model tasks in Backend Phase
- [ ] Backend contract tests come before backend implementation (RED-GREEN-REFACTOR) - JUnit reports guide implementation decisions
- [ ] **CRITICAL**: Contract tests must fully pass (verified via JUnit reports) before resiliency tests begin
- [ ] Resiliency tests come after contract test completion (sequential assessment and fix approach) - JUnit reports identify specific issues to address
- [ ] Resiliency tests use pragmatic approach: assess current state via JUnit reports, fix any failures found
- [ ] **🚨 CRITICAL PARALLEL EXECUTION VERIFICATION**: Backend and Frontend phases can launch simultaneously using single message with multiple Task tool calls
- [ ] **PARALLEL AGENT VERIFICATION**: backend-api-engineer and frontend-react-engineer agents are configured for simultaneous launch
- [ ] **SEQUENTIAL EXECUTION PREVENTION**: No dependencies between Backend Phase and Frontend Phase that would force sequential execution
- [ ] Backend completion gate: Contract and resiliency tests must pass (confirmed via JUnit reports) before Integration Phase
- [ ] Frontend phase uses mock servers properly with shutdown when done
- [ ] Integration phase properly coordinates backend startup and frontend reconfiguration ONLY after both Backend and Frontend phases complete
- [ ] Parallel tasks truly independent within each phase
- [ ] Each task specifies exact file path
- [ ] No task modifies same file as another [P] task
- [ ] Mock lifecycle properly managed (startup → use → shutdown)
- [ ] Three-phase barrier dependencies enforced with parallel execution capability verified