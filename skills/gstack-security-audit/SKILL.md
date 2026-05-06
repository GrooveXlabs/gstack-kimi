# gstack Security Audit (CSO) — Defense in Depth

> **Upgraded for Kimi CLI** — Hardened with AGENTS.md principles, automated CVE scanning, OWASP + STRIDE + custom GrooveXlabs security checklist.

## Overview

This is not a generic security scan. It's a **pre-release gate** that blocks shipping until every critical control is verified. Inspired by Garry Tan's `/cso` skill but enhanced with the full Security-First Development Agent doctrine.

**Rule: No GrooveXlabs tool ships without passing this audit.**

## When to Trigger

```
kimi> Run security audit on groovefetch
kimi> /cso on [repo-name]
kimi> Security review before we ship [tool]
kimi> Audit this codebase for vulnerabilities
```

## The Three-Layer Audit

### Layer 1: OWASP Top 10 Scan

Check every item against the codebase:

- [ ] **A01: Broken Access Control** — Every endpoint checks permissions. Deny by default.
- [ ] **A02: Cryptographic Failures** — No plaintext secrets. Argon2/bcrypt for passwords. TLS 1.2+.
- [ ] **A03: Injection** — Parameterized queries only. No `eval()`, `exec()`, `os.system()` with user input.
- [ ] **A04: Insecure Design** — Business logic validated server-side. No trust in frontend.
- [ ] **A05: Security Misconfiguration** — Secure defaults. No debug mode in production. Minimal error info.
- [ ] **A06: Vulnerable Components** — Dependencies scanned for CVEs (automated via `pip-audit` / `npm audit`).
- [ ] **A07: Auth Failures** — Session management, MFA support, password reset security.
- [ ] **A08: Data Integrity** — Signed uploads, CSRF tokens where needed.
- [ ] **A09: Logging Failures** — Audit logs for auth, admin, data exports. No secrets in logs.
- [ ] **A10: SSRF** — URL validation blocks internal IPs. No metadata endpoint access.

### Layer 2: STRIDE Threat Modeling

For each critical component, answer:

| Threat | Question | Mitigation |
|--------|----------|------------|
| **S**poofing | Can someone pretend to be another user? | Auth tokens, session validation |
| **T**ampering | Can data be modified in transit/storage? | Integrity checks, signed requests |
| **R**epudiation | Can actions happen without a trace? | Immutable audit logs |
| **I**nformation Disclosure | Can sensitive data leak? | Encryption, least privilege |
| **D**enial of Service | Can the system be crashed? | Rate limits, timeouts, max sizes |
| **E**levation | Can a user gain admin rights? | RBAC, IDOR prevention |

### Layer 3: GrooveXlabs Custom Checks

These are above and beyond standard audits:

- [ ] **No secrets in code** — Run `trufflehog` or `git-secrets` scan
- [ ] **No secrets in logs** — Redact API keys, tokens, passwords from all output
- [ ] **SSRF protection** — `validate_url()` blocks `localhost`, `169.254.169.254`, `10.0.0.0/8`, etc.
- [ ] **Path traversal blocked** — No `../`, absolute paths, or user-controlled filenames without sanitization
- [ ] **File uploads safe** — Magic bytes checked, size limited, stored outside webroot
- [ ] **Rate limiting** — Auth endpoints have aggressive limits. 429 with Retry-After.
- [ ] **CSP headers** — If web component exists, strict Content-Security-Policy
- [ ] **Clickjacking protection** — `X-Frame-Options: DENY` or CSP `frame-ancestors`
- [ ] **Dependency pinning** — Lockfiles committed. No `*` or `latest` versions.
- [ ] **Container security** — If Docker exists: non-root user, minimal base image, no secrets in layers

## Automated Scanning (Kimi MCP Integration)

```
# Kimi can trigger these automatically via Shell MCP:
pip-audit --format=json                    # Python CVE scan
npm audit --json                           # Node CVE scan
trufflehog filesystem . --json             # Secret scan
bandit -r . -f json                        # Python security linter
semgrep --config=auto --json               # Static analysis
```

## Output Format

```markdown
# Security Audit: [Repo Name]

## Overall Grade: [A / B / C / F]
## Ship Status: [PASS / PASS WITH NOTES / BLOCKED]

### OWASP Findings
| ID | Severity | Finding | Fixed? |
|----|----------|---------|--------|
| A03 | HIGH | SQL injection risk in login | ❌ |

### STRIDE Threats
| Component | Threat | Risk | Mitigation |
|-----------|--------|------|------------|
| API layer | Tampering | Medium | Add HMAC signatures |

### GrooveXlabs Custom
| Check | Status | Notes |
|-------|--------|-------|
| Secret scan | ✅ PASS | Clean |
| SSRF protection | ⚠️ NOTE | Only basic checks |

### Action Items (MUST fix before ship)
1. [CRITICAL] Fix SQL injection in auth.py line 47
2. [HIGH] Add rate limiting to /api/login

### Nice-to-Have
1. Add CSP headers
2. Pin all dependencies
```

## Integration with ToolSmith

When ToolSmith generates a tool, this audit auto-runs:
```
ToolSmith: Generated GrooveFetch v0.1.0
Kimi:      Running security audit...
Kimi:      Grade: B (2 issues found)
Kimi:      Auto-fixing OWASP A03...
Kimi:      Re-audit: Grade A
Kimi:      Proceeding to /ship
```

## Upgrades Over Original gstack

| Feature | Original gstack | This Version |
|---------|----------------|--------------|
| OWASP coverage | Top 10 listed | Top 10 + auto-scanned |
| STRIDE | Manual analysis | Per-component table |
| Secret detection | Ask user | Auto `trufflehog` scan |
| Dependency CVEs | Ask user | Auto `pip-audit` / `npm audit` |
| SSRF checks | Basic | AGENTS.md hardened rules |
| Auto-fix | No | Attempts auto-fix for common issues |
| Output | Text | Structured markdown with grades |
