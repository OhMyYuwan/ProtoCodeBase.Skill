# ACP Host Adapter Model

**Document ID:** `acp_host_adapter_model`
**Version:** `v0.1-draft`
**Status:** `Working Draft`
**Class:** `Informative Companion`
**Authority:** `Runtime-side interpretation guidance`
**Anchors:** `acp_agent_consumption_model`, `acp_workspace_application_model`, `acp_runtime_bootstrap_spec`

---

## 1. Purpose

This companion defines how a host environment should attach ACP-compatible
behavior to an agent without turning host-local prompt configuration into ACP
protocol authority.

It exists to separate:

- ACP protocol and runtime semantics
- project-local ACP state
- optional host-local adapter behavior

---

## 2. What a Host Adapter Is

A host adapter is a thin host-local integration layer that helps an agent:

- detect when ACP applies
- activate ACP bootstrap before broad exploration
- prefer staged intake over repository-wide scanning
- route to project-local `.acp/` as the project state source

A host adapter MAY be implemented as:

- a short system-prompt append
- a host-local configuration file
- a host plugin or extension
- an agent-installable rule bundle

The host adapter is not itself ACP authority.

---

## 3. What a Host Adapter Is Not

A host adapter MUST NOT be treated as:

- a replacement for ACP protocol specifications
- a replacement for ACP runtime specifications
- a replacement for project-local `.acp/`
- a reason to duplicate full ACP protocol text into every application repository

A host adapter SHOULD remain thin and activation-oriented rather than carrying a
full restatement of ACP.

---

## 4. Minimal Activation Contract

At minimum, a host adapter SHOULD instruct an agent to follow this behavior:

1. if `.acp/version.yaml` exists, ACP is active
2. if the current workspace is an ACP authority workspace and the task is new
   application creation, default to ACP-aware initialization or scaffolding
3. enter ACP bootstrap before repository-wide exploration
4. prefer support-guided and capability-guided routing when enabled
5. treat mutation-oriented work as governed by `Request -> Plan -> Change`
6. treat host-local configuration as guidance only, not as protocol authority

This activation contract SHOULD be short enough to be attached to a host without
replicating the protocol corpus.

A copyable first-pass prompt append is provided in `host/install.md` in the
current package.

The canonical reusable prompt text is defined in `host/minimal_system_prompt.md`.

---

## 5. Relationship to Project Structure

ACP-managed application repositories SHOULD carry project-local ACP artifacts
such as:

- `.acp/version.yaml`
- `.acp/kernel/*`
- `.acp/support/*` when support is enabled
- `.acp/capability/*` when capability is enabled

ACP-managed application repositories SHOULD NOT require host-local adapter
directories such as `.opencode/` in order to remain ACP-manageable.

Host-local adapter directories MAY exist in an ACP authority workspace for local
workflow convenience, but they SHOULD NOT be treated as part of the minimum ACP
project payload.

---

## 6. Distribution Guidance

The recommended distribution model is:

- keep ACP protocol authority in the ACP source tree
- keep project-local ACP state in each ACP-managed repository
- keep host adapters optional and separately attachable

This means a host adapter SHOULD usually be distributed as:

- a reusable prompt append
- a short installation guide
- optional host-local config templates

It SHOULD NOT require copying host-specific directories into every generated ACP
project.

---

## 7. Current Repository Posture

In this repository, host-local configuration such as `.opencode/` MAY exist
because this repository also acts as an ACP authority workspace.

That does not make `.opencode/` part of the ACP project minimum.

For ACP initialization and ACP scaffolding, the preferred baseline remains:

- project-local `.acp/`
- project source files
- optional project-local `AGENTS.md`

---

## 8. Recommended Next Layer

The minimal reusable package for host attachment is:

- `host/host_adapter.md`
- `host/install.md`
- `host/minimal_system_prompt.md`

Host-specific adapters MAY be added later, but ACP does not require separate
host-specific adapter packages in order to define the host-attachment model.
