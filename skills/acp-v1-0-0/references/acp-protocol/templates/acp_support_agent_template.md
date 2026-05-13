# AGENT.md Template

**Document ID:** `acp_support_agent_template`
**Class:** `Template`
**Authority:** `ACP Support Layer — AGENT.md authoring template`

---

Copy this file to `.acp/support/AGENT.md` and fill in the placeholders.

---

```yaml
quick_entry:
  project_name: [your-project-name]
  project_type: [web_service | mobile_app | cli_tool | library | other]
  capabilities:
    - [capability-id-1]
    - [capability-id-2]
  active_request_id: null
  status: [active development | stable | prototype | maintenance]
  next_step: read PROJECT_MAP for module routing
```

# [Your Project Name] Agent Guide

## ACP Configuration

```yaml
acp:
  kernel_root: .acp/kernel
  support_root: .acp/support

  execution_order:
    - AGENT.md
    - PROJECT_MAP.yaml
    - LOAD_RULES.yaml
```

## Project Overview

[One paragraph describing what this project does and its primary purpose.]

## Working Rules

- Read `PROJECT_MAP.yaml` before scanning source directories
- Follow `LOAD_RULES.yaml` forbidden paths — they are hard boundaries
- Do not begin mutation work without a confirmed Request and Plan
- Kernel objects are in `.acp/kernel/`

## Notes

[Any project-specific notes for agents — known constraints, active work areas, etc.]
