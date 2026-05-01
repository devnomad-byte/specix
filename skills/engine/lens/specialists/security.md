# Security Specialist Checklist

OWASP-focused security review for code changesets.

---

## Checklist

### Input Validation

- [ ] All user-supplied input is validated at the point of entry
- [ ] Input is sanitized before use in queries, commands, or templates
- [ ] File uploads are validated for type, size, and content (not just extension)
- [ ] Request body size limits are enforced
- [ ] Query parameters are validated, not trusted

### Authentication and Authorization

- [ ] Every protected endpoint verifies authentication before processing
- [ ] Authorization checks use the principle of least privilege
- [ ] Session tokens are validated on every request (not cached across requests)
- [ ] Password storage uses current hashing algorithms (bcrypt, argon2)
- [ ] Failed authentication attempts are rate-limited

### Injection Prevention (OWASP A03)

- [ ] SQL queries use parameterized statements (no string concatenation)
- [ ] HTML output is escaped before rendering
- [ ] Shell commands do not include unsanitized user input
- [ ] No `eval()` or equivalent dynamic code execution with user-controlled strings
- [ ] Template engines use auto-escaping where available

### Data Exposure (OWASP A01)

- [ ] Error responses do not leak stack traces, file paths, or internal state
- [ ] Sensitive data (PII, credentials, tokens) is not logged
- [ ] API responses do not include more fields than the consumer needs
- [ ] Secrets are loaded from environment variables or vault, not hardcoded
- [ ] Database queries select only needed columns (no `SELECT *` when unnecessary)

### Cryptography

- [ ] TLS is used for all network communication
- [ ] Encryption algorithms are current (AES-256, not DES or RC4)
- [ ] Random values use cryptographically secure generators
- [ ] JWTs (if used) are validated for signature, expiration, and issuer
- [ ] Encryption keys are rotated according to policy

### Dependency Security

- [ ] No known vulnerable dependencies in the changeset
- [ ] Third-party packages are pulled from trusted registries
- [ ] Lock files are present and committed
- [ ] No unnecessary dependencies introduced

---

## Report Format

```
SECURITY REVIEW
===============

Status: [PASS | CONDITIONAL | FAIL]

Findings:
---
[SEC-1] [CRITICAL] [file:line] [OWASP Category] Description
  Vulnerability: [what the attack looks like]
  Remediation: [what to change]

[SEC-2] [IMPORTANT] [file:line] Description
  Risk: [what could happen]
  Remediation: [what to change]
---

Summary: [1-2 sentences on the overall security posture of this changeset]
```
