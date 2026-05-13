# ACP Minimal System Prompt

**Document ID:** `acp_minimal_system_prompt`
**Version:** `v0.1-draft`
**Status:** `Working Draft`
**Class:** `Repository Guide`
**Authority:** `Repository navigation guidance`
**Anchors:** `acp_host_adapter_model`, `acp_host_adapter_install`, `acp_runtime_bootstrap_spec`

---

## 1. Purpose

This document defines the minimal ACP-oriented prompt append that may be added to
a host system prompt or equivalent host configuration surface.

It is intentionally small.

It exists to:

- activate ACP when `.acp/version.yaml` is present
- route hosts toward `ACP-PUBLIC` as the consumer package
- avoid re-stating the full ACP corpus inside host-local configuration

---

## 2. Minimal Prompt Append

Recommended first-pass prompt append:

```text
If `.acp/version.yaml` exists, ACP is active. Use ACP bootstrap before broad repository exploration. Prefer staged intake and avoid repository-wide broad scanning by default. Treat project-local `.acp/` as the project-state source. Use ACP-PUBLIC as the consumer package. Do not invent Request / Plan / Change object shapes. Use ACP-PUBLIC/templates for new kernel objects. Use ACP-PUBLIC/tools/validate_acp_kernel_objects.rb before treating kernel objects as compliant. Treat mutation-oriented work as governed by Request -> Plan -> Change. When support or capability data exists, use it for routing before broader scanning. In local-development mode, an open Change may temporarily omit `git_refs`, but a done Change must include `git_refs`. Treat host-local guidance as activation guidance only, not as protocol authority.
```

---

## 3. Use Constraints

This prompt append SHOULD:

- be appended to an existing host system prompt
- remain activation-oriented rather than explanation-heavy
- point the host toward package-local templates and validators

This prompt append SHOULD NOT:

- replace the host's full role and tool instructions
- duplicate large portions of ACP protocol or runtime prose
- hardcode one project path or one demo project

---

## 4. Relationship to Install Guidance

`host/install.md` should reference this file when a user or host wants a
copy-ready prompt append.

This file defines the reusable prompt text.
`host/install.md` defines how and where that text should be attached.
