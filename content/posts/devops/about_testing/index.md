---
title: About Testing
date: 2025-12-25
summary: 📝 Quick notes on Testing...
tags:
  - devops
  - testing
---

## Structured Testing
- Impact Analysis (Pre-Development)
- Before implementation, categorize the scope of change to determine the testing depth:
  - Domain / Logic Layer: Business rules or calculations
  - Data Persistence Layer: Schemas, queries or indexing
  - Transport / Interface Layer: API contracts, routing, or middleware
  - Cross-Cutting Concerns: Performance, security or concurrency
### Test Categorization & Strategy
- Unit Testing (Isolated Logic)
  - Target: Individual functions / methods in the Service or Domain layer.
  - Success Scenarios: Validate that valid inputs yield expected outputs.
  - Boundary Value Analysis (BVA): Test minimum, maximum, and just-outside-the-range values.
  - Error handling: Ensure the system throws specific exceptions/errors for invalid inputs.
  - Isolation: Use Mocks / Stubs for external dependencies.
- Integration Testing (Component Interaction)
  - Target: Repository layer and inter-module communication.
  - Ephemeral Environments: Use Docker (Test containers) to simulate a real database/Redis instance for testing.
  - State Verification: Ensure that data written by the Repository can be correctly retrieved and follows schema constraints.
  - Side Effects: Verify that events (MQ triggers) are correctly worked after a logic change.
- API & Contract Testing
  - Target: HTTP/gRPC request and response behavior (handler layer).
  - Payload Validation: Ensure the API rejects malformed JSON and returns correct HTTP status codes.
  - Authentication / Authorization: Test that protected routes are inaccessible without valid tokens.
  - End-to-End Flow: Request → Handler → Service → DB → Response.
- Continuous Quality Assurance
  - Bug-Driven Development: Whenever a bug is found, write a test case that reproduces it before fixing it to ensure it never returns.
  - Idempotency Tests: Ensure that calling the same API/function multiple times doesn't result in inconsistent data states.
- Automation & CI Pipeline
  - Configure CI (GitHub Actions) to block Merges if:
    - Tests fail
    - Test coverage decreases
    - Linting/Style checks fail
      - Dead Code: Variables declared but never used.
      - Security Risks: Hardcoded passwords or insecure / deprecated function calls.

## CI/CD

### Definition

- CI: Continuous Integration
  - Every time you push code → tests run → code builds → ensures nothing breaks.
- CD: Continuous Deployment / Delivery
  - Your code automatically deploys to a server or hosting platform after CI succeeds.

### Workflow

GitHub Actions:

1. Push code to GitHub
2. GitHub Actions starts running (CI)
   - Install dependencies
   - Build the project
   - Run tests
3. If CI succeeds → Deployment job runs (CD)
4. Your app is updated automatically
