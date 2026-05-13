# ACP Host Adapter Install Guide

**Document ID:** `acp_host_adapter_install`
**Version:** `v0.1-draft`
**Status:** `Working Draft`
**Class:** `Repository Guide`
**Authority:** `Repository navigation guidance`
**Anchors:** `acp_host_adapter_model`, `acp_runtime_bootstrap_spec`, `acp_agent_consumption_model`

---

## 1. Purpose

This guide explains how a user or agent should attach ACP-compatible behavior to
a host environment without copying host-specific directories into every ACP
project.

---

## 2. Recommended Installation Model

The recommended model is:

1. keep ACP project state inside the project's `.acp/`
2. keep host attachment logic outside the project payload
3. attach a short ACP activation contract to the host's system prompt or host
   configuration

This keeps:

- ACP project repositories smaller
- host configuration optional
- protocol semantics centralized in ACP rather than duplicated in host files

---

## 3. Minimal Host Prompt Append

A minimal ACP host attachment SHOULD communicate the following behavior.

Use:

- `host/minimal_system_prompt.md`
- `host/operation_glossary.md`

Recommended first-pass prompt append:

```text
If `.acp/version.yaml` exists, ACP is active. Use ACP bootstrap before broad repository exploration. Prefer staged intake. When support or capability data exists, use it for routing before broader scanning. Treat mutation-oriented work as governed by Request -> Plan -> Change. If the current workspace is an ACP authority workspace and the task is new application creation, default to ACP-aware initialization or scaffolding unless the user explicitly opts out. Treat host-local guidance as activation guidance only, not as protocol authority.
```

This prompt append SHOULD remain short and activation-oriented, and it SHOULD be
appended to the host's existing instructions rather than replacing them.

---

## 4. Installation by a Human

A human installing ACP-compatible behavior into a host SHOULD:

1. read `host/host_adapter.md` in the current package
2. read `host/minimal_system_prompt.md`
3. read `host/operation_glossary.md` when host-side operation interpretation is needed
4. add a short ACP activation prompt append to the host's system prompt or
   equivalent configuration surface
5. avoid copying the full ACP protocol text into the host configuration
6. verify that the host can detect `.acp/version.yaml` and enter bootstrap

---

## 5. Installation by an Agent

An agent installing ACP-compatible behavior into a host SHOULD:

1. attach only the minimal ACP activation contract
2. preserve the host's existing role and tool instructions
3. avoid replacing the full system prompt with ACP-specific text
4. avoid writing host-specific adapter directories into generated ACP projects
   unless the user explicitly requests that behavior
5. prefer the prompt text defined in `host/minimal_system_prompt.md`
6. use `host/operation_glossary.md` only as a lightweight operation vocabulary, not as protocol authority

---

## 6. What Should Be Installed Into Projects

For package-local project creation guidance, see:

- `../templates/acp_init_guide.md`

By default, ACP project initialization or scaffolding SHOULD create:

- project source files
- project-local `.acp/`
- optional project-local `AGENTS.md`

By default, ACP project initialization or scaffolding SHOULD NOT create:

- `.opencode/`
- `.codex/`
- other host-specific adapter directories

Host-specific directories are optional local workflow layers, not part of the
minimum ACP project payload.

---

## 7. Authority-Workspace Host Config

An ACP authority workspace MAY contain local host configuration such as
`.opencode/`.

That workspace-local host configuration:

- is workspace-local
- is not part of the ACP minimum project payload
- SHOULD NOT be copied into generated ACP projects by default
- MAY be ignored when a user attaches ACP through another host

---

## 8. Verification

After host attachment, the host or agent SHOULD be able to do the following on
an ACP-managed project:

1. detect `.acp/version.yaml`
2. enter ACP bootstrap
3. follow staged intake
4. form or continue `Request -> Plan -> Change`
5. avoid repository-wide broad scanning by default

If these five behaviors are not present, the host attachment is incomplete.
