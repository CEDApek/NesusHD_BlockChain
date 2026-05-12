# Overall Design Report (Extract)

Project Name: Nexus BT Resource Sharing and Blockchain Incentive Platform (Planned Version)

## 1.4.4 Middle Layer - Authentication and User Module Design
The Authentication and User Module will be positioned in the middle layer and will serve as the orchestration hub between frontend identity interactions and backend authorization execution, while also connecting downward to ledger identity mapping capabilities. Within the overall architecture, this module is planned to standardize login, registration, role recognition, and ledger identity binding into a unified authentication chain, ensuring consistency between UI-level identity state and ledger-level identity identifiers. In future multi-role and multi-terminal access scenarios, this module will continue to act as the unified identity gateway for access boundary control and session governance.

This module will handle user registration, user login, credential verification, role resolution, session/token generation, and ledger identity mapping. To improve maintainability, the system is planned to adopt a segmented processing model of “authentication handling → authorization resolution → identity mapping”: credentials will be validated first, role scope will be resolved next, and chain/ledger identity binding will be completed afterward. With this design, downstream business modules will be able to reuse standardized authentication results and will not need to duplicate identity checks.

On the input side, this module will accept username, password, login session context, role request parameters, and registration data. On the output side, it will return token payloads, role information, user profile data, ledger identity identifiers, and authentication failure messages. The follow-up plan will include unified error semantics and error-code conventions (e.g., invalid credentials, insufficient role privileges, identity mapping exceptions), so the frontend can provide precise guidance and rollback handling.

In terms of dependencies, this module will rely on credential storage structures, password-encryption components, token management strategy, role configuration rules, and ledger user-registration interfaces. The implementation roadmap is planned to include persistent user storage, encrypted password persistence, token expiry/refresh mechanisms, RBAC-based access control, and safer ledger identity-binding strategies, followed by session revocation and abnormal-login protection.

Authentication and User Module Context Diagram:
```mermaid
graph TD
 U[User/Administrator] --> F[Frontend Login & Registration UI]
 F --> A[Authentication and User Module]
 A --> C[Credential Storage]
 A --> R[Role Configuration and Access Rules]
 A --> L[Ledger User Registration Interface]
 A --> T[Token Generation and Session Management]
 A --> O[Return Token, Role, and Ledger Identity]
```

Authentication and User Module Processing Flow:
```mermaid
flowchart TD
    A[Receive Login or Registration Request] --> B[Parse Username and Password]
    B --> C[Validate Input Format]
    C --> D[Query or Create User Credentials]
    D --> E[Verify Password or Store Encrypted Password]
    E --> F[Resolve User Role]
    F --> G[Ensure Ledger Identity Mapping]
    G --> H[Generate Token and User Profile]
    H --> I[Return Authentication Result]
```

Authentication and User Module Prototype Relationship Diagram:
```mermaid
graph LR
    A[Credential Input] --> B[Credential Validator]
    B --> C[Role Resolver]
    C --> D[Ledger Identity Mapper]
    D --> E[Token Manager]
    E --> F[Authentication Response]
```

## 1.4.5 Middle Layer - Resource File Management Module Design
The Resource File Management Module will be located in the middle-layer resource-governance position and will connect frontend resource interactions, backend service orchestration, underlying storage, and ledger resource managers. This module is planned to act as the unified entry point for the resource lifecycle, covering submission, validation, registration, query, download, and audit-trigger operations. Through this single-entry architecture, the system will maintain consistency, traceability, and governance control for shared resources.

This module will process file upload intake, metadata extraction, category management, hash-based duplicate checking, detail query, download permission checks, and download response generation. The system is planned to adopt a “split upload/download processing path”: the upload path will emphasize integrity, uniqueness, and registrability, while the download path will emphasize authorization legality, file existence, and behavior traceability. This approach will provide stable event sources for the reward subsystem and reduce cross-module coupling.

Inputs will include uploaded file streams, file names, file types, business metadata, query filters, and download requests. Outputs will include resource catalog lists, detail objects, deduplication results, binary download streams, and validation error information. For future scalability, the system is planned to support filtering by pagination/category/publisher/time and include auditable context in download responses.

Dependency-wise, this module will rely on hash-computation components, upload directories/object storage, file metadata models, ledger resource managers, authentication/authorization modules, and storage adapters. The follow-up implementation plan will extend object-storage support, asynchronous scanning, access auditing, rate limiting, and finer-grained file-type validation, along with configurable policy controls for high-concurrency and large-scale file scenarios.

Resource File Management Module Context Diagram:
```mermaid
graph TD
    U[User/Administrator] --> F[Resource Upload and Query UI]
    F --> M[Resource File Management Module]
    M --> H[Hash Computation Component]
    M --> S[(Upload Directory/Object Storage)]
    M --> D[File Metadata Structure]
    M --> L[Ledger Resource Manager]
    M --> A[Authentication and Authorization Module]
    M --> O[File List, Detail, or Download Stream]
```

Upload and Deduplication Flow:
```mermaid
flowchart TD
    A[Receive Uploaded File Stream] --> B[Read File Metadata]
    B --> C[Compute File Hash]
    C --> D{Duplicate File Exists?}
    D -- Yes --> E[Return Duplicate Validation Message]
    D -- No --> F[Persist File to Upload Directory or Object Storage]
    F --> G[Write Resource Metadata]
    G --> H[Register Resource on Ledger]
    H --> I[Return Upload Result]
```

Query and Download Flow:
```mermaid
flowchart TD
    A[Receive Query or Download Request] --> B[Verify User Identity and Permissions]
    B --> C[Parse Filter Conditions or Resource ID]
    C --> D[Query Resource Metadata]
    D --> E{Is Download Request?}
    E -- No --> F[Return Resource List or Details]
    E -- Yes --> G[Check Download Permission and File Existence]
    G --> H[Generate Binary Download Stream]
    H --> I[Record Download Event or Trigger Reward]
    I --> J[Return Download Response]
```

Resource File Management Module Prototype Relationship Diagram:
```mermaid
graph LR
    A[File Input] --> B[Metadata Extractor]
    B --> C[Hash Checker]
    C --> D[Storage Adapter]
    D --> E[Ledger Resource Manager]
    E --> F[Resource Response]
```

## 1.4.6 Middle Layer - Block Mining and Reward Module Design
The Block Mining and Reward Module will be positioned as the settlement core in the middle layer, connecting resource-download events, pending transaction pools, administrator mining operations, and ledger chain structures. This module will transform business actions into confirmable transactions and then produce block metadata and reward outcomes through mining confirmation workflows. As the platform evolves, this module is expected to remain a key carrier for on-chain governance and incentive-policy iteration.

This module will process pending-transaction aggregation, mining-trigger execution, reward calculation, block retrieval, block export, and wealth updates. The system is planned to adopt a two-stage model: “event intake to pending pool” and “mining confirmation for block settlement.” In this model, business-triggered events (such as download rewards) are first queued, and then administrators trigger mining to finalize block generation and reward settlement.

Inputs will include mining requests, pending transaction lists, block query filters, download reward events, and administrator operation context. Outputs will include block metadata, mining results, reward results, wealth-change outcomes, and pending-transaction statistics. A unified query interface is planned to support retrieval by block number, miner identifier, hash fragment, and time window, while standardized result payloads will support export/report workflows.

Dependencies will include ledger chain structures, pending transaction pools, timestamp formatting capabilities, administrator permission control, resource-download events, and future chaincode interfaces. The follow-up roadmap will connect real chaincode invocation, add on-chain event subscriptions, optimize reward rules, strengthen block-export capabilities, and improve audit traceability for cross-node collaboration and policy evolution.

Block Mining and Reward Module Context Diagram:
```mermaid
graph TD
    Admin[Administrator] --> UI[Block Management/Mining UI]
    User[Normal User] --> R[Resource Download Event]
    UI --> M[Block Mining and Reward Module]
    R --> M
    M --> P[Pending Transaction Pool]
    M --> C[Ledger Chain Structure]
    M --> W[Wealth/Reward Records]
    M --> A[Administrator Permission Control]
    M --> E[Block Export Interface]
    M --> O[Block Metadata and Reward Result]
```

Mining Processing Flow:
```mermaid
flowchart TD
    A[Receive Admin Mining Request] --> B[Validate Admin Permission]
    B --> C[Read Pending Transactions]
    C --> D{Pending Transactions Exist?}
    D -- No --> E[Return No Pending Transaction Message]
    D -- Yes --> F[Compute Block Metadata]
    F --> G[Generate New Block]
    G --> H[Update Ledger Chain Structure]
    H --> I[Settle Mining Reward]
    I --> J[Clear or Update Pending Transaction Pool]
    J --> K[Return Mining Result]
```

Download-Reward Trigger Flow:
```mermaid
flowchart TD
    A[User Triggers Resource Download] --> B[Resource Module Completes Permission Check]
    B --> C[Generate Download Event]
    C --> D[Mining/Reward Module Receives Event]
    D --> E[Compute Contribution or Download Reward]
    E --> F[Write to Pending Transaction Pool]
    F --> G[Update User Wealth-Change Record]
    G --> H[Wait for Subsequent Mining Confirmation]
```

Block Mining and Reward Module Prototype Relationship Diagram:
```mermaid
graph LR
    A[Pending Transactions] --> B[Mining Controller]
    B --> C[Block Generator]
    C --> D[Reward Calculator]
    D --> E[Ledger Updater]
    E --> F[Block/Reward Result]
```
