# gstack QA — Comprehensive Testing Pipeline

> **Upgraded for Kimi CLI** — Enhanced with automated test discovery, Playwright MCP integration for browser testing, coverage enforcement, and flaky test detection.

## Overview

QA is not just "does it work?" It's "does it work under stress, with bad input, across browsers, and after repeated runs?" This skill runs a complete test battery before any GrooveXlabs tool ships.

## When to Trigger

```
kimi> Run QA on groovefetch
kimi> /qa [repo-name]
kimi> Test this before we ship
kimi> Run full test suite
```

## The QA Pipeline

### Stage 1: Unit Tests
```bash
# Auto-discovered by Kimi:
pytest -xvs                    # Python
jest --coverage               # JavaScript/Node
pytest --tb=short --cov=.    # With coverage
```

Checks:
- [ ] All tests pass (`pytest` exit code 0)
- [ ] Coverage >80% for core modules
- [ ] No test warnings
- [ ] Fast execution (<30s for unit tests)

### Stage 2: Integration Tests
- [ ] External APIs mocked correctly
- [ ] Database/state transitions work end-to-end
- [ ] Error paths tested (network failure, bad auth, rate limits)
- [ ] File I/O tested with temp directories

### Stage 3: Security Tests
- [ ] SSRF attempts blocked (test with `http://localhost/admin`)
- [ ] Path traversal blocked (test with `../../../etc/passwd`)
- [ ] SQL injection attempts fail safely
- [ ] File upload rejects dangerous types
- [ ] Rate limiting enforced (rapid requests get 429)

### Stage 4: Browser Tests (Playwright MCP)
For web components:
```
Kimi can use Playwright MCP to:
- Navigate to test pages
- Click buttons, fill forms
- Take screenshots for visual regression
- Check console for JS errors
- Test responsive layouts
```

### Stage 5: Performance Tests
- [ ] No infinite loops on edge cases
- [ ] Memory usage stable (no leaks in repeated operations)
- [ ] Timeout handling works (slow responses don't hang forever)
- [ ] Concurrent operations don't race

### Stage 6: Flaky Test Detection
Run tests 3× and check:
- [ ] Same results every time
- [ ] No timing-dependent failures
- [ ] No ordering-dependent failures (test isolation)

### Stage 7: Cross-Platform (if applicable)
- [ ] Works on Windows (PowerShell compatibility)
- [ ] Works on macOS/Linux
- [ ] No hardcoded paths (`/` vs `\`)

## Test Isolation Fix Protocol

If tests fail due to state leakage (like GrooveFetch's `DomainProfile` persistence):

1. **Identify the leak:** What's shared between test runs?
2. **Use fixtures:** Create temp directories with `tmp_path` or `monkeypatch`
3. **Mock storage:** Replace `~/.groovefetch/profiles/` with temp path
4. **Reset state:** Add teardown that cleans up files/registry
5. **Verify:** Run 3×, confirm consistent results

## Output Format

```markdown
# QA Report: [Repo Name]

## Overall: [PASS / PASS WITH WARNINGS / FAIL]
## Test Count: [X passed, Y failed, Z skipped]
## Coverage: [X%] (Target: [Y]%)

### Unit Tests
| Test File | Passed | Failed | Time |
|-----------|--------|--------|------|
| test_core.py | 12/12 | 0 | 2.3s |
| test_utils.py | 5/5 | 0 | 0.8s |

### Integration Tests
[Results]

### Security Tests
| Test | Result |
|------|--------|
| SSRF block | ✅ PASS |
| Path traversal | ✅ PASS |

### Browser Tests (if applicable)
| Test | Result |
|------|--------|
| Page load | ✅ PASS |
| Form submit | ⚠️ Flaky (1/3 failed) |

### Performance
| Metric | Value | Target |
|--------|-------|--------|
| Suite time | 45s | <60s |
| Memory peak | 120MB | <200MB |

### Issues Found
1. `[FLAKY]` test_learning_records fails on 2nd run (state leak)
2. `[COVERAGE]` adaptive.py at 45% (target 80%)

### Recommendations
1. Fix flaky test with tmp_path fixture
2. Add tests for error paths in adaptive.py
```

## Integration with ToolSmith

```
ToolSmith: Generated GrooveFetch v0.1.0
Kimi:      Running QA pipeline...
Kimi:      Unit tests: 17/18 passed
Kimi:      Security tests: All passed
Kimi:      Coverage: 78% (target 80%) — adding 2 more tests...
Kimi:      Re-run: 19/19 passed, 82% coverage
Kimi:      QA: PASS — Proceeding to /ship
```

## Upgrades Over Original gstack

| Feature | Original gstack | This Version |
|---------|----------------|--------------|
| Test running | Manual | Auto-discovered + executed |
| Browser testing | Not mentioned | Playwright MCP integration |
| Security tests | Separate /cso | Integrated in QA pipeline |
| Flaky detection | Manual | 3× run automatic |
| Cross-platform | Not mentioned | Windows/macOS/Linux checks |
| Coverage | Ask user | Enforced with target |
| Test isolation | Not mentioned | Fix protocol included |
