# ACP Operation Glossary

**Document ID:** `acp_operation_glossary`
**Version:** `v0.1-draft`
**Status:** `Working Draft`
**Class:** `Repository Guide`
**Authority:** `Host-neutral operation interpretation guidance`
**Anchors:** `acp_operation_glossary`, `acp_runtime_operation_model`

---

## Purpose

This glossary provides a small host-facing interpretation layer for common ACP
project operations.

It does not replace the full runtime operation model.
It exists to give hosts and lightweight ACP consumers a minimal shared
vocabulary for common user-facing actions.

---

## Operation Principles

- operations are user-facing action labels
- operations are not Kernel objects
- mutation-oriented operations still remain governed by `Request -> Plan -> Change`
- operation words do not bypass authorization, boundary, or state-legality rules

---

## Core Operation Set

### `create`

Use when the task is to create a new ACP-managed project or a new project-level
artifact.

Typical behavior:

- initialize or scaffold project structure
- create initial `.acp/`
- create first Request / Plan / Change when mutation work begins

### `inspect`

Use when the task is primarily observational and does not yet require mutation.

Typical behavior:

- read project state
- summarize structure
- diagnose current ACP status

### `modify`

Use when the task changes existing project behavior, structure, or state.

Typical behavior:

- form or continue Request
- form or continue Plan
- execute bounded modification
- record Change

### `repair`

Use when the task begins from a broken, invalid, or contradictory state.

Typical behavior:

- diagnose failure
- identify invalid or unsafe state
- enter restricted mode if legality cannot be established
- proceed through Request / Plan / Change when actual correction is prepared

### `validate`

Use when the task is to determine whether the current state satisfies required
conditions.

Typical behavior:

- inspect build, test, or acceptance state
- check kernel object validity
- report pass / fail / uncertain result

### `build`

Use when the task is to compile, assemble, or otherwise produce a runnable or
checkable artifact.

Typical behavior:

- run project build steps
- capture build result
- treat failures as validation or repair input when relevant

### `run`

Use when the task is to execute the project or a runnable artifact.

Typical behavior:

- launch runtime or app behavior
- observe result
- feed result into validation or repair when needed

### `export`

Use when output leaves the current continuation context.

Typical behavior:

- classify output mode
- preserve ACP metadata only when required
- avoid confusing project continuation output with product output

---

## Minimal Routing Rule

Hosts and lightweight ACP consumers SHOULD interpret operations as follows:

- `inspect` and `validate` may stay read-oriented until mutation is actually needed
- `create`, `modify`, and `repair` commonly become mutation-oriented and therefore
  should enter `Request -> Plan -> Change`
- `build`, `run`, and `export` may remain narrower than full modification work,
  but they MUST still respect ACP guards when they mutate project state

---

## Non-Goals

This glossary does not define:

- the full runtime stage model
- skill composition
- detailed operator-to-skill mappings
- project-specific operation extensions

For fuller runtime-side semantics, use the ACP runtime operation model.

