# ACP-PUBLIC Usage

**Document ID:** `acp_public_usage`
**Version:** `v1.0.0-public-draft`
**Status:** `Research Draft`
**Class:** `Release / Publication File`
**Authority:** `Controlled public distribution draft`
**Anchors:** `acp_host_adapter_install`, `acp_public_distribution_profile`

---

## Purpose

This guide explains how to use `ACP-PUBLIC` as a standalone consumer package for
ACP-managed projects.

`ACP-PUBLIC` is sufficient for host attachment and first-pass ACP consumption.
It is not intended for protocol maintenance or source revision work.

---

## What You Need

To use `ACP-PUBLIC`, you need:

1. a host or agent that can accept a short ACP prompt append
2. this `ACP-PUBLIC` package
3. an ACP-managed project containing `.acp/version.yaml`

You do not need additional ACP source materials in order to consume an existing
ACP-managed project through this package.

---

## Step 1: Attach ACP Behavior to the Host

Use:

- `host/minimal_system_prompt.md`
- `host/install.md`
- `host/operation_glossary.md`

Apply the minimal ACP activation contract to the host's existing prompt or
configuration surface.

Do not replace the host's full system prompt.

---

## Step 2: Enter an ACP-Managed Project

Open the target project and verify that the project root contains:

- `.acp/version.yaml`

That file is the ACP activation point for project-local continuation.

---

## Step 3: Use the Derived Agent Profile

Use:

- `ACP_AGENT_CONSUMPTION_PROFILE.yaml`

This file is the machine-facing derived rule surface for:

- ACP activation
- bootstrap
- staged intake
- `Request -> Plan -> Change`
- retained guard families

This file does not replace project-local `.acp/`.

---

## Step 4: Use Package Templates for New Kernel Objects

Use:

- `templates/acp_init_guide.md`
- `templates/acp_request_template.yaml`
- `templates/acp_change_template.yaml`
- `templates/acp_plan_template_compact.md`
- `templates/acp_plan_template_light.md`
- `templates/acp_plan_template_full.md`

These files provide the minimum retained project creation guidance and object
shapes needed for compliant ACP initialization and Request / Plan / Change
authoring.

---

## Step 5: Use Project-Local ACP State

The project's `.acp/` remains the project-state source.

Typical project-local ACP state includes:

- `.acp/version.yaml`
- `.acp/kernel/*`
- `.acp/support/*` when support is enabled
- `.acp/capability/*` when capability is enabled

Use `ACP-PUBLIC` for ACP rule consumption.
Use project-local `.acp/` for project state.

---

## Supported Behavior

This package currently supports public consumption for:

- ACP activation and bootstrap
- staged intake
- support-guided and capability-guided routing
- `Request -> Plan -> Change` continuation flow
- sample-level state audit already retained in the current cut

This package should not yet be treated as a complete substitute for protocol
maintenance or semantic evolution work.

---

## Where Projects Live

ACP-managed projects should live outside `ACP-PUBLIC`.

If `ACP-PUBLIC` is distributed independently, the target ACP-managed project
should be a separate project directory containing its own `.acp/`.

---

## Validation

Use:

- `tools/validate_acp_kernel_objects.rb`

Run the validator against the target project before treating newly created or
revised kernel objects as compliant.

By default the validator uses local-development rules.
Use `--release-mode` only when validating release-style exported ACP state.

---

## Minimum Success Check

A host or agent using only `ACP-PUBLIC` should be able to:

1. detect `.acp/version.yaml`
2. enter ACP bootstrap
3. avoid repository-wide broad scanning by default
4. continue `Request -> Plan -> Change`
5. use `.acp/` as project-state source rather than treating `ACP-PUBLIC` as project state
6. validate newly created kernel objects before treating them as compliant

If these six checks fail, the host attachment or consumer setup is incomplete.
