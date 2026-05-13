# ACP Git Workflow Policy

**Document ID:** `acp_git_workflow_policy`
**Version:** `1.0.0`
**Status:** `Normative Guidance`
**Class:** `Host Attachment Layer`
**Authority:** `ACP Protocol — Git integration guidance`

---

## 1. Purpose

This document defines how ACP-managed projects should integrate with Git version control.

It provides normative guidance for:
- Branch naming conventions aligned with ACP Request/Plan/Change flow
- Commit message standards
- Change object ↔ Git commit binding
- Merge strategies

---

## 2. Branch Strategy

### 2.1 Core Branches

**`main`** — Production branch
- Always stable and deployable
- Protected: no direct push, requires PR + CI + code review
- Only accepts merges from `develop`
- Every merge should be tagged with version (e.g., `v1.0.0`)

**`develop`** — Integration branch
- Contains latest development code for next release
- Protected: requires PR + basic CI
- Accepts merges from personal development branches
- Should always be buildable

### 2.2 Personal Development Branches

All development work MUST happen on personal branches.

**Naming format:**
```
<username>/<type>/<description>
```

**Branch types:**

| Type | Purpose | Example |
|---|---|---|
| `feature` | New feature development | `yuwan/feature/observation-agent` |
| `bugfix` | Bug fix | `yuwan/bugfix/ax-helper-crash` |
| `hotfix` | Emergency fix from main | `yuwan/hotfix/security-patch` |
| `refactor` | Code refactoring | `yuwan/refactor/coordinator-logic` |
| `docs` | Documentation update | `yuwan/docs/api-documentation` |
| `test` | Test-related work | `yuwan/test/integration-tests` |
| `chore` | Build/tool/dependency updates | `yuwan/chore/update-dependencies` |

### 2.3 ACP Change ↔ Branch Binding

**Rule:** Each ACP Change object SHOULD correspond to one feature branch.

When a Change is created:
1. Create a branch: `<username>/<type>/<change-id-or-description>`
2. Record branch name in Change object's `branch_name` field (optional but recommended)
3. Work on the branch until Change is done
4. When Change status → `done`, record commits in `git_refs`

**Example:**
```yaml
# .acp/kernel/changes/CHG-001.yaml
change_id: CHG-001
plan_id: PLN-001
status: done
branch_name: yuwan/feature/observation-agent
git_refs:
  - commit: a1b2c3d
    message: "feat(observation): implement AX element tree extraction"
  - commit: e4f5g6h
    message: "feat(observation): add accessibility hierarchy parser"
```

---

## 3. Commit Message Standard

ACP projects SHOULD follow **Conventional Commits** format:

```
<type>(<scope>): <subject>

<body>

<footer>
```

### 3.1 Commit Types

| Type | Description |
|---|---|
| `feat` | New feature |
| `fix` | Bug fix |
| `docs` | Documentation update |
| `style` | Code formatting (no logic change) |
| `refactor` | Refactoring (neither feature nor fix) |
| `perf` | Performance optimization |
| `test` | Test-related |
| `chore` | Build process or auxiliary tool changes |
| `revert` | Revert previous commit |

### 3.2 Examples

```bash
feat(observation): implement AX element tree extraction
fix(coordinator): resolve null pointer exception in task allocation
docs(readme): update installation instructions
refactor(agent): simplify Agent base class initialization
```

### 3.3 Commit Best Practices

1. **Atomic commits**: Each commit does one thing
2. **Clear messages**: Explain "why" not just "what"
3. **Frequent commits**: Small steps, easy to revert
4. **Test before commit**: Ensure code builds and runs

---

## 4. Workflow

### 4.1 Start New Work (Create Change)

```bash
# 1. Ensure local develop is up-to-date
git checkout develop
git pull origin develop

# 2. Create personal development branch
git checkout -b <username>/<type>/<description>

# Example:
git checkout -b yuwan/feature/observation-agent
```

### 4.2 Daily Development

```bash
# 1. Commit regularly
git add <files>
git commit -m "feat: implement Observation Agent base framework"

# 2. Sync with develop regularly
git fetch origin develop
git rebase origin/develop

# 3. Push to remote
git push origin <your-branch>

# First push needs upstream
git push -u origin <your-branch>
```

### 4.3 Submit Pull Request (Close Change)

```bash
# 1. Ensure branch is up-to-date
git fetch origin develop
git rebase origin/develop

# 2. Push to remote
git push origin <your-branch>

# 3. Create PR on GitHub
# - Base: develop
# - Compare: <your-branch>
# - Fill PR template
# - Request code review
```

### 4.4 After Merge

```bash
# After PR is approved and merged to develop

# Delete remote branch
git push origin --delete <your-branch>

# Delete local branch
git checkout develop
git pull origin develop
git branch -d <your-branch>
```

### 4.5 Release to Production

```bash
# 1. Create release branch from develop
git checkout develop
git pull origin develop
git checkout -b release/v1.0.0

# 2. Final testing and bug fixes only (no new features)

# 3. Merge to main
git checkout main
git pull origin main
git merge --no-ff release/v1.0.0
git tag -a v1.0.0 -m "Release version 1.0.0"
git push origin main --tags

# 4. Merge back to develop
git checkout develop
git merge --no-ff release/v1.0.0
git push origin develop

# 5. Delete release branch
git branch -d release/v1.0.0
git push origin --delete release/v1.0.0
```

---

## 5. Merge Rules

### 5.1 Pull Request Requirements

**Merge to `develop`:**
- ✅ All CI checks pass
- ✅ Code review approved (at least 1 person)
- ✅ No merge conflicts
- ✅ Branch based on latest develop
- ⚠️ Unit test coverage not lower than current level

**Merge to `main`:**
- ✅ All CI/CD checks pass
- ✅ Code review approved (at least 2 people)
- ✅ Integration tests pass
- ✅ Performance tests pass
- ✅ Security scan pass
- ✅ Documentation updated
- ✅ CHANGELOG updated

### 5.2 Merge Strategies

| Scenario | Strategy | Reason |
|---|---|---|
| Personal branch → develop | Squash and merge | Keep develop history clean |
| develop → main | Merge commit | Preserve full development history |
| hotfix → main | Merge commit | Emergency fixes need traceability |

---

## 6. ACP Integration Points

### 6.1 Change Object Git Binding

When Change status changes:

**`open` → `in_progress`:**
- Create feature branch
- Optionally record `branch_name` in Change object

**`in_progress` → `done`:**
- MUST record `git_refs` with commit hashes and messages
- In local-development mode, `git_refs` MAY be temporarily omitted for `open` Changes
- In release mode, `done` Changes MUST have `git_refs`

**Example Change object:**
```yaml
change_id: CHG-001
plan_id: PLN-001
status: done
branch_name: yuwan/feature/observation-agent
git_refs:
  - commit: a1b2c3d
    message: "feat(observation): implement AX element tree extraction"
    timestamp: "2026-04-23T10:30:00Z"
  - commit: e4f5g6h
    message: "feat(observation): add accessibility hierarchy parser"
    timestamp: "2026-04-23T11:45:00Z"
```

### 6.2 Request ↔ Pull Request Mapping

**Recommended mapping:**
- 1 Request = 1 or more Changes
- 1 Change = 1 feature branch
- 1 feature branch = 1 Pull Request

When Request is completed:
- All associated Changes should be `done`
- All feature branches should be merged to `develop`
- Request can be marked `done`

---

## 7. Hotfix Flow

```bash
# 1. Create hotfix branch from main
git checkout main
git pull origin main
git checkout -b <username>/hotfix/<description>

# 2. Fix issue and test
git add <files>
git commit -m "hotfix: fix XXX issue in production"

# 3. Merge to main
git checkout main
git merge --no-ff <username>/hotfix/<description>
git tag -a v1.0.1 -m "Hotfix version 1.0.1"
git push origin main --tags

# 4. Merge to develop
git checkout develop
git merge --no-ff <username>/hotfix/<description>
git push origin develop

# 5. Delete hotfix branch
git branch -d <username>/hotfix/<description>
```

---

## 8. Constraints

- Personal branches MUST follow `<username>/<type>/<description>` naming
- Commit messages SHOULD follow Conventional Commits format
- `done` Changes MUST include `git_refs` in release mode
- Direct push to `main` or `develop` is FORBIDDEN
- Force push to `main` is FORBIDDEN
- Force push to `develop` is ALLOWED only in emergencies

---

## 9. Tools and Automation

### 9.1 Git Aliases

Add to `~/.gitconfig`:

```ini
[alias]
    st = status
    co = checkout
    br = branch
    ci = commit
    lg = log --oneline --graph --all --decorate
    sync = !git fetch origin develop && git rebase origin/develop
```

### 9.2 Git Hooks

Recommended hooks in `.git/hooks/`:

- `pre-commit`: Check code format
- `commit-msg`: Validate commit message format
- `pre-push`: Run tests before push

---

## 10. References

- [Conventional Commits](https://www.conventionalcommits.org/)
- [GitHub Flow](https://guides.github.com/introduction/flow/)
- [Git Official Documentation](https://git-scm.com/doc)
