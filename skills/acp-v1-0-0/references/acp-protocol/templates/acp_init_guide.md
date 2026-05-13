# ACP Project Initialization Guide

**Document ID:** `acp_init_guide`
**Version:** `v0.1-draft`
**Status:** `Working Draft`
**Class:** `Template Guide`
**Authority:** `Project initialization guidance`
**Anchors:** `acp_init_minimum_project`, `acp_init_kernel_only`, `acp_init_full_profile`

---

## 1. Purpose

This guide explains the minimum steps needed to create a new ACP-managed
project.

It is intended for:

- human authors creating a new ACP project
- agents scaffolding a new ACP project
- hosts that need a small, package-local ACP project creation reference

This guide does not replace the protocol or runtime specifications.
It gives the minimum initialization shape needed to start a valid ACP project.

---

## 2. Minimum Project Layout

Every ACP-managed project MUST have its own project root.

The minimum ACP project layout is:

```text
project-root/
├── .acp/
│   ├── version.yaml
│   └── kernel/
│       ├── requests/
│       ├── plans/
│       └── changes/
├── src/ or app source files
└── optional AGENTS.md
```

Host-local directories such as `.opencode/` or `.codex/` are not part of the
minimum ACP payload.

---

## 3. Choose an Initial Profile

### 3.1 Kernel-only

Use `kernel-only` when:

- the project is small or early-stage
- semantic capability routing is not yet needed
- support-layer guidance is not yet needed
- the goal is to start with the minimum ACP overhead

Recommended initial declaration:

```yaml
acp_version: "1.0.0"
profiles:
  kernel: required
  capability: disabled
  support: disabled
```

### 3.2 Full Profile

Use `full` when:

- the project is expected to grow into a long-lived system
- capability-grounded routing is needed
- support-guided intake and boundary control are needed
- multiple agents or more structured continuation are expected

Recommended initial declaration:

```yaml
acp_version: "1.0.0"
profiles:
  kernel: required
  capability: enabled
  support: enabled
```

If `capability` is enabled, create `.acp/capability/`.
If `support` is enabled, create `.acp/support/`.

---

## 4. Create `.acp/version.yaml`

At minimum, create:

```yaml
acp_version: "1.0.0"
profiles:
  kernel: required
  capability: disabled
  support: disabled
```

This file is the ACP activation point for project-local continuation.

---

## 5. Create the First Kernel Objects

For a newly initialized project, create:

- one initial Request
- one initial Plan
- one initial Change

Use:

- `acp_request_template.yaml`
- `acp_plan_template_compact.md` or `acp_plan_template_light.md`
- `acp_change_template.yaml`

The initial Change MAY remain `open` while work is still in progress.

In local-development mode:

- an `open` Change MAY temporarily omit `git_refs`
- a `done` Change MUST include `git_refs`

---

## 6. Recommended First Request Shape

The first Request usually describes one of:

- create the initial application
- establish the initial project structure
- add the first bounded feature

For kernel-only projects, prefer:

- `target_type: project`

For capability-enabled projects, prefer:

- `target_type: capability`

Use `target_type: custom` only if the project already uses a stable
project-specific semantic vocabulary.

---

## 7. Validate Before Treating Objects as Compliant

After creating or editing kernel objects, run the package-local validator from
the `ACP-PUBLIC` package root:

```bash
./tools/validate_acp_kernel_objects.rb <project_root>
```

For example:

```bash
cd /path/to/ACP-PUBLIC
./tools/validate_acp_kernel_objects.rb /path/to/project-root
```

Or, from the canonical workspace:

```bash
./scripts/validate_acp_kernel_objects.rb <project_root>
```

The validator checks:

- required Request / Plan / Change fields
- status vocabulary
- cross references
- local-development vs release-mode git binding

---

## 8. Initial Success Criteria

A newly initialized ACP project is minimally usable when:

1. `.acp/version.yaml` exists
2. `.acp/kernel/requests/`, `plans/`, and `changes/` exist
3. the first Request / Plan / Change objects validate
4. the project can be continued through `Request -> Plan -> Change`

---

## 9. Non-Goals

This guide does not require:

- host-specific directories in the project
- capability or support for every project
- full runtime source materials inside the project
- immediate Git binding for an `open` Change in local development
