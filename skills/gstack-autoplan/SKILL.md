# gstack Autoplan — Task Breakdown & Estimation

> **Upgraded for Kimi CLI** — Enhanced with dependency graphs, parallel task identification, MCP resource awareness, and automatic task file generation.

## Overview

Before coding starts, autoplan breaks the project into discrete, estimable, parallelizable tasks. It identifies the critical path, estimates complexity, and generates a task tracking file that the whole virtual team follows.

## When to Trigger

```
kimi> Autoplan groovefetch
kimi> /autoplan [project-name]
kimi> Break this down into tasks
kimi> Create a project plan for [tool]
```

## The Autoplan Process

### Step 1: Decompose the Concept
Break the tool into major components:

```
GrooveFetch Example:
├── Core Engine (async HTTP + stealth browser)
│   ├── HTTP fetcher with retries
│   ├── Stealth browser (Playwright)
│   └── Auto-mode selector
├── Schema Validation
│   ├── Pydantic wrapper
│   ├── Type coercion
│   └── Result container
├── Security Layer
│   ├── URL validation (SSRF protection)
│   ├── Secret redaction
│   └── Input sanitization
├── Adaptive Learning
│   ├── Domain profiles
│   ├── Strategy optimizer
│   └── Profile persistence
├── Tests
│   ├── Unit tests (core, schema, utils, adaptive)
│   ├── Integration tests
│   └── Security tests
├── Packaging
│   ├── pyproject.toml
│   ├── README
│   └── CI/CD workflow
```

### Step 2: Estimate Complexity

| Task | Complexity | Estimated Time | Parallel? | Dependencies |
|------|-----------|----------------|-----------|--------------|
| Core engine scaffold | Medium | 2h | ✅ | None |
| HTTP fetcher | Medium | 3h | ✅ | Scaffold |
| Schema validation | Low | 2h | ✅ | Scaffold |
| Security layer | High | 4h | ✅ | Scaffold |
| Adaptive learning | High | 4h | ✅ | Core engine |
| Unit tests | Medium | 3h | ❌ | All features |
| Integration tests | Medium | 2h | ❌ | All features |
| README & docs | Low | 1h | ✅ | None |
| CI/CD setup | Low | 1h | ✅ | None |

**Critical path:** Scaffold → Core → Security → Tests = ~11h
**Parallel track:** README, CI/CD can happen anytime = ~2h

### Step 3: Identify Risks

| Risk | Impact | Mitigation |
|------|--------|------------|
| Playwright install fails | High | Fallback to HTTP-only mode |
| Anti-bot detection | Medium | Adaptive learning v2 later |
| Schema complexity | Low | Start with simple models |

### Step 4: Generate Task File

Kimi auto-creates a `TASKS.md` in the project root:

```markdown
# GrooveFetch Development Tasks

## Phase 1: Foundation (Parallel)
- [ ] [P1-1] Scaffold project structure (pyproject.toml, src/, tests/)
- [ ] [P1-2] Set up CI/CD (.github/workflows/test.yml)
- [ ] [P1-3] Write README skeleton

## Phase 2: Core (Sequential)
- [ ] [P2-1] HTTP fetcher with retry logic
- [ ] [P2-2] URL validation & SSRF protection
- [ ] [P2-3] Stealth browser integration
- [ ] [P2-4] Auto-mode selector

## Phase 3: Features (Parallel after P2)
- [ ] [P3-1] Schema validation with Pydantic
- [ ] [P3-2] Adaptive learning engine
- [ ] [P3-3] Secret redaction

## Phase 4: Polish (Sequential)
- [ ] [P4-1] Unit tests (>80% coverage)
- [ ] [P4-2] Integration tests
- [ ] [P4-3] Security audit
- [ ] [P4-4] Design review
- [ ] [P4-5] Performance benchmark
```

### Step 5: MCP Integration Points

Mark which tasks need MCP tools:
```
- [P2-3] Stealth browser → Requires Playwright MCP
- [P1-2] CI/CD → Requires GitHub MCP
- [P4-3] Security audit → Requires Shell MCP (pip-audit, bandit)
```

## Task Tracking Protocol

As work progresses, Kimi updates the task file:
```
kimi> Mark P2-1 as done
# Updates TASKS.md checkbox

kimi> What's next?
# Shows next unblocked task

kimi> Blocked on P2-3 — Playwright not installing
# Kimi suggests: Skip to P3-1, come back later
```

## Output Format

```markdown
# Autoplan: [Project Name]

## Estimated Total: [X] hours
## Critical Path: [Y] hours
## Parallelizable: [Z] hours saved

## Task Breakdown
[Table]

## Risk Assessment
[Table]

## Generated Files
- TASKS.md — Task tracking
- .cursor/plan.md — Implementation notes (if using Cursor)

## Suggested First Task
[Next action with full context]
```

## Integration with ToolSmith

```
ToolSmith: Generated GrooveFetch concept (CEO review: SHIP)
Kimi:      Running autoplan...
Kimi:      14 tasks identified, 8 parallelizable
Kimi:      Critical path: 11h
Kimi:      TASKS.md created
Kimi:      Suggested first task: P1-1 (scaffold)
Kimi:      Proceeding to build...
```

## Upgrades Over Original gstack

| Feature | Original gstack | This Version |
|---------|----------------|--------------|
| Task breakdown | Bullet list | Structured phases |
| Estimation | Ask user | Complexity-based auto-estimate |
| Dependencies | Mentioned | Explicit dependency graph |
| Parallel tasks | Mentioned | Marked with time savings |
| Risk assessment | Ask user | Structured risk table |
| Task tracking | None | Auto-generated TASKS.md |
| MCP awareness | None | Marks which tasks need which MCP |
