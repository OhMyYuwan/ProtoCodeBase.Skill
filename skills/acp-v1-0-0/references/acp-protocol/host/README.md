# ACP Host Layer

**Document ID:** `acp_host_readme`
**Version:** `v0.1-draft`
**Status:** `Working Draft`
**Class:** `Repository Guide`
**Authority:** `Repository navigation guidance`
**Anchors:** `acp_host_adapter_model`, `acp_host_adapter_install`, `acp_minimal_system_prompt`

---

## Purpose

This directory contains ACP's host attachment layer.

It defines how a host environment can attach ACP-compatible behavior to an
agent without turning host-local configuration into ACP protocol authority.

This layer is:

- not part of ACP protocol core
- not part of ACP runtime core
- not part of the minimum ACP project payload

This layer exists to describe host-neutral attachment and installation behavior.

## Contents

- `minimal_system_prompt.md` - copy-ready minimal ACP prompt append for host systems
- `operation_glossary.md` - minimal host-facing vocabulary for common ACP project operations
- `host_adapter.md` - host-neutral ACP attachment model
- `install.md` - host-neutral ACP installation guidance

## Recommended Reading Order

1. `minimal_system_prompt.md`
2. `operation_glossary.md`
3. `install.md`
4. `host_adapter.md`
