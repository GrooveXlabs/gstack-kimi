# gstack CEO Review — Strategic Product Validation

> **Upgraded for Kimi CLI** — Enhanced with market intelligence, competitive analysis, and ToolSmith integration.

## Overview

The CEO Review skill interrogates any product concept or tool with 6 forcing questions that separate real opportunities from "AI slop" ideas. This is Garry Tan's methodology enhanced with automated competitive intelligence.

Use this **before building anything** to validate that ToolSmith's generated concept is worth shipping.

## When to Trigger

```
kimi> Run CEO review on groovefetch
kimi> Should we build this? Review the concept for [tool-name]
kimi> CEO review: analyze the competitive landscape for [idea]
```

## The 6 Forcing Questions (Enhanced)

### 1. What Problem Are We Solving?
- Is this a **hair-on-fire** problem or a **nice-to-have**?
- Who feels the pain most acutely?
- What are they doing **right now** as a workaround?

### 2. Why Now?
- What changed in the market/technology/stack that makes this viable today?
- Is there a **platform shift** we can ride? (AI agents, MCP, browser automation)
- What's the **tailwind**?

### 3. Why Us / Why GrooveXlabs?
- Do we have **unfair advantages**? (security expertise, AI tooling, speed)
- Can we ship 10× faster than incumbents?
- What's our **secret sauce**?

### 4. Competitive Intelligence (NEW — Auto-fetched)
```
# Kimi automatically runs this via GitHub MCP:
- Search GitHub for repos in this category
- Star counts, last commit dates, maintainer activity
- Identify direct competitors and adjacent tools
- Find gaps the incumbent has ignored
```

### 5. Differentiation Stack Rank
List ALL competitors and rank GrooveXlabs' version on:
| Dimension | Us | Incumbent #1 | Incumbent #2 |
|-----------|-----|--------------|--------------|
| Speed | ? | ? | ? |
| Security | ? | ? | ? |
| AI-Native | ? | ? | ? |
| Ease of Use | ? | ? | ? |
| Price | ? | ? | ? |

**We must win on at least 2 dimensions decisively.**

### 6. Go-to-Market in 30 Words
If you can't explain who buys this and why in 30 words, you don't have clarity.

## Enhanced Output Format

```markdown
# CEO Review: [Tool Name]

## Verdict: [SHIP / KILL / PARK]
- Confidence: [High / Medium / Low]
- Time to build: [estimate]

## Problem Validation
[Hair-on-fire assessment]

## Market Timing
[Why now analysis]

## Competitive Landscape
[Auto-fetched competitor data]

## Differentiation
[Stack rank table]

## Biggest Risk
[What could kill this project]

## Recommended Scope
[MVP vs v1 vs full build]

## Next Step
[Run /autoplan, /kill, or /park]
```

## Integration with ToolSmith

When ToolSmith generates a concept, the CEO Review auto-triggers:
```
ToolSmith: Generated "GrooveFetch" concept
Kimi:      Auto-running CEO review...
Kimi:      Verdict: SHIP — 8.5/10 confidence
Kimi:      Proceeding to /autoplan
```

## Upgrades Over Original gstack

| Feature | Original gstack | This Version |
|---------|----------------|--------------|
| Competitor research | Manual | GitHub MCP auto-fetches |
| Market timing | Ask user | Auto-detects platform shifts |
| Star counts | Ask user | Live GitHub data |
| Decision | Binary GO/KILL | Confidence-scored with scope |
| Output | Text | Structured markdown + next action |
