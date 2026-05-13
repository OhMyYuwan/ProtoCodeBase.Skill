# ACP Compilation Output

**Document ID:** `acp_compilation_output_readme`
**Version:** `1.0.0`
**Status:** `Working Draft`
**Class:** `Release / Publication File`
**Authority:** `Compilation output — derived from ACPv1.0.0/spec/`

---

## Purpose

This directory contains the compiled, consumer-facing artifacts derived from
the ACP canonical spec at `ACPv1.0.0/spec/`.

These artifacts are NOT the spec. They are derived outputs organized by
consumer type. In any conflict, the spec is authoritative.

---

## Contents

### For humans (concept understanding)

- `ACP_PUBLIC_DRAFT.md` — Human-readable protocol overview. Covers core
  concepts, object model, conformance shapes, and runtime behavior with
  sufficient context for evaluation and review. (~1,500 lines)

### For agents (behavioral execution)

- `acp_agent_playbook.yaml` — Stage-organized behavioral instruction playbook.
  Tells agents what to DO, MUST NOT do, and what comes NEXT at each execution
  stage: bootstrap → intake → work_intake → request → plan → execution →
  change → validation. Also contains cross-stage guards and absolute
  prohibitions. (~500 lines)

### For auditors (rule tracing)

- `ACP_AGENT_CONSUMPTION_PROFILE.yaml` — Rule-level tracing index. Links
  retained normative rules back to their canonical source anchors in spec/.
  Use for audit, compliance review, and traceability verification.
- `trace_manifest.yaml` — Section-level tracing index. Maps retained spec
  sections to their source documents and anchors.

### Distribution attachments

- `templates/` — Copies of Kernel object authoring templates from
  `ACPv1.0.0/templates/`. Use these to author Request, Plan, and Change
  objects in ACP-managed projects.
- `host/` — Copies of host attachment files from `ACPv1.0.0/host/`. Use
  these to attach ACP-compatible behavior to agent host environments.

### Package metadata

- `USAGE.md` — Usage guidance for this output package
- `VERSIONING.md` — Version alignment information
- `AUTHORS.md` — Authorship and stewardship
- `CITATION.cff` — Citation metadata
- `LICENSE` — Governing rights terms

---

## How to Use

**For human review**: Read `ACP_PUBLIC_DRAFT.md`.

**For agent integration**: Load `acp_agent_playbook.yaml` at bootstrap
activation. This file tells the agent exactly what to do at each stage.
Combine with `host/minimal_system_prompt.md` for host attachment.

**For audit**: Use `ACP_AGENT_CONSUMPTION_PROFILE.yaml` and
`trace_manifest.yaml` to verify rule provenance.

**For project setup**: Use `templates/` for authoring kernel objects and
`host/install.md` for host attachment setup.

---

## Authority Note

This package is a controlled compilation output. It is not a standalone
authority. All rules trace back to `ACPv1.0.0/spec/`. When in doubt, the
spec is the source of truth.

---

## Purpose

This directory is the first packaged `ACP-PUBLIC v1.0.0` surface.

It exists to provide a smaller, separately distributable ACP package for:

- external human review
- controlled public evaluation
- agent-facing experimentation against the derived rule surface

This package is intended to be self-describing as a consumer-facing ACP package.

---

## Contents

- `ACP_PUBLIC_DRAFT.md` - human-facing public draft
- `ACP_AGENT_CONSUMPTION_PROFILE.yaml` - machine-facing derived agent profile
- `trace_manifest.yaml` - trace surface linking retained public rules back to canonical source
- `host/` - host-neutral attachment and installation layer for public consumption
- `host/minimal_system_prompt.md` - copy-ready host prompt append
- `host/operation_glossary.md` - minimal host-facing operation vocabulary
- `templates/` - authoring templates plus ACP project initialization guidance
- `tools/` - package-local validation tooling
- `LICENSE` - governing rights terms
- `AUTHORS.md` - package authorship and stewardship
- `CITATION.cff` - package citation metadata

---

## Status

This package is a controlled public distribution draft.

Current confidence:

- human-facing public draft: available
- machine-facing derived profile: available
- traceability surface: available
- sample-level round-trip fidelity: established on one ACP sample
- broader multi-sample confidence: not yet established

This package should be treated as a controlled public distribution draft.

---

## Operating Model

`ACP-PUBLIC` is a standalone consumer package.

It is intended to be used as follows:

1. a host attaches ACP-compatible behavior using `host/`
2. an agent detects project-local `.acp/`
3. the host or agent consumes `ACP_AGENT_CONSUMPTION_PROFILE.yaml`
4. the project's `.acp/` remains the project-state source

This package is meant for ACP project consumption and continuation, not protocol
maintenance or source revision work.

---

## Supported Scope

This package is intended to support:

- ACP activation and bootstrap
- staged intake
- `Request -> Plan -> Change` continuation behavior
- Request / Plan / Change object authoring through shipped templates
- kernel object validation through the shipped validator
- sample-level state-audit behavior already retained in the current cut
- host attachment through the included `host/` layer

This package should not yet be treated as a complete replacement for ACP
maintenance and authoring materials.

---

## Project Location

`ACP-PUBLIC` is not the place where managed ACP projects should accumulate.

Generated or managed ACP projects should live outside this package in their own
project directories.

---

## Source Line

`ACP-PUBLIC` is aligned to the ACP 1.0.0 line.

This package contains the consumer-facing materials needed for public use. It
does not include protocol-maintenance source materials or internal working
records.

---

## Rights

This package is distributed under the included `LICENSE`.

Read `LICENSE` before reuse, redistribution, or commercial evaluation.
