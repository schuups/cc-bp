# Guardrail: Data Safety and Security

These constraints apply in every mode, every role, and every session. No protocol overrides them.

---

## Secrets and Credentials

**Never read, log, or include in any output:**
- API keys, tokens, secrets, passwords
- Private keys, certificates
- Connection strings with credentials embedded
- `.env` files, `*.secret`, `credentials.json`, `*_key.pem`, or similar

**Before staging any file:** check its name and content for credentials. If uncertain, ask before proceeding.

**If credentials are discovered in the working tree:**
1. Stop immediately — do not commit, do not log the value
2. Surface to the user: "Found what appears to be a credential in `<file>`. Do not commit this."
3. Wait for explicit user instruction before touching that file

**If credentials appear in a file to be read:** read only the structural/non-sensitive parts. Redact sensitive values in any output: `[REDACTED]`.

---

## Personally Identifiable Information (PII)

Do not include real PII in test fixtures, seed data, log statements, or documentation examples. Use synthetic data (e.g. `user@example.com`, `+1-555-0100`, `1234 Test Street`).

If PII is discovered where it should not be (e.g. real emails in a test database dump), surface it as a CRITICAL finding before proceeding.

---

## Sensitive File Patterns

Treat these with extra caution — ask before reading or staging:

```
.env  .env.*  *.secret  *.key  *.pem  *.p12  *.pfx
credentials.*  secrets.*  *_credentials.*  *_secrets.*
*.token  auth.json  config.local.*  settings.local.*
```

---

## Security in Code

When writing code that handles credentials or sensitive data:
- Use environment variables or a secrets manager — never hardcode
- Do not log credential values even at DEBUG level
- Validate and sanitize all external input at system boundaries
- Reference `specs/invariants.md` for project-specific security invariants

---

## Reporting Security Findings

Security findings from the adversary role go to `specs/findings.md` with severity CRITICAL or HIGH. Do not minimize or defer security findings to reduce apparent scope.
