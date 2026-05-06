# gstack Code Review — Production-Grade Engineering

> **Upgraded for Kimi CLI** — Enhanced with security-first review, performance analysis, test coverage checks, and AI slop detection.

## Overview

This is not a "LGTM" review. It's a rigorous production gate that checks architecture, correctness, performance, security, readability, and maintainability. Every GrooveXlabs tool must pass before shipping.

## When to Trigger

```
kimi> Run code review on groovefetch
kimi> /review [repo-name]
kimi> Review this PR before merge
kimi> Production review on [branch]
```

## The 7 Pillars of Review

### Pillar 1: Architecture & Design
- [ ] Does the code solve the right problem?
- [ ] Is the abstraction level appropriate? (Not over-engineered, not under-engineered)
- [ ] Are responsibilities separated? (SRP — Single Responsibility Principle)
- [ ] Is the public API clean and intuitive?
- [ ] Are there clear boundaries between modules?

### Pillar 2: Correctness & Logic
- [ ] Are there off-by-one errors?
- [ ] Are race conditions possible? (async/await, threading)
- [ ] Are errors handled or silently swallowed?
- [ ] Are edge cases covered? (empty input, max size, malformed data)
- [ ] Is state management correct? (no mutations where immutability expected)

### Pillar 3: Security (from AGENTS.md)
- [ ] Input validation on ALL entry points
- [ ] No secrets hardcoded
- [ ] No SQL injection, command injection, XSS vectors
- [ ] Proper auth/authorization checks
- [ ] Safe defaults (deny by default)

### Pillar 4: Performance
- [ ] Are there N+1 queries?
- [ ] Are large datasets loaded into memory unnecessarily?
- [ ] Is caching used where appropriate?
- [ ] Are there unnecessary computations in loops?
- [ ] Is async used for I/O-bound operations?

**Auto-benchmark:** If benchmarks exist, compare against previous runs.

### Pillar 5: Testing
- [ ] Are there unit tests for core logic?
- [ ] Are there integration tests for external dependencies?
- [ ] Is error path tested?
- [ ] What's the coverage? (Target: >80% for core, >60% overall)
- [ ] Are flaky tests identified?

### Pillar 6: Readability & Maintainability
- [ ] Are variable/function names descriptive?
- [ ] Are complex sections commented?
- [ ] Is there dead code? (unused imports, functions, variables)
- [ ] Is formatting consistent? (ruff, black, prettier)
- [ ] Are type hints used? (Python) / Types strict? (TS)

### Pillar 7: AI Slop Detection (NEW)
Look for these "AI smell" patterns:

| Smell | Example | Fix |
|-------|---------|-----|
| **Placeholder comments** | `# TODO: implement this` | Implement or remove |
| **Fake error handling** | `except: pass` | Proper logging + recovery |
| **Hallucinated APIs** | `requests.get_json()` (doesn't exist) | Use correct API |
| **Overly verbose** | 50 lines for a 5-line task | Refactor to essence |
| **Inconsistent style** | Mix of snake_case and camelCase | Standardize |
| **Missing docstrings** | Public functions without docs | Add docstrings |
| **Copy-paste drift** | Similar blocks with tiny differences | Extract function |
| **Magic numbers** | `timeout=30` (why 30?) | Named constant |
| **Leaky abstractions** | Exposing internal data structures | Clean interfaces |

## Review Output Format

```markdown
# Code Review: [Repo/Branch]

## Overall: [APPROVE / APPROVE WITH COMMENTS / REQUEST CHANGES]
## Quality Score: [1-10]

### Architecture
[Assessment + suggestions]

### Critical Issues (Must Fix)
1. `[CRITICAL]` [File]:[Line] — [Issue] — [Suggested fix]
2. `[HIGH]` [File]:[Line] — [Issue] — [Suggested fix]

### Medium Issues (Should Fix)
1. `[MEDIUM]` [File]:[Line] — [Issue]

### AI Slop Detected
1. `[SLOP]` [File]:[Line] — [Pattern] — [Fix]

### Performance Notes
[Benchmarks or concerns]

### Testing Assessment
- Coverage: [X]% (Target: [Y]%)
- Missing tests: [List]

### Praise (What We Like)
1. [Good pattern worth keeping]

### Suggested Refactors
1. [Improvement that could be follow-up PR]
```

## Auto-Fix Mode

For common issues, Kimi can suggest or apply fixes:
```
kimi> Review and auto-fix groovefetch
# Kimi will:
# 1. Identify issues
# 2. Propose fixes
# 3. Apply safe fixes automatically (formatting, imports, simple refactors)
# 4. Leave complex fixes as PR comments
```

## Integration with ToolSmith

```
ToolSmith: Generated GrooveFetch code
Kimi:      Running code review...
Kimi:      2 critical issues found, 1 AI slop pattern
Kimi:      Auto-fixing imports and formatting...
Kimi:      Manual fix needed: core.py:45 — race condition in async fetcher
Kimi:      Waiting for developer fix...
```

## Upgrades Over Original gstack

| Feature | Original gstack | This Version |
|---------|----------------|--------------|
| AI slop detection | Basic | 9-pattern checklist |
| Performance | Ask user | Auto-benchmark comparison |
| Test coverage | Ask user | Coverage target enforcement |
| Auto-fix | No | Safe fixes auto-applied |
| Security review | Separate /cso | Integrated in Pillar 3 |
| Output | Text | Structured with quality score |
