---
description: Test API endpoints with automated test generation
model: claude-sonnet-4-5
---

Generate comprehensive API tests for the specified endpoint. Follow the compounding dev cycle and project rules: `.cursor/rules/compounding-dev-cycle.mdc`, `.cursor/rules/api-routes.mdc`. Use the api-testing skill for structure and coverage.

## Target

$ARGUMENTS

## Compounding dev cycle

This command supports both **Code** and **Review/Test** phases. When tests are added as part of implementation (Code), they are part of the handoff to Review/Test—backend-reviewer uses **test status** (which acceptance criteria are covered, failing/missing tests) to verify gates. When generating tests for an existing endpoint, produce tests that map to the plan’s acceptance criteria where possible, plus success, validation, auth, and error cases per api-routes and api-testing skill. Produce a short **implementation notes** (scenarios covered, deferred, setup required) so backend-reviewer has full context.

## Agent Definitions

**CRITICAL**: Before generating API tests, you MUST:

1. **Read Agent Definitions**: Load and review the agent definitions from `.cursor-plugin/plugin.json`
   - Read the `agents` array to understand available specialized agents
   - Identify agent definitions relevant to API testing

2. **Identify Relevant Agents**: For API testing, the following agents are relevant:
   - **backend-reviewer** (PRIMARY for handoff): Their review uses **test status**—which AC are covered, which tests pass/fail. Generate tests that satisfy their checklist (correctness, validation, auth, error cases) so the review phase can verify gates without redoing work.
   - **backend-architect**: Reliability, data integrity, and API contract coverage in tests (success, validation, error paths).
   - **security-engineer**: Security test cases (auth bypass, injection, invalid tokens, rate limiting) so security audit can verify.
   - **performance-engineer**: Performance-related tests only when in scope (response time, load); measure-first per performance-profiling skill.
   - **system-architect**: Integration and system-level test scenarios when the API crosses boundaries.

3. **Load Agent Definitions**: Read the agent definition files from `.cursor/agents/` directory as needed:
   - `.cursor/agents/backend-reviewer.md` – test status and gates (AC covered, no rule violations); align tests with their checklist.
   - `.cursor/agents/backend-architect.md` – API contract and reliability test coverage.
   - `.cursor/agents/security-engineer.md` – when security test cases are in scope.
   - `.cursor/agents/performance-engineer.md` – when performance test scenarios are in scope.
   - `.cursor/agents/system-architect.md` – when integration/system tests are needed.

4. **Apply Agent Roles**: Use the agent definitions and api-testing skill to inform your approach:
   - Cover success, validation, auth, and error cases per api-routes and backend-reviewer expectations.
   - Map tests to acceptance criteria when a plan exists (traceability for Review/Test).
   - If part of a Plan → Code → Review/Test cycle: produce implementation notes so backend-reviewer can report test status and rework list if needed.

5. **Handoff for Review/Test**: The test suite should enable backend-reviewer to answer: which acceptance criteria are covered, which tests pass/fail, and what (if anything) is missing. Document scenarios covered and any setup (mocks, env, DB) in implementation notes.

**Note**: Follow `.cursor/rules/compounding-dev-cycle.mdc` and the api-testing skill. Tests are the bridge between Code and Review/Test; backend-reviewer uses them to verify gates and produce rework list or sign-off.

## Test Strategy for Solo Developers

Create practical, maintainable tests using modern tools:

### 1. **Testing Approach**
- Unit tests for validation logic
- Integration tests for full API flow
- Edge case coverage
- Error scenario testing

### 2. **Tools** (choose based on project)
- **Vitest** - Fast, modern (recommended for new projects)
- **Jest** - Established, widely used
- **Supertest** - HTTP assertions
- **MSW** - API mocking

### 3. **Test Coverage**

**Happy Paths**
- Valid inputs return expected results
- Proper status codes
- Correct response structure

**Error Paths**
- Invalid input validation
- Authentication failures
- Rate limiting
- Server errors
- Missing required fields

**Edge Cases**
- Empty requests
- Malformed JSON
- Large payloads
- Special characters
- SQL injection attempts
- XSS attempts

### 4. **Test Structure**

```typescript
describe('API Endpoint', () => {
  describe('Success Cases', () => {
    it('should handle valid request', () => {})
    it('should return correct status code', () => {})
  })

  describe('Validation', () => {
    it('should reject invalid input', () => {})
    it('should validate required fields', () => {})
  })

  describe('Error Handling', () => {
    it('should handle server errors', () => {})
    it('should return proper error format', () => {})
  })
})
```

### 5. **What to Generate**

1. **Test File** - Complete test suite with all scenarios
2. **Mock Data** - Realistic test fixtures
3. **Helper Functions** - Reusable test utilities
4. **Setup/Teardown** - Database/state management
5. **Quick Test Script** - npm script to run tests

## Key Testing Principles

-  Test behavior, not implementation
-  Clear, descriptive test names
-  Arrange-Act-Assert pattern
-  Independent tests (no shared state)
-  Fast execution (<5s for unit tests)
-  Realistic mock data
-  Test error messages
- L Don't test framework internals
- L Don't mock what you don't own
- L Avoid brittle tests

## Additional Scenarios to Cover

1. **Authentication/Authorization**
   - Valid tokens
   - Expired tokens
   - Missing tokens
   - Invalid permissions

2. **Data Validation**
   - Type mismatches
   - Out of range values
   - SQL/NoSQL injection
   - XSS payloads

3. **Rate Limiting**
   - Within limits
   - Exceeding limits
   - Reset behavior

4. **Performance**
   - Response times
   - Large dataset handling
   - Concurrent requests

## Output (handoff for Review/Test)

Deliver:

1. **Test file and helpers** – Complete test suite, fixtures, setup/teardown, and run script (e.g. `npm test`).
2. **Test status / coverage summary** – Which scenarios are covered (success, validation, auth, errors); which acceptance criteria (if any) the tests map to. Enables backend-reviewer to report test status in the review handoff.
3. **Implementation notes** – Scenarios covered, scenarios deferred, setup required (mocks, env, DB), and any assumptions. Enables Review/Test to re-run and verify without guessing.

Generate production-ready tests that are handoff-ready for the compounding dev cycle and that you can run immediately with `npm test`.
