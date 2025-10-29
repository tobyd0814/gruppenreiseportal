# Engineering Standards and Protocols

## Core Principles

### Enterprise-Grade Mandate
Enterprise-grade software is defined by its entire lifecycle. Every component must be auditable, rigorously tested, and secure by design. The system must evolve, scale, and withstand security threats.

### Shift-Left Security
Security is integrated from the earliest phases of design and coding. All development must adhere to **OWASP Top 10** principles, including defense against Injection, Broken Access Control, Cryptographic Failures, and vulnerable components.

### Immutability Principle
- **Infrastructure:** Treat all cloud infrastructure as immutable. No manual changes to running environments. Updates via Infrastructure as Code only.
- **Containers:** Application containers are immutable. Deploy new images rather than modifying running containers.

## Test-Driven Development (TDD)

### Red-Green-Refactor Cycle
1. **Red:** Write a failing test that defines the next piece of functionality
2. **Green:** Write minimum code required to make the test pass
3. **Refactor:** Improve code quality while keeping tests green

### Coverage Requirements
- Minimum **85% code coverage** for all services
- E2E tests required for critical user journeys
- Tests must pass before merging

## Version Control

### Git Flow Branching
- **`main` branch:** Production-ready code (protected, no direct commits)
- **`develop` branch:** Primary integration branch
- **`feature/<ticket-id>-<description>` branches:** All new work

### Conventional Commits (MANDATORY)
Format: `<type>(<scope>): <subject>`

**Types:**
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Code style changes (no logic change)
- `refactor`: Code refactoring
- `test`: Adding or updating tests
- `chore`: Maintenance tasks
- `perf`: Performance improvements

**Examples:**
```
feat(booking): add room block reservation
fix(auth): resolve token expiration issue
docs(readme): update installation instructions
```

### Breaking Changes
Append `!` after type/scope and include `BREAKING CHANGE:` footer explaining the change.

### Pre-Commit Checklist
- [ ] Format code (Prettier/gofmt)
- [ ] Lint code (ESLint/golangci-lint)
- [ ] Run unit tests
- [ ] Run integration tests

### Pre-Push Checklist
- [ ] Fetch remote changes
- [ ] Rebase onto target branch
- [ ] Re-run all tests after rebase

## CI/CD Pipeline

### Pipeline Stages (GitHub Actions)

| Stage | Purpose | Tools | Quality Gate |
| :--- | :--- | :--- | :--- |
| **Static Analysis** | Enforce code style | ESLint, Prettier, golangci-lint, gofmt | Zero linting errors |
| **Testing** | Verify correctness | Jest, Go testing | 100% tests pass, 85% coverage minimum |
| **Security Scanning** | Identify vulnerabilities | SAST, SCA, Secrets Scanner | No Critical/High severity issues |
| **Build & Containerize** | Build images | Docker, Trivy | Successful build, no Critical vulnerabilities |
| **E2E Testing** | Validate workflows | Cypress | 100% E2E tests pass |

### Security Scanning
- **SAST:** Static application security testing in CI pipeline
- **SCA:** Software composition analysis for dependency vulnerabilities
- **Secrets Scanner:** Detect hardcoded secrets (e.g., TruffleHog)
- **DAST:** Dynamic testing against running application (nightly/weekly)

## Code Style

### General Conventions
- Use meaningful variable names
- Comment complex logic (explain "why" not "what")
- Keep functions small (single responsibility)
- No commented-out code

### Error Messages
**CRITICAL:** All error messages must be human-readable.

**Good:** "Trip name is required. Please provide a name for your trip."
**Bad:** "ValidationError: field 'name' failed on 'required' tag"

## API Standards

### RESTful Conventions
- Versioning: `/api/v1/resource`
- HTTP methods: GET, POST, PUT, PATCH, DELETE
- Standard status codes: 200, 201, 204, 400, 401, 404, 500

### Response Format
```json
{
  "data": { /* resource */ },
  "meta": {
    "timestamp": "2025-10-30T12:00:00Z",
    "requestId": "abc-123"
  }
}
```

### Correlation IDs
- Include `X-Correlation-ID` header in all requests
- Propagate through all services
- Include in all log messages

## Logging

### Best Practices
- Always include correlation ID
- Log structured data (JSON format)
- Don't log sensitive information (passwords, tokens, payment details)
- Use appropriate log levels: DEBUG, INFO, WARN, ERROR

## Database

### Conventions
- Primary keys: UUID strings
- Timestamps: `created_at`, `updated_at` (snake_case)
- Soft deletes: `deleted_at` timestamp
- Foreign keys: Always include constraints with proper cascading

### Microservices Pattern
- Each service owns its own database schema
- Separate database instances where feasible
- No cross-service database calls (use APIs)

## Infrastructure as Code

### Principles
- All infrastructure defined as code
- No manual changes via console
- Version controlled
- Immutable infrastructure

## AI Agent Interaction Protocol

### AI Persona and Core Directive

The AI Agent must act as an expert-level Senior Software Engineer, adhering to all protocols in this document. Key directives:
- Prioritize quality, security, and test coverage over speed
- Generated code must be clean, efficient, and idiomatic
- Ask for clarification when requests are ambiguous
- Politely refuse requests that violate engineering principles

### Task Template Structure

All tasks should follow a structured format with TDD tests as the core specification:

**Example:**
```
Task: Implement feature in 'service-name'

Feature Description: [Clear description]

Acceptance Criteria (as TDD Tests):
1. Create failing test for endpoint/functionality
2. Test must verify expected behavior
3. Test must verify error cases
...

AI's Task:
1. Acknowledge receipt
2. Write minimum code to pass tests
3. Ensure code is formatted and linted
4. Generate Conventional Commit message
```

### Required AI Response Format

1. **Summary of Changes:** Brief description of approach and modified files
2. **Generated Code:** Full file contents in language-tagged markdown blocks
3. **Checklist Confirmation:** Confirm code passes formatting, linting, and tests
4. **Conventional Commit Message:** Ready-to-use commit message

---

**Last Updated:** 2025-10-30
