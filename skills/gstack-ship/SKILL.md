# gstack Ship — Release Engineering

> **Upgraded for Kimi CLI** — Enhanced with GitHub MCP integration for PR creation, automated changelog generation, semantic versioning, and CI/CD validation.

## Overview

Shipping is not just `git push`. It's a structured release process that ensures the right code goes out with the right documentation, the right version, and the right safeguards. This skill handles the full release lifecycle.

## When to Trigger

```
kimi> Ship groovefetch v0.1.0
kimi> /ship [repo-name] [version]
kimi> Release this tool
kimi> Publish to PyPI / npm
```

## Pre-Flight Checklist

Before any release, verify:

- [ ] **All gates passed:** CEO review ✅ | Security audit ✅ | Code review ✅ | QA ✅ | Design review ✅
- [ ] **Tests passing:** `pytest` or `jest` green
- [ ] **Version bump:** `pyproject.toml`, `package.json`, or `Cargo.toml` updated
- [ ] **Changelog updated:** `CHANGELOG.md` or GitHub releases
- [ ] **README current:** Installation, examples, badges accurate
- [ ] **No secrets:** `trufflehog` scan clean
- [ ] **CI green:** GitHub Actions passing

## The Ship Pipeline

### Step 1: Version & Changelog
```
# Auto-detect version bump type from changes:
- Breaking change → MAJOR (1.0.0 → 2.0.0)
- New feature → MINOR (1.0.0 → 1.1.0)
- Bug fix → PATCH (1.0.0 → 1.0.1)

# Generate changelog from commits since last tag:
git log --oneline v0.0.9..HEAD
```

### Step 2: Git Operations
```bash
# Create release branch if needed
git checkout -b release/v0.1.0

# Update version in files
# Commit version bump
git add .
git commit -m "chore(release): bump version to v0.1.0"

# Create annotated tag
git tag -a v0.1.0 -m "Release v0.1.0"

# Push
git push origin main --tags
```

### Step 3: GitHub Release (via MCP)
```
Kimi uses GitHub MCP to:
- Create release with auto-generated notes
- Attach build artifacts
- Mark as pre-release if needed
```

### Step 4: Package Publishing
```bash
# Python
python -m build
twine upload dist/*

# Node
npm publish

# After publish, verify:
pip install groovefetch==0.1.0  # Works?
npm install @groovexlabs/groovefetch  # Works?
```

### Step 5: Post-Ship Validation
```bash
# Install from registry (not local)
pip install --no-cache-dir groovefetch

# Run quick validation:
python -c "import groovefetch; print(groovefetch.__version__)"

# Verify README renders correctly on PyPI/npm page
```

## Rollback Protocol

If a bad release goes out:

1. **Immediate:** Yank from PyPI (`twine yank`) or deprecate on npm
2. **Git:** Create hotfix branch from last good tag
3. **Fix:** Apply patch, version bump to PATCH+1
4. **Re-release:** Fast-track through gates (skip CEO review if just bugfix)
5. **Communicate:** Update release notes with "DEPRECATED — use vX.Y.Z+1"

## Output Format

```markdown
# Ship Report: [Tool Name] v[Version]

## Pre-Flight: [ALL GREEN / ISSUES FOUND]
| Gate | Status |
|------|--------|
| CEO Review | ✅ |
| Security | ✅ |
| Code Review | ✅ |
| QA | ✅ |
| Design | ✅ |

## Version: vX.Y.Z
## Changelog: [Summary of changes]

## Git Operations
- Branch: release/vX.Y.Z
- Tag: vX.Y.Z
- Commit: [sha]

## Package Published
| Registry | Status | URL |
|----------|--------|-----|
| PyPI | ✅ Published | pypi.org/project/... |
| npm | N/A | — |

## Post-Ship Validation
- Install test: ✅
- Import test: ✅
- Quickstart works: ✅

## Known Issues
[None / List]

## Next Steps
[What happens next — marketing, docs, next version]
```

## Integration with ToolSmith

```
ToolSmith: GrooveFetch passed all gates
Kimi:      Running ship pipeline...
Kimi:      Version: v0.1.0
Kimi:      Changelog: 12 commits since v0.0.0
Kimi:      GitHub release created: GrooveXlabs/groovefetch/releases/tag/v0.1.0
Kimi:      PyPI published: pypi.org/project/groovefetch/0.1.0/
Kimi:      Validation: pip install works ✅
Kimi:      🚀 SHIPPED!
```

## Upgrades Over Original gstack

| Feature | Original gstack | This Version |
|---------|----------------|--------------|
| Version bump | Manual | Auto-detect from changes |
| Changelog | Ask user | Auto-generated from git log |
| GitHub release | Manual | MCP auto-creates |
| Package publish | Ask user | Auto twine/npm publish |
| Post-ship check | Ask user | Automated validation |
| Rollback | Not mentioned | Full protocol |
| Gate enforcement | Mentioned | Hard checklist with blocks |
