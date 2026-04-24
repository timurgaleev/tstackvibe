---
name: document-release
description: |
  Generate release notes and update CHANGELOG for a new version. Reads git history,
  categorizes changes, and writes clear user-facing release notes. Use when asked to
  "write release notes", "update changelog", "document release", "what's in this release".
allowed-tools:
  - Bash
  - Read
  - Write
  - Edit
  - Grep
  - AskUserQuestion
triggers:
  - write release notes
  - update changelog
  - document release
  - whats in this release
---

## Release Documentation Workflow

### Step 1 — Gather changes

```bash
# Get version info
cat VERSION 2>/dev/null || cat package.json | grep '"version"' | head -1

# Get changes since last tag
git log $(git describe --tags --abbrev=0 2>/dev/null || echo "HEAD~20")..HEAD --oneline

# Get merged PRs
gh pr list --state merged --limit 30 2>/dev/null | head -20
```

Ask: "What version is this? What's the release date?"

### Step 2 — Categorize changes

Sort each commit/PR into:
- **Added** — new features, new endpoints, new UI
- **Changed** — behavior changes, performance improvements
- **Fixed** — bug fixes
- **Removed** — deprecated features removed
- **Security** — security patches (always highlight)
- **Breaking** — API changes that break existing integrations

Drop: internal refactoring, test changes, dependency bumps (unless security).

### Step 3 — Write release notes

**User-facing language** (not "fixed null pointer exception" → "Fixed crash when loading empty list"):

```markdown
## [X.Y.Z] — YYYY-MM-DD

### Highlights
<2-3 sentence summary of what this release does>

### Added
- Feature name: what it does and why it matters

### Fixed
- Bug description: what was broken and how it affected users

### Breaking changes
- What changed, how to migrate

### Security
- CVE-XXXX-XXXX: what was fixed
```

### Step 4 — Update CHANGELOG

Prepend to `CHANGELOG.md`. If no CHANGELOG exists, create it with the standard format.

### Step 5 — Optional: GitHub release

```bash
gh release create v<version> --title "v<version>" --notes "$(cat <<'EOF'
<paste release notes>
EOF
)"
```

Ask: "Should I create a GitHub release as well?"
