# [SE] Group XX Project Development Plan

**Group Number: XX**  
**Member A: [Name]**  
**Member B: [Name]**  
**Member C: [Name]**  
**Member D: [Name]**  
**Member E: [Name]**

---

## Repository Findings Summary / 仓库调研摘要

### 1) Main folders / 主要目录
- `backend/`: Flask REST API service, file upload/download handling, user account bootstrapping, ledger-related endpoints, and local upload storage integration via `backend/uploads/`.  
  `backend/app.py` is the runtime entry and includes route logic for login, registration, file catalogue, mining rewards, and block query.  
- `frontend/`: Vue 3 + Vite single-page web client containing login, file list/detail, upload form, wealth panel, and mined-block views.  
  Key components include `LoginForm.vue`, `FileList.vue`, `FileDetail.vue`, `UploadForm.vue`, and `MinedBlocks.vue`.  
- `hyperledger/`: Python in-memory mock ledger module (`ledger.py`) implementing users, resources, transactions, and chain/block abstractions to simulate Hyperledger-like behavior without external network dependency.

### 2) Main technologies / 主要技术栈
- Backend: **Python + Flask + Flask-CORS**.  
- Frontend: **Vue 3 + Vite + Axios**.  
- Blockchain layer: **Mock Hyperledger/Fabric-style in-memory ledger** (not real Fabric deployment yet).  
- Data persistence strategy: in-memory ledger state + local filesystem (`backend/uploads`) for uploaded binaries.

### 3) Main implemented features / 已实现核心功能
- Demo user authentication and account registration (`/api/login`, `/api/register`).
- Resource catalogue and metadata retrieval (`/api/files`, `/api/files/<owner>/<file_id>`).
- File upload with size limits, category normalization, and duplicate checks.
- File download endpoint with per-user attempt limit.
- Mining/reward simulation and wealth query (`/api/ledger/reward`, `/api/ledger/balance`).
- Block history browsing with role-based visibility (`/api/blocks`), including broader view for admin and personal-only view for members.
- Frontend dashboard with tabs for community files, upload workflow, and mined blocks, including mining-progress overlay and identity masking.

---

## 1. Project Overview / 项目概述

### English
This project aims to deliver a course-level prototype of a **blockchain-inspired BT resource sharing platform** (“Nexus Demo Stack”), combining a web frontend, a REST backend, and a simulated ledger module. The implemented system demonstrates how users can register/login, upload and share files, perform controlled downloads, and obtain wealth/reward updates through a simplified mining process.

From a software engineering perspective, the plan focuses on **structured completion** rather than theoretical blockchain design. The team will treat the existing repository as a baseline and execute a controlled development process covering requirement stabilization, architecture clarification, feature hardening, test completion, documentation, and final demonstration packaging.

### 中文
本项目面向课程实践，目标是完成一个“**区块链理念驱动的BT资源共享平台原型**”（Nexus Demo Stack）。系统由前端单页应用、后端 REST API 以及模拟账本模块组成，实现了用户注册登录、资源上传下载、奖励累积与区块记录查询等核心流程。

本计划书不重复需求说明书内容，而是强调“**如何把项目做完并交付**”：以当前仓库的已有实现为基线，通过任务拆分、时间安排、团队协作、测试验收、风险控制等手段，完成可演示、可测试、可复现的课程项目交付。

---

## 2. Development Scope / 开发范围

### 2.1 In-scope features / 范围内功能
1. **Authentication and account lifecycle**: keep demo credential model and complete registration/login flow consistency across frontend and backend.
2. **File resource workflow**:
   - List/search/filter community files.
   - Upload files with size check (0–100MB), category selection, and metadata persistence.
   - File detail view and download processing.
3. **Integrity and anti-duplication controls**:
   - Duplicate name/hash validation in upload flow.
   - Consistent backend enforcement independent of frontend checks.
4. **Reward and mining simulation**:
   - Pending transaction accumulation.
   - Mining action to settle rewards and create block entries.
5. **Block visibility and role permissions**:
   - Admin can inspect broader chain records.
   - Member can inspect self-mined blocks only.
6. **Operational quality**:
   - API contract alignment.
   - Frontend interaction stability.
   - Regression test suite and project documentation.

### 2.2 Out-of-scope features / 范围外功能
1. Real Hyperledger Fabric network deployment (peer/orderer/CA orchestration).
2. Production-grade auth/token refresh/secure session management.
3. Distributed storage layer (IPFS, cloud object storage, etc.).
4. Horizontal scaling, high-availability deployment, and container orchestration.
5. Advanced legal compliance modules (copyright adjudication workflow, audit system).
6. Native desktop/mobile clients.

---

## 3. System Architecture and Technical Approach / 系统架构与技术路线

### 3.1 Repository-based architecture / 基于仓库的系统结构
The architecture follows a **three-layer prototype model**:

- **Presentation Layer (`frontend/`)**: Vue SPA with component-based UI; Axios consumes backend APIs; Vite dev server and proxy support local integration testing.
- **Service Layer (`backend/`)**: Flask route handlers orchestrate login, file operations, mining, and block query; also enforce upload/download constraints.
- **Ledger Domain Layer (`hyperledger/`)**: Python in-memory domain objects (`SharedFile`, transactions, block records, user managers) emulate ledger behavior.

### 3.2 Technical stack actually present / 实际技术栈说明
- Python runtime with Flask API style.
- Vue 3 composition API + Vite build toolchain.
- Axios as frontend HTTP client.
- Local file system for upload binaries (`backend/uploads`).
- Unit-test scaffolding in Python (`backend/test_app.py`, `hyperledger/test.py`), though quality and alignment need cleanup.

### 3.3 Technical approach for course completion / 课程完成技术路径
1. **Contract-first stabilization**: establish an endpoint table from current `app.py`; align frontend calls and error handling.
2. **Feature closure by workflow**: login → upload → file list/detail → download → mine → block inspection as one end-to-end chain.
3. **Validation hardening**: ensure backend is source of truth for limits (size, duplicates, download attempts).
4. **Testability improvement**: prepare deterministic test data and scripted API checks.
5. **Documentation-driven delivery**: README update, API usage, setup scripts, testing report, demo script.

---

## 4. Team Roles and Responsibilities / 团队角色与职责（5人）

| Member | Role | Concrete Responsibilities | Output Artifacts |
|---|---|---|---|
| Member A: [Name] | Project Lead & Integration Owner | Own sprint planning, integration branch control, release checklist, milestone review, cross-module conflict resolution. Maintain final release branch and tag. | Iteration plan, release checklist, integration logs, final tagged release. |
| Member B: [Name] | Backend/API Engineer | Maintain `backend/app.py` routes, validation logic, upload/download handling, role access rules, and API error schema. | Updated backend code, API contract table, backend test cases, bug-fix records. |
| Member C: [Name] | Ledger Module Engineer | Refactor/extend `hyperledger/ledger.py` domain logic (reward settlement, block query support, user/resource indexing), keep deterministic behavior for tests. | Ledger module improvements, ledger unit tests, data model notes. |
| Member D: [Name] | Frontend Engineer | Implement/adjust Vue components and state flows (login, tabs, detail, upload, mined blocks), UI feedback for backend errors, demo usability polish. | Frontend code updates, interaction test checklist, screenshots/demo pages. |
| Member E: [Name] | QA & Documentation Engineer | Build test matrix (unit/integration/functional), execute test runs, collect evidence, maintain user/deployment docs and final acceptance report. | Test report, acceptance checklist, deployment guide, final PDF document pack. |

---

## 5. Task Breakdown / 任务分解

| Task ID | Task Description | Responsible Member(s) | Expected Output | Dependency |
|---|---|---|---|---|
| T1 | Baseline audit: map current modules, endpoints, and component interactions | A + B + D | Architecture map + API inventory v1 | None |
| T2 | Define API response/error normalization rules | B | API contract spec (status code, message, payload schema) | T1 |
| T3 | Backend input validation hardening (upload size/category/hash/name checks) | B | Merged backend validation patch + test cases | T2 |
| T4 | Ledger reward/block data consistency refactor | C | Stable ledger behavior for mining and block listing | T1 |
| T5 | Download attempt limit verification and owner exception handling tests | B + C | Download-policy logic test report | T3, T4 |
| T6 | Frontend API adaptor cleanup (consistent parsing and fallback messages) | D | Updated API service calls and UI error prompts | T2 |
| T7 | File list/detail/upload UX alignment with backend constraints | D | Updated `FileList`/`FileDetail`/`UploadForm` behavior | T3, T6 |
| T8 | Mined blocks view filtering and role-permission behavior checks | D + C | Verified admin/member display behavior | T4, T6 |
| T9 | End-to-end flow script (register/login/upload/download/mine/query blocks) | E + A | Reproducible E2E test script and run logs | T3–T8 |
| T10 | Unit tests for ledger and critical API routes | E + B + C | Automated test files + pass/fail report | T3, T4 |
| T11 | Documentation pack (README, API guide, deployment steps, test report) | E + A | Submission-ready docs set | T9, T10 |
| T12 | Final demo rehearsal and release packaging | A + all | Demo checklist, release tag, presentation materials | T11 |

---

## 6. Development Schedule and Timeline / 开发进度与时间安排

> Proposed for a 10-week university cycle.

| Week / Date Range | Planned Task | Responsible Member(s) | Output |
|---|---|---|---|
| Week 1 (W1) | Kickoff, repository audit, architecture and scope confirmation | A, B, C, D, E | Project charter, scope baseline, task board initialized |
| Week 2 (W2) | API contract extraction and normalization rules | B, A | API contract v1 + endpoint checklist |
| Week 3 (W3) | Backend validation hardening + ledger consistency patch design | B, C | Technical design notes + first backend PR |
| Week 4 (W4) | Implement ledger/data consistency changes and backend integration | C, B | Integrated backend-ledger branch |
| Week 5 (W5) | Frontend API integration cleanup and UI error-state alignment | D | Frontend integration PR + UI behavior notes |
| Week 6 (W6) | Feature chain verification (upload/download/mine/blocks) | D, B, C | End-to-end function pass report v1 |
| Week 7 (W7) | Unit test completion (backend + ledger), bug fixing sprint | E, B, C | Test suite v1 + defect closure list |
| Week 8 (W8) | Integration testing and acceptance pre-check | E, A, D | Integration test report + acceptance gap list |
| Week 9 (W9) | Documentation freeze, deployment guide, demo script finalization | E, A | Final docs package draft |
| Week 10 (W10) | Final rehearsal, delivery packaging, presentation submission | A, B, C, D, E | Final release candidate + course submission bundle |

---

## 7. Milestones / 阶段里程碑

| Milestone | Target Time | Exit Criteria | Deliverables |
|---|---|---|---|
| M1: Scope & Architecture Freeze | End of W2 | Scope confirmed; API inventory complete | Scope statement, architecture diagram, API list |
| M2: Core Backend-Ledger Stable | End of W4 | Upload/download/reward/block core endpoints stable | Backend + ledger integrated build |
| M3: Frontend Functional Closure | End of W6 | Main tabs and workflows runnable without critical blockers | Frontend integrated demo build |
| M4: Test Completion Gate | End of W8 | Unit + integration + functional checks executed, major defects fixed | Full test report and bug closure sheet |
| M5: Final Delivery Gate | End of W10 | Documentation complete, demo rehearsed, release packaged | Final source package, docs, presentation assets |

---

## 8. Collaboration and Workflow / 协作机制与研发流程

### 8.1 GitHub workflow
- Repository as single source of truth; all work tracked by Issues and Pull Requests.
- Every task ID (T1–T12) mapped to an Issue label and assignee.
- Commit convention: `type(scope): summary` (e.g., `fix(backend): enforce upload size limit`).

### 8.2 Branching strategy
- `main`: protected stable branch for accepted increments only.
- `develop` (or integration branch): staging branch for weekly integration.
- Feature branches: `feature/<task-id>-<short-topic>`.
- Hotfix branches: `hotfix/<defect-id>` for urgent regression fixes.

### 8.3 Issue and task tracking
- Use GitHub Projects board columns: **Backlog → In Progress → In Review → Done**.
- Each Issue contains: objective, technical plan, acceptance checks, dependency, and estimated effort.
- Weekly burndown snapshot maintained by Member A.

### 8.4 Code review protocol
- Minimum 1 reviewer for normal changes; 2 reviewers for core backend/ledger modifications.
- Mandatory checklist in PR:
  1) Scope alignment with Issue.
  2) Test evidence attached.
  3) No breaking change to agreed API schema (or explicit migration notes).
  4) Documentation updated if interface behavior changed.

### 8.5 Meeting and reporting
- Weekly fixed meeting (60–90 min): progress, blockers, next-week commitments.
- Mid-week async status update in group channel using template:
  - Completed this week
  - Current blockers
  - Planned next steps
- Member E consolidates test/document progress; Member A publishes weekly summary.

---

## 9. Testing and Acceptance Plan / 测试与验收计划

### 9.1 Unit testing
**Objective**: verify correctness of isolated functions and module-level logic.

- Backend unit tests (Flask route behavior and validators):
  - Registration/login success and failure paths.
  - Upload size boundary (0 byte reject, >100MB reject).
  - Category normalization and invalid input fallback.
  - Duplicate name/hash rejection.
  - Download attempt counter (up to 2 allowed for non-owner).
- Ledger unit tests (`hyperledger/ledger.py`):
  - User registration and address mapping.
  - File add/remove/search logic.
  - Transaction creation and block append behavior.
  - Role-sensitive block listing helper behavior where applicable.

### 9.2 Integration testing
**Objective**: verify backend + ledger + frontend API interaction.

- API integration chains:
  1) Register → Login → Fetch categories → Upload → Query file list.
  2) Member download action → pending reward creation → miner rewards settlement.
  3) Mine block → query block list with admin vs member identities.
- Data consistency checks:
  - Uploaded file visible in list/detail with aligned metadata.
  - Wealth and pending transactions update after mining.
  - Block record count increments with each mining completion.

### 9.3 Functional testing (user-perspective)
**Objective**: validate major user stories.

| Scenario | Steps | Expected Result |
|---|---|---|
| F1 New account onboarding | Register account, login, access dashboard | Account created, role shown, initial wealth state available |
| F2 File publishing | Upload valid file with category and description | Upload succeeds; file appears in community catalogue |
| F3 Duplicate prevention | Upload file with duplicated base name/hash | Backend rejects and frontend shows clear error |
| F4 Controlled download | Download same community file three times | 1st/2nd succeed, 3rd rejected with limit message |
| F5 Mining and wealth update | Trigger mine after pending actions | New block generated, wealth and pending stats refreshed |
| F6 Role-based block view | Query blocks as admin and member | Admin sees broader chain; member sees personal mining history |

### 9.4 Final acceptance criteria
Project is accepted when all criteria below are met:
1. Core workflow (register/login/upload/download/mine/block query) runs end-to-end without critical defects.
2. API behavior matches documented contract for major endpoints.
3. Unit + integration + functional test reports are submitted with reproducible evidence.
4. Deployment steps on a clean machine can reproduce the demo environment.
5. Final presentation demonstrates at least two roles (admin/member) and one complete mining cycle.

---

## 10. Risk Analysis and Mitigation / 风险分析与应对

| Risk | Impact | Probability | Mitigation |
|---|---|---|---|
| Mock ledger diverges from backend assumptions | Inconsistent behavior, integration defects | Medium | Define explicit interface contract between `app.py` and ledger methods; add integration tests for each route-chain. |
| Frontend-backend API mismatch during parallel development | Feature delays and unstable demo | High | Freeze API schema at W2; enforce PR checks with payload examples; D & B run joint verification sessions weekly. |
| Upload/download edge-case bugs (size, duplicates, attempt limits) | Core feature failure in demo | Medium | Boundary-focused test cases; bug triage within 24h; prioritize fixes before new features. |
| Overly optimistic schedule near submission | Incomplete testing or documentation | Medium | Reserve W9–W10 for stabilization only; no major new feature after W8 gate. |
| Team member workload imbalance | Bottleneck and quality drop | Medium | Member A monitors task distribution weekly; rebalance via paired work on blocked tasks. |
| Environment setup inconsistency across members | “Works on my machine” failures | Medium | Standardize setup scripts and versions; maintain a verified deployment guide and clean-env validation log. |

---

## 11. Final Deliverables / 最终交付物

1. **Source Code Package / 源代码包**
   - Complete repository with tagged release version.
   - Clean branch history for deliverable scope.

2. **Documentation Set / 文档集**
   - Project Development Plan (this document).
   - Updated README with setup/run instructions.
   - API usage notes and known constraints.

3. **Deployment Instructions / 部署说明**
   - Backend environment setup and launch commands.
   - Frontend install/build/run commands.
   - Local demo data and account preparation guide.

4. **Test Report / 测试报告**
   - Unit, integration, and functional test cases.
   - Execution results, defect list, and closure status.

5. **Final Demo/Prototype / 最终演示原型**
   - Demonstrable web interface.
   - At least one full scenario script: register/login → upload/download → mine → block and wealth verification.

---

## Appendix: Suggested Week-by-Week Reporting Template / 附录：周报模板（建议）

- Week No.
- Tasks committed last week
- Tasks completed this week
- Defects discovered/resolved
- Risks/blockers requiring support
- Plan for next week
- Evidence links (PRs, test logs, screenshots)

