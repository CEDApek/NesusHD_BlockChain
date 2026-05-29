# Software Engineering Test Report: Blockchain-Based Resource Sharing System

## 1. Introduction

### 1.1 Purpose of Writing
Briefly state that the report evaluates whether the blockchain-based resource sharing prototype meets its requirements, design objectives, functional expectations, and non-functional quality goals. Explain that the report is also used to assess implementation quality, test coverage, and remaining risks during the software engineering testing stage.

### 1.2 Background
Introduce the need for resource sharing platforms and the typical problems of such systems, including file ownership traceability, file integrity, download/payment records, reward fairness, and transparent transaction history. Explain why this repository uses a Vue 3 frontend, Flask REST API, and mocked ledger module to simulate blockchain-style upload, download, mining, and block query behavior.

### 1.3 Definitions
List and define the terms used in the test report: Functional Testing, Boundary Testing, Stress Testing, Interface / UI Testing, Unit Testing, Integration Testing, Blockchain Ledger, Transaction, Mining / Reward Confirmation, and File Hash / Integrity Verification.

### 1.4 References
List the materials referenced while preparing the report, such as repository source code, README, backend API implementation, `hyperledger/ledger.py`, frontend Vue components, Flask documentation, Vue/Vite documentation, and course software engineering testing references.

## 2. Test Overview
Summarize the overall testing strategy and scope. Include a table with the columns `Test Item`, `Test Purpose`, and `Test Content`. The table should cover unit/module testing, module-structure testing, functional verification, boundary testing, stress testing, and UI testing.

## 3. Object-Oriented / Module Structure Testing

### 3.1 Overall System Architecture
Describe the actual repository architecture: `frontend/` for the Vue 3 single-page application, `backend/app.py` for Flask routes, `hyperledger/ledger.py` for mock ledger classes, and `backend/uploads/` for local uploaded files. Include a Mermaid architecture diagram.

### 3.2 Specific Module Testing
Explain that module testing checks independent correctness first and then checks interactions among frontend components, REST endpoints, file storage, and ledger state.

#### 3.2.1 Unit Testing
Create module-level unit test tables for authentication, file upload, search/download, transaction/reward, block/ledger, and state management. Each table should use the columns `Test Content`, `Test Result`, and `Notes`.

#### 3.2.2 Module Consistency / CRC-Style Testing
Create a CRC-style module responsibility table. For each module, include Module Name, ID, Description, Responsibility / Function, and Collaborating Modules. Explain consistency checks between UI state, REST responses, file metadata, upload hashes, transaction records, and mined blocks.

## 4. Functional Verification Testing
Use black-box testing to verify visible behavior through the REST API and Vue UI. Create subsections for login/registration, resource publishing/upload, search/download, transaction/reward, blockchain ledger/block query, and system/admin/data management. Each table should use `Function Name`, `Input`, `Expected Output`, and `Actual Output`.

## 5. Boundary Testing
Explain how edge cases are tested to identify robustness issues. Create subsections for login/registration, upload, search/download, transaction/mining, and blockchain data validation. Each table should use `Function Name`, `Boundary Input`, `Expected Output`, and `Actual Output`.

## 6. Stress Testing
Describe simulated stress testing with non-production sample data. Include test environment, concurrent users, average response time, success rate, CPU usage, memory usage, and observed bottlenecks. Include at least one Markdown table and one Mermaid `xychart-beta` response-time chart.

## 7. User Interface Testing
Describe UI tests for major Vue pages/components: login, registration, home/resource list, upload, file detail/download, mining/reward, and block query/ledger. Each table should use `Function Name`, `Expected Operation`, and `Actual Operation`.

## 8. Conclusion on Software Functions
Summarize the test conclusion for each major module: login/registration, resource management, transaction/reward, blockchain ledger, and user interface. Indicate whether each part is normal, partially complete, or still requires improvement.

## 9. Analysis Summary

### 9.1 Capability
Summarize what the current prototype can do well, including account handling, file publishing, duplicate checking, resource listing, download limits, reward simulation, and block query.

### 9.2 Limitations
Summarize limitations discovered during testing, such as local simulation instead of real distributed blockchain nodes, limited stress environment, in-memory state, simplified mining/reward rules, incomplete security controls, and UI/error prompt improvements.

## 10. Test Resource Consumption
Describe the testing resources used: local development PC, browser, Python/Flask environment, Node.js/Vite environment, backend server, frontend dev server, local upload storage, and simulated concurrent HTTP requests.

