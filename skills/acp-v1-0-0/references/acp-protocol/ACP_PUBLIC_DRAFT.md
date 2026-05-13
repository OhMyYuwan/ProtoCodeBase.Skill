# ACP-PUBLIC v1.0.0 Draft

**Document ID:** `acp_public_draft`
**Version:** `v1.0.0-public-draft`
**Status:** `Research Draft`
**Class:** `Release / Publication File`
**Authority:** `Controlled public distribution draft`
**Anchors:** `acp_public_distribution_profile`, `first_cut_retained_section_manifest`

---

## Title / Version / Status

- Source protocol version: `1.0.0`
- Publication status: ACP-PUBLIC v1.0.0 controlled public draft
- Rights posture: public distribution only; see included LICENSE

## What This Public Draft Is

This document is the public draft shipped with ACP-PUBLIC.
It does not include protocol-maintenance materials or internal working records.

## Core ACP Model

### What is ACP


ACP (Agent Context Protocol) is a **software evolution governance protocol** for agent-driven continuation and ACP-aware project creation.

ACP is not:
- A project description format
- A prompt engineering framework
- A documentation standard
- An agent coordination system

ACP is:
- A protocol for recording and constraining software evolution
- A semantic interface between user intent and code modification
- A project-centric governance model
- An append-only history system

### Evolution Model


```
User Intent
    ↓
Request (semantic target)
    ↓
Plan (execution strategy)
    ↓
Change (actual modification)
    ↓
Git (version control)
```

The evolution unit is **Request**, not commit.

### Four-Layer Architecture


ACP consists of four layers:

| Layer | Status | Purpose |
|-------|--------|---------|
| **Top** | REQUIRED | Governance and layer coordination |
| **Kernel** | REQUIRED | Evolution semantics and state machine |
| **Capability** | OPTIONAL | Semantic target vocabulary |
| **Support** | OPTIONAL | Project context and loading rules |

### Layer Dependencies


```
Top (REQUIRED)
  ↓ defines layer model
  ↓
Kernel (REQUIRED)
  ├─ may reference semantic targets from Capability (OPTIONAL)
  └─ may be operationally constrained by Support (OPTIONAL)

Capability does not require Support.
Support MAY reference Capability when capability-oriented routing, mapping, or
boundary policy is used.
```

**Rules:**
- Kernel must exist in all ACP projects
- Capability and Support are optional
- Capability does not require Support
- Support MAY exist with or without Capability
- Support MAY reference Capability when Capability is present
- Lower layers cannot redefine upper layer rules

### Strict Boundaries


**Forbidden:**
- ❌ Capability in Kernel (Kernel only references capability_id as opaque)
- ❌ Support defines evolution (evolution belongs to Kernel)
- ❌ Kernel defines capability meaning (capability belongs to Capability layer)
- ❌ Support defines global governance (governance belongs to Top)

### Deployment Model and Interpretation Boundary


ACP separates three artifact scopes:

1. **Protocol Specification**
2. **Agent Runtime**
3. **Project-local ACP Instance**

These scopes are related, but they are NOT the same artifact and MUST NOT be conflated.

## Kernel Closure

### Request as the atomic evolution unit


ACP defines Request as the minimal unit of evolution.

A Request represents a single user intent that can be planned and executed.

A Request may correspond to:

- a new feature
- a modification
- a refactor
- a fix
- a configuration change

A Request may result in multiple Plans and multiple Changes.

This allows ACP to support imprecise user requirements while still maintaining
a strict evolution history.

### Baseline Request Creation Requirements


Rules:

- Request MUST define an intended modification summary
- Request MUST define a scope descriptor
- Request SHOULD define a semantic target
- Request MUST have unique id
- Request MUST be immutable after execution starts, except through append-only supersession or resynchronization defined by this specification
- Request MUST represent the current formal execution unit for downstream Plan and Change
- Request MUST NOT be treated as identical to raw user wording

Request represents the atomic evolution unit.

Agents MUST NOT modify code without Request.

### Request schema


Request is the atomic evolution unit.

Required fields:

```
id: string
title: string
target_type: string
target_id: string
target_slice: string
operation: string
summary: string
status: string
created_at: string
```

Optional fields:

```
intent: string
notes: string
priority: string
profile_refs: object
capability_id: string
derived_from_plan: string
request_sync_state: string
supersedes_request: string
superseded_by_request: string
```

Rules:

- target_type MUST define how the semantic target interface is interpreted
- target_type SHOULD use a stable core vocabulary when possible:
  - `capability` for capability-grounded semantic targets
  - `project` for project-wide operations or project-scoped governance targets
  - `custom` when the project uses a stable project-specific semantic target vocabulary
- target_id MUST exist unless the project uses purely structural scoping
- target_slice SHOULD exist
- `_root` SHOULD be used if no slice exists
- Request SHOULD bind to exactly one primary semantic target
- If `target_type` is `custom`, its interpretation MUST remain stable within the
  project and MUST NOT vary across equivalent Requests
- Request MUST NOT use file path, module path, or repository path as its primary
  semantic identity through `target_type`
- Request MUST NOT change after execution starts, except through append-only supersession or traceable synchronization defined by the Kernel
- intent is optional
- Request MAY omit intent if the project does not actively use Intent objects
- capability_id is optional
- If the capability profile is disabled, Request MUST NOT require capability_id
- Kernel treats capability_id as opaque mirror metadata
- derived_from_plan MAY be used when the current Request is synchronized to a revised Plan
- request_sync_state MAY be used to record whether Request is aligned with the current execution Plan
- supersedes_request and superseded_by_request MAY be used to preserve append-only history when Request alignment changes

Request remains the formal execution unit, but it MAY be replaced by a successor
Request if the current confirmed Plan materially redefines execution scope,
target binding, or user constraints.

Allowed status:

```
open
planned
executing
done
cancelled
superseded
```

If the Capability profile is enabled, the primary semantic target SHOULD use
Capability identifiers and slices defined by the Capability Specification.

### Plan as executable blueprint


ACP defines Plan as an executable blueprint, not as a paraphrase of Request.

A Plan exists to make execution reviewable, transferable, and controllable.

Therefore:

- Plan MUST NOT merely restate Request in different words
- Plan MUST define an execution strategy
- Plan SHOULD be sufficient for another qualified agent to execute or review
- Plan MAY include execution steps, affected modules, risks, validation logic, and fallback direction
- Plan MAY be more detailed than Request
- Plan MUST remain semantically aligned with Request unless superseded or resynchronized under Kernel rules

Request defines what should be changed.

Plan defines how that change will be executed.

This distinction is required for safe agent-driven evolution.

### Baseline Plan Generation Requirements


Rules:

- Plan MUST reference one Request
- Plan MUST describe execution steps
- Plan MUST define affected modules
- Plan SHOULD define risk classification when execution carries meaningful risk
- Plan MUST have status
- Plan MUST define execution strategy rather than merely restating Request
- Plan MUST remain traceably aligned with its Request

Plan MUST exist before Change.

Agents MUST NOT create Change without Plan.

A Plan is the executable blueprint of a Request.

### Plan confirmation


Plan confirmation may be skipped by users.

Rules:

- Plan SHOULD be confirmed by user
- User MAY allow auto-confirm
- Agent MUST record confirmation state
- Agent MUST NOT execute unconfirmed Plan unless allowed

This allows both strict and relaxed workflows.

### Plan schema


Plan defines execution strategy.

Required fields:

```
id: string
request: string
summary: string
affected_modules: list
user_confirmation: bool
status: string
created_at: string
```

Optional fields:

```
risk: string
notes: string
supersedes_plan: string
plan_revision: integer
plan_change_type: string
request_alignment_status: string
```

Rules:

- Plan MUST reference Request
- Plan MUST exist before Change
- Plan MAY produce multiple Changes
- Plan MAY be auto-confirmed
- Plan SHOULD define risk classification when execution carries meaningful risk
- Plan MUST define an execution strategy
- Plan MUST NOT merely paraphrase Request
- Plan MAY be revised, replaced, or superseded
- if a Plan is materially revised, as defined by the Plan-to-Request synchronization rule, its Request alignment status MUST be made explicit before execution starts or resumes
- supersedes_plan MAY be used to preserve append-only history across Plan revisions
- plan_revision MAY be used to identify the current revision of a Plan lineage
- plan_change_type MAY distinguish editorial revision from material revision
- request_alignment_status MAY indicate whether the referenced Request is still aligned with the current Plan

A Plan is the current execution blueprint only when it is the latest valid,
traceable, and execution-eligible Plan for its Request lineage.

Allowed status:

```
draft
confirmed
executing
done
cancelled
superseded
```

### Change schema


Change records execution result.

Required fields:

```
id: string
request: string
plan: string
summary: string
affected_modules: list
status: string
created_at: string
```

Optional fields:

```
affected_semantic_targets: list
affected_capabilities: list
git_refs: list
notes: string
```

Rules:

- Change MUST reference Request
- Change MUST reference Plan
- Change MUST be append-only
- Change MUST summarize result
- Change MAY remain `open` while local Git binding is still pending
- Change MAY reference multiple commits
- Change MAY omit git_refs in release
- In local development, `done` Change MUST include git_refs
- affected_capabilities MAY be present if the Capability profile is enabled

Allowed status:

```
open
done
superseded
```

### Required execution sequence


All modifications MUST follow the sequence:

```
Intent → Request → Plan → Change → Git
```

Rules:

1. Code MUST NOT be modified without a Change
2. Change MUST reference a Plan
3. Plan MUST reference a Request
4. Request MUST define a semantic target or equivalent scope descriptor
5. Agent MUST follow the loop
6. User MAY skip confirmation
7. Agent MUST NOT skip recording

This defines the minimal evolution protocol.

### Direct modification prohibition


Agents MUST NOT directly modify code outside the loop.

Forbidden:

```
edit file
commit
without Request / Plan / Change
```

Allowed:

```
Request
Plan
Change
then edit
```

This rule guarantees traceability.

### Request as normalized execution authority


Request is the normalized execution authority derived from user input and, when
applicable, from Intent grouping.

Therefore:

- Request MUST be more explicit than user input
- Request MUST be stable enough for deterministic planning
- Request MUST define the formal execution target and scope
- Request MUST NOT be treated as a permanent verbatim proxy for the earliest user wording

The earliest user wording remains historical context.

The current Request defines the formal execution unit.

Later planning does not silently replace Request authority.

If the current Request and current executable Plan materially diverge,
Request synchronization and alignment blocking rules apply before execution
may safely continue.

### Request → Plan rule


Each Request MUST produce one active execution Plan at a time.

Plan defines execution strategy.

Plan MUST reference Request.

Plan MUST NOT exist without Request.

Request → Plan → Change

Plan MUST NOT combine unrelated Requests.

Plan MUST remain deterministic.

Plan MUST NOT merely paraphrase Request.

Plan MUST provide sufficient execution structure for controlled implementation,
review, or handoff.

### Plan required before Change

A Change MUST NOT exist without a Plan.

Rules:
- Plan MUST reference one Request
- Change MUST reference one Plan
- Change MUST reference one Request
- Plan MUST exist before Change

Forbidden:
```
Request → Change
```
Required:
```
Request → Plan → Change
```
This prevents uncontrolled modification.

### Plan-to-Request synchronization


If a materially revised Plan becomes the current to-be-executed Plan, the
corresponding Request MUST be synchronized before execution starts or resumes.

At minimum, synchronization MUST align:

- intended execution goal
- primary semantic target binding
- target slice or equivalent scope descriptor
- explicit inclusion and exclusion boundaries
- explicit user constraints that affect execution authority

A Plan revision is materially divergent when it changes one or more of:

- the primary semantic target binding
- target slice or equivalent scope descriptor
- explicit inclusion or exclusion boundaries
- user constraints that materially change what execution is authorized to do
- validation or acceptance obligations that redefine what counts as completion
- risk-bearing actions or execution route in a way that requires new boundary,
  confirmation, or authorization evaluation

A Plan revision is not materially divergent when it is limited to:

- wording cleanup
- formatting-only edits
- step reordering that does not change scope, target, constraints, or risk
- explanatory notes that do not change execution authority

Rules:

- editorial Plan changes MAY use lightweight synchronization metadata
- material Plan changes SHOULD trigger successor Request creation or equivalent append-only Request replacement
- execution MUST pause when Request alignment status is pending
- Change MUST NOT be created against a materially outdated Request / Plan pair
- the synchronized Request becomes the current formal execution unit for downstream execution history

This rule ensures that the currently executable blueprint and the currently
recorded formal Request describe the same authorized modification.

### Alignment blocking rule


If the current execution Plan has undergone a material revision and the
corresponding Request has not yet been synchronized, execution MUST be blocked.

Blocked conditions include:

- plan_change_type = material
- request_alignment_status = pending
- Plan lineage has a newer valid revision than the one currently used by the execution agent

When blocked:

- Plan MUST NOT enter executing state for further code modification
- Change MUST NOT be created
- Request MUST be synchronized, superseded, or otherwise made current before execution resumes

This rule prevents stale authorization from driving new modifications.

### Local mode rule

_Conditions: `local-development`_

In local mode:
- done Change MUST include git_refs
- done Change MUST include commit
- branch SHOULD exist
- done Change MUST bind commit
- open Change MAY temporarily omit git_refs while commit binding is pending

Required:
```
git_refs:
  - commit: xxxx
  - branch: main
```
Local mode guarantees rollback safety at Change closure.

### Release mode rule

_Conditions: `release-mode`_

In release mode:
- git_refs MAY be removed
- branch MAY be removed
- commit MAY be removed

Must keep:
```
Request
Plan
Change
Intent
```

Capability files, if any, belong to the Capability Specification rather than the Kernel.
Release packages must remain valid without Git.

This allows publishing lightweight prototypes.

## Capability and Support Boundaries

### Capability as routing key

_Conditions: `capability-enabled`_



Capability is used by:

Request
Plan
Change
LOAD_RULES
PROJECT_MAP
CHANGE_POLICY

When Capability Specification is used, capability is the primary semantic routing key.

Agents SHOULD rely on capability
instead of file scanning.

Support Layer remains the structural routing authority.

### Relationship to Support Layer

_Conditions: `capability-enabled`_



Support Layer maps capability to structure.

Capability Layer MUST NOT contain:

- file paths
- module paths
- directory structure
- repository loading order
- execution-time confirmation policy

Those belong to Support Layer.

Capability defines meaning, not location.

Support defines where and how that meaning is structurally resolved.

Capability MUST NOT be reduced to a file alias or module alias.

A capability MAY currently map to one module, but its identity MUST remain
semantic rather than structural.

Capability MAY support user-facing semantic navigation, but structural intake,
context loading, and execution boundary control remain Support responsibilities.

### Slice Routing Semantics

_Conditions: `capability-enabled`_

Slices MAY carry two optional routing fields to make them operationally useful
for agents and maintainers:

**`route_here_when`**: A concise statement of when agent work should be routed
to this slice. Helps agents consistently identify the right slice without
re-deriving ownership from scratch.

**`bridge_when`**: A concise statement of when work started in this slice needs
to cross into another capability or slice. A bridge condition signals that the
narrowest-scope slice is insufficient and that Request scope must be reviewed.

Rules:

- `route_here_when` and `bridge_when` are optional but recommended for projects
  with multiple slices.
- These fields are informative routing guidance. They do not replace Kernel
  Request authorization.
- `bridge_when` MUST NOT be treated as automatic permission to expand scope.
  It is a signal to surface the conflict and seek user confirmation.
- When a bridge condition is met, the agent SHOULD confirm with the user whether
  to handle the work as a single cross-surface Request or split it.

### Capability vs Module

_Conditions: `capability-enabled`_



Module is structural.

Capability is semantic.

A capability MAY use multiple modules.

A module MAY serve multiple capabilities.

ACP MUST NOT assume 1:1 mapping.

### Capability use in Request

_Conditions: `capability-enabled`_



When Capability Specification is enabled and the Request targets a capability
(`Request.target_type = capability`):

- `Request.target_id` SHOULD be the capability identifier
- `Request.target_slice` SHOULD be a valid slice or `_root`

This binding is the formal semantic landing point of the Request.

At user-interaction level, earlier navigation MAY begin from capability groups,
domains, or rough semantic areas.

However, before Request becomes a stable execution target, it SHOULD resolve to
a specific capability identifier.

Capability groups or domain labels MAY help first-contact navigation, but they
MUST NOT replace concrete capability binding in Request.

### Capability usage in Plan

_Conditions: `capability-enabled`_



Plan steps SHOULD reference capabilities.

  steps:

    - capability: notification.send
      action: modify

Plan SHOULD remain capability-oriented.

Capability reference in Plan is not only for routing.

It is also required for semantic visibility, reviewability, and traceability.

A Plan that affects multiple semantic targets SHOULD preserve capability context
per step so that another agent, reviewer, or user can understand which semantic
functionality each step belongs to.

Plan MUST NOT collapse semantic work into file-only descriptions when Capability
Specification is enabled.

### Support Layer is Optional



Support Layer is optional.

Kernel operates independently of Support Layer.

Projects may be Kernel-conformant without Support Layer.

Support Layer is recommended for:

multi-agent projects
public projects
long-term projects
large codebases

Projects without Support Layer remain valid.
Agents may operate with reduced context efficiency.

Support Layer does not affect Kernel conformance.

### Support as context interface

_Conditions: `support-enabled`_


Support Layer acts as the context interface between:
```
Agent
Kernel
Codebase
```
Support Layer tells the agent:
```
where to look
what to load
what to avoid
what modification boundaries exist
```
Support Layer MUST NOT contain evolution state.

Support Layer MUST NOT replace Request / Plan / Change.

### Support must not replace Kernel

_Conditions: `support-enabled`_


Support Layer MUST NOT:

```store Request
store Plan
store Change
define Capability meaning
override state machine
rewrite history
```
Kernel remains authoritative.

Support is only contextual.

### Capability-oriented navigation

_Conditions: `support-enabled`_


Agents SHOULD navigate the project using Capability,
not directory structure.

Capability defines semantic intent.

Support Layer maps Capability to modules, paths, and rules.

When structural routing is needed, agents SHOULD prefer `PROJECT_MAP.yaml`
before repository guessing.

This allows code to belong to ideas,
not to individual developers.

### Progressive disclosure

_Conditions: `support-enabled`_


Agents MUST load only the context required for the current Request.

Support Layer defines loading rules that allow minimal context usage.

This is required for:
```
large projects
agent teams
low-token execution
prototype second development
```
Support Layer MUST enable progressive context loading.

### Purpose

_Conditions: `support-enabled`_



PROJECT_MAP.yaml defines the structural map of the project.

It allows agents to discover:

modules
module paths
module entry files
capability → module routes

PROJECT_MAP.yaml MUST NOT contain:

Request
Plan
Change
Kernel history
Capability definitions
Change policies
Load rules

Those belong to other ACP components.

### Purpose

_Conditions: `support-enabled`_



`LOAD_RULES.yaml` defines how project context is progressively disclosed to the
agent.

Its purpose is not only to list what may be loaded, but to control when and how
additional context becomes justified.

`LOAD_RULES.yaml` SHOULD help the agent:

- begin with lightweight takeover context
- avoid unnecessary broad repository reading
- load context according to the current semantic target
- expand context only when planning, execution, or validation requires it
- keep unrelated or restricted areas unloaded by default
- reuse prior-stage outputs before broader reloading when possible

`LOAD_RULES.yaml` is therefore a staged context control mechanism, not just a
file inclusion list.

### AGENT.md quick_entry block

_Conditions: `support-enabled`_

AGENT.md MAY include a `quick_entry` block at the top of the file as a
structured, machine-readable bootstrap accelerator.

The `quick_entry` block is optional. Its purpose is to allow agents to
complete bootstrap routing from AGENT.md without reading the full file body.

A conformant `quick_entry` block SHOULD contain:

- `project`: project name and one-line description
- `acp_version`: ACP version in use
- `profiles`: enabled layer profiles
- `active_request_id`: currently active Request identifier, if any
- `entry_order`: prioritized list of support files to read next
- `primary_capabilities`: top-level capability names when capability is enabled
- `agent_hint`: optional one-sentence routing note

Rules:

- `quick_entry` block MUST appear at the top of AGENT.md, before the file body
- `quick_entry` block MUST be delimited as a fenced code block with `yaml` and
  `role: quick_entry` declared inside
- When `quick_entry` is present, agents SHOULD read it first and MAY defer
  reading the AGENT.md body until user intent is narrower
- `active_request_id` MUST be surfaced to the user when present, so active
  work-in-progress is not silently bypassed
- `quick_entry` MUST NOT contain evolution state (no Request, Plan, or Change
  content beyond the active_request_id pointer)
- `quick_entry` content MUST remain consistent with the AGENT.md body and the
  project's `.acp/version.yaml`

## Runtime Entry and Runtime Consumption

### 4. Bootstrap Entry Conditions


A runtime SHOULD enter ACP bootstrap when one or more of the following is true:

1. the project contains `.acp/version.yaml`
2. the user explicitly states that the project is ACP-managed
3. external runtime configuration declares ACP handling for the project
4. the current task explicitly requests ACP-compatible operation

A runtime MAY refuse ACP bootstrap if ACP applicability cannot be established.

If ACP applicability is uncertain, the runtime SHOULD prefer cautious bootstrap inquiry
or restricted bootstrap behavior over silent assumption.

### ACP creation-path recognition


If `.acp/version.yaml` is absent, bootstrap SHOULD still determine whether the task is:

- ACP continuation for an existing ACP-managed project, or
- ACP project creation through initialization or scaffolding

Bootstrap SHOULD prefer ACP project creation handling when the user request is primarily:

- creating a new ACP-managed project
- turning an existing non-ACP project into an ACP-managed project
- generating a starter project together with ACP structure

Bootstrap SHOULD NOT treat missing `.acp/version.yaml` as sufficient reason to abandon
ACP handling when ACP project creation is the explicit task.

### 5. Bootstrap Minimum Inputs


The minimum bootstrap inputs SHOULD include:

- project root or equivalent project access scope
- user intent or current task request
- `.acp/version.yaml` when present

Additional recommended bootstrap inputs include:

- `.acp/support/AGENT.md`
- `.acp/support/PROJECT_MAP.yaml`
- `.acp/support/LOAD_RULES.yaml`
- `.acp/support/CHANGE_POLICY.yaml`
- `.acp/capability/capabilities.yaml`
- recent kernel objects when continuation context is required

Bootstrap SHOULD begin with the smallest sufficient ACP input set.

Bootstrap SHOULD avoid unnecessary broad source-tree intake before ACP entry is established.

When ACP project creation is the active task, bootstrap SHOULD prefer creation-relevant
inputs over continuation-oriented project history or broad source reading.

### Version-first rule


Bootstrap MUST treat `.acp/version.yaml` as the primary ACP entry point
when it is available.

### Support-before-broad-scan rule

_Conditions: `support-enabled`_


If support guidance exists, bootstrap SHOULD use it before broad project scanning.

When support profile is enabled, bootstrap SHOULD prefer:

- `AGENT.md` for initial entry guidance
- `PROJECT_MAP.yaml` only when structural routing is needed
- `LOAD_RULES.yaml` only when deeper route expansion must be justified

When structural routing is needed during bootstrap,
bootstrap SHOULD prefer `PROJECT_MAP.yaml` over repository guessing.

Bootstrap SHOULD NOT treat support presence as permission for broad repository intake.

### Capability-first interpretation rule

_Conditions: `capability-enabled`_


If capability definitions exist, bootstrap SHOULD prefer capability-first project interpretation
before file-path-first or source-tree-first explanation.

### Need-based kernel loading


Bootstrap SHOULD load kernel history only to the extent required
to understand current continuation context, alignment state, or active task lineage.

Bootstrap SHOULD avoid default loading of detailed request, plan, and change history
when current entry can be established without it.

### Companion deferral rule


Bootstrap SHOULD prefer the primary operation model over secondary operation mapping companions.

In particular, bootstrap SHOULD NOT load `runtime/operator_map.md` by default.

Bootstrap MAY load lower-frequency operation mapping detail only when:

- the user request cannot be routed safely through the high-level operation model, or
- a later lifecycle stage requires finer operator/stage mapping.

### 7. Bootstrap Output Obligations


Bootstrap MUST produce ACP-appropriate initial runtime outputs.

At minimum, bootstrap SHOULD produce:

1. ACP applicability judgment
2. ACP version and enabled profile summary
3. capability-first project summary when capability data exists
4. current bootstrap constraints and risks
5. recommended next ACP step

When task progression begins, bootstrap SHOULD additionally support:

- request preview
- plan preview or plan-init guidance
- restricted mode explanation when applicable

When ACP project creation is the active task, bootstrap SHOULD additionally support:

- creation-path judgment: `init` or `scaffold`
- recommended initialization or scaffold mode
- recommended project root naming and folder creation
- minimum ACP structure preview
- first Request/Plan guidance for the created project when applicable

### Missing-structure disclosure


If required or recommended ACP structures are absent, bootstrap SHOULD disclose
that absence explicitly rather than silently inventing certainty.

### Restricted bootstrap triggers

_Conditions: `restricted-runtime`_


Restricted bootstrap SHOULD be entered when:

- `.acp/version.yaml` is missing but ACP handling is requested
- ACP version is unsupported or uncertain
- enabled profiles cannot be interpreted safely
- support files are malformed or contradictory
- project-local ACP structure is incomplete in a way that blocks safe intake
- current task asks for unsafe direct execution before ACP entry is established

### Restricted bootstrap behavior

_Conditions: `restricted-runtime`_


Restricted bootstrap SHOULD prefer:

- structure explanation over execution
- preview over mutation
- narrow ACP-safe intake over broad speculative analysis
- explicit disclosure of uncertainty
- user confirmation before risky continuation

### Restricted bootstrap limitations

_Conditions: `restricted-runtime`_


Restricted bootstrap MUST NOT be used to justify:

- silent full-project modification
- fabricated ACP compatibility
- fabricated capability understanding
- fabricated request/plan progression

### Initialization path

_Conditions: `init-path`_


Bootstrap SHOULD recommend ACP initialization when:

- a project root already exists
- ACP structure is absent or incomplete
- the task is to make the existing project ACP-manageable

When initialization is recommended, bootstrap SHOULD guide or invoke the minimum
ACP structure defined by `acp_init_spec`.

### Migration Mode: Initializing Existing Non-ACP Projects

When ACP initialization is applied to a project that already has existing code,
documentation, or historical planning artifacts, the agent SHOULD use a
migration-aware approach:

- Reflect actual existing structure in `PROJECT_MAP.yaml` and `LOAD_RULES.yaml`
  rather than using generic templates.
- Reflect observed feature domains in `capabilities.yaml`.
- Distill high-value routing knowledge from historical documents into Support
  files rather than copying raw historical documents into the Kernel.
- Do NOT convert prior planning documents into ACP Request, Plan, or Change
  objects. New Kernel lineage begins from the first Request opened after
  migration.
- Disclose migration status in `AGENT.md` or `project.yaml`.

**Historical document guard**: Historical planning documents, architecture
notes, and backlog items are routing aids only. They are NOT execution
authorization and MUST NOT be converted into Requests without explicit user
confirmation of scope.

Incremental migration is valid: a project may start with `kernel-only` and add
Support and Capability layers progressively.

### Scaffold Path Selection

_Conditions: `creation-task`_


Bootstrap SHOULD recommend ACP scaffolding when:

- the user is creating a new software project
- ACP structure and starter project structure should be created together
- project type selection is part of the task

When scaffolding is recommended, bootstrap SHOULD guide or invoke the starter
generation behavior defined by `acp_scaffold_spec`.

### Scaffold Root Layout Requirements

_Conditions: `scaffold-path`_


When scaffolding is selected for a new standalone application,
bootstrap SHOULD also require:

- creation of a dedicated project root folder named after the software or project name unless a different root name is explicitly requested
- placement of generated application entry files inside that new project root
- avoidance of writing standalone app entry files directly at the parent workspace root

### Default Intake Order


Default logical intake order SHOULD be:

1. `.acp/version.yaml`
2. `.acp/support/AGENT.md` when present
3. `.acp/support/PROJECT_MAP.yaml` when present
4. `.acp/support/LOAD_RULES.yaml` when present
5. `.acp/support/CHANGE_POLICY.yaml` when present
6. `.acp/capability/capabilities.yaml` when present
7. relevant `.acp/kernel/*` objects as needed

### Minimum-First Intake Discipline


Project Intake SHOULD be stage-gated and minimum-first.

The runtime SHOULD avoid unnecessary full-project scanning during initial intake
when support and capability information are available.

### Support-Guided Structural Routing

_Conditions: `support-enabled`_


When support profile is enabled, runtime SHOULD prefer:

- `AGENT.md` for entry framing
- `PROJECT_MAP.yaml` for structural navigation
- `CHANGE_POLICY.yaml` only when planning, authorization, or execution relevance exists

The runtime SHOULD prefer `PROJECT_MAP.yaml` over repository guessing
when support-based structural routing is available.

### Route Expansion Control

_Conditions: `support-enabled`_


When support profile is enabled, runtime SHOULD prefer:

- `LOAD_RULES.yaml` for deeper route expansion control

The runtime SHOULD also prefer `LOAD_RULES.yaml` before deeper route expansion
when support-guided staged loading is available.

### Reuse-Before-Reload Discipline


The runtime SHOULD follow reuse-before-reload
and prefer prior-stage outputs over repeated loading
of already-established protocol or project facts.

### Intent Authorization Boundary

A recorded intention is NOT execution authorization.

This boundary applies to all forms of pre-Request intent expression — Intent
objects, support file notes, historical planning documents, or any project
memory outside the Kernel.

Rules:

- An Intent object MUST NOT be treated as a substitute for a Request.
- Pending requirements outside the Kernel MUST NOT be treated as authorization
  to modify code.
- An agent MUST NOT begin plan generation or code modification on the basis of
  a recorded intention alone, regardless of how detailed it appears.
- Active execution authority begins at Request formation and confirmation —
  not at intention recording.
- When a user references a pending idea or prior plan, the agent SHOULD clarify
  whether the user intends to open a new Request, rather than treating the
  reference as implicit authorization.

### Intent Normalization

  
Intent Normalization converts user intent into ACP-operable task understanding.  
  
The runtime SHOULD prefer capability-first interpretation over raw file-path-first interpretation  
when sufficient capability data exists.  

When the current task is already narrow and stable, Intent handling MAY remain lightweight
so long as Request formation still preserves ACP-operable scope and meaning.

### Work Intake: Before a Request Is Formed

For mutation operations (`modify`, `repair`, `create`), agents SHOULD perform a
work intake step before forming a Request. Work intake is not a required ACP
lifecycle phase, but is recommended when capability and support layers are
enabled.

Work intake answers:

1. Which capability does this work belong to?
2. Which slice within that capability owns the primary behavior?
3. Which `.acp/` authority boundary covers the full scope?
4. Does this work cross a `bridge_when` condition in any slice?
5. What are the first files/modules to open per capability routing?

Work intake boundaries:

- Work intake results are NOT authorization — they are scope-classification aids.
- A pending requirement identified during intake MUST NOT be treated as a Request.
- If a bridge condition is found, the agent SHOULD surface it and confirm with
  the user before forming the Request.

### Request Formation

  
Request Formation produces a request preview or request object suitable for ACP progression.  
  
When confirmation is required, the runtime SHOULD present a request preview  
before finalizing the request object.  

Request-stage previews SHOULD remain lightweight.

The runtime SHOULD load only enough context to ground:

- current scope
- semantic target
- relevant boundary notes
- the next safe ACP step

### Plan as Execution Guidance


The runtime MUST treat the plan as execution guidance,
not as a mere repetition of request text.

### Plan Revision and Alignment Behavior


When the plan is materially revised, the runtime SHOULD ensure request-plan alignment
before execution continues.

### Compact Plan Expression Boundary


For bounded, low-risk work, the runtime MAY express the plan through a compact template
or compact preview format.

However, compact expression MUST NOT:

- remove required ACP meaning
- erase execution strategy
- suppress affected scope
- bypass later Change recording

### Authorization Gate

  
Before unsafe, high-impact, or policy-sensitive operations,  
the runtime MUST determine whether authorization is sufficient.  
  
Authorization decisions MAY depend on:  
  
- protocol rules  
- project-local support rules  
- current task scope  
- user confirmation state

### Execution

  
Execution performs the intended work within protocol and project-local boundaries.  
  
Execution MUST NOT:  
  
- violate kernel legality  
- violate declared change boundaries  
- ignore authorization status  
- silently bypass compatibility concerns

### Change Recording

  
Change Recording MUST occur before the runtime considers the task closed.  
  
Recorded change output SHOULD preserve:  
  
- request linkage  
- plan linkage  
- affected scope summary  
- status result  
- relevant references when available

### Validation

  
Validation evaluates whether execution achieved the intended result.  
  
Validation SHOULD consider:  
  
- explicit validation plan  
- acceptance criteria  
- project-local constraints  
- user-facing success signal

### Restricted Mode triggers

_Conditions: `restricted-runtime`_

  
The runtime SHOULD enter Restricted Mode when:  
  
- ACP version is unsupported  
- an enabled ACP profile is unsupported  
- required `.acp/` structures are missing or malformed  
- runtime cannot establish kernel legality  
- project-local support rules are contradictory or uninterpretable  
- authorization is insufficient for requested execution  
- boundary validity cannot be established

### Restricted Mode behavior

_Conditions: `restricted-runtime`_

  
Restricted Mode SHOULD prefer:  
  
- reading over writing  
- explanation over mutation  
- preview over execution  
- explicit confirmation over silent continuation  
- narrow-scope reasoning over broad speculative action

### Restricted Mode limitations

_Conditions: `restricted-runtime`_

  
Restricted Mode MUST NOT be used to justify:  
  
- unsafe direct modification  
- illegal kernel progression  
- silent policy bypass  
- unsupported profile interpretation treated as valid

### 11. Runtime and Project-local Support Boundary

_Conditions: `support-enabled`_

  
Project-local Support files provide project-scoped guidance.  
  
The runtime MUST treat them as project-local configuration and interpretation input,  
not as global protocol authority.  
  
Project-local Support MAY influence:  
  
- intake sequence details  
- path mapping  
- scoped loading  
- local change boundaries  
- local bootstrap guidance  
  
Project-local Support MUST NOT be allowed to:  
  
- redefine ACP protocol semantics  
- disable mandatory runtime guards  
- remove mandatory kernel recording behavior  
- redefine runtime-wide mandatory lifecycle obligations

## Conformance Surface

### Conformance Model


**Conformance** = Does the project follow ACP rules?

Conformance is **project-level**, not agent-level.

### Shape Meaning and Anti-Ranking Clarification


They are not:

- maturity levels
- quality rankings
- recommendation order
- or project-standard tiers

They only describe which ACP optional layers are enabled
within ACP base protocol conformance.

### Kernel-only

```yaml
profiles:
  kernel: required
  capability: disabled
  support: disabled
```
Project has `.acp/kernel/` but no capability or support.

This is a valid ACP base conformance shape.

### Kernel + Capability

_Conditions: `capability-enabled`_

```yaml
profiles:
  kernel: required
  capability: enabled
  support: disabled
```
Project has `.acp/kernel/` and `.acp/capability/`.

This is a valid ACP base conformance shape.

### Kernel + Support

_Conditions: `support-enabled`_

```yaml
profiles:
  kernel: required
  capability: disabled
  support: enabled
```
Project has `.acp/kernel/` and `.acp/support/`.

This is a valid ACP base conformance shape.

### Full ACP

_Conditions: `capability-enabled`, `support-enabled`_

```yaml
profiles:
  kernel: required
  capability: enabled
  support: enabled
```
Project has all three layers.

This is a valid ACP base conformance shape.

### Conformance Validation


A project is conformant if:
1. `.acp/version.yaml` exists and is valid
2. `.acp/kernel/` exists with required structure
3. Declared profiles match actual directory structure
4. All objects follow schema defined in layer specs

### Compatibility vs Conformance


**Conformance** (project):
- Project structure is valid
- Files follow schema
- State transitions are legal

**Compatibility** (agent):
- Agent can parse ACP version
- Agent understands declared profiles
- Agent can execute operations

### Authorization vs Conformance


**Conformance** (project):
- Project follows ACP rules

**Authorization** (agent):
- Agent is allowed to modify project

### Minimum bootstrap conformance


A minimally conformant ACP bootstrap MUST:

- recognize ACP entry conditions
- distinguish ACP continuation from ACP project creation when project creation is the task
- treat `.acp/version.yaml` as the primary project ACP entry when present
- establish ACP version and profile context
- prefer support-guided intake when support is present
- prefer capability-first initial interpretation when capability data is present
- defer secondary operation mapping companions until finer routing is actually needed
- avoid unsafe direct mutation before ACP entry is established
- degrade safely under uncertainty

### Non-conformant bootstrap behavior


Bootstrap is not ACP-compatible if it:

- skips ACP entry judgment entirely
- ignores `.acp/version.yaml` when present
- ignores enabled profile interpretation
- treats ACP project creation requests as non-ACP merely because `.acp/version.yaml` is absent
- performs uncontrolled broad scanning before ACP entry context is established
- performs unsafe execution as the initial ACP action
- fabricates ACP understanding under uncertainty

### Minimum runtime conformance


A minimally conformant ACP Runtime MUST:

* interpret ACP version and enabled profiles
* perform project intake from ACP project entry
* preserve stage-gated minimum-first protocol usage
* support request formation
* support plan formation
* support change recording
* enforce compatibility guard behavior
* enforce kernel state legality
* degrade safely under unsupported conditions

### Non-conformant runtime behavior


A runtime is not ACP-compatible if it:

* ignores ACP version or enabled profiles
* skips mandatory request/plan/change progression
* bypasses kernel legality checks
* treats project-local support as protocol authority
* performs unsafe execution under unsupported interpretation
* suppresses required degraded behavior when uncertainty exists

## What This Public Draft Deliberately Omits

- research-track design exploration
- protocol-maintenance source materials
- repository-local workflow guidance
- non-allowlisted companions and repository guides

## Traceability Notice

Every retained public rule in this draft is traced in `trace_manifest.yaml`.
