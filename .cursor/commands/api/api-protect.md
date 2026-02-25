---
description: Add authentication, authorization, and security to API endpoints
model: claude-sonnet-4-5
---

Add comprehensive security, authentication, and authorization to the specified API route. Follow the compounding dev cycle and project rules: `.cursor/rules/compounding-dev-cycle.mdc`, `.cursor/rules/api-routes.mdc`.

## Target API Route

$ARGUMENTS

## Compounding dev cycle

This command delivers **Code** phase output. If protection is part of a feature with a plan doc, implement to that plan (scope and acceptance criteria) and do not expand scope without updating the plan. Produce **implementation notes** (what was done, deferred, assumptions, env/config) so **backend-reviewer** and **security-engineer** can verify in Review/Test. Align with `api-routes.mdc` (401/403, error shape, no sensitive data in responses) and the security-audit skill (OWASP-aligned checks).

## Agent Definitions

**CRITICAL**: Before implementing security and authentication, you MUST:

1. **Read Agent Definitions**: Load and review the agent definitions from `.cursor-plugin/plugin.json`
   - Read the `agents` array to understand available specialized agents
   - Identify agent definitions relevant to security and API protection

2. **Identify Relevant Agents**: For API security and protection, the following agents are relevant:
   - **security-engineer** (PRIMARY): Authentication, authorization, vulnerability assessment, zero-trust principles. Participates in Review/Test when security is in scope; implement so their audit can verify (rework list = handback to Code).
   - **backend-architect**: Secure API design, data integrity, alignment with api-routes and core-standards.
   - **backend-reviewer**: Use their checklist (auth/authz, validation, error handling, tests) to self-check so output is handoff-ready for Review/Test.
   - **system-architect**: When security touches system boundaries or long-term architecture.

3. **Load Agent Definitions**: Read the agent definition files from `.cursor/agents/` directory as needed:
   - `.cursor/agents/security-engineer.md` – security-first mindset, OWASP-aligned checks, compounding dev cycle (Review/Test).
   - `.cursor/agents/backend-architect.md` – secure API design and handoff.
   - `.cursor/agents/backend-reviewer.md` – validate implementation against their checklist before considering done.
   - `.cursor/agents/system-architect.md` – when security has system-wide impact.

4. **Apply Agent Roles**: Use the agent definitions to inform your security approach:
   - Implement in line with `api-routes.mdc` (401/403, consistent error shape, no stack traces or sensitive data in responses).
   - Apply security-audit skill (OWASP Top 10 quick checks: access control, injection, auth failures, etc.).
   - If part of a Plan → Code → Review/Test cycle: do not add scope beyond the plan; produce implementation notes for handoff.

5. **Handoff for Review/Test**: The protected route should be verifiable by backend-reviewer and security-engineer: include tests or test guidance for auth/authz and validation; document what was done, deferred, and any env/config so the review phase has full context.

**Note**: Follow `.cursor/rules/compounding-dev-cycle.mdc` and `.cursor/rules/api-routes.mdc`. Agent definitions and backend-reviewer’s checklist ensure the protected API is handoff-ready for the compounding cycle.

## Security Layers to Implement

### 1. **Authentication** (Who are you?)
- Verify user identity
- Token validation (JWT, session, API keys)
- Handle expired/invalid tokens

### 2. **Authorization** (What can you do?)
- Role-based access control (RBAC)
- Resource-level permissions
- Check user ownership

### 3. **Input Validation**
- Sanitize all inputs
- SQL/NoSQL injection prevention
- XSS prevention
- Type validation with Zod

### 4. **Rate Limiting**
- Prevent abuse
- Per-user/IP limits
- Sliding window algorithm

### 5. **CORS** (if needed)
- Whitelist allowed origins
- Proper headers
- Credentials handling

## Implementation Approach

### For Supabase Projects:
```typescript
// Use Supabase Auth + RLS
- getUser() from server-side client
- RLS policies for data access
- Service role key for admin operations
```

### For NextAuth.js Projects:
```typescript
// Use NextAuth sessions
- getServerSession() in route handlers
- Protect with middleware
- Role checking logic
```

### For Custom Auth:
```typescript
// JWT validation
- Verify tokens
- Decode and validate claims
- Check expiration
```

## Security Checklist

**Authentication**
-  Verify authentication tokens
-  Handle missing/invalid tokens (401)
-  Check token expiration
-  Secure token storage recommendations

**Authorization**
-  Check user roles/permissions (403)
-  Verify resource ownership
-  Implement least privilege principle
-  Log authorization failures

**Input Validation**
-  Validate all inputs with Zod
-  Sanitize SQL/NoSQL inputs
-  Escape special characters
-  Limit payload sizes

**Rate Limiting**
-  Per-user limits
-  Per-IP limits
-  Clear error messages (429)
-  Retry-After headers

**CORS**
-  Whitelist specific origins
-  Handle preflight requests
-  Secure credentials
-  Appropriate headers

**Error Handling**
-  Don't expose stack traces
-  Generic error messages
-  Log detailed errors server-side
-  Consistent error format

**Logging & Monitoring**
-  Log authentication attempts
-  Log authorization failures
-  Track suspicious activity
-  Monitor rate limit hits

## What to Generate

1. **Protected Route Handler** - Secured version of the API route
2. **Middleware/Utilities** - Reusable auth helpers
3. **Type Definitions** - User, permissions, roles
4. **Error Responses** - Standardized auth errors
5. **Usage Examples** - Client-side integration

## Common Patterns for Solo Developers

**Pattern 1: Simple Token Auth**
```typescript
// For internal tools, admin panels
const token = request.headers.get('authorization')
if (token !== process.env.ADMIN_TOKEN) {
  return new Response('Unauthorized', { status: 401 })
}
```

**Pattern 2: User-based Auth**
```typescript
// For user-facing apps
const user = await getCurrentUser(request)
if (!user) {
  return new Response('Unauthorized', { status: 401 })
}
```

**Pattern 3: Role-based Auth**
```typescript
// For apps with different user types
const user = await getCurrentUser(request)
if (!user || !hasRole(user, 'admin')) {
  return new Response('Forbidden', { status: 403 })
}
```

## Output (handoff for Review/Test)

Deliver:

1. **Protected route and utilities** – Secured handler, middleware/helpers, types, and standardized auth error responses.
2. **Tests or test guidance** – Auth success/failure, authorization, validation, and rate-limit scenarios so backend-reviewer and security-engineer can verify.
3. **Implementation notes** – What was implemented, what was deferred (e.g. rate limiting, CORS), assumptions, and env/config (e.g. `ADMIN_TOKEN`, session secret). Enables Review/Test without guessing.

Generate production-ready, secure code that is handoff-ready for the compounding dev cycle and that follows the principle of least privilege.
