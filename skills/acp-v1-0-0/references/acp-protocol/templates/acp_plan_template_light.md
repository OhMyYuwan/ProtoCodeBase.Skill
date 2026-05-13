# ACP Plan Standard Template v1.0.0 (Light)

Use this template for standard modification work.
Use it when compact would hide relevant execution detail,
but full diagnostic or transfer depth is not required.

Light means a complete execution blueprint with moderate detail.

# PLAN: <plan_id>

## Meta
- Related Request: <request_id>
- Summary: <one-line execution summary; not a result report>
- Primary Capability: <capability_id or none>
- Target Slice: <slice_id | _root | none>
- Affected Modules:
  - <module_or_path directly or materially affected>
- User Confirmation: <true | false>
- Created At: <ISO8601 timestamp>
- Status: <draft | confirmed | executing | done | cancelled | superseded>

## Goal and Boundary
- Original Need:
- Agent Understanding: <structured understanding of goal and constraints>
- Expected Direction: <high-level satisfaction path, not step detail>
- In Scope: <work this plan is expected to carry out>
- Out of Scope: <excluded work, follow-up, or deferred depth>

## Execution Strategy
- Strategy Summary: <how this plan will be executed at moderate detail>
- Route / Entry Points: <starting capability, module, or entry file set>

## Optional Analysis
- Include this section only when diagnosis, uncertainty, or path comparison matters.
- Observed Symptoms:
- Working Analysis: <current diagnosis, if needed>
- Preferred Path:
- Alternative Paths:

Add only the step blocks required for execution. Duplicate as needed.

### Step 1
- Objective:
- Capability / Slice: <if helpful for semantic grounding>
- Target Files: <likely touch points>
- Execution Notes: <how this step should be carried out; concise but actionable>
- Patch Intent: <what will change and why>
- Expected Result: <intermediate state after this step, if relevant>
- Verification: <observable check for completion>

### Step 2 (Optional)
- Objective:
- Capability / Slice: <if helpful for semantic grounding>
- Target Files: <likely touch points>
- Execution Notes: <how this step should be carried out; concise but actionable>
- Patch Intent: <what will change and why>
- Expected Result: <intermediate state after this step, if relevant>
- Verification: <observable check for completion>

## File Matrix
| File / Module | Planned Action | Related Step | Capability / Slice |
|---|---|---|---|
| <path> | modify / create / remove / rename | Step 1 | <capability/slice> |
| <path> | modify / create / remove / rename | Step 2 | <capability/slice> |

## Validation and Risk
- Acceptance Criteria: <conditions required for success and safe closure>
- User-Facing Success Signal: <how the user can tell it worked; omit if internal-only>
- Main Risks: <main execution, regression, or scope risks>
- Fallback Direction: <next safe path if this approach fails>
