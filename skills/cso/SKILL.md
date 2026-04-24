---
name: cso
description: |
  Chief Security Officer review. Runs OWASP Top 10 + STRIDE threat model audit on
  the codebase. Finds vulnerabilities, rates severity, provides fixes. Use when asked
  to "security audit", "run cso", "owasp review", "check for vulnerabilities",
  "security review".
allowed-tools:
  - Bash
  - Read
  - Grep
  - Glob
  - AskUserQuestion
  - WebSearch
triggers:
  - security audit
  - run cso
  - owasp review
  - check for vulnerabilities
  - security review
---

## Security Audit Workflow

### Step 1 — Scope

```bash
git log --oneline -20
find . -type f -name "*.ts" -o -name "*.js" -o -name "*.py" -o -name "*.go" -o -name "*.rb" | grep -v node_modules | grep -v ".git" | head -50
```

Ask: "Full audit or just the recent changes on this branch?"

### Step 2 — OWASP Top 10 Check

For each category, grep for patterns and read the relevant code:

**A01 — Broken Access Control**
```bash
grep -r "req.user\|currentUser\|isAdmin\|hasRole\|authorize" --include="*.ts" --include="*.js" --include="*.py" -l
```
- Are authorization checks on every route that needs them?
- Can users access other users' data by changing an ID?
- IDOR (insecure direct object reference) risks?

**A02 — Cryptographic Failures**
```bash
grep -rn "MD5\|SHA1\|DES\|RC4\|password.*plain\|bcrypt\|argon2\|pbkdf2" --include="*.ts" --include="*.js" --include="*.py" | grep -v node_modules
```
- Passwords hashed with bcrypt/argon2 (not MD5/SHA1)?
- Sensitive data encrypted at rest?
- TLS everywhere?

**A03 — Injection**
```bash
grep -rn "query\|exec\|eval\|innerHTML\|dangerouslySetInnerHTML" --include="*.ts" --include="*.js" --include="*.py" | grep -v node_modules | head -20
```
- SQL: parameterized queries everywhere?
- NoSQL injection?
- XSS: output sanitized before inserting into DOM?
- Command injection: user input in shell commands?

**A04 — Insecure Design**
- Business logic flaws: can users skip payment? Double-spend? Abuse rate limits?
- Are negative test cases considered in design?

**A05 — Security Misconfiguration**
```bash
grep -rn "debug.*true\|NODE_ENV.*dev\|cors.*origin.*\*" --include="*.ts" --include="*.js" --include="*.py" | grep -v node_modules
find . -name ".env" -o -name ".env.local" | grep -v node_modules
```
- Default credentials?
- Debug mode in production?
- CORS too permissive?
- Error messages leaking stack traces?

**A06 — Vulnerable Components**
```bash
cat package.json 2>/dev/null | grep -E '"dependencies"|"devDependencies"' -A 50 | head -30
# Check for known vulnerable packages
npm audit 2>/dev/null || true
```

**A07 — Auth Failures**
```bash
grep -rn "jwt\|session\|cookie\|token\|login\|logout" --include="*.ts" --include="*.js" --include="*.py" -l | grep -v node_modules
```
- Session fixation?
- JWT verified properly (not just decoded)?
- Brute force protection on login?
- Password reset tokens expire?

**A08 — Integrity Failures**
- Deserialization of untrusted data?
- Dependencies from untrusted sources?

**A09 — Logging Failures**
```bash
grep -rn "console.log\|logger\|log\." --include="*.ts" --include="*.js" | grep -i "password\|token\|secret\|key" | grep -v node_modules | head -10
```
- Logging sensitive data?
- Security events logged (login failures, access denied)?
- Logs accessible to attackers?

**A10 — SSRF**
```bash
grep -rn "fetch\|axios\|http.get\|request\|url" --include="*.ts" --include="*.js" | grep -v node_modules | head -20
```
- User-controlled URLs fetched server-side?
- Internal network protected from SSRF?

### Step 3 — STRIDE Threat Model (brief)

For the main system components, check:
- **S**poofing: Can attackers impersonate users or services?
- **T**ampering: Can data be modified in transit or at rest?
- **R**epudiation: Can users deny actions they took?
- **I**nformation disclosure: Where is sensitive data exposed?
- **D**enial of service: What can be flooded/abused?
- **E**levation of privilege: Can users gain higher permissions?

### Step 4 — Secrets scan

```bash
grep -rn "API_KEY\|SECRET\|PASSWORD\|PRIVATE_KEY\|ACCESS_KEY" --include="*.ts" --include="*.js" --include="*.py" --include="*.env" | grep -v node_modules | grep -v ".example" | grep -v "process.env\|os.environ\|config\." | head -20
```

### Output

```
## Security Audit: <project name>

### CRITICAL (fix immediately, block deploy)
- [OWASP A03] SQL injection in FILE:LINE — fix: use parameterized query

### HIGH (fix before next release)
- ...

### MEDIUM (fix in next sprint)
- ...

### LOW (note for backlog)
- ...

### Verdict: SECURE / ISSUES FOUND

Risk score: X/10 (10 = critical, 0 = clean)
```
