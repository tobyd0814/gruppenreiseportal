# Project Constitution and Engineering Protocol for AI-Assisted Development

## I. Core Principles and Architectural Mandates

This document establishes the foundational principles and non-negotiable architectural mandates for the project. It serves as the single source of truth for all engineering standards, practices, and technical decisions. The primary consumer of this document is the AI Development Agent, which is required to adhere to these directives without exception. These principles define "enterprise-grade" not as a set of features, but as a holistic approach to building resilient, scalable, secure, and maintainable software.

### 1.1. The Enterprise-Grade Mandate: A Definition

Enterprise-grade software is defined by its entire lifecycle, not merely by its source code. Its quality is a function of its design, development process, security posture, operational stability, and maintainability over time. The AI Agent's primary directive is to uphold this standard, prioritizing long-term quality over short-term implementation velocity. Every component, configuration, and line of code generated must be auditable, rigorously tested, and secure by design.[1, 2] The system must be built with the explicit assumption that it will evolve, scale to handle significant load, and withstand security threats.[2, 3] This mandate informs every subsequent decision in this constitution.

### 1.2. Architectural Pattern: Hybrid Microservices and Serverless

To achieve the required balance of scalability, maintainability, and cost-efficiency, the project will adopt a hybrid architectural pattern. This pattern combines the strengths of containerized microservices with the event-driven nature of serverless functions.[4, 5] This is not a choice between two competing models but a strategic integration of both, using each for the tasks to which it is best suited.[6, 7]

  * **Microservices Architecture:** Core business domains that are stateful, require complex logic, or involve long-running processes will be implemented as independent microservices. Examples include services for "User Authentication," "Order Processing," or "Product Catalog".[6, 8] Each microservice will be independently deployable, loosely coupled, and will own its own data to ensure strong domain boundaries.[6] These services will be containerized and managed by an orchestration platform, providing a high degree of control over performance and the runtime environment.[9, 10]

  * **Serverless Architecture:** Tasks that are stateless, event-driven, intermittent, or have highly variable (spiky) workloads are best suited for serverless functions (e.g., AWS Lambda).[4, 5, 6] Examples include "Image Resizing" triggered by an S3 upload, "Notification Dispatch" in response to a queue message, or simple data transformation APIs.[6, 11] This approach minimizes operational overhead, as infrastructure management is handled by the cloud provider, and offers a cost-effective pay-per-execution model.[9, 11]

  * **API-First Development Mandate:** All communication between the frontend, microservices, and serverless functions MUST occur via well-defined, versioned, and documented Application Programming Interfaces (APIs). The project will adhere to an API-first development model, where the API contract is designed and agreed upon before implementation begins.[4, 12, 13] This practice is fundamental to enabling parallel development, ensuring loose coupling, and facilitating independent deployment and scaling of services.[6, 12]

The selection of this hybrid model is a deliberate trade-off between control and convenience. Containerized microservices offer maximum control over the environment and are suitable for consistent, high-traffic workloads, but come with higher operational overhead.[6, 10] Serverless functions offer supreme convenience and automatic scaling but come with constraints like execution time limits and potential vendor lock-in.[6] By combining them, the architecture can optimize for both performance and cost across different parts of the application.

### 1.3. The "Shift-Left" Security Imperative

Security is not a final stage in the development process; it is a continuous discipline that must be integrated from the earliest phases of design and coding. This "Shift-Left" approach is a core mandate of the project.[14, 15] The AI Agent is required to incorporate security considerations at every step of its workflow. This includes, but is not limited to, writing code that is resilient to common vulnerabilities, managing dependencies to avoid known exploits, handling secrets and credentials securely, and ensuring the CI/CD pipeline performs comprehensive, automated security scanning.[13, 16]

As a baseline, all development must adhere to the principles and mitigation strategies outlined in the **OWASP Top 10**. This includes active defense against threats such as Injection, Broken Access Control, Cryptographic Failures, and the use of vulnerable or outdated components.[17] Security is a shared responsibility, and by embedding it into the automated development process, it becomes an intrinsic property of the system rather than an external check.[15]

### 1.4. The Immutability Principle: Infrastructure and Containers

To eliminate configuration drift and ensure absolute consistency across all environments (local development, staging, production), the project will strictly adhere to the principle of immutability.

  * **Immutable Infrastructure:** All cloud infrastructure is to be treated as immutable. Manual changes to running environments are strictly forbidden. To apply an update, a new set of infrastructure resources is provisioned based on the updated code, traffic is shifted to the new environment, and the old environment is decommissioned. This process is enforced through the mandatory use of Infrastructure as Code (IaC).[1, 18, 19]

  * **Immutable Containers:** Application containers are immutable artifacts. To update an application, a new Docker image is built, tagged with a unique version, and deployed. The running containers are then replaced with new instances created from the new image. No changes (e.g., patching, configuration updates) are ever made to a running container.[20, 21]

This principle is not merely a best practice but a foundational element of the project's reliability and security strategy. The complexity introduced by a microservices architecture, with its many moving parts, makes manual management and deployment untenable. This complexity directly necessitates the adoption of robust automation through CI/CD pipelines and IaC. The increased number of services also expands the potential attack surface, making the "Shift-Left" security principle an absolute requirement for managing risk at scale. The need for services to be deployed and scaled independently makes an API-first approach the only viable communication strategy. Finally, the dynamic and distributed nature of the system makes configuration drift a primary operational risk, which in turn makes the principle of immutability a core requirement for ensuring predictability and reliability in both infrastructure and application containers. A violation of one of these principles compromises the integrity of the entire system.

## II. The Technology Stack Blueprint

This section provides the definitive, non-negotiable technology stack for the project. These choices have been made to align with the architectural mandates defined in Section I, with a focus on performance, scalability, security, ecosystem maturity, and productivity for both human and AI developers. The AI Agent must generate all code and configuration exclusively for this stack.

### 2.1. Frontend: React with TypeScript

  * **Framework:** React.[22] The React library is selected for its component-based architecture, which promotes code reuse and maintainability. Its vast ecosystem of tools and libraries, along with strong community support, provides a robust foundation for building a complex user interface.[23, 24] React's flexibility is particularly well-suited for a microservices backend, allowing the UI to compose experiences from multiple, independent data sources.[23]
  * **Language:** TypeScript. The use of TypeScript is mandatory. Its static type system is critical for building enterprise-grade applications, as it catches errors during compilation rather than at runtime, improves code clarity and maintainability, and provides superior context and autocompletion for the AI Agent during code generation.[25]
  * **State Management:**
      * **Global State:** Redux Toolkit will be used for managing complex, application-wide state.
      * **Server State:** React Query is mandated for managing asynchronous operations, caching, and synchronizing server state. It simplifies data fetching, reduces boilerplate, and improves the user experience by providing features like background refetching and stale-while-revalidate strategies.
  * **Justification:** While Angular offers a highly structured, "all-in-one" framework that is excellent for large enterprise teams [26, 27], React's greater flexibility and unopinionated nature provide a better fit for a frontend that must integrate with a diverse and evolving set of microservices.[23] Vue, while simpler and faster to learn, has a smaller ecosystem and is less proven in large-scale, complex enterprise deployments compared to React.[23, 26]

### 2.2. Backend (Microservices & Serverless): Go (Golang)

  * **Language:** Go (Golang).[25] Go is the exclusive language for all backend services. It is chosen for its exceptional performance, low memory footprint, and first-class support for concurrency via goroutines and channels. These features make it ideal for building high-throughput, scalable network services.[2, 25]
  * **Frameworks & Libraries:**
      * **Microservices (REST APIs):** The Gin web framework will be used for its high performance and minimalistic API.
      * **Serverless Functions:** The standard `net/http` library is sufficient and preferred for its simplicity and performance in AWS Lambda environments.
  * **Justification:** Go's characteristics are uniquely suited to the project's hybrid architecture. Its raw performance and efficient concurrency model are ideal for building robust microservices capable of handling heavy loads.[25] Furthermore, Go compiles to a single, statically-linked binary with no external runtime dependencies. This results in extremely small container images and helps mitigate cold-start latency in serverless functions—a critical advantage for performance and cost-efficiency.[10] While Node.js provides a unified JavaScript ecosystem [1, 28] and Python excels in data science and machine learning [25, 28], Go delivers the optimal combination of execution speed, operational simplicity, and static type safety required for this architectural pattern.

### 2.3. Database: PostgreSQL

  * **System:** PostgreSQL, managed via Amazon Relational Database Service (RDS) for operational stability, automated backups, and scalability.
  * **Interaction Model:** In accordance with microservices best practices, each service that requires data persistence MUST own its own database schema. To enforce the strongest data boundaries, services should, where feasible, use separate database instances.[6] Cross-service database calls are strictly forbidden; communication must occur via APIs.
  * **Justification:** A relational database management system (RDBMS) like PostgreSQL is selected over NoSQL alternatives (e.g., MongoDB) due to its strict data integrity guarantees, ACID compliance, and powerful SQL querying capabilities, all of which are paramount for enterprise applications with complex business logic.[29] While NoSQL databases offer schema flexibility [30], the structured and predictable nature of PostgreSQL provides a more robust foundation for the core transactional systems of the application.

### 2.4. Containerization and Orchestration

  * **Container Runtime:** Docker. All application services will be packaged as Docker containers, ensuring a consistent runtime environment from local development to production.[20, 21]
  * **Orchestration:** Amazon Elastic Kubernetes Service (EKS).[31, 32]
  * **Justification:** Kubernetes (via EKS) is chosen over the simpler Amazon Elastic Container Service (ECS) for strategic, long-term reasons. EKS provides the full, upstream Kubernetes experience, which prevents vendor lock-in and grants access to the vast and mature Kubernetes ecosystem of tools for monitoring, service mesh (e.g., Istio), and advanced deployment strategies.[31, 32] While ECS offers tighter integration with the AWS ecosystem and a lower initial learning curve, the investment in EKS provides superior flexibility, control, and portability, which are critical advantages for an evolving enterprise platform.

The following table provides a summary of these technology decisions and their justifications, serving as a permanent architectural record.

| Technology Layer | Selected Technology | Key Evaluation Criteria | Justification |
| :--- | :--- | :--- | :--- |
| **Frontend Framework** | React with TypeScript | Ecosystem, Flexibility, Performance, Type Safety | Chosen for its vast ecosystem and component-based architecture, which is ideal for integrating with a microservices backend. TypeScript is mandated for code quality and maintainability.[22, 23] |
| **Backend Language** | Go (Golang) | Performance, Concurrency, Deployment Simplicity | Delivers exceptional performance and built-in concurrency for high-throughput services. Single binary deployment is optimal for both lightweight containers and serverless functions.[25, 28] |
| **Database System** | PostgreSQL (on AWS RDS) | Data Integrity, ACID Compliance, Querying Power | Provides strong consistency and reliability through its relational model, which is essential for enterprise-grade transactional systems. Outweighs the flexibility of NoSQL for core business logic.[29] |
| **Container Orchestration** | Amazon EKS (Kubernetes) | Portability, Ecosystem, Control, Long-Term Strategy | Offers the full power of upstream Kubernetes, avoiding vendor lock-in and providing access to a massive ecosystem of tools. The higher complexity is a worthwhile trade-off for strategic flexibility.[31, 32] |

## III. The Development Environment Protocol

This section details the mandatory configuration for a standardized, reproducible local development environment. Consistency between local, CI, and production environments is paramount to eliminating environment-specific bugs. The AI Agent MUST use this configuration to ensure that all code it generates is developed and tested in an environment identical to that of human developers.

### 3.1. Containerized Environment with Docker Compose

The entire local development environment, including the frontend application, all backend microservices, the database, and any other required services (e.g., caching layers), MUST be defined and managed through a single `docker-compose.yml` file.[20] This approach encapsulates all service dependencies and configurations, allowing any developer (human or AI) to spin up a fully functional, isolated environment with a single command. This is the primary mechanism for eliminating the "it works on my machine" problem.[20, 21] The AI will be provided with a starter `docker-compose.yml` template and will be expected to extend it as new services are created.

### 3.2. Code Linting and Formatting Configuration

Automated code linting and formatting are non-negotiable quality gates. All code MUST be automatically formatted to a consistent style and linted to enforce best practices before it can be committed. This process offloads the burden of stylistic correction from human reviewers to automated tooling, allowing them to focus exclusively on the logic and correctness of the implementation. The AI-generated code, while often functionally correct, can be stylistically inconsistent; this automated enforcement ensures all contributions adhere to project standards from the moment of creation.

  * **Frontend (TypeScript/React):**

      * **Formatter:** Prettier. A project-level `.prettierrc` configuration file will enforce a consistent code style. All developers and the AI Agent must use this configuration.json
        //.prettierrc
        {
        "printWidth": 80,
        "tabWidth": 2,
        "useTabs": false,
        "semi": true,
        "singleQuote": true,
        "trailingComma": "es5",
        "bracketSpacing": true,
        "arrowParens": "always"
        }
        ```
        ```
      * **Linter:** ESLint. A `.eslintrc.js` file will be configured with a strict ruleset to catch potential bugs and enforce best practices. The configuration will extend a reputable base configuration, such as `eslint-config-airbnb-typescript`, and include plugins for React, hooks, and accessibility.[33, 34, 35]
        ```javascript
        //.eslintrc.js (Simplified Example)
        module.exports = {
          extends: [
            'airbnb',
            'airbnb-typescript',
            'airbnb/hooks',
            'plugin:@typescript-eslint/recommended',
            'plugin:jest/recommended',
            'prettier', // Must be last to override other configs
          ],
          parserOptions: {
            project: './tsconfig.json',
          },
          rules: {
            // Project-specific overrides
            'react/react-in-jsx-scope': 'off',
            'react/jsx-props-no-spreading': 'off',
          },
        };
        ```

  * **Backend (Go):**

      * **Formatter:** `gofmt`. The standard, non-configurable Go formatter is mandatory. All Go code must be formatted with `gofmt` before commit.
      * **Linter:** `golangci-lint`. A `.golangci.yml` configuration file will enable a curated set of linters to check for a wide range of issues, including correctness, performance, style, and complexity.
        ```yaml
        #.golangci.yml (Simplified Example)
        run:
          timeout: 5m
        linters:
          enable:
            - gofmt
            - govet
            - errcheck
            - staticcheck
            - unused
            - ineffassign
            - typecheck
            - unconvert
        ```

  * **IDE Integration (Visual Studio Code):** To streamline this process, a `.vscode/settings.json` file will be included in the repository to configure the editor for automatic format-on-save and to enable integrated linter feedback.[36, 37]

    ```json
    //.vscode/settings.json
    {
      "editor.formatOnSave": true,
      "editor.defaultFormatter": "esbenp.prettier-vscode",
      "[go]": {
        "editor.defaultFormatter": "golang.go"
      },
      "eslint.validate": ["javascript", "javascriptreact", "typescript", "typescriptreact"],
      "go.lintOnSave": "workspace"
    }
    ```

### 3.3. Local Testing Configuration

The local development environment MUST provide the capability to run all layers of the testing pyramid.

  * **Unit & Integration Tests:** The Docker services for the frontend and each backend microservice will be configured with test-specific commands. For example, the frontend service will have a command to run Jest, and backend services will have a command to run `go test`.[38, 39]
  * **End-to-End (E2E) Tests:** The `docker-compose.yml` file will include a dedicated service for running Cypress. This service will be configured to execute E2E tests against the frontend service running within the same Docker network, simulating a real user environment.[40]

## IV. The Test-Driven Development (TDD) Cadence

Test-Driven Development (TDD) is the mandated development methodology for this project. In an AI-assisted workflow, TDD transcends its role as a development discipline to become the primary mechanism for precise, unambiguous, and verifiable communication between the human architect and the AI Agent. A natural language prompt can be ambiguous, but a failing test is a formal, machine-readable specification of required behavior. This transforms the AI from a general-purpose "coder" into a highly efficient "test-passer," where the quality and design are dictated by the human-authored tests.

### 4.1. The Red-Green-Refactor Cycle: An AI-Adapted Workflow

All feature development and bug fixing must follow the TDD cycle. This cycle is the fundamental unit of work and defines the interaction protocol between the human developer and the AI Agent.[41, 42, 43]

  * **Step 1: Red (Human-Driven Specification):** The human developer or architect writes a single, focused, and failing test. This test precisely defines the next piece of functionality to be implemented or the specific bug to be fixed. This failing test serves as the formal "prompt" and specification for the AI.[41, 44] It must be committed to the repository before any implementation code is written.

  * **Step 2: Green (AI-Driven Implementation):** The AI Agent's task is to receive the failing test and write the *minimum possible production code* required to make that specific test pass. The AI is explicitly forbidden from implementing any functionality not directly required to satisfy the test.[43, 45] The goal is simply to get the test suite to a "green" state.

  * **Step 3: Refactor (Human-AI Collaboration):** Once the test is passing, the code is functionally correct but may not be optimal. The human developer reviews the AI-generated code for clarity, efficiency, and adherence to design principles. The developer may then refactor the code directly or provide a specific refactoring instruction to the AI (e.g., "Extract this logic into a new function named `calculateTotal`"). Throughout this phase, the entire test suite must be run continuously to ensure that no existing functionality is broken. The code is only considered complete when it is clean, well-designed, and all tests are still green.[41, 42]

### 4.2. Unit Testing Protocol

Unit tests are the foundation of the testing pyramid. They must be fast, isolated, and focused on a single unit of behavior.

  * **Frontend (Jest & React Testing Library):** Every React component, hook, and utility function MUST have a corresponding suite of unit tests. Tests will be written using Jest as the test runner and React Testing Library for rendering and interacting with components.[38] The focus must be on testing the component's behavior from a user's perspective (e.g., "when the button is clicked, the form is submitted") rather than testing internal implementation details. External dependencies, such as API calls or Redux actions, must be mocked using `jest.fn()` to ensure test isolation.[46, 47]

  * **Backend (Go `testing` package):** Every exported function within a Go package MUST have corresponding unit tests. The standard library's `testing` package is to be used. To maintain isolation, any external dependencies (e.g., database connections, calls to other services) must be abstracted behind interfaces and replaced with test doubles (mocks or stubs) during testing.[39]

### 4.3. Integration Testing (Module Testing) Protocol

Integration tests verify the collaboration between multiple components within a single service. They are slower than unit tests but provide higher confidence that the service's internal modules work together correctly.

  * **Backend (Go `testing` package):** Integration tests will be written to validate entire workflows within a single microservice, such as an API request flowing through the controller, service, and repository layers to interact with a database. These tests will run against a real database instance that is spun up and torn down for the test run, managed via Docker.[39, 48] These tests should be in separate files or packages from unit tests (e.g., using a `_test` package suffix) to allow them to be run independently.

### 4.4. End-to-End (E2E) Testing Protocol

E2E tests provide the highest level of confidence by validating the entire system from the user's perspective. They are the most expensive to write and run, and should be reserved for critical user workflows.

  * **Framework:** Cypress is the mandated E2E testing framework.[40, 49]
  * **Scope:** E2E tests will simulate real user journeys by programmatically controlling a browser that interacts with the live frontend application. They will cover critical paths such as user registration, login, creating a core business resource, and viewing that resource.[50]
  * **Strategy:** E2E tests will not mock the backend. They will run against a fully integrated environment, either locally via the Docker Compose setup or in a dedicated staging environment within the CI/CD pipeline. They serve as the final validation that all microservices and the frontend are working together as expected.

## V. The Version Control and Commit Protocol

A disciplined approach to version control is essential for maintaining a clean, understandable, and auditable project history. This is especially critical in an AI-assisted workflow, where automated tools rely on a semantic history to perform tasks like generating changelogs and determining version bumps.

### 5.1. Git Flow and Branching Strategy

The project will adhere to a simplified Git Flow branching model:

  * **`main` branch:** This branch represents the production-ready state of the application. It is fully protected, and direct commits to `main` are strictly forbidden. Code is only merged into `main` from the `develop` branch as part of a formal release process.
  * **`develop` branch:** This is the primary integration branch. All feature branches are created from `develop` and must be merged back into `develop` via a pull request. This branch represents the latest development state of the application.
  * **`feature/<ticket-id>-<short-description>` branches:** All new work, whether a new feature or a bug fix, MUST be performed on a dedicated feature branch. The branch name must be prefixed with `feature/` and include a reference to the corresponding ticket or issue ID, followed by a brief, kebab-case description (e.g., `feature/PROJ-123-add-password-reset`).

### 5.2. The Conventional Commits Mandate

All commit messages, without exception, MUST adhere to the **Conventional Commits v1.0.0 specification**.[51] This structured format is machine-readable and is crucial for automating release notes and semantic versioning.

  * **Format:** The commit message must follow the structure: `<type>(<scope>): <subject>`
      * **`<type>`:** Must be one of the following:
          * `feat`: A new feature for the user.
          * `fix`: A bug fix for the user.
          * `chore`: Changes to the build process or auxiliary tools. No production code changes.
          * `docs`: Documentation only changes.
          * `style`: Changes that do not affect the meaning of the code (white-space, formatting, etc.).
          * `refactor`: A code change that neither fixes a bug nor adds a feature.
          * `perf`: A code change that improves performance.
          * `test`: Adding missing tests or correcting existing tests.
      * **`<scope>` (optional):** A noun specifying the section of the codebase affected (e.g., `api`, `auth`, `ui`).
      * **`<subject>`:** A concise description of the change in the imperative mood (e.g., "add password validation").
  * **Breaking Changes:** Any commit that introduces a breaking API change MUST indicate this by appending a `!` after the type/scope (e.g., `feat(api)!:`) and MUST include a `BREAKING CHANGE:` footer in the commit body explaining the change and migration path.[51]
  * **AI Assistance:** The AI Agent may be prompted to generate a commit message based on the staged changes. It must be instructed to strictly follow the Conventional Commits format.[52]

### 5.3. AI Agent Checklists for Git Operations

To enforce quality at every step, the AI Agent MUST execute the following checklists before performing key Git operations. These are non-negotiable, automated quality gates that prevent broken or non-compliant code from ever entering the repository's history.

**AI Pre-Commit Checklist**

This checklist must be successfully completed before a `git commit` operation is executed. Its purpose is to ensure that every single commit represents a stable, high-quality, and self-contained unit of work.

| Check ID | Check Description | Command to Execute (Example) | Expected Outcome | Action on Failure |
| :--- | :--- | :--- | :--- | :--- |
| PC-01 | **Format Code** | `npm run format` or `gofmt -w.` | All relevant files are formatted according to project standards. Exit code 0. | Abort commit. Report formatting failure. |
| PC-02 | **Lint Code** | `npm run lint` or `golangci-lint run` | No linting errors are reported. Exit code 0. | Abort commit. Report linting errors. |
| PC-03 | **Run Unit Tests** | `npm run test:unit` or `go test./...` | All unit tests must pass. Exit code 0. | Abort commit. Report test failures. |
| PC-04 | **Run Integration Tests** | `npm run test:integration` or `go test -tags=integration./...` | All integration tests must pass. Exit code 0. | Abort commit. Report test failures. |

**AI Pre-Push Checklist**

This checklist must be successfully completed before a `git push` operation is executed. Its purpose is to prevent desynchronization with the remote repository and to ensure work is integrated with the latest changes from the team, minimizing complex merge conflicts later.

| Check ID | Check Description | Command to Execute (Example) | Expected Outcome | Action on Failure |
| :--- | :--- | :--- | :--- | :--- |
| PP-01 | **Fetch Remote Changes** | `git fetch origin` | Successfully fetches latest objects and refs from the `origin` remote. | Abort push. Report fetch failure. |
| PP-02 | **Rebase onto Target Branch** | `git rebase origin/develop` | The current feature branch is successfully rebased on top of the latest `develop` branch. | Abort push. Report rebase conflict and await human intervention. |
| PP-03 | **Re-run All Local Tests** | `npm test` or `go test -tags=integration./...` | All unit and integration tests must pass after the rebase. Exit code 0. | Abort push. Report test failures that occurred after rebase. |

**AI Pre-Merge (Pull Request) Checklist**

This checklist serves as the final quality gate before code is proposed for integration into the `develop` branch. The pull request (PR) itself is the formal request for review.

| Check ID | Check Description | Verification Method | Expected Outcome | Action on Failure |
| :--- | :--- | :--- | :--- | :--- |
| PM-01 | **Clear PR Title & Description** | API call to Git provider / Manual check | PR title follows Conventional Commits format. Description links to the work item and explains the "why" of the change. | Block merge. Await human correction. |
| PM-02 | **CI Pipeline Success** | API call to GitHub Actions | The CI pipeline (defined in Section VI) must have completed successfully for the latest commit on the branch. | Block merge. Report CI failure. |
| PM-03 | **No Merge Conflicts** | API call to Git provider | The PR can be merged into the `develop` branch without conflicts. | Block merge. Instruct developer to rebase and resolve conflicts. |
| PM-04 | **Peer Review Approval** | API call to Git provider | At least one human engineer has approved the pull request. | Block merge. Await required approvals. |

## VI. The CI/CD Pipeline and DevSecOps Framework

The Continuous Integration and Continuous Delivery (CI/CD) pipeline is the automated embodiment of this constitution. It is the ultimate enforcer of the project's quality and security standards. If a code change cannot pass the gauntlet of automated checks in the pipeline, it cannot be merged or deployed.

### 6.1. Pipeline Orchestrator: GitHub Actions

All CI/CD workflows will be implemented using GitHub Actions. Workflow definitions will be stored as YAML files within the `.github/workflows/` directory of the repository, ensuring that the pipeline itself is version-controlled and treated as code.[53, 54] Workflows will be configured to trigger automatically on every `push` to a feature branch and on every `pull_request` opened against the `develop` branch.[53]

### 6.2. Pipeline Stages and Quality Gates

The primary CI pipeline, which runs on every pull request, is structured into sequential stages. A failure in any stage will immediately fail the entire pipeline run and block the associated pull request from being merged. This provides fast feedback to developers and ensures that only code meeting all quality criteria can be integrated.[55, 56]

The following table outlines the stages, their purpose, the tools used, and the non-negotiable quality gates that must be passed.

| Stage | Purpose | Tools | Trigger | Quality Gate (Pass/Fail Criteria) |
| :--- | :--- | :--- | :--- | :--- |
| **1. Static Analysis** | Enforce code style and identify potential bugs without executing code. | ESLint, Prettier, `golangci-lint`, `gofmt` | On every push to a PR | **Gate:** Zero linting errors or formatting inconsistencies. The build fails if any rule is violated. |
| **2. Unit & Integration Testing** | Verify the correctness of individual components and their interactions within a service. | Jest (Frontend), Go `testing` (Backend) | On every push to a PR | **Gate:** 100% of all unit and integration tests must pass. A minimum code coverage threshold of 85% must be met or exceeded. |
| **3. Security Scanning (DevSecOps)** | Proactively identify security vulnerabilities in source code and dependencies. | SAST (SonarQube), SCA (OWASP Dependency-Check), Secrets Scanner (TruffleHog) | On every push to a PR | **Gate:** The build fails if any 'Critical' or 'High' severity vulnerabilities are detected by any of the security scanners. No hardcoded secrets are allowed. |
| **4. Build & Containerize** | Compile application code, build immutable Docker images, and scan them for vulnerabilities. | Docker, Go Compiler, Trivy (Container Scanner) | On every push to a PR | **Gate:** All services must build successfully. Docker images are pushed to Amazon ECR. The build fails if any 'Critical' OS-level vulnerabilities are found in the final images. |
| **5. End-to-End Testing** | Validate critical user workflows across the entire integrated application stack. | Cypress | On every push to a PR | **Gate:** 100% of all E2E tests must pass when run against a temporary, fully deployed staging environment created for the PR. |

### 6.3. Continuous Security Scanning

In addition to the scans integrated directly into the PR pipeline, a continuous security monitoring posture will be established.

  * **Dynamic Application Security Testing (DAST):** DAST tools (e.g., OWASP ZAP, Invicti) simulate external attacks against a running application to find vulnerabilities that are only apparent at runtime.[15, 57, 58] Due to their longer execution time, DAST scans will be configured to run on a nightly or weekly schedule against the persistent staging environment, not as a blocking step in the main CI pipeline.
  * **Continuous Monitoring:** The production environment will be continuously monitored for security threats and anomalies using cloud-native tools (e.g., AWS GuardDuty) and logging analysis.[15]

This multi-layered security approach, combining SAST, SCA, and secrets scanning "left" in the CI pipeline with DAST and continuous monitoring "right" in deployed environments, provides comprehensive protection throughout the software development lifecycle.[16, 58]

## VII. Infrastructure as Code (IaC) and AWS Deployment Strategy

All cloud infrastructure for this project MUST be defined, versioned, and managed as code. Manual provisioning or modification of infrastructure via the AWS console is strictly forbidden. This IaC approach is fundamental to achieving repeatability, auditability, disaster recovery, and applying software engineering best practices to infrastructure management.[18, 19]

### 7.1. IaC Framework: Terraform

  * **Framework:** Terraform is the mandated IaC tool for defining and provisioning all AWS resources, including networking, compute, databases, and IAM policies.[18]
  * **Justification:** Terraform is chosen over cloud-specific alternatives like AWS CDK for its cloud-agnostic nature, declarative syntax, and mature ecosystem. This provides long-term strategic flexibility and avoids vendor lock-in, which is a key consideration for enterprise architecture.[18, 59]
  * **State Management:** Terraform's state file, which maps the code to real-world resources, will be stored remotely in an Amazon S3 bucket. State locking will be enabled using Amazon DynamoDB to prevent concurrent modifications and ensure safe collaboration among team members and automation systems.[18]

### 7.2. AWS Target Architecture Blueprint

The Terraform code will provision the following target architecture in AWS:

  * **Networking:** A custom Virtual Private Cloud (VPC) will be created, spanning multiple Availability Zones (AZs) for high availability. The VPC will be segmented into public subnets for internet-facing resources (e.g., load balancers) and private subnets for backend services and databases to enhance security.
  * **Compute (Microservices):** An Amazon EKS cluster will be provisioned to run the containerized Go microservices. The cluster's worker nodes will be managed by EC2 Auto Scaling Groups to ensure the application can scale based on demand.[32]
  * **Compute (Serverless):** AWS Lambda will host the serverless Go functions. These functions will be triggered by events from other AWS services, such as Amazon API Gateway for HTTP requests or S3 for object creation events.
  * **Database:** A highly available PostgreSQL database instance will be provisioned using Amazon RDS with Multi-AZ deployment enabled. This ensures automatic failover in the event of an AZ failure.
  * **Storage:** Amazon S3 will be used for two primary purposes: storing static assets for the frontend web application and storing user-generated content (e.g., uploaded files).
  * **Content Delivery Network (CDN):** Amazon CloudFront will be configured as a CDN, with the frontend S3 bucket as its origin. This will cache the React application's static assets at edge locations globally, providing low-latency access for users and reducing load on the origin.[59]
  * **Deployment Pipeline:** While GitHub Actions handles CI, AWS CodePipeline will be used for continuous deployment to persistent environments (staging and production). A CodePipeline workflow will be triggered when new container images are pushed to Amazon ECR by the CI process. It will then orchestrate the deployment to the EKS cluster using a blue/green or rolling update strategy to ensure zero-downtime releases.[60, 61]

### 7.3. Environment Strategy

The Terraform codebase will be structured using modules to promote reusability and maintainability. This modular structure will be used to create multiple, fully isolated environments (e.g., `dev`, `staging`, `production`). Each environment will have its own dedicated set of AWS resources and will be managed via separate Terraform workspaces. This ensures that changes to the staging environment cannot inadvertently affect production.

## VIII. AI Agent Interaction Model and Task Execution

This final section provides a meta-protocol that governs how the AI Agent interacts with this constitution, human developers, and the codebase. It defines the AI's persona, its operational constraints, and the required format for prompts and responses. This structured interaction model is essential for reducing the variance in AI output and ensuring its contributions consistently meet the project's high standards. This entire document serves as a comprehensive, persistent context window, and the following protocols define the specific user prompts within that context.

### 8.1. AI Persona and Core Directive

The following system prompt MUST be used to initialize the AI Agent for all tasks related to this project. It establishes its role, constraints, and primary objective.

> **System Prompt:** "You are an expert-level Senior Software Engineer. Your sole purpose is to assist in the development of a web application according to the rules and protocols defined in the 'Project Constitution' document. You must adhere to every mandate, protocol, and checklist within it. You will prioritize quality, security, and test coverage over simplistic or incomplete solutions. Your generated code must be clean, efficient, and idiomatic for the specified technology stack. When a request is ambiguous or conflicts with the Constitution, you must ask for clarification rather than making an assumption. You will politely refuse any request that directly violates the principles of the Constitution, citing the specific section that would be violated."

### 8.2. Prompt Engineering and Task Templates

To ensure clarity and precision, all tasks assigned to the AI Agent by human developers should follow a structured template. The AI should be instructed to request that tasks be provided in this format if they are not.[62, 63] The use of a TDD test as the core of the specification is the most critical element of this template.[44]

**Example Prompt Template: New Backend Feature**

```
**Task:** Implement a new feature in the 'user-service'.

**Feature Description:** Add an API endpoint that allows an authenticated user to change their own password. The new password must be validated against complexity rules and securely hashed before being stored.

**Acceptance Criteria (as TDD Tests):**
1.  **** Create a failing integration test for a `PUT /v1/users/me/password` endpoint.
2.  The test must simulate an authenticated user and provide the current password and a new password in the request body.
3.  The test must verify that a `204 No Content` response is returned on successful password change.
4.  The test must verify that a `401 Unauthorized` response is returned if the provided current password is incorrect.
5.  The test must verify that a `400 Bad Request` response is returned if the new password is too short (less than 12 characters).
6.  The test must verify that after a successful change, the user's password hash in the database has been updated to a new value.

**AI's Task:**
1.  Acknowledge receipt of the failing test specification.
2.  **** Write the minimal amount of production code (controller, service, repository layers) required to make all specified tests pass. Do not add any functionality not covered by these tests.
3.  Ensure the new password is hashed using the project's standard hashing algorithm (bcrypt).
4.  Provide the complete, `gofmt` formatted, and `golangci-lint` clean Go code for all new or modified files.
5.  Generate a Conventional Commit message for this change.
```

### 8.3. Response Format and Deliverables

The AI Agent's responses to code-generation tasks MUST be structured and predictable to facilitate automated processing and human review. For any such task, the AI's output must include the following components in order:

1.  **Summary of Changes:** A brief, one-paragraph summary describing the approach taken and the files that were modified.
2.  **Generated Code:** The full contents of all new or modified files. Each file's content must be enclosed in a separate, language-tagged markdown code block (e.g., ` go...  ` or ` typescript...  `).
3.  **Checklist Confirmation:** A statement confirming that the generated code conceptually passes all relevant local checks from the Pre-Commit Checklist (e.g., "Confirmation: Code is formatted, linted, and passes all specified unit and integration tests.").
4.  **Conventional Commit Message:** A complete, correctly formatted commit message ready to be used.
      * **Example:**
        ```
        feat(auth): allow users to change their own password

        Implements a secure password change endpoint at PUT /v1/users/me/password.

        The endpoint requires the user's current password for verification and validates the new password against complexity rules before updating the stored hash.
        ```

<!-- end list -->

```
```
