---
name: acp-v1-0-0
description: "Use this skill for ACP v1.0.0 projects. Trigger whenever a repository declares protocol: acp with protocol_version 1.0.0, has src/.acp/version.yaml with acp_version 1.0.0, mentions ACP v1.0.0, or asks to follow Request -> Plan -> Change in an ACP-managed project."
version: "1.0.0"
protocol:
  id: acp
  version: "1.0.0"
---

# ACP v1.0.0 Skill

This skill activates Agent Content Protocol v1.0.0 behavior for a project.

Keep this skill lightweight. Use the bundled protocol references only when the
task needs exact rules, templates, or audit context.

## Bundled Resources

- `references/acp-protocol/acp_agent_playbook.yaml` — exact agent behavior playbook.
- `references/acp-protocol/ACP_PUBLIC_DRAFT.md` — human-readable protocol overview.
- `references/acp-protocol/ACP_AGENT_CONSUMPTION_PROFILE.yaml` — rule trace profile.
- `references/acp-protocol/trace_manifest.yaml` — section trace manifest.
- `templates/` — Request / Plan / Change authoring templates.
- `scripts/` — optional helper scripts for this skill.

## Activation

Use this skill when the project declares ACP v1.0.0 in either modern or legacy
form:

```yaml
protocol: acp
protocol_version: "1.0.0"
```

```yaml
acp_version: "1.0.0"
```

If the project declares another ACP version, stop and ask the user to install
the matching protocol skill.

## Bootstrap

1. Locate project state in this order:
   - `.acp.yaml`
   - `src/.acp/version.yaml`
   - `.acp/version.yaml`
2. Read the version/profile declaration.
3. Confirm the declared protocol is ACP v1.0.0.
4. If support is enabled, read project support in this order:
   - `src/.acp/support/AGENT.md`
   - `src/.acp/support/PROJECT_MAP.yaml`
   - `src/.acp/support/LOAD_RULES.yaml`
   - `src/.acp/support/CHANGE_POLICY.yaml`
5. If capability is enabled, read:
   - `src/.acp/capability/capabilities.yaml`

Do not broadly scan the repository before completing bootstrap and support
routing.

## Request -> Plan -> Change

For mutation-oriented work:

1. Classify the operation and semantic capability/slice.
2. Create a Request under the project's ACP kernel.
3. Create a Plan linked to that Request.
4. Execute only within the planned scope.
5. Record a Change with validation results.

Use bundled templates from `templates/` when creating new kernel objects.

## Legacy Project Support

Some ACP v1.0.0 projects vendor `acp-protocol/` in the repository root. This
skill no longer requires that. Prefer the bundled protocol references in this
skill for protocol behavior, and treat project-local `.acp/` files as the
project state.

If a user explicitly asks to inspect the vendored `acp-protocol/`, read it as
project-local reference context, not as mutable application code.

## Exact Rule Lookup

When behavior is ambiguous, open:

```text
references/acp-protocol/acp_agent_playbook.yaml
```

When explaining ACP to a human, prefer:

```text
references/acp-protocol/ACP_PUBLIC_DRAFT.md
```

When auditing provenance, use:

```text
references/acp-protocol/ACP_AGENT_CONSUMPTION_PROFILE.yaml
references/acp-protocol/trace_manifest.yaml
```
