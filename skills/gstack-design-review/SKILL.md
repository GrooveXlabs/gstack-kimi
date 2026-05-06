# gstack Design Review — Catching AI Slop

> **Upgraded for Kimi CLI** — Enhanced with UX heuristics, accessibility checks, CLI ergonomics, and developer experience (DevEx) analysis.

## Overview

AI-generated code works but often "feels wrong." This skill catches the subtle UX, design, and DevEx problems that make tools feel cheap or hard to use. It's the difference between a tool developers love and one they abandon.

## When to Trigger

```
kimi> Run design review on groovefetch
kimi> Does this feel like AI slop?
kimi> Design review before we ship [tool]
kimi> Check UX and developer experience
```

## The 5 Design Dimensions

### 1. CLI Ergonomics (For CLI Tools)

- [ ] **Naming:** Commands are intuitive (`scrape` not `extract_data_from_url`)
- [ ] **Consistency:** Same flags pattern as popular tools (`--verbose`, `--output`, `--help`)
- [ ] **Progress indicators:** Long operations show progress (spinner, bar, or dots)
- [ ] **Error messages:** Clear, actionable, no stack traces by default
- [ ] **Output formats:** Supports `--json` for scripting, human-readable by default
- [ ] **Help quality:** `--help` explains every option with examples
- [ ] **Default behavior:** Safe and useful out-of-the-box (no required config to start)

**Good:** `groovefetch scrape https://example.com --output results.json`
**Slop:** `groovefetch --url=https://example.com --mode=scrape --outfile=results.json --verbose=true`

### 2. API Design (For Libraries)

- [ ] **Discoverability:** Can a new user guess the right method in 30 seconds?
- [ ] **Composability:** Can functions be chained and combined?
- [ ] **Type safety:** Are types clear? (Pydantic models, TypeScript interfaces)
- [ ] **Sensible defaults:** `client.fetch(url)` works without 5 config objects
- [ ] **Error handling:** Exceptions are specific and actionable
- [ ] **Documentation:** Every public function has docstring + example

**Good:** `GrooveFetch().scrape(url, schema=Product)`
**Slop:** `GrooveFetch(config=Config(timeout=30, retry=3, headers={})).execute(ScrapeOperation(url=url, extractor=Extractor(schema=SchemaDefinition(model=Product))))`

### 3. README & Onboarding

- [ ] **10-second test:** Can someone understand what this does in 10 seconds?
- [ ] **Quickstart works:** Copy-paste the first example — does it run?
- [ ] **Installation:** One-liner install (`pip install groovefetch`)
- [ ] **Badges:** CI status, version, license visible
- [ ] **Screenshots/GIFs:** Visual proof it works
- [ ] **Troubleshooting:** Common errors and fixes

### 4. Configuration & Setup

- [ ] **12-Factor compliance:** Config via env vars, not hardcoded
- [ ] **Sensible defaults:** Works without any config file
- [ ] **Validation:** Invalid config fails fast with clear errors
- [ ] **Documentation:** Every env var documented in README

### 5. The "Feel" Test (Subjective but Critical)

Read the README and code. Does it feel like it was built by:
- **A:** Someone who cares about the user experience
- **B:** An AI that checked boxes

Signs of B:
- Generic descriptions ("A powerful tool for...")
- Missing edge case handling
- Inconsistent naming
- No personality or voice
- Examples that don't actually work

## AI Slop Pattern Detection

| Pattern | Detection | Fix |
|---------|-----------|-----|
| **Wall of text README** | >500 words before code example | Lead with working example |
| **Placeholder examples** | `example.com`, `your-api-key` | Real working examples |
| **Feature list without why** | "Supports X, Y, Z" | "Saves you time by..." |
| **Missing error examples** | Only happy path shown | Include common errors |
| **Inconsistent tense** | "Will fetch" vs "fetches" | Consistent present tense |
| **Over-engineered config** | 20 env vars for simple tool | Sensible defaults, minimal required |
| **No troubleshooting** | "It just works" | Common issues section |

## Output Format

```markdown
# Design Review: [Tool Name]

## Overall Feel: [POLISHED / NEEDS WORK / AI SLOP]
## Ship Status: [PASS / PASS WITH NOTES / BLOCKED]

### CLI Ergonomics: [Score 1-10]
[Findings]

### API Design: [Score 1-10]
[Findings]

### README Quality: [Score 1-10]
[Findings]

### Configuration: [Score 1-10]
[Findings]

### AI Slop Detected
1. `[PATTERN]` [Location] — [Fix]

### Quick Wins (30 min fixes)
1. [Easy improvement with high impact]

### Recommended Changes
1. [Bigger change worth doing]
```

## Integration with ToolSmith

```
ToolSmith: Generated GrooveFetch v0.1.0
Kimi:      Running design review...
Kimi:      AI slop detected: README has no working example in first 10 lines
Kimi:      Quick fix: Move quickstart to top
Kimi:      Design score: 6/10 → 8/10 after fixes
Kimi:      Proceeding to /qa
```

## Upgrades Over Original gstack

| Feature | Original gstack | This Version |
|---------|----------------|--------------|
| CLI ergonomics | Mentioned | 7-point checklist |
| API design | Basic | Composability + type safety focus |
| README review | Mentioned | 10-second test + quickstart validation |
| AI slop | General concept | 7 specific patterns |
| DevEx | Separate /devex-review | Integrated |
| Scoring | Pass/fail | 1-10 scored dimensions |
