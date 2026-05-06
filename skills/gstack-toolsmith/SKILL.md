# gstack ToolSmith — Master Orchestrator

> **The unified skill that orchestrates all other gstack skills into a fully autonomous tool-building pipeline.**

## Overview

ToolSmith is the conductor. It takes a raw idea, runs it through the entire GrooveXlabs virtual engineering team (CEO → Autoplan → Code → Security → Review → Design → QA → Ship), and produces a production-ready tool.

**One command builds a complete tool.**

## When to Trigger

```
kimi> toolsmith build a web scraper with schema validation
kimi> /ts discover web-scraping
kimi> /ts full-cycle --category security
kimi> Build a tool that [description]
```

## The Full Pipeline

```
┌─────────────────┐
│   1. DISCOVER   │  ← Find gaps in market via GitHub search
│   (CEO skill)   │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│   2. ANALYZE    │  ← Deep-dive READMEs and source of competitors
│   (CEO skill)   │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│   3. IDEATE     │  ← Generate unique concept with differentiation
│   (CEO skill)   │
└────────┬────────┘
         │ CEO Review Gate
         ▼
┌─────────────────┐
│  4. AUTOPLAN    │  ← Break into tasks, estimate, identify risks
│ (Autoplan skill)│
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│   5. BUILD      │  ← Generate code with Kimi Code CLI
│ (Code + Design) │
└────────┬────────┘
         │
    ┌────┴────┐
    ▼         ▼
┌────────┐ ┌────────┐
│Security│ │ Code   │  ← Parallel review gates
│ Audit  │ │ Review │
│  (CSO) │ │        │
└───┬────┘ └───┬────┘
    └────┬─────┘
         │
         ▼
┌─────────────────┐
│   6. QA         │  ← Test suite execution + browser tests
│   (QA skill)    │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│   7. DESIGN     │  ← UX, DevEx, AI slop detection
│ (Design skill)  │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│   8. SHIP       │  ← Version, changelog, publish, validate
│  (Ship skill)   │
└────────┬────────┘
         │
         ▼
    🚀 SHIPPED!
```

## Command Reference

### Full Cycle (End-to-End)
```
/ts full-cycle --category web-scraping --auto-publish
```
Runs all 8 stages. `--auto-publish` creates GitHub repo + publishes package.

### Stage-by-Stage
```
/ts discover --category ai-ml          # Find gaps
/ts analyze --repo owner/repo          # Deep-dive a competitor
/ts ideate --name "MyTool"             # Generate concept
/ts autoplan --project MyTool          # Create task breakdown
/ts build --project MyTool             # Generate code
/ts security --project MyTool          # Run security audit
/ts review --project MyTool            # Code review
/ts qa --project MyTool                # Run tests
/ts design --project MyTool            # Design review
/ts ship --project MyTool --version 0.1.0  # Release
```

### Quick Commands
```
/ts status MyTool                      # Show pipeline status
/ts next MyTool                        # What should I do next?
/ts fix MyTool --test test_foo         # Fix failing test
```

## Gate Enforcement

**No stage proceeds without the previous gate passing:**

| Gate | Skill | Auto-Trigger | Block Condition |
|------|-------|--------------|-----------------|
| CEO Review | gstack-ceo-review | After ideate | Verdict = KILL |
| Security | gstack-security-audit | After build | Grade = F |
| Code Review | gstack-code-review | After build | REQUEST CHANGES |
| QA | gstack-qa | After build | FAIL |
| Design | gstack-design-review | After QA | BLOCKED |

**Override:** `--force` flag skips gates (use with caution).

## MCP Integration Map

| MCP Server | Used By | Purpose |
|-----------|---------|---------|
| GitHub | CEO, Ship | Repo search, release creation, PRs |
| Playwright | QA | Browser testing, visual regression |
| Shell | Security, QA, Build | Run tests, audits, builds |
| Filesystem | All | Read/write project files |
| Context7 | Discover | Query library documentation |

## State Persistence

ToolSmith tracks pipeline state in `~/.toolsmith/projects/{name}/state.json`:
```json
{
  "project": "groovefetch",
  "stage": "qa",
  "gates": {
    "ceo": {"status": "pass", "verdict": "SHIP"},
    "security": {"status": "pass", "grade": "A"},
    "code_review": {"status": "pass", "score": 8.5},
    "qa": {"status": "in_progress", "tests": "17/18"},
    "design": {"status": "pending"}
  },
  "tasks_completed": 12,
  "tasks_total": 14,
  "repo_url": "https://github.com/GrooveXlabs/groovefetch",
  "version": "0.1.0"
}
```

## Output Format

```markdown
# ToolSmith Pipeline: [Project Name]

## Current Stage: [Stage Name]
## Progress: [X/Y tasks] ([Z]%)

### Gate Status
| Gate | Status | Result |
|------|--------|--------|
| CEO | ✅ PASS | SHIP (8.5/10) |
| Security | ✅ PASS | Grade A |
| Code Review | ✅ PASS | Score 8.5 |
| QA | ⏳ IN PROGRESS | 17/18 tests |
| Design | ⏳ PENDING | — |

### Current Task
[P3-2] Adaptive learning engine — In progress

### Next Actions
1. Fix test_learning_records (flaky test)
2. Run design review
3. Ship v0.1.0

### Time Estimate
- Remaining: ~3 hours
- Critical path: Fix test → Design → Ship
```

## Example Session

```
User: toolsmith build a web scraper that validates data with Pydantic

Kimi:  🔍 DISCOVER: Searching GitHub for web scraping tools...
Kimi:  Found 15 competitors. Scrapling (3.5K⭐), crawl4ai (31K⭐) dominate.
Kimi:  Gap identified: None combine schema validation + adaptive rate limiting + RAG export.

Kimi:  💡 IDEATE: "GrooveFetch" — unified scraper with Pydantic schemas,
       per-domain adaptive learning, and native AI tool-calling support.

Kimi:  👔 CEO REVIEW: Verdict: SHIP (8.5/10)
       - Problem: Real (developers waste hours on brittle scrapers)
       - Timing: Perfect (AI agents need structured data)
       - Differentiation: 3 unique features vs competitors

Kimi:  📋 AUTOPLAN: 14 tasks, 8 parallelizable, 11h critical path
       TASKS.md created

Kimi:  🔨 BUILD: Generating code...
       [Progress bar as files are written]

Kimi:  🔒 SECURITY AUDIT: Grade A (minor: add CSP headers)
       Auto-fixing: Added Content-Security-Policy meta tag

Kimi:  👁️ CODE REVIEW: Score 8.5/10
       2 minor issues auto-fixed. 1 suggestion: extract retry logic.

Kimi:  🧪 QA: 17/18 tests pass
       Issue: test_learning_records is flaky (state leak)
       Fix: Using tmp_path fixture
       Re-run: 19/19 pass, 82% coverage

Kimi:  🎨 DESIGN REVIEW: Score 8/10
       Quick win: Moved quickstart to top of README

Kimi:  🚀 SHIP: v0.1.0 released
       GitHub: GrooveXlabs/groovefetch/releases/tag/v0.1.0
       PyPI: pypi.org/project/groovefetch/0.1.0/
       Validation: pip install works ✅

Kimi:  ✅ DONE! GrooveFetch shipped in 4h 32m.
       Next: Add GrooveX branding, write blog post, plan v0.2.0.
```

## Upgrades Over Original gstack

| Feature | Original gstack | ToolSmith |
|---------|----------------|-----------|
| Pipeline | 5 skills, manual | 8 skills, fully automated |
| Gate enforcement | Suggested | Hard blocking |
| State tracking | None | JSON state file |
| MCP integration | None | Full MCP map |
| Progress tracking | None | Real-time status |
| Time estimation | None | Critical path analysis |
| Auto-publish | Manual | `--auto-publish` flag |
| Failure recovery | Manual | Auto-fix + retry protocol |
