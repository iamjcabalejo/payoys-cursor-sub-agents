---
description: Analyze and optimize code for performance, memory, and efficiency
model: claude-sonnet-4-5
---

Optimize the following code for performance and efficiency.

## Code to Optimize

$ARGUMENTS

## Agent Definitions

**CRITICAL**: Before optimizing the code, you MUST:

1. **Read Agent Definitions**: Load and review the agent definitions from `.cursor-plugin/plugin.json`
   - Read the `agents` array to understand available specialized agents
   - Identify agent definitions relevant to code optimization and performance

2. **Identify Relevant Agents**: For code optimization, the following agents are relevant (see `.cursor/rules/compounding-dev-cycle.mdc` for the Plan → Code → Review/Test cycle):
   - **performance-engineer** (PRIMARY): Measurement-driven analysis and bottleneck elimination. "Measure first, optimize second." Profile before and after; focus on critical path and user experience. When optimization is driven by a rework list from Review/Test (performance in scope), treat rework items as scope and produce implementation notes for handoff.
   - **refactoring-expert**: Preserve code quality and maintainability during optimization; avoid sacrificing readability for micro-optimizations. Safe, incremental changes.
   - **backend-architect**: Backend performance, API efficiency, and alignment with `api-routes.mdc` and core-standards when touching server code.
   - **frontend-architect**: Frontend performance, bundle optimization, Core Web Vitals, and alignment with `react.mdc`, `typescript.mdc` when touching UI.
   - **database-expert**: Query and data-access optimization: indexes, N+1 prevention, batching, parameterized queries; use postgresql or nosql-databases skills. Profile execution plans before changing queries.
   - **backend-reviewer**: Use their checklist to self-check backend code (correctness, tests) after optimization so output is handoff-ready if this work will be reviewed.
   - **frontend-reviewer**: Use their checklist to self-check frontend code (correctness, accessibility, tests) after optimization so output is handoff-ready if this work will be reviewed.
   - **system-architect**: Architectural-level performance and scalability when optimization crosses system boundaries.

3. **Load Agent Definitions**: Read the agent definition files from `.cursor/agents/` directory as needed:
   - `.cursor/agents/performance-engineer.md` (REQUIRED) – measurement-first mindset, compounding dev cycle (Review/Test when performance in scope).
   - `.cursor/agents/refactoring-expert.md` – code quality and safe transformations during optimization.
   - `.cursor/agents/backend-architect.md` – when optimizing backend/API code.
   - `.cursor/agents/frontend-architect.md` – when optimizing frontend/UI code.
   - `.cursor/agents/database-expert.md` – when optimizing queries, schema, or data access.
   - `.cursor/agents/backend-reviewer.md` – when optimizing backend; validate with their checklist before done.
   - `.cursor/agents/frontend-reviewer.md` – when optimizing frontend; validate with their checklist before done.
   - `.cursor/agents/system-architect.md` – when optimization crosses component boundaries.

4. **Apply Agent Roles**: Use the agent definitions to inform your optimization approach:
   - **Measure first**: Use performance-profiling skill; identify actual bottlenecks before changing code.
   - Incorporate perspectives from the relevant agents above; prioritize performance-engineer's measurement-driven approach.
   - Align with project rules: `core-standards.mdc`, and `api-routes.mdc` / `react.mdc` / `typescript.mdc` as applicable.
   - If optimization is part of a Plan → Code → Review/Test cycle: do not expand scope beyond the rework list or agreed optimization scope; produce implementation notes (what was done, deferred, before/after metrics) for handoff.

5. **Compounding dev cycle**: When optimization follows a **performance rework list** from Review/Test (e.g. performance-engineer or frontend-reviewer), treat it as the **Code** phase: implement only those items, then produce a short summary and before/after metrics so Review/Test can re-verify. When optimization is standalone, still produce a brief summary and metrics for traceability.

**Note**: Agent definitions and the compounding dev cycle rule (`.cursor/rules/compounding-dev-cycle.mdc`) provide the standards. Performance-engineer is the primary guide; use backend-reviewer and frontend-reviewer checklists to validate output when optimizing backend or frontend code.

## Optimization Strategy for Solo Developers

### 1. **Profiling First**
- Identify actual bottlenecks
- Don't optimize prematurely
- Measure before and after
- Focus on high-impact areas

### 2. **Performance Optimization Areas**

**React/Next.js**
- Memoization (React.memo, useMemo, useCallback)
- Code splitting and lazy loading
- Image optimization (next/image)
- Font optimization (next/font)
- Remove unnecessary re-renders
- Virtual scrolling for long lists

**Database Queries**
- Add indexes for frequently queried fields
- Batch queries (reduce N+1 problems)
- Use select to limit fields
- Implement pagination
- Cache frequent queries
- Use database views for complex joins

**API Calls**
- Implement caching (SWR, React Query)
- Debounce/throttle requests
- Parallel requests where possible
- Request deduplication
- Optimistic updates

**Bundle Size**
- Tree-shaking unused code
- Dynamic imports for large libraries
- Replace heavy dependencies
- Code splitting by route
- Lazy load below-the-fold content

**Memory**
- Fix memory leaks (cleanup useEffect)
- Avoid unnecessary object creation
- Use const for non-changing values
- Clear intervals/timeouts
- Remove event listeners

### 3. **Optimization Checklist**

**JavaScript/TypeScript**
-  Use const/let instead of var
-  Avoid nested loops where possible
-  Use Map/Set for lookups
-  Minimize DOM manipulations
-  Debounce/throttle expensive operations

**React**
-  Memo components that render often
-  Move static values outside components
-  Use keys properly in lists
-  Avoid inline functions in render
-  Lazy load routes and components

**Next.js**
-  Use Server Components where possible
-  Implement ISR for dynamic content
-  Optimize images with next/image
-  Prefetch critical routes
-  Use Suspense for streaming

**Database**
-  Add indexes on foreign keys
-  Use prepared statements
-  Batch inserts/updates
-  Implement connection pooling
-  Cache expensive queries

**Network**
-  Compress responses (gzip/brotli)
-  Use CDN for static assets
-  Implement HTTP/2
-  Set proper cache headers
-  Minimize payload size

### 4. **Measurement Tools**

**Frontend**
- Chrome DevTools Performance tab
- Lighthouse CI
- React DevTools Profiler
- Bundle Analyzer (next/bundle-analyzer)

**Backend**
- Node.js profiler
- Database query analyzer
- APM tools (DataDog, New Relic)
- Load testing (k6, Artillery)

### 5. **Common Optimizations**

**Replace inefficient array methods**
```typescript
// Before: Multiple iterations
const result = arr
  .filter(x => x > 0)
  .map(x => x * 2)
  .reduce((sum, x) => sum + x, 0)

// After: Single iteration
const result = arr.reduce((sum, x) => {
  return x > 0 ? sum + (x * 2) : sum
}, 0)
```

**Memoize expensive calculations**
```typescript
const expensiveValue = useMemo(() => {
  return complexCalculation(props.data)
}, [props.data])
```

**Virtual scrolling for long lists**
```typescript
import { useVirtual } from 'react-virtual'
// Only render visible items
```

## Output Format

1. **Analysis** – Identify performance bottlenecks (and, if driven by a rework list, which rework items were addressed).
2. **Optimized Code** – Improved version.
3. **Explanation** – What changed and why.
4. **Benchmarks** – Before/after metrics or expected improvement (required for handoff when performance is in scope).
5. **Trade-offs** – Any complexity or readability impact.
6. **Next Steps** – Further optimization opportunities.
7. **Implementation notes** (when part of a cycle) – Short list: what was done, what was deferred, before/after metrics. Enables handoff to Review/Test (e.g. performance-engineer, backend-reviewer, frontend-reviewer) for re-verification.

Focus on practical, measurable optimizations that provide real user value. Don't sacrifice readability for micro-optimizations. When optimization is driven by a rework list, limit scope to those items and keep the output traceable for the next phase.
