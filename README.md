# gstack-kimi 🛠️

> **Port of Garry Tan's gstack (70K⭐) to Kimi CLI — Enhanced with ToolSmith integration, MCP automation, and security-first development.**

[![GitHub](https://img.shields.io/badge/GrooveXlabs-gstack--kimi-blue)](https://github.com/GrooveXlabs/gstack-kimi)

## What is gstack?

[gstack](https://github.com/garrytan/gstack) by Garry Tan (Y Combinator CEO) is a virtual engineering team methodology that uses structured AI prompts to simulate a full software development team — CEO, Engineering Manager, Designer, Reviewer, QA, Security Officer, and more.

This repo ports those 23 personas to **Kimi-native skill files** with significant upgrades.

## The 8 Enhanced Skills

| Skill | Original gstack | Our Upgrade |
|-------|----------------|-------------|
| `gstack-ceo-review` | Manual competitive analysis | **GitHub MCP auto-fetches** live star counts, commit activity, and gaps |
| `gstack-security-audit` | Basic security checklist | **OWASP + STRIDE + AGENTS.md** hardened rules, auto CVE scanning |
| `gstack-code-review` | General code review | **7 pillars** including AI slop detection (9 patterns), auto-fix mode |
| `gstack-design-review` | UX critique | **CLI ergonomics + DevEx + AI slop** (7 patterns), 1-10 scoring |
| `gstack-qa` | Basic testing | **Playwright MCP** browser tests, flaky detection, cross-platform checks |
| `gstack-ship` | Manual release | **Auto semver**, changelog generation, MCP GitHub releases, rollback protocol |
| `gstack-autoplan` | Task breakdown | **Dependency graphs**, critical path analysis, MCP-aware task tracking |
| `gstack-toolsmith` | N/A (new) | **Master orchestrator** — one command runs the full 8-stage pipeline |

## Quick Start

### Install Skills

Copy the skill directories to your Kimi skills folder:

```bash
# macOS/Linux
cp -r skills/gstack-* ~/.kimi/skills/

# Windows
xcopy /E /I skills\gstack-* %USERPROFILE%\.kimi\skills\
```

### Use a Skill

```bash
kimi
> Run CEO review on my tool
> /security-audit on groovefetch
> toolsmith build a web scraper
```

## The ToolSmith Pipeline

```
🔍 DISCOVER  →  📊 ANALYZE  →  💡 IDEATE
       ↓
👔 CEO REVIEW (SHIP/KILL gate)
       ↓
📋 AUTOPLAN →  🔨 BUILD
       ↓
┌──────┴──────┐
🔒 SECURITY   👁️ CODE REVIEW (parallel gates)
└──────┬──────┘
       ↓
🧪 QA  →  🎨 DESIGN REVIEW
       ↓
🚀 SHIP
```

## Key Upgrades Over Original gstack

| Dimension | Original | gstack-kimi |
|-----------|----------|-------------|
| **Automation** | Manual prompts | MCP-integrated (GitHub, Playwright, Shell) |
| **Security** | Checklist | 3-layer audit (OWASP + STRIDE + Custom) |
| **AI Slop Detection** | None | 16 patterns across code + design |
| **Testing** | Unit tests only | Browser tests + flaky detection + cross-platform |
| **Gate Enforcement** | Suggested | Hard blocking with `--force` override |
| **State Tracking** | None | JSON state file + progress dashboard |
| **Auto-Publish** | Manual | `--auto-publish` flag |

## Architecture

```
skills/
├── gstack-ceo-review/       # Strategic product validation
├── gstack-security-audit/   # Defense-in-depth security gate
├── gstack-code-review/      # Production-grade engineering review
├── gstack-design-review/    # UX & AI slop detection
├── gstack-qa/               # Comprehensive testing pipeline
├── gstack-ship/             # Release engineering
├── gstack-autoplan/         # Task breakdown & estimation
└── gstack-toolsmith/        # Master orchestrator
```

## Integration with ToolSmith Agent

These skills are designed to work with the [ToolSmith Agent](https://github.com/GrooveXlabs/toolsmith-agent) — our autonomous tool-building pipeline. When ToolSmith generates code, these skills automatically review, test, and ship it.

## Security-First by Design

Every skill embeds the Security-First Development Agent principles:

- Never trust user input
- Least privilege by default
- Defense in depth
- Privacy by design
- Secure by default
- Fail securely

## Contributing

1. Fork the repo
2. Add your skill upgrade
3. Update the comparison table
4. Submit a PR

## License

MIT — Built with ❤️ by GrooveXlabs

---

> *"AI slop is the new tech debt. These skills catch it before it ships."*
