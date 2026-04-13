---
name: protocodebase
description: Activates ProtoCodeBase mode for the current project. Use when the user types "acp", "pcb", or requests ACP protocol initialization, ACP bootstrap, ACP-managed workflow, or wants to search/download code from ProtoCodeBase. Also activates when the user asks to start a new task, create a request, or follow ACP evolution flow in an ACP-managed project.
---

# ProtoCodeBase Skill

This skill covers two responsibilities:

1. **pcb CLI** — search and download ACP-compatible codebases from ProtoCodeBase
2. **Protocol activation** — bootstrap supported protocols (currently ACP) for the current project

---

## Part 1: pcb CLI

### When to Use

Trigger this section when the user requests:

- "Search for [topic] on ProtoCodeBase"
- "Download a project from ProtoCodeBase"
- "Pull code with slug [slug-id]"
- "pcb search", "pcb pull", "pcb map", "pcb token"

### Installation

```bash
npm i -g protocodebase-cli
# or without install:
npx protocodebase-cli <command>
```

### Authentication

```bash
pcb token save --token "PCBToken_xxx"   # one-time setup
pcb token clear                          # remove token
```

### Commands

**Search:**

```bash
pcb search --query "keyword"
pcb search --query "keyword" --pick 1 --project-name "my-project" --yes
pcb search --query "keyword" --api-ip "8.146.230.241:9528" --pick 1
```

**Pull by slug:**

```bash
pcb pull --slug "project-slug-id" --save-path "/abs/path" --yes
```

**Generate PROJECT_MAP:**

```bash
pcb map <project-root> [--output <path>] [--yes]
pcb map ./my-project --yes
```

After `pcb map`:
1. Review `entry_files` — remove irrelevant entries
2. Add `description` per module
3. Fill in `capability_routes` manually (requires semantic judgment)

**Token count:**

```bash
pcb token <file>
```

### Output Structure After Download

```
<project-root>/
├── acp-protocol/            # ACP consumer package
│   └── acp_agent_playbook.yaml
├── AGENTS.md
└── src/
    ├── .acp/                # Project ACP state
    │   ├── version.yaml
    │   ├── kernel/
    │   └── support/
    └── <source-code>/
```

### Network Configuration

Default endpoints: `https://protocodebase.com`

Override for local development:

```bash
export PROTOCODEBASE_WEB_BASE="http://8.146.230.241"
export PROTOCODEBASE_API_BASE="http:/8.146.230.241/api/v1"
```

Direct IP fallback: `--api-ip "8.146.230.241:9528"`

TLS compatibility: `export PROTOCODEBASE_CURL_TLS_MAX="1.2"`

### Agent Workflow for Search & Download

1. Check token: `pcb token save` if not set
2. Search: `pcb search --query "<keyword>" --pick 1 --project-name "<name>" --yes`
3. Verify output: check `acp-protocol/` and `src/.acp/version.yaml` exist
4. Generate PROJECT_MAP if absent: `pcb map src/`
5. Report: project location, ACP version, repo name, PROJECT_MAP status

### Error Handling

- **Token missing** → `pcb token save --token "PCBToken_xxx"`
- **Network failure** → use `--api-ip` flag
- **No results** → refine search query
- **TLS error** → CLI auto-retries with TLS 1.2; fallback: `PROTOCODEBASE_CURL_TLS_MAX=1.2`

---

## Part 2: Protocol Activation

### Supported Protocols

| Protocol | Description | Entry File |
|---|---|---|
| ACP | Agent Content Protocol v1.0 | `acp-protocol/acp_agent_playbook.yaml` |

### When to Use

Trigger this section when the user types:

`acp`, `pcb`, "use acp", "acp protocol", "initialize acp", "acp bootstrap",
"start acp", "new request", "create request", "follow acp", "acp workflow"

### Step 1: Detect Project Structure

Check the project root:

```
project/
├── AGENTS.md
├── acp-protocol/
│   └── acp_agent_playbook.yaml    ← protocol authority
└── src/
    └── .acp/
        └── version.yaml           ← project ACP state
```

**If `acp-protocol/acp_agent_playbook.yaml` exists** → load it, proceed to Step 2.

**If `acp-protocol/` does not exist** → tell the user:
> "This project does not have `acp-protocol/` installed.
> Copy the ACP distribution package:
> 1. `compilation/acp-protocol/` → `project/acp-protocol/`
> 2. `compilation/AGENTS.md` → `project/AGENTS.md`
> Then re-run `acp` or `pcb`."

### Step 2: Load Protocol

**ACP:** Read `acp-protocol/acp_agent_playbook.yaml`.

This file contains behavioral instructions for all ACP lifecycle stages:
bootstrap → intake → work_intake → request → plan → execution → change → validation.

Do not proceed with mutation-oriented work before loading this file.

### Step 3: Bootstrap Project State

Read `src/.acp/version.yaml`. Extract:
- `acp_version`
- enabled profiles: `kernel`, `capability`, `support`

If `support: enabled`:
→ Read `src/.acp/support/AGENT.md`
→ If `quick_entry` block exists, read it first
→ Surface `active_request_id` to user if present

If `capability: enabled`:
→ Read `src/.acp/capability/capabilities.yaml` when routing is needed

### Step 4: Report Status

```
ProtoCodeBase — ACP Active
──────────────────────────────
Version:   <acp_version>
Profiles:  kernel + <enabled profiles>
State:     src/.acp/
Protocol:  acp-protocol/acp_agent_playbook.yaml

Active Request: <active_request_id or "none">
Next Step:      <recommended ACP action>
```

Surface active request prominently if set.

### Step 5: ACP Work Mode

All mutation-oriented work follows:

```
Request → Plan → Change
```

- Do NOT modify source files without a confirmed Request and Plan
- Do NOT create a Change without a Plan
- Kernel objects → `src/.acp/kernel/`
- Templates → `acp-protocol/templates/`
